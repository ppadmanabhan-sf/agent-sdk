# Salesforce AgentForce SDK

[![PyPI version](https://badge.fury.io/py/agentforce-sdk.svg)](https://badge.fury.io/py/agentforce-sdk)
[![Python 3.9+](https://img.shields.io/badge/python-3.9+-blue.svg)](https://www.python.org/downloads/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

A Python SDK for creating, managing, and deploying AI agents in Salesforce.

## Introduction

The Salesforce AgentForce SDK provides a programmatic interface to Salesforce's Agent infrastructure, allowing developers to define and interact with agents using Python code.

## Installation

```bash
pip install agentforce-sdk
```

## Documentation

Comprehensive documentation for the SDK is available in the [docs](https://github.com/salesforce/agent-sdk/tree/main/docs) directory:

- [API Documentation](https://github.com/salesforce/agent-sdk/blob/main/docs/api_documentation.md): Detailed documentation for all SDK components, classes, and methods.
- [JSON Schemas](https://github.com/salesforce/agent-sdk/tree/main/docs/schemas): JSON schemas for validating agent definitions in various formats.

## Directory Structure Support

The SDK supports multiple formats for defining agents:

1. **Single JSON File**: A complete agent definition in a single JSON file.
2. **Nested Directory Structure**: A hierarchical directory structure with topics and actions in subdirectories.
3. **Modular Directory Structure**: A flat directory structure with references between components.

For more information, see the [Directory Structures section](https://github.com/salesforce/agent-sdk/blob/main/docs/api_documentation.md#directory-structures) in the API documentation.

## MCP Server

The SDK includes a Message Control Protocol (MCP) server that allows you to expose the SDK functionality over HTTP. This makes it easy to integrate the SDK with other systems and languages.

### Starting the Server

```python
from agent_sdk.server import start_server

# Start the server with default settings (localhost:8000)
server = start_server()

# Or customize the server settings
server = start_server(
    host="0.0.0.0",  # Listen on all interfaces
    port=8080,       # Custom port
    auth_token="your-secret-token",  # Enable authentication
    debug=True       # Enable debug logging
)
```

You can also start the server from the command line:

```bash
python -m agent_sdk.server --host 0.0.0.0 --port 8080 --token your-secret-token --debug
```

### Using the Client

The SDK provides a simple HTTP client example for interacting with the MCP server:

```python
# Example from examples/mcp_server_example.py
from examples.mcp_server_example import AgentforceClient

# Create a client
client = AgentforceClient("http://localhost:8000")

# Create a session
client.create_session("your-username", "your-password", "your-security-token")

# Create an agent
agent_data = {
    "name": "Hello World Agent",
    "description": "A simple agent that says hello",
    # ... other agent data
}
result = client.create_agent(agent_data)
agent_id = result.get("id")

# Run the agent
response = client.run_agent(agent_id, "Hello, Agent!")
```

For a complete example, see [MCP Server Example](https://github.com/salesforce/agent-sdk/blob/main/examples/mcp_server_example.py).

## Examples

The [examples](https://github.com/salesforce/agent-sdk/tree/main/examples) directory contains sample code demonstrating how to use the SDK:

- [Creating an agent programmatically](https://github.com/salesforce/agent-sdk/blob/main/examples/create_agent_programmatically.py)
- [Creating an agent from a JSON file](https://github.com/salesforce/agent-sdk/blob/main/examples/create_agent_from_json_file.py)
- [Creating an agent from a nested directory](https://github.com/salesforce/agent-sdk/blob/main/examples/create_agent_from_nested_directory.py)
- [Creating an agent from a modular directory](https://github.com/salesforce/agent-sdk/blob/main/examples/create_agent_from_modular_directory.py)
- [Creating an agent from a description](https://github.com/salesforce/agent-sdk/blob/main/examples/create_agent_from_description.py)
- [Running an agent](https://github.com/salesforce/agent-sdk/blob/main/examples/run_agent.py)
- [Exporting an agent](https://github.com/salesforce/agent-sdk/blob/main/examples/export_salesforce_agent_example.py)
- [Using the MCP server](https://github.com/salesforce/agent-sdk/blob/main/examples/mcp_server_example.py)

## Quick Start

```python
from agent_sdk import Agentforce
from agent_sdk.models import Agent, Topic, Action, Input, Output

# Initialize the client
client = Agentforce(username="your_username", password="your_password")

# Create a simple agent
agent = Agent(
    name="Hello World Agent",
    description="A simple agent that says hello",
    agent_type="External",
    agent_template_type="Einstein",
    company_name="Salesforce"
)

# Add a topic
topic = Topic(
    name="Greetings",
    description="Handle greetings",
    scope="Handle greeting requests"
)

# Add an action
action = Action(
    name="SayHello",
    description="Say hello to the user",
    inputs=[
        Input(
            name="name",
            description="Name of the person to greet",
            data_type="String",
            required=True
        )
    ],
    example_output=Output(
        status="success",
        details={"message": "Hello, World!"}
    )
)

# Add the action to the topic
topic.actions = [action]

# Add the topic to the agent
agent.topics = [topic]

# Create the agent in Salesforce
result = client.create(agent)
print(f"Agent created with ID: {result['id']}")
```

## IDE Integration

The SDK includes JSON schema files that can help with IDE integration:

- VSCode: Use the schemas to validate your agent definition files
- Cursor: Leverage the detailed API documentation for intelligent code completion
- WindSurf: Use the schema files to provide structured editing experiences

## License

This project is licensed under the MIT License - see the [LICENSE](https://github.com/salesforce/agent-sdk/blob/main/LICENSE) file for details. 
