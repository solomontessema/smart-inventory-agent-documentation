### Step 0.

---

The recommended way is to **re-create the project** based on the instructions in the following section.  
However, if you prefer to clone the project directly, use the following Git command to review and modify it in your local Python environment:

```bash
git clone -b step-0 https://github.com/solomontessema/smart-inventory-agent
cd smart-inventory-agent
```

Or, if you want to open it in Google Colab, click the badge below:

<a href="https://colab.research.google.com/github/solomontessema/Agentic-AI-with-Python/blob/main/notebooks/Final%20Project/inventory_agent_step_0.ipynb" target="_parent"> <img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/> </a>

---

#### 📁 smart-inventory-agent (Project Structure)

```
├── agents/
│   └── inventory_agent.py          
│
|
├── database/                       
│
|                       
├── tools/
│   └── database_reader.py                  
│   └── email_sender.py             
│   └── log_tracker.py             
│   └── web_search_tool.py            
│           
|       
|── .env                            
|── .gitignore                     
├── config.py                       
├── main.py                         
├── README.md                            
└── requirements.txt              
```

Create the project root directory, open a CMD prompt there, and run the following script to initialize the structure.

```
mkdir agents
mkdir tools
mkdir database
type nul > agents\inventory_agent.py
type nul > tools\database_reader.py
type nul > tools\web_search_tool.py
type nul > tools\email_sender.py
type nul > tools\log_tracker.py
type nul > config.py
type nul > main.py
type nul > requirements.txt
type nul > README.md
type nul > .env
type nul > .gitignore
echo Project directories and files created successfully.
pause
```

