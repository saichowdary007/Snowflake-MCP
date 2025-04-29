# VS Code Snippets for Snowflake MCP Templates

This directory contains VS Code snippets that allow you to quickly insert MCP templates into your workflow.

## Installation

1. Open VS Code
2. Navigate to **File > Preferences > User Snippets**
3. Select **markdown.json** (or create it if it doesn't exist)
4. Copy the contents of [mcp-snippets.json](mcp-snippets.json) into your markdown.json file
5. Save the file

## Available Snippets

| Prefix | Description | Template |
|--------|-------------|----------|
| `!mcp-minimal` | Inserts the minimal MCP template | [minimal.md](../templates/minimal.md) |
| `!mcp-debugging` | Inserts the debugging MCP template | [debugging.md](../templates/debugging.md) |
| `!mcp-optimization` | Inserts the performance optimization MCP template | [optimization.md](../templates/optimization.md) |
| `!mcp-visualization` | Inserts the visualization MCP template | [visualization.md](../templates/visualization.md) |
| `!mcp-complete` | Inserts the comprehensive MCP template | [complete.md](../templates/complete.md) |

## Usage

1. Create or open a markdown file in VS Code
2. Type one of the prefixes (e.g., `!mcp-minimal`)
3. Press Tab or Enter to expand the snippet
4. Fill in the template sections
5. Use in your communication with AI assistants

## Example

![VS Code Snippet Demo](../assets/vs-code-snippet-demo.gif)

## Customizing Snippets

You can modify the snippets to match your organization's specific templates:

1. Edit the [mcp-snippets.json](mcp-snippets.json) file
2. Add or modify snippets according to your needs
3. Install the updated snippets in VS Code

## Integrating with AI Assistant Extensions

If you're using AI assistant extensions in VS Code:

1. Create a new markdown file
2. Use the snippet to insert your template
3. Fill in the relevant sections
4. Highlight the filled template and your query
5. Right-click and select your AI assistant's context menu option

For more detailed instructions, see the [VS Code Integration Guide](../guides/vscode-integration.md). 