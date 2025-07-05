# AI Agent to Chat with and Analyze Airtable Data

This project deploys a sophisticated AI agent that allows you to have a conversation with your Airtable data. Instead of manually searching through bases and tables, you can ask questions in natural language to retrieve, filter, and even perform calculations on your records. [cite_start]The agent is equipped with a suite of tools to dynamically query datasets, generate data visualizations, and retain conversational context. [cite: 111]

## Overview

This solution is designed to make data interaction more intuitive and efficient. The AI agent acts as a knowledgeable assistant for your Airtable data, capable of understanding complex requests and executing the necessary steps to provide an answer. [cite_start]It can fetch records, calculate sums and averages, and even generate maps from geographic data. [cite: 112]

## Features

* [cite_start]**Conversational Interface:** Interact with your data using natural language through a chat interface. [cite: 111]
* [cite_start]**Dynamic Data Retrieval:** The agent can query any base or table you specify, fetching records dynamically based on your request. [cite: 113]
* [cite_start]**Advanced Filtering:** Can construct complex filter formulas from simple text descriptions to perform tailored searches. [cite: 115]
* [cite_start]**Data Analysis & Math Functions:** Equipped with a code tool to perform aggregations and mathematical functions like counting records, summing values, or calculating averages. [cite: 105]
* **Map Generation:** Can generate and display map images from location data stored in your tables.
* [cite_start]**Context-Aware Memory:** Retains the context of the conversation, allowing for follow-up questions and a more natural dialogue. [cite: 114]

## How It Works

This project is structured as two separate but connected workflows:

1.  **Main Agent Workflow:** This is the primary workflow that you interact with.
    * A `Chat Trigger` receives the user's message.
    * The `AI Agent` node processes the request. Based on the user's prompt, it decides which tool to use.
    * The agent has access to several tools (`get_bases`, `get_base_tables_schema`, `search`, `code`, `create_map`), which are implemented as calls to the second workflow.
    * [cite_start]The agent maintains conversation history using the `Window Buffer Memory` node. [cite: 114]

2.  **Airtable Tools Workflow:** This workflow contains the logic for each tool the agent can use.
    * An `Execute Workflow Trigger` receives the command from the agent (e.g., 'search').
    * A `Switch` node routes the request to the correct logic block.
    * For **searches**, it uses an OpenAI call to translate a natural language filter (e.g., "show me products under $50") into a valid Airtable `filterByFormula`, then queries the Airtable API.
    * For **code execution**, it uses the OpenAI Assistants API (Code Interpreter) to run math functions or generate images.
    * It then returns the result to the main agent workflow.

## Technologies Used

* n8n
* Airtable
* OpenAI (Chat and Assistants API)
* Mapbox (for map generation)

## Setup & Configuration

1.  **Separate Workflows:** This project requires two workflows. [cite_start]You need to create a new, blank workflow and move the nodes from "Workflow 2" (starting from the `Execute Workflow Trigger` node) into it. [cite: 116]
2.  **Replace Credentials:** You must replace all placeholder credentials in both workflows:
    * **OpenAI:** Add your OpenAI API key to the `OpenAI Chat Model` node and the `OpenAI - ...` HTTP Request nodes.
    * **Airtable:** Add your Airtable Personal Access Token to the `Get Bases`, `Get Base/Tables schema`, and `Airtable - Search records` nodes.
3.  **Update Workflow IDs:** In the main agent workflow, update the `toolWorkflow` nodes (`get_bases`, `search`, etc.) to point to the correct ID of your newly created "Airtable Tools" workflow.
4.  **Mapbox Public Key:** In the `Create map image` tool node, you must replace `<your_public_key>` with your actual public key from Mapbox.

## How to Use

1.  Activate both workflows.
2.  Open the chat interface for the main agent workflow.
3.  Start by asking the agent what bases it has access to (e.g., "list all bases").
4.  Then, ask it for the schema of a specific base to see the tables and fields.
5.  [cite_start]Finally, ask it to search, filter, or analyze the data within a table (e.g., "how many records are in the Orders table in the Sales CRM base?"). [cite: 117]

---

### Need Custom Automation? Let's Talk!

If you're looking to implement powerful business automations, custom integrations, or full-stack projects like this one, I'm available for consulting. Let's discuss how I can help you streamline your business processes.

Schedule a free 15-minute coffee chat to explore your project needs.
[Book a chat here😁](https://cal.com/closegem/coffee-chat)
