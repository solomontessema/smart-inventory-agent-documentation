
<img src="https://ionnova.com/img/ionnova_logo_2.png" alt="IONNOVA Logo" width="80" height="80" />

### Step 1.

#### Add your API Keys to the .env file

```python
OPENAI_API_KEY=your open api key
TAVILY_API_KEY=your tabily api key
```

#### Create a Virtual Environment

```python
python -m venv venv
```

This creates a venv/ folder containing the Python interpreter and site packages.

#### Activate the Environment

Windows:

```python
venv\Scripts\activate
```

macOS/Linux:

```python
source venv/bin/activate
```

Once activated, your terminal will show the environment name (e.g., (venv)), and all pip installs will go into this isolated space.

#### Add the following to your  requirements.txt file:

```
langchain==0.3.27
langchain-core==0.3.78
langchain-community==0.3.31
langchain-tavily==0.2.12
langchain-text-splitters==0.3.11
openai==2.6.0
python-dotenv==1.1.1
flask==3.1.2
```

#### Install Dependencies

```
pip install -r requirements.txt
```

#### Git Initialization

To enable version control and streamline collaboration, initialize Git in the root of your project:

```
git init
```

Then, configure your .gitignore file to exclude sensitive and unnecessary files from version tracking:

```
venv/
__pycache__/
*.pyc
*.db
.env
logs/
.vscode/
.idea/
.tox/
.coverage
.cache/
*.log
*.tmp
```

#### First Commit

git add .
git commit -m "Initial project structure for Smart Inventory Agent"


#### Optional: Create a GitHub Repo

```
git remote add origin https://github.com/yourusername/smart-inventory-agent.git
git push -u origin main
```

This keeps your main branch clean while you builand

#### Agent Implementation

We'll start by creating the core LangChain agents using GPT-4o-mini. Your API key will be loaded from the .env file using python-dotenv.

**Environment Setup**

In config.py, load your OpenAI key:

```python
import os
from dotenv import load_dotenv

load_dotenv()
OPENAI_API_KEY = os.getenv("OPENAI_API_KEY")
TAVILY_API_KEY=os.getenv("TAVILY_API_KEY") 
```
 
#### Supplier Search Tool (tools/web_searcher.py)

```python
import os
from langchain_tavily import TavilySearch
from config import TAVILY_API_KEY

# Initialize Tavily search tool
search_tool = TavilySearch(
    api_key=TAVILY_API_KEY,
    max_results=3,
    include_answer=True
)

def web_search_tool(query: str) -> str:

    if not query:
        return "No query provided."

    try:
        results = search_tool.invoke({"query": query})

        if not results or not results.get("results"):
            return f"No results found for '{query}'."

        output = f"Search results for **{query}**:\n"
        for result in results["results"]:
            title = result.get("title", "No title")
            url = result.get("url", "")
            snippet = result.get("content", "")
            output += f"- \\[{title}\\]({url})\n  {snippet}\n\n"

        return output.strip()

    except Exception as e:
        return f"Error during search: {str(e)}"

```

#### agents/inventory_agent.py

```python
from langchain.agents import Tool, AgentExecutor, create_react_agent
from langchain_community.chat_models import ChatOpenAI
from langchain.prompts import PromptTemplate
from config import OPENAI_API_KEY 

llm = ChatOpenAI(
    model="gpt-4o-mini",
    temperature=0,
    api_key=OPENAI_API_KEY
)


tools = [
    Tool(
        name="Supplier Finder",
        func=web_search_tool,
        description="Search suppliers by product name, barcode, or category."
    )
]


prompt_template = PromptTemplate(
    input_variables=["input", "tools", "tool_names"],
    template="""
You are a helpful assistant. You have access to the following tools:
{tools}

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

agent = create_react_agent(llm=llm, tools=tools, prompt=prompt_template)

inventory_agent = AgentExecutor(
    agent=agent,
    tools=tools,
    verbose=True, 
    handle_parsing_errors=True,
)


def run_inventory_agent(user_input: str) -> str:
    """Invokes the agent with a user query."""
    result = inventory_agent.invoke({
        "input": user_input,
    })
    return result.get("output", "")

print(run_inventory_agent("Check the current stock of product X."))
```

Now the agent can use the only tool available, the supplier finder tool.