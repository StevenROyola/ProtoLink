
# ProtoLinkAI 🚀 

**ProtoLink AI** is a standardized **tool wrapping framework** for implementing and managing diverse tools in a unified way. It is designed to help developers quickly integrate and launch tool-based use cases.

### Key Features
- 🔧 **Standardized Wrapping**: Provides an abstraction layer for building tools using the MCP protocol.
- 🚀 **Flexible Use Cases**: Easily add or remove tools to fit your specific requirements.
- ✨ **Out-of-the-Box Tools**: Includes pre-built tools for common scenarios:
  - 🐦 **Twitter Management**: Automate tweeting, replying, and managing Twitter interactions.
  - 💸 Crypto: Get the latest cryptocurrency prices.
  - 🤖 [ElizaOS](https://github.com/elizaos/eliza) Integration: Seamlessly connect and interact with ElizaOS for enhanced automation.
  - 🕑 Time utilities
  - ☁️ Weather information (API)
  - 📚 Dictionary lookups
  - 🧮 Calculator for mathematical expressions
  - 💵 Currency exchange (API)
  - 📈 Stocks Data: Access real-time and historical stock market information.
  - [WIP] 📰 News: Retrieve the latest news headlines.

### Tech Stack 🛠️
- **Python**: Core programming language
- **[MCP](https://pypi.org/project/mcp/) Framework**: Communication protocol
- **Docker**: Containerization

#### 🤔 What is MCP?

The **Model Context Protocol ([MCP](https://modelcontextprotocol.io/introduction))** is a cutting-edge standard for **context sharing and management** across AI models and systems. Think of it as the **language** AI agents use to interact seamlessly. 🧠✨

Here’s why **MCP** matters:

- 🧩 **Standardization**: MCP defines how context can be shared across models, enabling **interoperability**.
- ⚡ **Scalability**: It’s built to handle large-scale AI systems with high throughput.
- 🔒 **Security**: Robust authentication and fine-grained access control.
- 🌐 **Flexibility**: Works across diverse systems and AI architectures.

![mcp_architecture](https://imageio.forbes.com/specials-images/imageserve/674aaa6ac3007d55b62fc2bf/MCP-Architecture/960x0.png?height=559&width=711&fit=bounds)
[source](https://www.forbes.com/sites/janakirammsv/2024/11/30/why-anthropics-model-context-protocol-is-a-big-step-in-the-evolution-of-ai-agents/)
---

## Installation 📦

### Install via PyPI
```bash
pip install ProtoLinkai
```

---

## Usage 💻

### Run Locally
```bash
ProtoLinkai --local-timezone "America/New_York"
```

### Run in Docker
1. **Build the Docker image:**
   `docker build -t ProtoLinkai .`

2. **Run the container:**
   `docker run -i --rm ProtoLinkai`

---

## Twitter Integration 🐦

MProtoLinkAI offers robust Twitter integration, allowing you to automate tweeting, replying, and managing Twitter interactions. This section provides detailed instructions on configuring and using the Twitter integration, both via Docker and `.env` + `scripts/run_agent.sh`.

### Docker Environment Variables for Twitter Integration

When running ProtoLinkAI within Docker, it's essential to configure environment variables for Twitter integration. These variables are divided into two categories:

#### 1. Agent Node Client Credentials
These credentials are used by the **Node.js client** within the agent for managing Twitter interactions.

```dockerfile
ENV TWITTER_USERNAME=
ENV TWITTER_PASSWORD=
ENV TWITTER_EMAIL=
```

#### 2. Tweepy (Twitter API v2) Credentials
These credentials are utilized by **Tweepy** for interacting with Twitter's API v2.

```dockerfile
ENV TWITTER_API_KEY=
ENV TWITTER_API_SECRET=
ENV TWITTER_ACCESS_TOKEN=
ENV TWITTER_ACCESS_SECRET=
ENV TWITTER_CLIENT_ID=
ENV TWITTER_CLIENT_SECRET=
ENV TWITTER_BEARER_TOKEN=
```

### Running ProtoLinkAI with Docker

1. **Build the Docker image:**
   ```bash
   docker build -t ProtoLinkai .
   ```

2. **Run the container:**
   ```bash
   docker run -i --rm ProtoLinkai
   ```

### Running ProtoLink with `.env` + `scripts/run_agent.sh`

#### Setting Up Environment Variables

Create a `.env` file in the root directory of your project and add the following environment variables:

```dotenv
ANTHROPIC_API_KEY=your_anthropic_api_key
ELIZA_PATH=/path/to/eliza
TWITTER_USERNAME=your_twitter_username
TWITTER_EMAIL=your_twitter_email
TWITTER_PASSWORD=your_twitter_password
PERSONALITY_CONFIG=/path/to/personality_config.json
RUN_AGENT=True

# Tweepy (Twitter API v2) Credentials
TWITTER_API_KEY=your_twitter_api_key
TWITTER_API_SECRET=your_twitter_api_secret
TWITTER_ACCESS_TOKEN=your_twitter_access_token
TWITTER_ACCESS_SECRET=your_twitter_access_secret
TWITTER_CLIENT_ID=your_twitter_client_id
TWITTER_CLIENT_SECRET=your_twitter_client_secret
TWITTER_BEARER_TOKEN=your_twitter_bearer_token
```

#### Running the Agent

1. **Make the script executable:**
   ```bash
   chmod +x scripts/run_agent.sh
   ```

2. **Run the agent:**
   ```bash
   bash scripts/run_agent.sh
   ```


### Summary
You can configure ProtoLink to run with Twitter integration either using Docker or by setting up environment variables in a `.env` file and running the `scripts/run_agent.sh` script.

This flexibility allows you to choose the method that best fits your deployment environment.

---

## ElizaOS Integration 🤖

### **1. Directly Use Eliza Agents from ProtoLink**
This approach allows you to use Eliza Agents without running the Eliza Framework in the background. It simplifies the setup by embedding Eliza functionality directly within ProtoLink.

**Steps:**

1. **Configure ProtoLink to Use Eliza MCP Agent:**
   In your Python code, add Eliza MCP Agent to the `MultiToolAgent`:
    ```python
    from ProtoLink.core.multi_tool_agent import MultiToolAgent
    from ProtoLink.tools.eliza_mcp_agent import eliza_mcp_agent

    multi_tool_agent = MultiToolAgent([
        # ... other agents
        eliza_mcp_agent
    ])
   ```

**Advantages:**
- **Simplified Setup:** No need to manage separate background processes.
- **Easier Monitoring:** All functionalities are encapsulated within MCPAgentAI.
- **Highlight Feature:** Emphasizes the flexibility of MCPAgentAI in integrating various tools seamlessly.


### **2. Run Eliza Framework from ProtoLinkai**
This method involves running the Eliza Framework as a separate background process alongside ProtoLinkAI.

**Steps:**

1. **Start Eliza Framework:**
   `bash src/ProtoLinkai/tools/eliza/scripts/run.sh`

2. **Monitor Eliza Processes:**
   `bash src/ProtoLinkai/tools/eliza/scripts/monitor.sh`

3. **Configure MCPAgentAI to Use Eliza Agent:**
   In your Python code, add Eliza Agent to the `MultiToolAgent`:
    ```python
   from ProtoLink.core.multi_tool_agent import MultiToolAgent
   from ProtoLink.tools.eliza_agent import eliza_agent

   multi_tool_agent = MultiToolAgent([
       # ... other agents
       eliza_agent
   ])
   ```
---

## Tutorial: Selecting Specific Tools

You can configure ProtoLink to run only certain tools by modifying the agent configuration in your server or by updating the `server.py` file to only load desired agents. For example:

```python
from ProtoLinkai.tools.time_agent import TimeAgent
from ProtoLinkai.tools.weather_agent import WeatherAgent
from ProtoLinkai.core.multi_tool_agent import MultiToolAgent

multi_tool_agent = MultiToolAgent([
    TimeAgent(),
    WeatherAgent()
])
This setup will only enable **Time** and **Weather** tools.
```
---

## Integration Example: Claude Desktop Configuration

You can integrate ProtoLinkAI with Claude Desktop using the following configuration (`claude_desktop_config.json`), **note that** local ElizaOS repo is optional arg:
```json
{
    "mcpServers": {
        "mcpagentai": {
            "command": "docker",
            "args": ["run", "-i", "-v", "/path/to/local/eliza:/app/eliza", "--rm", "mcpagentai"]
        }
    }
}
```
---

## Development 🛠️

1. **Clone this repository:**
   ```bash
   git clone https://github.com/StevenROyola/ProtoLink.git
   cd mcpagentai
   ```

2. **(Optional) Create a virtual environment:**
   ```bash
   python3 -m venv .venv
   source .venv/bin/activate
   ```

3. **Install dependencies:**
   ```bash
   pip install -e .
   ```

4. **Build the package:**
   ```bash
   python -m build
   ```
---

---

**License**: MIT  
Enjoy! 🎉
