# kvasir-mcp

**MCP server for verifiable German, Austrian, Swiss and EU law.** Resolve, search
and verify legal citations against the official sources — from any MCP client.

This repository documents the **hosted** server. There is nothing to install and
no local build: point your client at the endpoint below.

```
https://kvasir.legal/mcp
```

Transport: streamable HTTP. Auth: OAuth (most clients discover it automatically)
or an `X-API-Key` header. A free account with 2,000 monthly units is at
[kvasir.legal/signup](https://kvasir.legal/signup); reading law on the site needs
no account at all.

## Why

Language models answer legal questions from a training snapshot. They paraphrase
amended provisions, cite repealed directives, and cannot say which *version* an
answer relied on. This server gives an agent the two operations that fix that:
resolve a citation to the wording in force, and verify the citations the agent
itself produced.

## Coverage

| Jurisdiction | Statutes | Case law |
|---|---|---|
| Germany — federal | ✅ | ✅ (federal courts) |
| Bavaria | ✅ | ✅ |
| North Rhine-Westphalia | ✅ | ✅ |
| Saxony · Brandenburg · Bremen | ✅ | — |
| Austria | ✅ | ✅ (OGH, VfGH, VwGH) |
| Switzerland | ✅ | — |
| EU | ✅ incl. delegated & implementing acts, Commission guidelines | ✅ (CJEU, General Court) |

~460,000 norms, ~512,000 decisions, ~1.4 million typed citation edges. Sources are
checked daily (weekly for Saxony and Bremen). Repealed acts stay in the corpus,
flagged as repealed. Live counts and per-source timestamps:
[`/api/v1/status`](https://kvasir.legal/api/v1/status).

## Tools

| Tool | Use it when |
|---|---|
| `resolve_citation` | you have a citation string — „§ 573 Abs. 2 BGB", „Art. 6 GDPR" |
| `search_legal` | you have a question or topic, not a citation |
| `check_draft` | you have a full draft, memo or brief — citations **and** quoted wording |
| `verify_citations` | you have a bare list of references to check |
| `get_legal_object` | you have a canonical URI |
| `get_legal_objects` | you need several objects at once |
| `get_legal_relations` | you want the citation graph from one node |
| `legal_uri_grammar` | you want to build URIs yourself |

Arguments, limits and response shapes:
[kvasir.legal/developers/mcp-tools](https://kvasir.legal/developers/mcp-tools).

## Example

Ask your agent: *„Which decisions cite Art. 6 GDPR?"* — it calls
`get_legal_relations` on `norm:eu:GDPR:art_6` and gets the citing decisions with
court, docket number and canonical URI, ordered by authority.

Or without any client:

```bash
curl -s "https://kvasir.legal/api/v1/relations/norm:eu:GDPR:art_6?direction=in&limit=5"
```

## Honest limits

- Verification is a fact about **this corpus**. A miss means „not found here" —
  a typo or a coverage gap — not „this citation is fake".
- A citation can exist, be current, be quoted correctly, and still be the wrong
  authority for the question. This is not legal advice.
- Commission guidelines are interpretive, not binding law, and are labelled as such.
- No case law yet for Switzerland, Saxony, Brandenburg and Bremen.

## Links

- Website — https://kvasir.legal
- API docs — https://kvasir.legal/developers
- Benchmark — https://kvasir.legal/developers/benchmark
- Sources & rights — https://kvasir.legal/sources
- Contact — hello@kvasir.legal

Licence note: the corpus is served under the terms at
[kvasir.legal/agb](https://kvasir.legal/agb). Search and answer engines are
welcome; crawling for model training is not permitted.
