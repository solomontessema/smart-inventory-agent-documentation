\# Course Syllabus: Foundations of Agentic AI



\## Course Title



Foundations of Agentic AI



\## Course Description



This course introduces the fundamental concepts, architecture, and design principles of Agentic AI systems. Students will learn how these proactive, autonomous, and multi-step intelligent systems differ from traditional, reactive AI models. The lesson covers the core components of an agent (LLMs, tools, memory), decision-making loops (AgentExecutor), key reasoning strategies (ReAct), and the role of prompt engineering in guiding agent behavior and autonomy.



\## Learning Objectives



Upon completion of this lesson, students will be able to:



1\. \*\*Define Agentic AI\*\* and articulate how it differs from traditional, reactive AI models.



2\. \*\*Identify the core components\*\* (Language Models, Tools, Memory, Executor) that constitute an Agentic AI system.



3\. \*\*Explain the conceptual flow\*\* of an agent, including input interpretation, planning, tool usage, reflection, and iteration.



4\. \*\*Describe\*\* the ReAct strategy and differentiate between various agent behavior models (Zero-Shot ReAct, Conversational ReAct, Plan-and-Execute).



5\. \*\*Design effective prompt templates\*\* that guide an agent's reasoning, tool selection, and reporting.



6\. \*\*Analyze the concept of autonomy\*\* in AI and explain how agents manage decision-making, complex task completion, and self-correction.



7\. \*\*Implement agents and chains\*\* in the LangChain framework for structured, multi-step tasks.



8\. \*\*Design and orchestrate complex task flows\*\* using LangGraph's graph-based reasoning model.



9\. \*\*Integrate agents with external data sources\*\* (SQLite) and communication channels (SMTP Email) to achieve operational utility.



\## Course Modules and Topics



\### Module 1: Foundations of Agentic AI: Overview



| Topic | Description | 

&nbsp;| ----- | ----- | 

| \*\*1.1 What is Agentic AI?\*\* | Definition of Agentic AI as a new class of intelligent systems. | 

| \*\*1.2 Reactive vs. Agentic Systems\*\* | Contrasting Agentic AI with traditional, reactive, and stateless AI. | 

| \*\*1.3 Key Agent Capabilities\*\* | Proactivity, adaptive behavior, dynamic tool selection, and contextual memory. | 

| \*\*1.4 Multi-Modal Interactions\*\* | How agents integrate and reason over text, data, visuals, and APIs. | 



\### Module 2: Understanding Agents and System Architecture



| Topic | Description | 

&nbsp;| ----- | ----- | 

| \*\*2.1 The Agent Concept\*\* | Agents as persistent entities for interpreting tasks and reasoning. | 

| \*\*2.2 Agents in Frameworks (LangChain/LangGraph)\*\* | Agents as control structures and decision-making loops. | 

| \*\*2.3 The Agent Conceptual Flow\*\* | The iterative process: input $\\rightarrow$ planning $\\rightarrow$ tool use $\\rightarrow$ reflection $\\rightarrow$ output. | 

| \*\*2.4 Core Agent Components\*\* | Role of Language Models, Prompt Templates, Tools, Memory, and the AgentExecutor. | 

| \*\*2.5 Reasoning Strategy: ReAct\*\* | Introduction to the ReAct strategy for dynamic reasoning and tool use. | 

| \*\*2.6 Agent Behavior Models\*\* | Zero-Shot ReAct, Conversational ReAct, and Plan-and-Execute. | 

| \*\*2.7 Designing Prompt Templates\*\* | Guiding agent reasoning, tool use, and reporting through structured prompts. | 

| \*\*2.8 Autonomy in Agentic AI\*\* | Decision-making, planning, and knowing when to stop or ask for human help. | 



\### Module 3: Building with LangChain



| Topic | Description | 

&nbsp;| ----- | ----- | 

| \*\*3.1 Overview and Object Model\*\* | LangChain as a modular framework connecting LLMs with real-world applications. | 

| \*\*3.2 Building and Composing Chains\*\* | Creating deterministic, predefined pipelines for structured, multi-step tasks. | 

| \*\*3.3 Prompt Engineering in LangChain\*\* | Dynamically shaping inputs to LLMs using templates, context, and conditional paths. | 

| \*\*3.4 Implementing and Registering Tools\*\* | Practical guidance on creating and integrating tools for agent execution. | 

| \*\*3.5 Advanced Agent Construction\*\* | Building adaptive systems with dynamic tool selection, memory, and long-term planning. | 

| \*\*3.6 Testing and Evaluation\*\* | Methods for ensuring agent accuracy, coherence, and safety in real-world scenarios. | 



\### Module 4: Task Flow with LangGraph



| Topic | Description | 

&nbsp;| ----- | ----- | 

| \*\*4.1 Overview of LangGraph\*\* | Introduction to the graph-based reasoning workflow model. | 

| \*\*4.2 Designing Graph-Based Workflows\*\* | Using nodes and edges to define structured task flows with shared state. | 

| \*\*4.3 Conditional and Branching Execution\*\* | Implementing dynamic branching based on state evolution and conditional logic (`next` key). | 

| \*\*4.4 Stateful and Modular Components\*\* | Designing nodes that share and update a common mutable state for context-aware workflows. | 

| \*\*4.5 Integrating LangChain within LangGraph\*\* | Orchestrating LangChain agents, tools, and chains as modular nodes (Agent-as-a-Node pattern). | 

| \*\*4.6 Testing and Observation\*\* | Techniques for testing and observing execution paths using structured logs and simulations. | 



\### Module 5: Data Connectivity \& Email Services



| Topic | Description | 

&nbsp;| ----- | ----- | 

| \*\*5.1 Overview: Operational Utility\*\* | Integrating agents with real-world systems and communication channels. | 

| \*\*5.2 Connecting to SQLite\*\* | Building the foundation for agents to reason over and query lightweight, structured SQL data. | 

| \*\*5.3 Natural Language to SQL with GPT\*\* | Leveraging LLMs to convert natural language questions into valid, data-driven SQL queries. | 

| \*\*5.4 Sending Emails with SMTP\*\* | Incorporating SMTP for outbound communication (alerts, summaries, updates). | 

| \*\*5.5 Formatter Replies and Attaching Insights\*\* | Formatting agent outputs for clarity, actionability, and well-structured email delivery; autonomous workflow. |

