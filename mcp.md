# GitHub MCP — Overview and Usage

**Short summary**

The Model Context Protocol (MCP) is a set of APIs and conventions for making model-driven automation interact with repository and issue data in a structured, auditable way. The GitHub MCP Server exposes a family of tools that let automated agents and integrations manage issues, pull requests, comments, reviews, and code search programmatically while preserving context and traceability.

The MCP tools include methods for creating, updating and searching issues and pull requests, adding comments and review threads, and performing targeted repository searches. These tools are designed to be used by authenticated automation or bot accounts, and they make it straightforward to integrate model-driven workflows into standard GitHub operations.

**Example: create an issue (pseudocode)**

```js
// Pseudocode calling the MCP issue write tool
mcp_io_github_git_issue_write({
  method: 'create',
  owner: 'Z1-Test',
  repo: 'temp-shubh-st16',
  title: 'Bug: unexpected failure in X',
  body: 'Steps to reproduce:\n1. ...\n2. ...\nExpected: ...\nActual: ...',
  labels: ['bug', 'triage']
})
```

**Permissions & notes**

- Always verify the authenticated user with `get_me` before performing privileged actions (create/modify/delete). This helps ensure the agent has the expected permissions and identity.
- Use `search_` tools when you need to query across repositories or do text-based searches with filters (e.g., find issues with a specific phrase, or PRs by author). Use `list_` tools when you want deterministic, paginated enumerations of resources for a specific scope (e.g., list issues for a repo or list branches).

**Best practices**

- Use a distinct automation identity and verify it with `get_me` for auditability. ✅
- Prefer `search_` for broad, fuzzy queries and `list_` for deterministic pagination; combine them when needed. 🔧
- Include context in comments and issue bodies (links, reproducible steps, and references) so other tools and humans can act on them quickly. 💡
