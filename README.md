[![MseeP.ai Security Assessment Badge](https://mseep.net/pr/lex-tools-codebase-context-dumper-badge.png)](https://mseep.ai/app/lex-tools-codebase-context-dumper)

# codebase-context-dumper MCP Server

[![smithery badge](https://smithery.ai/badge/@lex-tools/codebase-context-dumper)](https://smithery.ai/server/@lex-tools/codebase-context-dumper)
[![npm version](https://badge.fury.io/js/%40lex-tools%2Fcodebase-context-dumper.svg)](https://badge.fury.io/js/%40lex-tools%2Fcodebase-context-dumper)
[![License: Apache-2.0](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)

A Model Context Protocol (MCP) server designed to easily dump your codebase context into Large Language Models (LLMs).

## Why Use This?

Large context windows in LLMs are powerful, but manually selecting and formatting files from a large codebase is tedious. This tool automates the process by:

*   Recursively scanning your project directory.
*   Including text files from the specified directory tree that are not excluded by `.gitignore` rules.
*   Automatically skipping binary files.
*   Concatenating the content with clear file path markers.
*   Supporting chunking to handle codebases larger than the LLM's context window.
*   Integrating seamlessly with MCP-compatible clients.

## Usage (Recommended: npx)

The easiest way to use this tool is via `npx`, which runs the latest version without needing a local installation.

Configure your MCP client (e.g., Claude Desktop, VS Code extensions) to use the following command:

```json
{
  "mcpServers": {
    "codebase-context-dumper": {
      "command": "npx",
      "args": [
        "-y",
        "@lex-tools/codebase-context-dumper"
      ]
    }
  }
}
```

The MCP client will then be able to invoke the `dump_codebase_context` tool provided by this server.

## Features & Tool Details

### Tool: `dump_codebase_context`

Recursively reads text files from a specified directory, respecting `.gitignore` rules and skipping binary files. Concatenates content with file path headers/footers. Supports chunking the output for large codebases.

**Functionality**:

*   Scans the directory provided in `base_path`.
*   Respects `.gitignore` files at all levels (including nested ones and `.git` by default).
*   Detects and skips binary files.
*   Reads the content of each valid text file.
*   Prepends a header (`--- START: relative/path/to/file ---`) and appends a footer (`--- END: relative/path/to/file ---`) to each file's content.
*   Concatenates all processed file contents into a single string.

**Input Parameters**:

*   `base_path` (string, required): The absolute path to the project directory to scan.
*   `num_chunks` (integer, optional, default: 1): The total number of chunks to divide the output into. Must be >= 1.
*   `chunk_index` (integer, optional, default: 1): The 1-based index of the chunk to return. Requires `num_chunks > 1` and `chunk_index <= num_chunks`.

**Output**: Returns the concatenated (and potentially chunked) text content.

## Local Installation & Usage (Advanced)

If you prefer to run a local version (e.g., for development):

1.  Clone the repository:
    ```bash
    git clone git@github.com:lex-tools/codebase-context-dumper.git
    cd codebase-context-dumper
    ```
2.  Install dependencies:
    ```bash
    npm install
    ```
3.  Build the server:
    ```bash
    npm run build
    ```
4.  Configure your MCP client to point to the local build output:
    ```json
    {
      "mcpServers": {
        "codebase-context-dumper": {
          "command": "/path/to/your/local/codebase-context-dumper/build/index.js" // Adjust path
        }
      }
    }
    ```

## Contributing

Contributions are welcome! Please see [CONTRIBUTING.md](CONTRIBUTING.md) for details on development, debugging, and releasing new versions.

## License

This project is licensed under the Apache License 2.0. See the [LICENSE](LICENSE) file for details.
