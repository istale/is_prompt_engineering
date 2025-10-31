# Cline Prompt — FastMCP Minimal MCP Server (Single File)

## Goal
Generate a **single-file**, **minimal**, and **MCP-compliant** FastMCP server based on the user’s new requirement:
- Use `mcp.server.fastmcp.FastMCP` and `@mcp.tool()` decorators.
- Follow the same consistent structure and error-handling style.
- Run via `stdio` transport (`mcp.run(transport="stdio")`).
- No extra folders—single-file layout only.

## Requirements
1. **MCP Compliance**: 
    - Each tool must use `@mcp.tool()` with proper type hints and detailed docstrings.
    - Each tool must follow docstring and argument documentation of **Minimal Template Example**.
2. **Single-file Template**: Output only one executable file (e.g., `server.py`).
3. **Error Handling**: Must return LLM-readable messages instead of raising exceptions.
4. **Consistent Structure**: The output file must follow this order:
   - Imports
   - FastMCP init
   - Tool functions
   - `main` section with `mcp.run(transport="stdio")`
5. **Output Style**: Return LLM-readable text or data structure

---

## 🧩 Output Format (Single File)
- Filename: `server.py`
- Tools: Defined per user need, one per function with `@mcp.tool()`
- Entrypoint: `mcp.run(transport="stdio")`

---

## **Minimal Template Example**
```python:server.py
# import package
from mcp.server.fastmcp import FastMCP

# Create FastMCP server, 假設Server名稱為 csv_reader_mcp
mcp = FastMCP("csv_reader_mcp", log_level="ERROR")

# 宣告tool
# 假設tool名稱為 get_csv_first_column_value
# 假設tool功能與使用者可能使用的prompt為 Read a CSV file and get first column value for an LLM-friendly response. User prompt example: get csv information of file 'data.csv'
# 假設Args為 file: The name of the CSV file to read
# 假設Returns為 str: Column name and first column value
# 假設Raises為 str: File not found error
# 假設Example為 result = get_csv_first_column_value("data.csv")
@mcp.tool()
async def get_csv_first_column_value(file: str) -> str:
    """Read a CSV file and get first column value for an LLM-friendly response.
       User prompt example: get csv info of file 'data.csv'
    
    Args:
        file: The name of the CSV file to read

    Returns:
        str: Column name and first column value
    
    Raises:
        str: File not found error
    
    Example:
        result = get_csv_first_column_value("data.csv")
    """
    # Implementation goes here


if __name__ == "__main__":
    # Initialize and run the server
    mcp.run(transport='stdio')
```
  
## Configuration JSON
When you finish creating the new MCP server, provide configuration JSON with the following content:
{
  "csv_reader_mcp": {
    "command": "/opt/homebrew/Caskroom/miniforge/base/envs/xx_dev_env/bin/python",
    "args": [
      "/Users/istale/Documents/isprogram/mcp_csv_reader_LLM/server.py"
    ],
    "disabled": false,
    "transportType": "stdio",
    "timeout": 60,
    "autoApprove": []
  }
}