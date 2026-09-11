# ✈️ TravelPilot-MCP — MCP-Powered Multi-Agent Travel Planner

> **Plan the trip. Connect the tools. Let the agents orchestrate the journey.**

TravelPilot-MCP is a **real-world multi-agent AI travel planning system** built with **LangGraph, Groq Llama, and the Model Context Protocol (MCP)**.

The system uses multiple specialized AI agents to collaboratively plan a complete trip, including **flights, hotels, weather, sightseeing, and itinerary planning**.

The key architectural difference is the use of **MCP as a standardized tool integration layer**. Instead of tightly coupling the application with individual API implementations, external capabilities such as flight and weather services are exposed through MCP servers and consumed by the AI workflow as reusable tools.

This makes the architecture more **modular, extensible, and tool-agnostic**.

---

## 🌟 Why TravelPilot-MCP?

Traditional AI applications often integrate every external API directly into the application:

```text
Application
   │
   ├── Flight API Code
   ├── Weather API Code
   ├── Hotel API Code
   └── Search API Code
```

As the number of tools grows, the application can become tightly coupled to individual services.

TravelPilot-MCP introduces a standardized tool layer:

```text
                  AI Application
                       │
                       ▼
                  LangGraph
                       │
                       ▼
                    MCP
                       │
          ┌────────────┼────────────┐
          ▼            ▼            ▼
      Flight MCP   Weather MCP   Search MCP
          │            │            │
          ▼            ▼            ▼
     Flight Data   Weather Data  Web Results
```

The AI agents interact with capabilities through MCP rather than needing to understand the implementation details of each external service.

---

# 🤖 Multi-Agent Architecture

TravelPilot-MCP uses specialized agents that collaborate to build a complete travel plan.

```text
                         👤 USER
                            │
                            ▼
                    💬 Travel Request
                            │
                            ▼
                    🧠 LangGraph
                            │
          ┌─────────────────┼─────────────────┐
          │                 │                 │
          ▼                 ▼                 ▼
    ✈️ Flight Agent    🏨 Hotel Agent   🗓️ Itinerary Agent
          │                 │                 │
          ▼                 ▼                 ▼
      Flight MCP       Search / Data      Planning Logic
          │                 │                 │
          └─────────────────┼─────────────────┘
                            │
                            ▼
                    🤖 Final Response Agent
                            │
                            ▼
                     ✨ Complete Trip Plan
```

Each agent focuses on a specific responsibility while LangGraph coordinates the overall workflow.

---

# 🔌 MCP Integration

The **Model Context Protocol (MCP)** provides a standardized way for AI applications to interact with external tools and capabilities.

In TravelPilot-MCP, MCP is used to expose travel-related functionality to the AI system.

### Current MCP integrations

* ✈️ **AviationStack MCP** — Flight-related information
* 🌤️ **Weather MCP** — Weather information
* 🔎 **Tavily** — Web search capabilities

Conceptually:

```text
             LangGraph AI Agents
                     │
                     ▼
              MCP Tool Layer
                     │
        ┌────────────┼────────────┐
        ▼            ▼            ▼
   ✈️ Flight      🌤️ Weather    🔎 Search
      MCP            MCP           Tool
        │            │              │
        ▼            ▼              ▼
 AviationStack  OpenWeatherMap    Tavily
```

This approach allows external capabilities to be separated from the core agent workflow.

---

# 🧠 How MCP Changes the Architecture

Without MCP, an application may directly implement API-specific integrations:

```text
Agent
 │
 ├── Flight API Implementation
 ├── Weather API Implementation
 └── Search API Implementation
```

With MCP:

```text
Agent
 │
 ▼
MCP Client
 │
 ├── Flight MCP Server
 ├── Weather MCP Server
 └── Other MCP Servers
```

This separation makes it easier to add or replace tools without significantly changing the core agent workflow.

For example, a future version could add:

```text
              MCP Client
                   │
       ┌───────────┼───────────┐
       ▼           ▼           ▼
    Flights     Weather      Hotels
       │           │           │
       ▼           ▼           ▼
     MCP          MCP          MCP
```

---

# 🔄 Complete Workflow

A typical travel request flows through the system like this:

