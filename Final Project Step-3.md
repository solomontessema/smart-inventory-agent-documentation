###  Creating a database reader tool.

---

The recommended way is to **re-create the project** based on the instructions in the following section.  
However, if you prefer to clone the project directly, use the following Git command to review and modify it in your local Python environment:

```bash
git clone -b step-3 https://github.com/solomontessema/smart-inventory-agent
cd smart-inventory-agent
```

Or, if you want to open it in Google Colab, click the badge below:

<a href="https://colab.research.google.com/github/solomontessema/Agentic-AI-with-Python/blob/main/notebooks/Final%20Project/inventory_agent_step_3.ipynb" target="_parent"> <img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/> </a>

---

**tools/database_reader.py**

```python
from typing import List, Tuple, Any
import sqlite3
from langchain.tools import tool
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

@tool
def read_database_tool(sql: str) -> str:
    """Execute a SQL query against the SQLite database and return formatted results."""
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

**Add the Database Reader Tool (agents/inventory_agent.py)**

```python
from langchain.agents import create_agent
from langchain_openai import ChatOpenAI
from tools.web_search_tool import web_search_tool
from tools.database_reader import read_database_tool # ✨ import read_database_tool
from config import OPENAI_API_KEY 

llm = ChatOpenAI(
    model="gpt-4o-mini",
    temperature=0
)

inventory_agent = create_agent(
    model=llm, 
    tools=[web_search_tool,read_database_tool], # ✨ Add read_database_tool
    system_prompt="You are a helpful assistant."
    )

```

**Replace the code in main.py with the following code.**

```python

from agents.inventory_agent import  inventory_agent

result = inventory_agent.invoke({"messages": [{"role": "user", 
                                               "content": """How many distinct products are there in inventory? 
                                               List their barcode and name. 
                                               Search online for bulk suppliers for those products."""} ]})

agent_answer = result["messages"][-1]
print(agent_answer.content)

```