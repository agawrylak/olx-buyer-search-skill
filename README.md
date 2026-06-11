# OLX Buyer Search Skill

Portable agent skill for searching OLX.pl listings from natural-language buyer intent.

The skill focuses on useful buyer recommendations rather than keyword matches. It separates hard requirements from preferences, searches OLX with realistic Polish queries, verifies recognizable products with external product facts, and rejects wrong listing types or unsuitable offers.

## Use Cases

- Find real OLX.pl offers matching a buyer intent.
- Compare listings with evidence from OLX and product research.
- Avoid false positives from unrelated categories.
- Reject accessories, parts, services, damaged items, wrong versions, or listings that fail hard requirements.

## Installation

Copy `SKILL.md` into any agent or assistant system that supports reusable skill/instruction files.

For systems that use directory-based skills, copy this directory into that system's skills directory, for example:

```text
skills/olx-buyer-search/
```

Then use the skill when searching OLX.pl listings with the browser, web-fetch, or search capabilities available in your agent environment.

## Contents

- `SKILL.md` - skill instructions and workflow.

## Notes

This skill is optimized for OLX.pl buyer searches. It does not scrape private data and should only use publicly available listing information.
