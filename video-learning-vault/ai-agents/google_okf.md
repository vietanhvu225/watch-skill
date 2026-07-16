# Google OKF (Open Knowledge Format): Structuring Enterprise Data for AI Agents

- **Video URL:** [YouTube Link](https://www.youtube.com/watch?v=fI7hZap7mZ4)
- **Watch Skill ID:** `8a893f42dc4c486c`
- **Category:** #ai-agents
- **Date Processed:** 2026-07-16
- **Duration:** 08:01

---

## 1. The Problem: Context Assembly From Scratch
AI agents excel at executing tools (writing code, querying databases), but they lack institutional knowledge (e.g. *"What is our official definition of an active user?"*). 
- **The Current State:** Enterprise knowledge is scattered across Confluence, Notion, Slack, code comments, and database schemas. 
- **The Issue:** Every agent must retrieve and figure out the same context from scratch, leading to repetitive guesswork.
- **The Solution:** **Open Knowledge Format (OKF)** — an open specification from Google Cloud (Apache License) designed to represent company knowledge in a single format any AI agent can understand.

---

## 2. Anatomy of an OKF Document
An OKF knowledge base is simply a **folder of plain Markdown files** sitting in a Git repository. A collection of these folders is called a **bundle**.

```
my-okf-bundle/
├── index.md                 # Root table of contents
├── changelog.md             # Change history (newest first)
├── datasets/
│   ├── orders.md            # OKF Document (type: BigQuery table)
│   └── customers.md         # OKF Document (type: BigQuery table)
└── metrics/
    └── active_users.md      # OKF Document (type: metric)
```

Each OKF file is split into two sections:

### I. Structured Metadata (YAML Front Matter)
Located at the very top. Only **one required field**:
- `type`: String (defines the type of resource, e.g., `BigQuery table`, `metric`, `run book`, `play book`). Used by agents to route, filter, and identify the content.

*Recommended Optional Fields:*
- `title`: Name of the resource.
- `description`: A one-line summary.
- `resource_link`: URI linking to the real asset (e.g. BigQuery console URL).
- `tags`: List of categories.
- `timestamp`: Date modified.
- *(Custom fields are allowed and parser-forgiving).*

### II. Free-form Body (Markdown)
Located below the YAML block. Contains human-and-machine-readable documentation:
- For tables: schemas, descriptions of columns, and join paths.
- For metrics: exact mathematical formulas or SQL code.
- For runbooks: step-by-step instructions.

---

## 3. Key Concepts & Mechanics

- **Navigable Knowledge Graph:** Documents link to each other using standard Markdown links (e.g. `[Orders Table](../datasets/orders.md)`). Combined, they form a graph representing the whole business.
- **Progressive Disclosure:** Folders contain `index.md` files serving as tables of contents. Instead of loading the entire folder into its context window, the agent reads the index first to decide which specific files are worth opening.
- **Git-Native Management:** Knowledge is managed exactly like code—kept in version control, reviewed via Pull Requests, and fully auditable.
- **Google's Reference Enrichment Agent:** Google provides an agent that crawls BigQuery tables and views to draft OKF docs automatically. A second pass populates schemas, join paths, and citations from docs, which humans then review.

---

## 4. Architectural Rules of OKF
1. **Minimally Opinionated:** Only one required field (`type`).
2. **Independent Producers & Consumers:** Independent format, allowing any tool to write it and any agent to read it.
3. **Format over Platform:** Just files. No SDK, no database, no accounts, and no vendor lock-in.

---

## 5. OKF vs. Other Technologies

| Feature | Wiki (Notion/Confluence) | RAG / Vector DB | MCP (Model Context Protocol) | OKF (Open Knowledge Format) |
|---|---|---|---|---|
| **Audience** | Written for humans in prose. | Fuzzy chunks retrieval. | The "socket" / execution tool layer. | The **content** / meaning layer. |
| **Structure** | Unstructured, locked in vendor tool. | Unstructured chunks, probabilistically found. | Dynamic APIs, tools, and live data. | Git-versioned Markdown files, navigable graph. |
| **Role** | Reference library. | Broad, noisy search. | Connection & action. | Structured context & definition. |

---

## 6. Limitations & Best Practices
- **Early Stage:** Currently at version 0.1, a starting point.
- **Staleness:** OKF does not automatically update itself. 
- **API vs. Doc:** Only use OKF for **stable expertise** (definitions, schemas, runbooks). Volatile data that changes frequently (live prices, inventory) belongs behind an API, not in a static document.

---

## 7. Useful Search Keywords
- `open knowledge format`, `google okf`, `progressive disclosure`, `okf bundle`, `harness context`

*Run: `watch-skill search "<keyword>"` to query this video and others in your vault.*
