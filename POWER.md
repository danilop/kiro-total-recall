---
name: "kiro-total-recall"
version: "0.1.0"
displayName: "Kiro Total Recall"
description: "Semantic search across Kiro CLI and IDE conversation history - find past discussions by meaning, not just keywords"
keywords: ["history", "recall", "remember", "past conversation", "previous chat", "what did we discuss", "how did we", "like we discussed", "as I mentioned"]
author: "Danilo Poccia"
---

# Kiro Total Recall

Search past Kiro conversation history using semantic search. Works with both Kiro CLI and Kiro IDE conversations.

## When to Trigger

### 1. Recovering Context
When the user references something discussed earlier:
- "Continue with the approach we discussed"
- "Like I mentioned before..."
- "What did we decide about..."

```
User: "Continue implementing the auth system like we discussed"
→ search_project_history(query="auth system implementation")
```

### 2. Cross-Session Memory
Find discussions from previous sessions:
- "How did we fix that bug yesterday?"
- "What approach did we decide on last week?"
- "Remember when we refactored the database?"

```
User: "How did we fix that auth bug last week?"
→ search_project_history(query="auth bug fix", after="2025-01-25")
```

### 3. Cross-Project Patterns
Find user preferences across all projects:
- "How do I usually handle errors?"
- "What's my preferred testing approach?"
- "What package manager do I use?"

```
User: "How do I usually structure React components?"
→ search_global_history(query="React component structure")
```

## Available Tools

| Tool | Scope | Use Case |
|------|-------|----------|
| `search_project_history` | Current workspace | Bugs, decisions, implementations in *this* codebase |
| `search_global_history` | All workspaces | User preferences, patterns across *all* work |
| `search_cli_history` | CLI only | Conversations from Kiro CLI sessions |
| `search_ide_history` | IDE only | Conversations from Kiro IDE sessions |

## Parameters

All tools accept:
- `query` (required): Keywords or sentence to search
- `after` (optional): Filter to messages on/after this date (ISO 8601)
- `before` (optional): Filter to messages before this date (ISO 8601)
- `context_size` (default: 3): Messages before/after each match
- `threshold` (default: 0.2): Minimum similarity (0-1)
- `max_results` (default: 10): Results to return
- `offset` (default: 0): Skip results for pagination

## Date Filtering Examples

**Single day:**
```
search_project_history(query="auth", after="2025-01-31", before="2025-02-01")
```

**Past week:**
```
search_project_history(query="database", after="2025-01-25")
```

## Tips

- Results include **context** (surrounding messages)
- Use `offset` to paginate when `has_more` is true
- Higher scores (closer to 1.0) = better semantic matches
- The `source` field shows whether a result is from CLI or IDE