```text
👤 User
 │
 ▼
"Plan a 7-day Japan trip under ₹2,00,000"
 │
 ▼
🧠 LangGraph
 │
 ├───────────────┐
 ▼               ▼
✈️ Flight      🏨 Hotel
Agent           Agent
 │               │
 ▼               ▼
Flight MCP    Search / Data
 │               │
 └───────┬───────┘
         ▼
  🗓️ Itinerary Agent
         │
         ▼
  🌤️ Weather MCP
         │
         ▼
🤖 Final Response Agent
         │
         ▼
✨ Complete Travel Plan
```

---

# ✨ Key Features

* 🤖 **Multi-Agent Architecture** using LangGraph
* 🔌 **MCP-Based Tool Integration**
* ✈️ **Flight Search** through AviationStack MCP
* 🏨 **Hotel Search** and travel information
* 🌤️ **Weather Information** through Weather MCP
* 🔎 **Tavily Web Search**
* 🗓️ **Automated Itinerary Planning**
* 🧠 **PostgreSQL Conversation Memory**
* ⚡ **Groq Llama** for fast LLM inference
* 🌐 **Real-Time Travel Information**
* 🧩 **Modular Tool Architecture**
* 💬 **Interactive Travel Planning Interface**

---

# 🧩 Agent Responsibilities

## ✈️ Flight Search Agent

Responsible for finding relevant flight information for the requested trip.

The agent can interact with the flight capability through the **AviationStack MCP server**.

```text
Travel Requirements
       │
       ▼
Flight Agent
       │
       ▼
AviationStack MCP
       │
       ▼
Flight Information
```

---

## 🏨 Hotel Search Agent

Responsible for researching suitable accommodation options based on the travel requirements.

The agent can use available search capabilities to identify relevant hotel information.

---

## 🗓️ Itinerary Planning Agent

Combines the travel requirements and retrieved information to construct a structured itinerary.

It considers factors such as:

* Destination
* Duration
* Activities
* Travel requirements
* Budget
* Available information

---

## 🌤️ Weather Capability

Weather information can be accessed through an MCP-based weather server.

```text
Agent
 │
 ▼
Weather MCP
 │
 ▼
OpenWeatherMap
 │
 ▼
Weather Information
```

This allows weather information to become another reusable capability within the agent ecosystem.

---

## 🤖 Final Response Agent

The final agent combines the outputs from the different components and produces a unified travel plan.

```text
Flights
   │
Hotels
   │
Weather
   │
Itinerary
   │
   ▼
🤖 Final Agent
   │
   ▼
Complete Travel Plan
```

---

# 🧠 PostgreSQL Memory

TravelPilot-MCP uses **PostgreSQL** for persistent conversation memory.

This allows the application to maintain information from previous interactions rather than treating every conversation as completely independent.

Conceptually:

```text
User Conversation
       │
       ▼
   LangGraph
       │
       ▼
PostgreSQL Memory
       │
       ▼
Previous Context
       │
       ▼
Better Follow-up Responses
```

---

# 🛠️ Tech Stack

## 🤖 AI & Agent Framework

* **LangGraph** — Multi-agent workflow orchestration
* **LangChain** — LLM and tool integration
* **Groq** — LLM inference
* **Llama** — Large Language Model

## 🔌 Tool Integration

* **Model Context Protocol (MCP)**
* AviationStack MCP
* Weather MCP
* Tavily Search

## 💾 Database

* **PostgreSQL**
* LangGraph PostgreSQL checkpointing / memory

## 🖥️ Application

* Python
* Streamlit

---

# 🔐 API Keys

The project requires API keys for the external services used by the application.

### Groq

Used for LLM inference.

```text
GROQ_API_KEY=your_groq_api_key
```

### Tavily

Used for web search.

```text
TAVILY_API_KEY=your_tavily_api_key
```

### AviationStack

Used by the AviationStack MCP server.

```text
AVIATION_STACK_API_KEY=your_api_key
```

### OpenWeatherMap

Used by the weather MCP server.

```text
OPENWEATHER_API_KEY=your_api_key
```

### PostgreSQL

Example database connection:

```text
DATABASE_URL=postgresql://username:password@localhost:5432/database_name
```

> ⚠️ Never commit API keys, passwords, or `.env` files to GitHub.

---

# 🚀 Setup

## 1️⃣ Create Python Environment

Create a virtual environment:

```bash
python -m venv langgraph_env
```

