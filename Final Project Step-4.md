### Environment Setup

**In .env file add the following:**

```python
...
...
AGENT_EMAIL_ADDRESS = your_agent_email@gmail.com
AGENT_EMAIL_PASSWORD = gmail_app_password

```



**In config.py, add the following:**

```python 
...
...
...
AGENT_EMAIL_ADDRESS= os.getenv("AGENT_EMAIL_ADDRESS") 
AGENT_EMAIL_PASSWORD= os.getenv("AGENT_EMAIL_PASSWORD")  
```

**In tools/email_sender.py, add the following:**

```python

import smtplib
from email.mime.text import MIMEText
from email.mime.multipart import MIMEMultipart
from email.mime.application import MIMEApplication
from typing import Optional
from config import (
    AGENT_EMAIL_ADDRESS,
    AGENT_EMAIL_PASSWORD,
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


def send_email_tool(input_str: str) -> str:

    from config import BOSS_EMAIL_ADDRESS
    subject, body = [x.strip() for x in input_str.split("||", 1)]

    try:
        result = send_email(
            to_address=BOSS_EMAIL_ADDRESS,
            subject=subject,
            body=body
        )
        return result
    except Exception as e:
        return f"Failed to send email: {e}"

```

**Add the email_sender tool to the agent's tool list and modify the prompt template in (agents/inventory_agent.py)**

```python
...
...
from tools.email_sender import send_email_tool
...
...
...

tools = [
 ...
 ...
 ...
    Tool(
        name="Email Sender",
        func=send_email_tool,
        description="Send an email using: subject || body"
    ),
]
...
...
prompt_template = PromptTemplate(
...
...
...

When using the Email Sender tool:
- Your name is Agent Smith. Address the recipients as "Team"
- Format the body as professional HTML. Use clear title and heading. Include sentences, table of summary, and greetings

Available tools names:{tool_names}
)
```

**And finally modify the input to include an email summary request.**

```python

...

run_inventory_agent(
    '''
    .....
    .....
    send me an email summary of the low stock items and the suppliers with links. 
    '''
)
 
```