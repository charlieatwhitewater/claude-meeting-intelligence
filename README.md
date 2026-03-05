# Claude Meeting Intelligence

A ready-to-go workspace for turning meeting transcripts into structured summaries, action items, and persistent meeting memory — using Claude Desktop (Cowork) or Claude Code.

**No coding required.** Clone this repo, follow the setup steps, and start dropping meeting transcripts in.

---

## What This Does

- **Processes meeting transcripts** into clean, structured summaries with action items
- **Maintains meeting memory** — Claude remembers context across meetings (who said what, what was decided, what's still open)
- **Tracks action items** across meetings with owners and due dates
- **Compares across meetings** — "What changed since last week's sync?" or "What has Jane committed to this month?"
- **Connects to Notion** (optional) for storing and organizing meeting notes
- **Connects to Google Calendar** (optional) for matching transcripts to calendar events

---

## Quick Start (5 minutes)

### Prerequisites

- [Claude Desktop](https://claude.ai/download) installed (free tier works, Pro recommended)
- OR [Claude Code](https://docs.anthropic.com/en/docs/claude-code) installed (`npm install -g @anthropic-ai/claude-code`)

### Step 1: Clone this repo

```bash
git clone https://github.com/charlieatwhitewater/claude-meeting-intelligence.git
cd claude-meeting-intelligence
```

### Step 2: Install MCP servers

```bash
# Install Node.js if you don't have it (https://nodejs.org)
npm install
```

### Step 3: Choose your setup path

<details>
<summary><strong>Option A: Claude Desktop / Cowork (Recommended for non-developers)</strong></summary>

1. Open Claude Desktop
2. Go to **Settings > Developer > Edit Config**
3. Copy the contents of `claude-desktop-config.json` from this repo into your config
4. Restart Claude Desktop
5. Open **Cowork** mode (the middle tab)
6. Navigate to this repo's folder
7. Start chatting! Try: *"Read the transcript in meetings/example-transcript.txt and create a structured summary"*

</details>

<details>
<summary><strong>Option B: Claude Code (For developers / CLI users)</strong></summary>

1. The `.mcp.json` is already configured in this repo
2. Run `claude` from this directory
3. Try: *"Read the transcript in meetings/example-transcript.txt and create a structured summary"*

</details>

### Step 4: Drop in your first transcript

1. Copy a meeting transcript (from Tactiq, Otter.ai, Teams, Zoom, etc.) into the `meetings/incoming/` folder as a `.txt` or `.md` file
2. Ask Claude: *"Process the new transcript in meetings/incoming/"*
3. Claude will create a structured summary in `meetings/processed/`

---

## How It Works

```
meetings/
├── incoming/              ← Drop raw transcripts here
├── processed/             ← Structured summaries appear here
├── example-transcript.txt ← Try this first
└── example-output.md      ← What the output looks like

memory/
├── people.md              ← Claude tracks people across meetings
├── action-items.md        ← Running action item tracker
├── decisions.md           ← Key decisions log
└── open-questions.md      ← Unresolved items

context/
├── team-roster.md         ← Edit this: who's on your team
└── projects.md            ← Edit this: active projects for context
```

### The Memory System

Claude maintains memory across sessions:

- **People**: Names, roles, communication patterns, what they tend to own
- **Action Items**: Tracked across meetings — new, in-progress, completed, overdue
- **Decisions**: What was decided, when, by whom, with context
- **Open Questions**: Things that were raised but not resolved

Each time you process a transcript, Claude updates these files. Over time, it builds a comprehensive picture of your team's work.

---

## Customizing for Your Team

### 1. Edit the team roster

Open `context/team-roster.md` and add your team members:

```markdown
## Team Members

- **Jane Smith** - Director of Operations, Vancouver. Cross-functional planning.
- **Alex Chen** - Engineering Lead, focus on technical strategy.
- **Sam Rivera** - Design Manager, focus on AI tool evaluation.
```

### 2. Edit active projects

Open `context/projects.md` and add what your team is working on:

```markdown
## Active Projects

- **AI Tool Evaluation** - Assessing Claude, Copilot, Gemini for company-wide adoption
- **Architecture Design Platform** - Artlist rollout for pre-contract work
```

### 3. Add transcription corrections (optional)

If your transcription tool consistently mangles certain names or terms, add them to `context/corrections.md`:

```markdown
## Known Transcription Errors

| Heard As | Correct |
|----------|---------|
| "cloud code" | "Claude Code" |
| "co-work" | "Cowork" |
```

---

## Optional: Connect to Notion

If you want meeting summaries to automatically publish to Notion:

### Step 1: Get a Notion API key

1. Go to [notion.so/my-integrations](https://www.notion.so/my-integrations)
2. Click **New Integration**
3. Name it "Claude Meeting Intelligence"
4. Copy the **Internal Integration Secret**

### Step 2: Create a Notion database

Create a database in Notion with these properties:
- **Name** (title) — Meeting name
- **Date** (date) — Meeting date
- **Participants** (multi-select) — Who attended
- **Status** (select) — Draft / Final

### Step 3: Share the database with your integration

Click **...** on the database > **Connections** > Add your integration

### Step 4: Update config

Add your Notion token to the MCP config:

**Claude Desktop**: Edit your `claude_desktop_config.json`:
```json
{
  "mcpServers": {
    "notion": {
      "command": "npx",
      "args": ["-y", "@notionhq/notion-mcp-server"],
      "env": {
        "OPENAPI_MCP_HEADERS": "{\"Authorization\": \"Bearer YOUR_NOTION_TOKEN\", \"Notion-Version\": \"2022-06-28\"}"
      }
    }
  }
}
```

**Claude Code**: Add to `.mcp.json`:
```json
{
  "mcpServers": {
    "notion": {
      "command": "npx",
      "args": ["-y", "@notionhq/notion-mcp-server"],
      "env": {
        "OPENAPI_MCP_HEADERS": "{\"Authorization\": \"Bearer YOUR_NOTION_TOKEN\", \"Notion-Version\": \"2022-06-28\"}"
      }
    }
  }
}
```

Then ask Claude: *"Publish the latest meeting summary to Notion"*

---

## Example Prompts

### Processing transcripts
- *"Process the transcript in meetings/incoming/standup-march-5.txt"*
- *"Summarize today's meeting and extract action items"*
- *"Process all unprocessed transcripts in incoming/"*

### Querying memory
- *"What action items does Alex have open?"*
- *"What was decided about the AI platform last week?"*
- *"Give me a summary of everything discussed about architecture in the last month"*
- *"What open questions do we still need to resolve?"*

### Comparing across meetings
- *"What changed between this week's sync and last week's?"*
- *"Show me Jane's action items across all meetings this month"*
- *"Are there any recurring topics that keep coming up unresolved?"*

### Meeting prep
- *"I have a meeting with Sam tomorrow about design tools. What context should I have?"*
- *"What were the key decisions from the last three architecture meetings?"*

---

## Troubleshooting

**"Claude doesn't see the MCP tools"**
- Make sure you restarted Claude Desktop after editing the config
- Check that `npm install` completed without errors
- In Claude Code, run `claude` from this directory (not a parent)

**"The summaries are generic / missing context"**
- Edit `context/team-roster.md` and `context/projects.md` with your team's info
- The more context Claude has, the better the output
- Add transcription corrections for commonly mangled terms

**"I want to use this with Teams/Zoom/Otter transcripts"**
- Just export the transcript as text and drop it in `meetings/incoming/`
- Claude handles any transcript format — it just needs the raw text

---

## License

MIT — Use however you'd like.