### Windows

```bash
langgraph_env\Scripts\activate
```

### macOS / Linux

```bash
source langgraph_env/bin/activate
```

---

# 2️⃣ Install Dependencies

Install the required packages for the main application.

```bash
pip install langgraph langchain langchain-groq langchain-community langchain-tavily psycopg[binary] psycopg_pool python-dotenv tavily-python requests streamlit mcp
```

Depending on the implementation, additional dependencies may be required.

---

# 3️⃣ Configure Environment Variables

Create a `.env` file in the main project directory.

Example:

```env
GROQ_API_KEY=your_groq_api_key
TAVILY_API_KEY=your_tavily_api_key
AVIATION_STACK_API_KEY=your_aviationstack_api_key
OPENWEATHER_API_KEY=your_openweather_api_key

DATABASE_URL=postgresql://postgres:password@localhost:5432/langgraph_memory
```

---

# 🔌 Setup AviationStack MCP Server

TravelPilot-MCP uses the AviationStack MCP server to expose flight-related capabilities.

### MCP Server Repository

The AviationStack MCP server used by this project is available here:

https://github.com/Pradumnasaraf/aviationstack-mcp

---

## 1️⃣ Clone the MCP Server

```bash
git clone https://github.com/Pradumnasaraf/aviationstack-mcp.git
```

Navigate into the project:

```bash
cd aviationstack-mcp
```

---

## 2️⃣ Install UV

Check whether UV is installed:

```bash
uv --version
```

If required:

```bash
pip install uv
```

If installation through pip does not work, Windows users can install UV using:

```powershell
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

---

## 3️⃣ Configure AviationStack API Key

Create a `.env` file inside the AviationStack MCP server project:

```env
AVIATION_STACK_API_KEY=your_api_key_here
```

---

## 4️⃣ Install MCP Server Dependencies

Run:

```bash
uv sync
```

This will create the environment and install the required dependencies.

---

## 5️⃣ Activate the Environment

### Windows

```bash
.venv\Scripts\activate
```

---

## 6️⃣ Start the AviationStack MCP Server

Run:

```bash
uv run -m aviationstack_mcp mcp run
```

Alternatively:

```bash
python -m aviationstack_mcp mcp run
```

The MCP server will continue running and wait for incoming MCP requests.

> Keep this terminal running while using the main TravelPilot-MCP application.

To stop the server:

```text
CTRL + C
```

---

# 🌤️ Setup Weather MCP Server

The project also uses a weather MCP server to provide weather information.

Obtain an API key from OpenWeatherMap and configure it in your environment.

Install the required packages:

```bash
pip install mcp requests
```

Configure the weather API key:

```env
OPENWEATHER_API_KEY=your_api_key
```

The weather capability can then be exposed to the travel planning workflow through MCP.

---

# 🗄️ PostgreSQL Setup

Create a PostgreSQL database for LangGraph memory.

Example:

```sql
CREATE DATABASE langgraph_memory;
```

Then configure the connection:

```env
DATABASE_URL=postgresql://postgres:password@localhost:5432/langgraph_memory
```

Make sure PostgreSQL is running before starting the application.

---

# ▶️ Run the Application

Once the required MCP servers and PostgreSQL database are configured, start the main application.

## Terminal Version

```bash
python main.py
```

The terminal application allows you to interact directly with the multi-agent travel planning workflow.

---

## 🖥️ Streamlit Web App

If the project contains the Streamlit frontend, run:

```bash
streamlit run frontend.py
```

The application will open in your browser.

---

# 💬 Example Prompt

Try a request such as:

```text
Plan a complete 7 days Japan trip including flights, hotels and sightseeing under ₹2,00,000.
```

The system can then coordinate the available capabilities to build a complete travel plan.

---

# 🔍 Example Agent Interaction

```text
USER
 │
 │ "Plan a 7-day Japan trip under ₹2 lakh"
 │
 ▼
LANGGRAPH
 │
 ├──► ✈️ Flight Agent
 │        │
 │        └──► AviationStack MCP
 │
 ├──► 🏨 Hotel Agent
 │        │
 │        └──► Search Capability
 │
 ├──► 🌤️ Weather
 │        │
 │        └──► Weather MCP
 │
 └──► 🗓️ Itinerary Agent
          │
          ▼
     🤖 Final Agent
          │
          ▼
   ✨ Complete Trip Plan
