### Step 1. Creating the Web Search Tool

---

The recommended way is to **re-create the project** based on the instructions in the following section.  
However, if you prefer to clone the project directly, use the following Git command to review and modify it in your local Python environment:

```bash
git clone -b step-1 https://github.com/solomontessema/smart-inventory-agent
cd smart-inventory-agent
```

Or, if you want to open it in Google Colab, click the badge below:

<a href="https://colab.research.google.com/github/solomontessema/Agentic-AI-with-Python/blob/main/notebooks/Final%20Project/inventory_agent_step_1.ipynb" target="_parent"> <img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/> </a>

---

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
langchain==1.1.0
langchain-openai==1.1.0
langgraph==1.0.4
langchain-tavily==0.2.12
python-dotenv==1.1.1
streamlit==1.51.0
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
 
#### Supplier Search Tool (tools/web_search_tool.py)

```python
import os
from langchain_tavily import TavilySearch
from langchain.tools import tool
from config import TAVILY_API_KEY

# Initialize Tavily search tool
search_tool = TavilySearch(
    api_key=TAVILY_API_KEY,
    max_results=5,
    include_answer=True
)

@tool
def web_search_tool(query: str) -> str:
    """Perform a web search using Tavily and return formatted results."""
    results = search_tool.invoke({"query": query})
    output = f"Search results for **{query}**:\n"
    for result in results["results"]:
        title = result.get("title", "No title")
        url = result.get("url", "")
        snippet = result.get("content", "")
        output += f"- \\[{title}\\]({url})\n  {snippet}\n\n"

    return output.strip()

```

#### agents/inventory_agent.py

```python
ffrom langchain.agents import create_agent
from langchain_openai import ChatOpenAI
from tools.web_search_tool import web_search_tool
from config import OPENAI_API_KEY 

llm = ChatOpenAI(
    model="gpt-4o-mini",
    temperature=0
)


inventory_agent = create_agent(
    model=llm, 
    tools=[web_search_tool], 
    system_prompt="You are a helpful assistant."
    )

```

#### main.py

```python
from agents.inventory_agent import  inventory_agent
result = inventory_agent.invoke({"messages": [{"role": "user", "content": "Find suppliers for coca cola in bulk."}]})
agent_answer = result["messages"][-1]
print(agent_answer.content)

```

Now the agent can use the only tool available, the supplier finder tool.