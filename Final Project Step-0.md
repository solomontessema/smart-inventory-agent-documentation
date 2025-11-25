### Step 0.

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