```

---

# 🏗️ Architecture

The high-level architecture of TravelPilot-MCP can be represented as:

```text
                         ┌───────────────────┐
                         │       USER        │
                         └─────────┬─────────┘
                                   │
                                   ▼
                         ┌───────────────────┐
                         │    LangGraph      │
                         │  Agent Workflow   │
                         └─────────┬─────────┘
                                   │
                  ┌────────────────┼────────────────┐
                  │                │                │
                  ▼                ▼                ▼
           ✈️ Flight Agent   🏨 Hotel Agent   🗓️ Itinerary
                  │                │                │
                  ▼                ▼                │
             MCP Tools        Search Tools          │
                  │                │                │
             ┌────┴────┐           │                │
             ▼         ▼           ▼                │
         Aviation   Weather      Tavily             │
           MCP        MCP         Search             │
             │         │           │                │
             └─────────┴───────────┴────────────────┘
                                   │
                                   ▼
                         🤖 Final Response Agent
                                   │
                                   ▼
                            ✨ Travel Plan
                                   │
                                   ▼
                         💾 PostgreSQL Memory
```

---

# 🎯 What This Project Demonstrates

TravelPilot-MCP provides practical experience with:

* 🤖 Multi-Agent AI systems
* 🧠 LangGraph
* 🔗 LangChain
* 🔌 Model Context Protocol
* 🛠️ MCP server integration
* 🧩 Tool-based AI architectures
* ⚡ Groq Llama
* 🌐 External API integration through MCP
* 💾 PostgreSQL-based agent memory
* 🔄 Stateful AI workflows
* 🗓️ Automated itinerary generation
* ✈️ Travel information retrieval
* 🌤️ Weather integration
* 🔎 Web search
* 🖥️ Streamlit application development

---

# 🆚 Traditional API Integration vs MCP

One of the main architectural concepts demonstrated by this project is the difference between directly integrating APIs and exposing capabilities through MCP.

### Traditional Approach

```text
AI Agent
   │
   ├── Direct Flight API Code
   ├── Direct Weather API Code
   └── Direct Search API Code
```

### MCP Approach

```text
AI Agent
   │
   ▼
MCP Client
   │
   ├── Flight MCP Server
   ├── Weather MCP Server
   └── Search / Other Tools
```

With MCP, the AI application can interact with tools through a standardized interface, making the overall system easier to extend with additional capabilities.

---

# 🔮 Future Enhancements

The architecture can be extended with additional MCP servers and AI capabilities.

### 🏨 Dedicated Hotel MCP

Introduce a dedicated hotel MCP server for accommodation search.

### 🚆 Transportation MCP

Add trains, buses, rental cars, and other transportation capabilities.

### 🍽️ Restaurant MCP

Allow the travel planner to discover restaurants and dining options.

### 🗺️ Maps MCP

Add maps, routes, distances, and location-based recommendations.

### 💳 Budget Agent

Introduce a dedicated budget agent that tracks:

* Flights
* Hotels
* Food
* Transportation
* Activities

and keeps the complete itinerary within the user's budget.

### 🧠 Advanced Memory

Improve long-term memory so the system can remember travel preferences across sessions.

### 📱 Modern Frontend

Add a dedicated React frontend for a richer travel-planning experience.

---

# 🌟 Key Takeaway

TravelPilot-MCP demonstrates how **multi-agent systems and MCP can work together to build modular AI applications**.

Instead of creating a tightly coupled application where every agent directly implements individual APIs, MCP introduces a reusable tool layer between the AI system and external capabilities.

```text
              AI Agents
                  │
                  ▼
              LangGraph
                  │
                  ▼
            MCP Tool Layer
                  │
        ┌─────────┼─────────┐
        ▼         ▼         ▼
      Flight    Weather    Search
        │         │         │
        ▼         ▼         ▼
    External Services
```

The result is a travel-planning system that is not only capable of generating complete itineraries, but also demonstrates a more **modular and extensible architecture for tool-enabled AI agents**.

---

# ⭐ If You Like This Project

If you find TravelPilot-MCP useful or interesting, consider giving the repository a ⭐.

Suggestions, improvements, and contributions are welcome!

---

## 📜 License

This project is intended for educational and demonstration purposes.
