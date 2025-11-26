###  Creating a database reader tool.

**tools/database_reader.py**

```python
from typing import List, Tuple, Any
import sqlite3
from config import DB_PATH

class SQLiteConnector:
    def __init__(self, db_path=DB_PATH):

        uri_path = f"file:///{db_path}"
        self.conn = sqlite3.connect(uri_path, uri=True, check_same_thread=False)
        self.cursor = self.conn.cursor()

    def list_tables(self):
        self.cursor.execute("SELECT name FROM sqlite_master WHERE type='table';")
        return [row[0] for row in self.cursor.fetchall()]

    def describe_table(self, table_name):
        self.cursor.execute(f"PRAGMA table_info({table_name});")
        return self.cursor.fetchall()

    def get_schema_summary(self):
        summary = []
        for table in self.list_tables():
            cols = self.describe_table(table)
            formatted = f"Table: {table} Columns: {[col[1] for col in cols]}"
            summary.append(formatted)
        return "\n".join(summary)

connector = SQLiteConnector()

def _exec_sql(sql: str) -> Tuple[List[str], List[Tuple[Any, ...]]]:
    cur = connector.conn.cursor()
    cur.execute(sql)
    rows = cur.fetchall()
    cols = [d[0] for d in cur.description] if cur.description else []
    return cols, rows

def _format_table(cols: List[str], rows: List[Tuple[Any, ...]]) -> str:
    if not cols:
        return "No result columns."
    header = "|".join(cols)
    if not rows:
        return header  # header only when no matches
    lines = [header]
    for r in rows:
        lines.append("|".join("" if v is None else str(v) for v in r))
    return "\n".join(lines)

def read_database_tool(sql: str) -> str:

    if not isinstance(sql, str) or not sql.strip():
        return "Error executing SQL: empty query."

    try:
        cols, rows = _exec_sql(sql)
        return _format_table(cols, rows)
    except Exception as e:
        return f"Error executing SQL: {e}"
```

**Add the Following lines of code to config.py file.**

```python
BASE_DIR = os.path.dirname(os.path.abspath(__file__))
DB_PATH = os.path.join(BASE_DIR, "database", "data.db")
```

**Modify the prompt template**
**Add the Database Reader Tool (agents/inventory_agent.py)**

```python
#....
#....
from tools.database_reader import read_database_tool
#....
#....
#....


prompt_template = PromptTemplate(
    input_variables=["input", "tools", "tool_names"],
    template="""
You are a helpful assistant. You have access to the following tools:
{tools}
To answer inventory questions use products and transactions tables. Join them if needed.
The user's query is: {input}

You must follow the ReAct format:
Thought: I need to determine which tool to use.
Action: [tool_name]
Action Input: [input to the tool]
Observation: [Tool result]
...
Thought: I have the final answer.
Final Answer: [The answer to the user]

Available tool names: {tool_names}

{agent_scratchpad}
"""
)


#....
#....

tools = [
    Tool(
        name="Database Reader",
        func=read_database_tool,
        description="Execute a SQL query and return raw tabular results. Use for inventory & thresholds."
    ),
    Tool(
        name="Supplier Finder",
        func=web_search_tool,
        description="Search suppliers by product name, barcode, or category."
    ),
]

```

**Replace the code in main.py with the following code.**

```python

from agents.inventory_agent import run_inventory_agent

run_inventory_agent(
    '''
    check our inventory, identify low stock items where sum(quantity) below threshold level, 
    search for suppliers for our low stock items if there is any lowstock item.
    '''
)
 
```