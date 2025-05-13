# weather-mcp-server

### MCP Weather Server Explanation

This file implements a Model Context Protocol (MCP) server that provides weather information tools. MCP enables AI models to interact with external tools in a structured way.

Overview
The server exposes two weather tools:

- `get-alerts`: Gets weather alerts for a specific US state
- `get-forecast`: Gets weather forecast for a specific location by coordinates
It uses the National Weather Service (NWS) API to fetch real weather data.

### How MCP Clients Understand Tool Inputs
The key to how MCP clients understand tool inputs lies in how tools are registered:

```typescript
server.tool(
  "get-forecast",                   // Tool name
  "Get weather forecast for a location",  // Tool description
  {                                // Input schema
    latitude: z.number().min(-90).max(90).describe("Latitude of the location"),
    longitude: z.number().min(-180).max(180).describe("Longitude of the location"),
  },
  async ({ latitude, longitude }) => {   // Handler function
    // Implementation
  }
);
```
Here's how clients understand the inputs:

1. Schema Definition: The third parameter uses Zod (z) to define the expected inputs and their constraints:

- Input parameter names (e.g., latitude, longitude)
- Data types (z.number(), z.string())
- Validation rules (min(-90), max(90), length(2))
- Descriptions (describe("Latitude of the location"))

2. Client Discovery: When an MCP client connects to this server:

- It receives metadata about available tools
- For each tool, it gets the name, description, and input schema
- The client can use this information to validate inputs before sending them

3. Communication: The StdioServerTransport handles the serialization and communication of this schema information between the server and client.

This schema-based approach ensures that clients know exactly what parameters each tool expects, their types, constraints, and what they represent.