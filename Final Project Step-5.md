### Create chat.py file in the root directory and add the following code to it.

---

The recommended way is to **re-create the project** based on the instructions in the following section.  
However, if you prefer to clone the project directly, use the following Git command to review and modify it in your local Python environment:

```bash
git clone -b step-2 https://github.com/solomontessema/smart-inventory-agent
cd smart-inventory-agent
```

Or, if you want to open it in Google Colab, click the badge below:

<a href="https://colab.research.google.com/github/solomontessema/Agentic-AI-with-Python/blob/main/notebooks/Final%20Project/inventory_agent_step_2.ipynb" target="_parent"> <img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/> </a>

---
In the root directory create file `app.py`

```python
import streamlit as st
from agents.inventory_agent import inventory_agent

st.set_page_config(page_title="Chat with Inventory Agent", layout="centered")
st.title("📦 Chat with Inventory Agent")

  
# Initialize chat history
if "messages" not in st.session_state:
    st.session_state.messages = []

# Display chat history
for msg in st.session_state.messages:
    with st.chat_message(msg["role"]):
        st.markdown(msg["content"])

# User input
user_input = st.chat_input("Ask the inventory agent...")

if user_input:
    # Add user message to history
    st.session_state.messages.append({"role": "user", "content": user_input})
    with st.chat_message("user"):
        st.markdown(user_input)

    # Invoke the agent
    result = inventory_agent.invoke({"messages": st.session_state.messages})
    agent_msg = result["messages"][-1]      
    agent_text = agent_msg.content          

    st.session_state.messages.append({"role": "assistant", "content": agent_text})
    with st.chat_message("assistant"):
        st.markdown(agent_text)
```

Open a new terminal and run `streamlit run app.py`
