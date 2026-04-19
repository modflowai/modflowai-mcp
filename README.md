<div align="center">
  <img src="./assets/modflow-ai-wordmark-cream.png" alt="MODFLOW AI" width="400">
</div>

<br>

# MODFLOW-AI MCP Server

A hosted Model Context Protocol (MCP) server that gives AI assistants grounded access to MODFLOW, PEST, FloPy, and PyEMU documentation, code, and tutorials. Your assistant searches and retrieves real sources instead of guessing.

## What It Does

MODFLOW-AI MCP Server exposes seven tools over the [Model Context Protocol](https://modelcontextprotocol.io/). An AI assistant calls them to search documentation, retrieve files, and return cited answers.

### Key Features

- **Multi-repository search** across MODFLOW 6, MODFLOW-USG, PEST, PEST++, PEST_HP, plproc, gwutils, FloPy, and PyEMU.
- **Text and semantic search**, each tuned for a specific content type (docs, code, tutorials).
- **Acronym expansion** for MODFLOW/PEST terms (WEL, RIV, MAW, CHD, DRN, UZF, …).
- **GitHub URLs** returned with every code or tutorial result.
- **File retrieval by exact path**, with pagination for files over 30 KB.
- **Authenticated access** — queries are not stored or logged.

## Getting Started

### 1. Request access

For access, visit [www.modflow.ai](https://www.modflow.ai). You'll receive configuration instructions by email.

### 2. Compatible AI Assistants

**HTTP transport** (direct connection):
- VS Code
- Cursor

**MCP-Remote required**:
- Claude Desktop
- Claude.ai (Claude Code)
- Windsurf

**Web UI integration**:
- CopilotKit (React / Next.js)
- Mastra AI Framework
- Dual-panel interface with file content viewer
- GitHub Flavored Markdown with syntax highlighting

### 3. Configuration

Your access email includes the endpoint URL and the exact configuration block for your client.

## CopilotKit UI Implementation

The repository ships a CopilotKit React reference app that demonstrates MCP tool integration end-to-end.

### Highlights

- **Dual-panel layout**: chat on the left, document viewer on the right.
- **GitHub Flavored Markdown** with tables, strikethrough, and task lists.
- **Syntax highlighting** across 8+ languages via a shared `CustomSyntaxHighlighter` component.
- **File viewer** wired to the `get_file_content` tool.
- **Tool cards** that surface MCP invocations with status.
- **Responsive layout** sized for desktop development workflows.

### Technical Notes

- `react-markdown` v9+ with corrected inline code detection.
- `CoAgent` state shared between chat and document panels.
- Graceful fallbacks for unknown file types and large files.

### File Structure

```
copilotkit-app/
├── components/
│   ├── CustomSyntaxHighlighter.tsx  # Centralized syntax highlighting
│   ├── FileContentCard.tsx         # Document display component
│   ├── MCPToolAction.tsx           # Tool visualization
│   └── ToolCard.tsx                # Tool cards
├── app/
│   ├── page.tsx                    # Dual-panel main interface
│   └── globals.css                 # Styling and themes
└── package.json                    # Dependencies, incl. react-syntax-highlighter
```

Use it as a reference for building MCP-aware interfaces.

## 📚 Available Tools

### Search

#### search_docs
Full-text search across documentation, Python modules, and tutorial notebooks.
- Ultra-flexible `repository` parameter (array, comma / space / pipe / semicolon separated).
- Wildcards (`*`) and boolean operators (`AND` / `OR` / `NOT`).
- Acronym expansion (`UZF` → Unsaturated Zone Flow).
- Omit `repository` to search everything.

#### search_code
API and module search for FloPy and PyEMU.
- Returns signatures, parameters, docstrings.
- Includes package codes (WEL, RCH, …) and model families.
- Direct GitHub links to source.

#### search_tutorials
Tutorials and workflows.
- Filters by complexity (beginner / intermediate / advanced).
- Shows prerequisites and common modifications.
- Array search inside use cases and implementation tips.

#### semantic_search_docs
Concept-based documentation search using OpenAI embeddings. Best for "how to" and exploratory queries.

#### semantic_search_tutorials
Semantic search over tutorials with domain-aware matching (e.g., uncertainty vs. flow modeling).

### Retrieval

#### get_file_content
Fetch a complete file by exact path. Paginates files over 30 KB.

#### get_modflow_ai_info
Server overview: available repositories, tools, and statistics. No parameters.

## 💡 Usage Examples

### How AI agents use these tools

**User**: "How do I set up a pumping well in MODFLOW 6?"
**Agent calls**: `search_docs` with `query="WEL package MODFLOW 6"`
→ WEL package docs, examples, API.

**User**: "Show me a beginner tutorial for FloPy"
**Agent calls**: `search_tutorials` with `query="getting started"`, `complexity="beginner"`
→ Step-by-step FloPy tutorials with code.

**User**: "Explain how particle tracking works in groundwater models"
**Agent calls**: `semantic_search_docs` with a conceptual query
→ Theory and mathematical explanations.

**User**: "I need the NPF package documentation file"
**Agent calls**: `get_file_content` with the exact path
→ Full NPF docs.

**User**: "What is MODFLOW AI?"
**Agent calls**: `get_modflow_ai_info`
→ Server overview.

### Query tips

- Use `search_docs` without a `repository` to search everything at once.
- Use specific terms or acronyms (`UZF`, `WEL package`) rather than long sentences.
- Start with `get_modflow_ai_info` to see what's available.
- Use `semantic_search_docs` for "how / why" conceptual questions.
- Avoid overlapping the same query across multiple tools in one turn.
- Use `search_code` — not semantic search — for exact function or class names.

## 📊 Available Repositories

### Code
- **FloPy** — Python package for MODFLOW (modules and tutorials).
- **pyEMU** — Python tools for uncertainty analysis and PEST++ integration.

### Documentation
- **MODFLOW AI** — Server documentation and guides.
- **MODFLOW 6** — USGS modular groundwater flow model.
- **MODFLOW-USG** — Unstructured grid version.
- **PEST** — Parameter estimation toolkit.
- **PEST++** — Next-generation PEST tools.
- **PEST_HP** — High-performance computing version.
- **gwutils** — Groundwater utility programs.
- **plproc** — Pilot point processor.

## 🔍 Search Intelligence

### Acronym Recognition

The server expands common MODFLOW/PEST acronyms automatically:

- `WEL` → Well Package
- `RIV` → River Package
- `MAW` → Multi-Aquifer Well
- `CHD` → Constant Head Boundary
- `DRN` → Drain Package
- `EVT` → Evapotranspiration
- `RCH` → Recharge
- `SFR` → Streamflow Routing
- … and more.

### Method Selection

- **Text search** for exact terms, acronyms, quoted phrases.
- **Semantic search** for conceptual / "how to" questions.
- **Hybrid search** when a query benefits from both.

### GitHub URLs

Code results include direct links:
- FloPy modules: `github.com/modflowpy/flopy/blob/develop/…`
- PyEMU modules: `github.com/pypest/pyemu/blob/develop/…`

## 💬 Feedback & Support

- **Issues and questions**: reach out via the contact in your access email.
- **Feature requests**: tell us what would help your workflow.
- **Corrections**: suggest improvements to docs or coverage.

## 📄 License & Terms

MODFLOW-AI MCP Server is a proprietary hosted service. By using it you agree to:
- Use the service within rate limits.
- Not reverse-engineer or abuse the service.

The service is provided as-is. No source code is licensed for redistribution.

For questions or access: [LinkedIn](https://www.linkedin.com/in/dlz800).

## 🙏 Acknowledgments

Built with data from:
- [USGS MODFLOW](https://www.usgs.gov/mission-areas/water-resources/science/modflow-and-related-programs)
- [FloPy Project](https://github.com/modflowpy/flopy)
- [PEST Suite](https://pesthomepage.org/)
- The broader groundwater modeling community.

---

*For access, visit [www.modflow.ai](https://www.modflow.ai).*
