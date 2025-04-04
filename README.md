# codebase-context-dumper MCP Server

A Model Context Protocol server

This is a TypeScript-based MCP server designed to provide context from a software project's codebase. It exposes a tool that recursively reads files from a specified directory, respecting `.gitignore` rules, skipping binary files, and concatenating the content of valid text files. It supports chunking the output to handle large codebases.

## Features

### Tools

- **`dump_codebase_context`**: Recursively reads text files from a specified directory.
    - **Functionality**:
        - Scans the directory provided in `base_path`.
        - Respects `.gitignore` files at all levels to exclude unwanted files/directories (including `.git` by default).
        - Detects and skips binary files.
        - Reads the content of each valid text file.
        - Prepends a header (`--- START: relative/path/to/file ---`) and appends a footer (`--- END: relative/path/to/file ---`) to each file's content.
        - Concatenates all processed file contents into a single string.
    - **Input Parameters**:
        - `base_path` (string, required): The absolute path to the project directory to scan.
        - `num_chunks` (integer, optional, default: 1): The total number of chunks to divide the output into. Must be >= 1.
        - `chunk_index` (integer, optional, default: 1): The 1-based index of the chunk to return. Requires `num_chunks > 1` and `chunk_index <= num_chunks`.
    - **Output**: Returns the concatenated (and potentially chunked) text content.

## Development

Install dependencies:
```bash
npm install
```

Build the server:
```bash
npm run build
```

For development with auto-rebuild:
```bash
npm run watch
```

## Releasing

This project uses [standard-version](https://github.com/conventional-changelog/standard-version) combined with [Conventional Commits](https://www.conventionalcommits.org/) for automated versioning, changelog generation, and tagging.

To create a new release:

1.  Ensure your Git working directory is clean.
2.  Make sure your commit messages follow the Conventional Commits format (e.g., `feat: ...`, `fix: ...`, `BREAKING CHANGE: ...`).
3.  Run the release script:
    ```bash
    npm run release
    ```
    This will bump the version in `package.json`, update `CHANGELOG.md`, commit the changes, and create a Git tag.
4.  Push the commit and tag to the remote repository:
    ```bash
    git push --follow-tags origin main # Or your default branch
    ```
5.  (Optional) Publish the new version to npm:
    ```bash
    npm publish
    ```


## Local Installation & Usage

To use with Claude Desktop, add the server config:

On MacOS: `~/Library/Application Support/Claude/claude_desktop_config.json`
On Windows: `%APPDATA%/Claude/claude_desktop_config.json`

```json
{
  "mcpServers": {
    "codebase-context-dumper": {
      "command": "/path/to/local/codebase-context-dumper/build/index.js" // Path to your local build
    }
  }
}
```

## Publishing to npm & Usage with npx

Once the package is ready and you have appropriate permissions on npm:

1.  **Publish the package:**
    ```bash
    npm publish
    ```

2.  **Configure MCP Client (e.g., Claude Desktop):**
    Update your MCP client configuration to use `npx` to run the published package. The client will automatically download and execute the latest version.

    ```json
    {
      "mcpServers": {
        "codebase-context-dumper": {
          "command": "npx",
          "args": [
            "-y",
            "@lex-tools/codebase-context-dumper"
          ],
        }
      }
    }
    ```


### Debugging

Since MCP servers communicate over stdio, debugging can be challenging. We recommend using the [MCP Inspector](https://github.com/modelcontextprotocol/inspector), which is available as a package script:

```bash
npm run inspector
```

The Inspector will provide a URL to access debugging tools in your browser.
