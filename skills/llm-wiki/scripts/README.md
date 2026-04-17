# Wiki Helper Scripts

These scripts reduce repetitive bookkeeping work.

## wiki-new-page

Create wiki pages with correct naming and formatting.

```bash
wiki-new-page <type> <name>
```

Types: `concept`, `entity`, `decision`, `comparison`, `synthesis`, `source`

**Examples:**
```bash
wiki-new-page concept distributed-consensus
wiki-new-page decision use-postgres
wiki-new-page source paper-raft
```

## wiki-update-index

Manage the content catalog.

```bash
wiki-update-index add <type> <name> <description>
wiki-update-index remove <type> <name>
wiki-update-index stats
```

**Examples:**
```bash
wiki-update-index add concept three-phase-commit "protocol for distributed commit"
wiki-update-index stats
```

## wiki-log

Append standardized entries to the changelog.

```bash
wiki-log ingest <source-title> <summary>
wiki-log lint <summary>
wiki-log synthesis <type> <name> <summary>
```

**Examples:**
```bash
wiki-log ingest "Paper: Raft Consensus" "Added summary. Updated 3 concepts."
wiki-log lint "Fixed 2 broken links. Created 1 stub."
```