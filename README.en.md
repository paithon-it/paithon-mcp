# paithon MCP

English · [Italiano](README.md)

The public MCP server of [paithon.it](https://paithon.it), the Italian magazine on artificial intelligence. It gives an AI assistant (Claude, Cursor, Codex and any other MCP client) read-only access to what the site shows an anonymous visitor: articles, dossiers, paper write-ups, [Paithon Book](https://book.paithon.it/main/), the AI Act page and the Italia 2070 figures. No account and no key are needed. The content is in Italian.

Endpoint: `https://paithon.it/mcp` (Streamable HTTP, no authentication).

## What it is for

Each tool answers a question you can really ask.

| Tool | A question you can ask it |
|---|---|
| `cerca` | "What has paithon written about RAG?" |
| `leggi` | "Read the article at this address and summarise it." |
| `raccolta` | "Which dossiers exist, and in what order should I read the one on agents?" |
| `capitoli` | "What does the table of contents of Paithon Book look like?" |
| `rimando` | "Where does the book explain attention?" |
| `sezione` | "Explain gradient descent the way the book tells it, at the elementary level." |
| `paper` | "What does paithon have on arXiv paper 1706.03762?" |
| `settimana` | "Which papers were in last week's Rassegna?" |
| `mappa` | "How is the site organised, and where do I start looking?" |
| `orienta` | "I want to understand agents: what should I read in the book, and what on the site?" |
| `ai_act` | "What must someone who puts a chatbot on their website do under the AI Act?" |
| `percorso` | "I already program in Python and want to understand LLMs: what reading path do you suggest?" |
| `dati_italia` | "How many Italian companies use AI, and from which source?" |

## How to install

The server needs no account and no key: where a client offers a sign-in (OAuth, for instance), leave the optional authentication fields empty. The service has been live since 15 November 2026. The Claude Code line is tested with a real client; for the other clients, their official page, linked at the end, prevails.

### Claude Code

```
claude mcp add --transport http paithon https://paithon.it/mcp
```

### Claude Desktop and claude.ai

A remote server is not added to the `claude_desktop_config.json` file (that file is for local servers): it is added as a custom connector from the interface. In Customize, Connectors, "Add", "Add custom connector", enter the name `paithon` and the address `https://paithon.it/mcp`, and leave the optional authentication fields empty. According to Claude's documentation, custom connectors are available on the Free (one only), Pro, Max, Ultra, Team and Enterprise plans; on Team and Enterprise an organisation owner adds it first. Menu labels may change: the official page cited at the bottom is the reference.

### Cursor

In `~/.cursor/mcp.json` (all projects) or `.cursor/mcp.json` (one project):

```json
{
  "mcpServers": {
    "paithon": {
      "url": "https://paithon.it/mcp"
    }
  }
}
```

### Codex

From the command line:

```
codex mcp add paithon --url https://paithon.it/mcp
```

or in `~/.codex/config.toml`:

```toml
[mcp_servers.paithon]
url = "https://paithon.it/mcp"
```

## The tools

| Tool | Arguments | What it returns |
|---|---|---|
| `cerca` | `testo` (at least 2 characters), `area`, `formato`, `limite` (1 to 20, 10 if missing) | One line per published article: title, link, excerpt |
| `leggi` | `url` (a paithon.it address or a slug), `parte` (from 1, 1 if missing) | The article text, with the citation to use. An article over 40,000 characters comes in parts: each one says how many there are and how to ask for the next |
| `raccolta` | `slug` (optional) | With no arguments, the public dossiers and the areas; with a slug, its articles in reading order |
| `capitoli` | none | The table of contents of Paithon Book: parts, chapters, sections, with links |
| `rimando` | `concetto` | The chapters and sections of the book that explain it; if the book does not cover it, it says so |
| `sezione` | `url` (the address of a section, as `capitoli` and `rimando` give it, or its title), `livello` (`elementare` or `superiore`, optional), `parte` | The text of a section of Paithon Book, with formulas in LaTeX and the code. Without a level you get both, labelled; a long section comes in parts |
| `paper` | `arxiv` (an identifier or an arxiv.org address) | A paper's card: abstract, licence, key points, our write-up, related papers. Only when the site's paper section is open |
| `settimana` | `indietro` (0 to 12, 0 if missing) | The papers of the latest Rassegna, or of earlier weeks. Only when the paper section is open |
| `mappa` | none | How the site is organised: guide pages, formats, areas with article counts, dossiers |
| `orienta` | `argomento` | For a topic, what the book and the site say about it, split into "understand", "do", "what is happening", "go deeper", with a suggested starting point |
| `ai_act` | `tema` (optional) | What the site's page says today about the AI Act and Italian law, with its update date and primary sources |
| `percorso` | `profilo` (`da-zero`, `programmo`, `llm`, `insegno`, `decido`) or `obiettivo` | Reading stages by profile or by goal; with no arguments it lists the five profiles |
| `dati_italia` | `tema` (optional) | The Italia 2070 figures exactly as the page holds them, with the chapter's primary source |

## The rules

- **Published content only.** No drafts, no hidden content, no member-only articles beyond the preview anyone sees.
- **Every answer carries links**, and the server asks the client to cite them. The book is cited by chapter title and address, and its text (licensed CC BY-NC-ND 4.0) is there to explain from: quote passages with their source, and do not redistribute the book inside another product or in modified versions.
- **The book is read only from its index**: `sezione` downloads a page of book.paithon.it only if it is one of those in the book's public index.
- **Articles that asked to stay out of AI systems** can be found with `cerca`, but `leggi` returns only their title, excerpt and link.
- **Limits**: 120 calls every 10 minutes per address group; a request of at most 65,536 bytes; one message at a time; `leggi` returns long articles in parts of 40,000 characters, so no answer goes over a client's limit; `cerca` returns at most 20 results.

## What it does not do

- **It is not legal advice.** `ai_act` is an orientation with sources: it tells you what the site's page says and where the legal texts are, and it does not replace an opinion.
- **It does not give the full text of papers.** You get metadata, the abstract, key points and our write-up; the text is on arXiv.
- **It makes no forecasts.** `dati_italia` reports the Italia 2070 page's figures and their sources, with no recalculation and no estimates or probabilities of paithon's own.
- **It writes and changes nothing** on the site: every tool is read-only.
- **It does not replace the book**: Paithon Book explains how something works and is read in order; the tools take you there, and `sezione` gives you its text to cite.

## Privacy

Questions and answers are not stored, and callers are not tracked. Only the number of calls per tool per day is counted, in aggregate, to know which tools are useful. The network address is used only for the call limit, as a signed fingerprint that does not let anyone recover the address. The rest is in the notice at [paithon.it/privacy](https://paithon.it/privacy). To contact paithon, see the references the site publishes.

## Licence

This documentation is released under the [Creative Commons Attribution 4.0 International](https://creativecommons.org/licenses/by/4.0/) licence (CC BY 4.0); the official text is in the `LICENSE` file. The licence covers the documentation: the server code is not in this repository. The content the server returns remains subject to the licences of the site and of Paithon Book (CC BY-NC-ND 4.0).

<!-- Pages consulted on 2026-11-13 for client syntax:
- Claude Code: https://code.claude.com/docs/en/mcp
- Claude Desktop and claude.ai: https://support.claude.com/en/articles/11175166-get-started-with-custom-connectors-using-remote-mcp
- Cursor: https://cursor.com/docs/context/mcp
- Codex: https://developers.openai.com/codex/mcp (redirects to https://learn.chatgpt.com/docs/extend/mcp?surface=cli)
- Licence: https://creativecommons.org/licenses/by/4.0/legalcode.txt
-->
