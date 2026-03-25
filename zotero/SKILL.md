---
name: zotero
description: >
  Search and read items from the user's Zotero library via the Zotero Web API v3.
  Use when the user mentions Zotero, references, bibliography, citations, papers,
  or asks to find/search/list items in their reference library.
---

# Zotero Library (Read Access)

Query the user's Zotero library using the Zotero Web API v3.
Credentials are in environment variables `ZOTERO_USER_ID` and `ZOTERO_API_KEY`.

## API Basics

Base URL: `https://api.zotero.org`

All requests must include:
```
Zotero-API-Key: $ZOTERO_API_KEY
Zotero-API-Version: 3
```

Use `curl -s` for all requests.

## Common Endpoints

### Search items
```bash
curl -s -H "Zotero-API-Key: $ZOTERO_API_KEY" -H "Zotero-API-Version: 3" \
  "https://api.zotero.org/users/$ZOTERO_USER_ID/items?q=SEARCH_TERM&limit=10&format=json"
```

### List top-level items (most recent)
```bash
curl -s -H "Zotero-API-Key: $ZOTERO_API_KEY" -H "Zotero-API-Version: 3" \
  "https://api.zotero.org/users/$ZOTERO_USER_ID/items/top?limit=25&sort=dateModified&direction=desc&format=json"
```

### Get a specific item by key
```bash
curl -s -H "Zotero-API-Key: $ZOTERO_API_KEY" -H "Zotero-API-Version: 3" \
  "https://api.zotero.org/users/$ZOTERO_USER_ID/items/ITEM_KEY?format=json"
```

### List collections
```bash
curl -s -H "Zotero-API-Key: $ZOTERO_API_KEY" -H "Zotero-API-Version: 3" \
  "https://api.zotero.org/users/$ZOTERO_USER_ID/collections?format=json"
```

### List items in a collection
```bash
curl -s -H "Zotero-API-Key: $ZOTERO_API_KEY" -H "Zotero-API-Version: 3" \
  "https://api.zotero.org/users/$ZOTERO_USER_ID/collections/COLLECTION_KEY/items/top?format=json&limit=25"
```

### Search by tag
```bash
curl -s -H "Zotero-API-Key: $ZOTERO_API_KEY" -H "Zotero-API-Version: 3" \
  "https://api.zotero.org/users/$ZOTERO_USER_ID/items?tag=TAG_NAME&format=json&limit=25"
```

### List all tags
```bash
curl -s -H "Zotero-API-Key: $ZOTERO_API_KEY" -H "Zotero-API-Version: 3" \
  "https://api.zotero.org/users/$ZOTERO_USER_ID/tags?limit=100&format=json"
```

## Useful Query Parameters

- `q=TERM` — full-text search
- `qmode=titleCreatorYear` — restrict search to title, creator, year
- `tag=NAME` — filter by tag (repeat for AND: `tag=a&tag=b`)
- `itemType=journalArticle` — filter by type (book, conferencePaper, thesis, etc.)
- `sort=dateAdded|dateModified|title|creator|date` — sort field
- `direction=asc|desc` — sort direction
- `limit=N` — max results (1-100, default 50)
- `start=N` — offset for pagination

## Formatting Results

When presenting items to the user, extract and display:
- **Title**: `data.title`
- **Authors**: `data.creators[]` (each has `firstName`, `lastName`, `creatorType`)
- **Year**: `data.date`
- **Publication**: `data.publicationTitle` or `data.bookTitle`
- **DOI**: `data.DOI`
- **URL**: `data.url`
- **Tags**: `data.tags[].tag`
- **Abstract**: `data.abstractNote`
- **Item key**: `data.key` (for follow-up queries)

Format authors as "LastName, FirstName" and present results in a readable way.
Mention the item key so the user can ask for more details on specific items.

## Notes

- This is READ-ONLY access. Do not attempt write/delete operations.
- Pagination: check the `Total-Results` header if you need counts. Use `start` param to page through large result sets.
- For group libraries, replace `/users/$ZOTERO_USER_ID/` with `/groups/GROUP_ID/`.
