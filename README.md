# Zotero Skill for Claude Code

A Claude Code skill that gives Claude read-only access to your Zotero library via the Zotero Web API v3. Once installed, you can ask Claude to search your references, list recent papers, browse collections, and more.

## Setup

### Step 1: Get your Zotero API credentials

1. Go to [Zotero API Keys](https://www.zotero.org/settings/keys) (you need to be logged in)
2. Your **userID** is shown at the top of that settings page
3. Click **"Create new private key"**
4. Give it a description (e.g., "Claude Code")
5. Under **"Personal Library"**, enable **"Allow library access"** — select **read-only** (do not enable write access)
6. Save the key and copy it

### Step 2: Store your credentials

Add these environment variables to your shell profile (`~/.zshrc` or `~/.bashrc`):

```bash
export ZOTERO_API_KEY="your-key-here"
export ZOTERO_USER_ID="your-user-id"
```

Then reload your shell:

```bash
source ~/.zshrc  # or source ~/.bashrc
```

### Step 3: Install the skill

Copy the skill file into your global Claude Code skills directory:

```bash
mkdir -p ~/.claude/skills
cp zotero/SKILL.md ~/.claude/skills/zotero.md
```

### Step 4: Allow network access

The first time Claude uses the skill, it will need to make `curl` requests to `api.zotero.org`. Approve the network access when prompted, or pre-allow it by adding this to your Claude Code settings (`~/.claude/settings.json`):

```json
{
  "permissions": {
    "allow": [
      "Bash(curl -s *api.zotero.org*)"
    ]
  }
}
```

## Usage

Once installed, just ask Claude about your Zotero library in natural language:

- "Search my Zotero for papers on transformer architectures"
- "Show me my most recently added references"
- "List my Zotero collections"
- "Find papers tagged with 'review' in my library"
- "Get the details for Zotero item ABC123"
- "What journal articles do I have from 2024?"

## What it can do

- Search items by keyword, title, author, or year
- List and browse collections
- Filter by tags or item type
- Show item details (title, authors, abstract, DOI, etc.)
- Paginate through large result sets

## Limitations

- **Read-only** — the skill cannot add, edit, or delete items in your library
- API rate limits apply (see [Zotero API docs](https://www.zotero.org/support/dev/web_api/v3/start))
