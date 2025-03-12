# mcpsx cli

Command line interface for mcpsx (Model Context Protocol Server eXecutable), allowing you to manage, query, and interact with Model Context Protocol (MCP) servers and tools.

## Installation

```bash
# Install globally
npm install -g @jasonkneen/mcpsx

# Or use with npx
npx @jasonkneen/mcpsx
```

## Usage

The CLI can be accessed using any of these commands:
- `mcps` (primary command)
- `mcpsx` (extended alias)

```bash
# Show help
mcpsx help

# Start MCPS as a stdio server
mcpsx run

# Start with specific servers and tools
mcpsx run --server="sleep"
mcpsx run --servers="python,pytorch" --tool="predict"

# Server group management
mcpsx groups add "python-stack" --servers="python,pytorch,huggingface"
mcpsx run --servers="python-stack"

# Add a new MCP configuration
mcpsx add "My Server" --command "node" --args "server.js"

# List MCP configurations
mcpsx list

# Remove an MCP configuration
mcpsx remove "My Server"

# Use a specific MCP configuration
mcpsx use "My Server"
```

## Key Features

MCPSX CLI provides powerful capabilities for working with Model Context Protocol servers:

1. **Run Servers & Tools**: Start MCP servers and tools individually or in combination
2. **Add & Remove**: Easily manage your MCP configurations
3. **Query & List**: View available servers and tools at any time
4. **Grouping**: Create and manage groups of servers and tools for simplified workflows
5. **Flexible Filtering**: Run specific servers, tools, or combinations

## Commands

### `stdio`

Start MCPS as a stdio server. This is the main command used by the VSCode extension to communicate with MCP servers.

```bash
mcpsx run [options]
```

Options:
- `-s, --server <n>` - Load only a specific server
- `-S, --servers <names>` - Load only specific servers (comma-separated)
- `-t, --tool <n>` - Load only a specific tool
- `-T, --tools <names>` - Load only specific tools (comma-separated)

Examples:
```bash
# Load all servers and tools
mcpsx run

# Load only the 'sleep' server
mcpsx run --server="sleep"

# Load multiple servers
mcpsx run --servers="python,pytorch"

# Load specific tools from specific servers
mcpsx run --servers="python" --tools="predict,generate"

# Use a server group
mcpsx run --servers="python-stack"
```

### `groups`

Manage server and tool groups. Groups allow you to create collections of MCP servers and tools that can be used together.

```bash
mcpsx groups <command>
```

Subcommands:

#### `groups add`

Create a new server group.

```bash
mcpsx groups add <n> --servers="server1,server2,..."
```

Example:
```bash
# Create a 'python-stack' group containing multiple servers
mcpsx groups add "python-stack" --servers="python,pytorch,huggingface"

# Create a 'favorites' group
mcpsx groups add "favorites" --servers="openai,anthropic"
```

#### `groups remove`

Remove a server group.

```bash
mcpsx groups remove <n>
```

#### `groups list`

List all server groups.

```bash
mcpsx groups list
```

## Server & Tool Groups

Groups allow you to create collections of MCP servers and tools that can be used together. This is useful for organizing related components and simplifying command-line usage.

Groups act as "virtual MCPs" - when you reference a group name with `--servers` or `--tools`, it expands to include all servers or tools in that group.

Example workflow:

```bash
# Create groups for different use cases
mcpsx groups add "ai-models" --servers="openai,anthropic,llama"
mcpsx groups add "data-tools" --servers="pandas,numpy,sklearn"

# Use a specific group
mcpsx run --servers="ai-models"

# Combine groups with individual servers/tools
mcpsx run --servers="ai-models,custom-server" --tools="predict"
```

### `add`

Add a new MCP configuration.

```bash
mcpsx add <n> [options]
```

Options:
- `-c, --command <command>` - Command to run the MCP server
- `-a, --args <args>` - Arguments for the command (comma-separated)
- `-e, --env <env>` - Environment variables (key=value,key2=value2)

Example:
```bash
mcpsx add "GPT Server" --command "node" --args "server.js,--port=3000" --env "API_KEY=abc123,DEBUG=true"
```

### `remove`

Remove an MCP configuration.

```bash
mcpsx remove <n>
```

### `list`

List all MCP configurations.

```bash
mcpsx list
```

### `use`

Use a specific MCP configuration.

```bash
mcpsx use <n>
```

### `help`

Display help information.

```bash
mcpsx help
```

## Configuration

MCPSX CLI uses the configuration file located at `~/.mcpsx/config.json`. This file is shared with the MCPS VSCode extension.
