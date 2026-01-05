### Environment Setup

---

The recommended way is to **re-create the project** based on the instructions in the following section.  
However, if you prefer to clone the project directly, use the following Git command to review and modify it in your local Python environment:

```bash
git clone -b step-4 https://github.com/solomontessema/smart-inventory-agent
cd smart-inventory-agent
```

Or, if you want to open it in Google Colab, click the badge below:

<a href="https://colab.research.google.com/github/solomontessema/Agentic-AI-with-Python/blob/main/notebooks/Final%20Project/inventory_agent_step_4.ipynb" target="_parent"> <img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/> </a>

---


**In .env file add the following:**

```python
...
...
AGENT_NAME = your_agent_name
AGENT_EMAIL_ADDRESS = your_agent_email@gmail.com
AGENT_EMAIL_PASSWORD = gmail_app_password
RECIPIENT_NAME = Recipient_First_Name
RECIPIENT_EMAIL_ADDRESS = recipient_emailaddress@gmail.com

```


**In config.py, add the following:**

```python 
...
...
...
AGENT_NAME = os.getenv("AGENT_NAME")
AGENT_EMAIL_ADDRESS = os.getenv("AGENT_EMAIL_ADDRESS")
AGENT_EMAIL_PASSWORD = os.getenv("AGENT_EMAIL_PASSWORD")
RECIPIENT_NAME = os.getenv("RECIPIENT_NAME")
RECIPIENT_EMAIL_ADDRESS = os.getenv("RECIPIENT_EMAIL_ADDRESS")
```

**In tools/email_sender.py, add the following:**

```python

import smtplib
from email.mime.text import MIMEText
from email.mime.multipart import MIMEMultipart
from email.mime.application import MIMEApplication
from typing import Optional
from langchain.tools import tool
from config import (
    AGENT_EMAIL_ADDRESS,
    AGENT_EMAIL_PASSWORD,
    RECIPIENT_EMAIL_ADDRESS
)

# If not in config, define fallbacks:
SMTP_SERVER = "smtp.gmail.com"
SMTP_PORT = 587


def send_email(
    to_address: str,
    subject: str,
    body: str,
    attachment_path: Optional[str] = None
) -> str:
    """Send a plaintext email with optional attachment."""
    if not AGENT_EMAIL_ADDRESS or not AGENT_EMAIL_PASSWORD:
        raise ValueError("Missing email credentials (AGENT_EMAIL_ADDRESS or AGENT_EMAIL_PASSWORD).")

    msg = MIMEMultipart("alternative")    
    msg["From"] = AGENT_EMAIL_ADDRESS
    msg["To"] = to_address
    msg["Subject"] = subject
    msg.attach(MIMEText(body, "plain"))
    msg.attach(MIMEText(body, "html")) 

    if attachment_path:
        with open(attachment_path, "rb") as f:
            part = MIMEApplication(f.read(), Name=attachment_path)
        part["Content-Disposition"] = f'attachment; filename="{attachment_path}"'
        msg.attach(part)
    with smtplib.SMTP(SMTP_SERVER, SMTP_PORT) as server:
        server.starttls()
        server.login(AGENT_EMAIL_ADDRESS, AGENT_EMAIL_PASSWORD)
        server.send_message(msg)

    return "Email sent successfully."

@tool
def send_email_tool(input_str: str) -> str:
    """Send an email to the recipient. Input format: 'subject || body'."""
    subject, body = [x.strip() for x in input_str.split("||", 1)]

    try:
        result = send_email(
            to_address=RECIPIENT_EMAIL_ADDRESS,
            subject=subject,
            body=body
        )
        return result
    except Exception as e:
        return f"Failed to send email: {e}"

```

**In tools/log_tracker.py, add the following:**

```python
import sqlite3
from langchain.tools import tool
from datetime import datetime
from config import DB_PATH

 
def track_log(action: str, details: str = "") -> str:
 
    conn = sqlite3.connect(DB_PATH)
    cursor = conn.cursor()

    timestamp = datetime.now().isoformat()
    cursor.execute(
        "INSERT INTO logs (timestamp, action, details) VALUES (?, ?, ?)",
        (timestamp, action, details)
    )

    conn.commit()
    conn.close()

    return f"Logged action: {action}"

@tool
def track_log_tool(input: str) -> str:
    """Log actions. Input format "Queried inventory | Checked products below threshold"""
    try:
        action, details = input.split(" | ", 1)
    except ValueError:
        action, details = input, ""
    return track_log(action.strip(), details.strip())
```

**Add the email_sender and log_tracker tools to the agent's tool list**

```python
from langchain.agents import create_agent
from langchain_openai import ChatOpenAI
from tools.web_search_tool import web_search_tool
from tools.database_reader import read_database_tool
from tools.email_sender import send_email_tool
from tools.log_tracker import track_log_tool
from config import OPENAI_API_KEY 
from config import AGENT_NAME

llm = ChatOpenAI(
    model="gpt-4o-mini",
    temperature=0
)


inventory_agent = create_agent(

    model=llm,

    tools=[web_search_tool,read_database_tool,send_email_tool,track_log_tool],

    system_prompt=f"""You are a helpful assistant to manage inventory and find bulk suppliers online for products in the database.

    when replying, use well formatted markdown.

    when sending email, format it well with greetings and signature your name is {AGENT_NAME}. Format it well as html. After completing the requested task, log your actions using the track_log_tool.""",

    )

```

**Modify main.py as follows.**

```python

from agents.inventory_agent import  inventory_agent

result = inventory_agent.invoke({"messages": [{"role": "user", 
                                               "content": """How many distinct products are there in inventory? 
                                               List their barcode and name. 
                                               Search online for bulk suppliers for those products and send me well formatted email. """} ]})

agent_answer = result["messages"][-1]
print(agent_answer.content)

```

