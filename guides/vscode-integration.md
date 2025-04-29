# VS Code Integration Guide for Snowflake MCP

This guide shows how to effectively integrate the Snowflake Model Context Protocol (MCP) with Visual Studio Code to streamline your workflow with AI assistants.

## Prerequisites

- Visual Studio Code installed
- Basic familiarity with VS Code snippets
- One or more AI assistant extensions (optional but recommended)

## Setting Up MCP Snippets

### Step 1: Install the Snippets

1. Open VS Code
2. Navigate to **File > Preferences > User Snippets**
3. Select **markdown.json** (or create it if it doesn't exist)
4. Copy the contents of [`/snippets/mcp-snippets.json`](../snippets/mcp-snippets.json) into your markdown.json file
5. Save the file

### Step 2: Verify Installation

1. Create a new markdown file (File > New File)
2. Type `!mcp-` and check if the snippet suggestions appear
3. Select one of the templates to verify it expands correctly

## Using MCP Snippets in Your Workflow

### Basic Workflow

1. Create a new markdown file in VS Code
2. Type `!mcp-` followed by the template type you need (e.g., `!mcp-minimal`)
3. Press Tab or Enter to expand the snippet
4. Fill in the template sections
5. Copy the filled template to use with your AI assistant

### With AI Assistant Extensions

If you're using AI assistant extensions directly in VS Code (e.g., GitHub Copilot, Cursor AI):

1. Create a new markdown file
2. Insert and fill out the appropriate MCP template
3. Add your specific SQL question below the template
4. Select both the template and your question
5. Right-click and choose the option to send to your AI assistant

## Recommended Extensions

These VS Code extensions work well with the MCP workflow:

1. **Markdown All in One** - For better markdown editing experience
2. **SQL Formatter** - Helps format SQL code blocks in your templates
3. **GitHub Copilot** or **Cursor AI** - For direct AI assistant integration

## Time-Saving Techniques

### Creating Custom Keybindings

You can set up keybindings to insert MCP templates even faster:

1. Open **File > Preferences > Keyboard Shortcuts**
2. Click the "Open Keyboard Shortcuts (JSON)" icon
3. Add bindings like this:

```json
[
  {
    "key": "ctrl+shift+1",
    "command": "editor.action.insertSnippet",
    "args": {
      "name": "Minimal MCP Template"
    },
    "when": "editorTextFocus && editorLangId == 'markdown'"
  },
  {
    "key": "ctrl+shift+2",
    "command": "editor.action.insertSnippet",
    "args": {
      "name": "Debugging MCP Template"
    },
    "when": "editorTextFocus && editorLangId == 'markdown'"
  }
]
```

### Using Workspace Snippets

For team-specific templates:

1. Create a `.vscode` folder in your project
2. Add a `markdown.code-snippets` file
3. Copy and customize your snippets there
4. These snippets will be available to anyone using this workspace

## MCP Template Selection Tips

- **For quick queries** - Use the minimal template (`!mcp-minimal`)
- **For error troubleshooting** - Use the debugging template (`!mcp-debugging`)
- **For slow queries** - Use the optimization template (`!mcp-optimization`)
- **For dashboard creation** - Use the visualization template (`!mcp-visualization`)
- **For complex analytics** - Use the complete template (`!mcp-complete`)

## Best Practices

1. **Maintain a library** of filled templates for common queries
2. **Create task-specific templates** for recurring workflows
3. **Share effective templates** with your team
4. **Document successful patterns** for your organization
5. **Update your snippets** as your MCP templates evolve 