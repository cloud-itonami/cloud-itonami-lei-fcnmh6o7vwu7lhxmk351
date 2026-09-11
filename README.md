# cloud-itonami-lei-fcnmh6o7vwu7lhxmk351

**独立した第三者による分析/アーカイブであり、Coterra Energy Inc. と提携・後援関係にありません。**
This is an independent third-party archive/analysis. Not affiliated with,
endorsed by, or sponsored by Coterra Energy Inc..

## What this is

A per-company reference/archive repository in the `cloud-itonami-lei-*`
family (ADR-2607110300, [com-junkawasaki/root design
rationale](https://github.com/com-junkawasaki/root/blob/main/90-docs/adr/2607110300-cloud-itonami-lei-corporate-tos-catalog.md)).
It archives Coterra Energy Inc.'s publicly published Terms of Service / Terms of Use
text, keyed by the company's ISO 17442 Legal Entity Identifier (LEI), with
full source-url + retrieved-at + sha256 provenance for every entry so the
document's revision history can be tracked over time via git history.

This is a **read-only reference/archive**, not an Advisor⊣Governor actuation
actor — it proposes or executes nothing on Coterra Energy Inc.'s behalf.

## Company identity

| Field | Value |
|---|---|
| Legal name | Coterra Energy Inc. |
| LEI | [FCNMH6O7VWU7LHXMK351](https://search.gleif.org/#/record/FCNMH6O7VWU7LHXMK351) |
| Jurisdiction | US-DE |
| Website | https://www.coterra.com |
| Ticker | NYSE:CTRA |
| Industry (this repo's discovery context) | ISIC 0620 Natural gas extraction |

## Data

- `80-data/public/tos.journal.edn` — an EDN quad-log
  `[<doc-id> :attr value <tx> :add]`. See `NOTICE` for copyright/attribution
  of the archived third-party text.
- `facts.edn` — 18 verified registry facts with per-fact provenance. **Generated** — see below.
- `scripts/verify-facts.cljk` — re-fetches every source `facts.edn` cites and fails if
  the live record disagrees. Vendored from `com-junkawasaki/root`
  (`scripts/lei-verify-facts.cljs`); fix issues in the canonical and re-vendor.
- `blueprint.edn` — machine-readable company identity record.

## Verifying the record

The identity table above used to be assertions with nothing in the repository
behind them. `facts.edn` now carries them as data, and every value in it was read
out of a public registry response whose URL and retrieval time sit next to the
value:

```
kbb --backend sci scripts/verify-facts.cljk           # check the recorded facts against the live sources
kbb --backend sci scripts/verify-facts.cljk --write   # re-fetch and rewrite facts.edn
```

Eleven GLEIF/ISO URLs were fetched and eighteen facts recorded — the LEI record
(legal name as GLEIF spells it, **`COTERRA ENERGY INC.`**, upper case, `en`;
entity **ACTIVE**, registration **ISSUED**, `FULLY_CORROBORATED` / `CONFORMING`;
entity status and registration status are different fields and are recorded
separately; legal address the Corporation Trust Company registered-agent address
in Wilmington and headquarters `840 Gessner, Suite 1400, 77024, Houston, US-TX, US`,
both as GLEIF spells them; entity creation date `1989-12-14`, initial LEI
registration `2012-06-06`, last updated `2026-07-16`, next renewal `2027-06-20`;
S&P Global id `258181`; OpenCorporates id `us_de/2216250`; no BIC), its ISIN
mapping (**9** instrument identifiers — a count read from `meta.pagination.total`
of the cited page, and because the list fits in one page each identifier is also
recorded as its own entity; all nine are of the form `US127097A…`, and GLEIF's
mapping does not say what kind of instrument each one is, so neither does this
file), its managing LOU and LEI-issuer accreditation (Bloomberg Finance L.P.,
US-DE, accredited 2017-04-13), registration authority `RA000602` (Delaware
Division of Corporations — `corp.delaware.gov` — where the entity is file number
`2216250`), ISO 20275 legal form `XTIQ` (`Corporation`, US-DE), and **both
consolidation levels**: the direct parent and the ultimate parent are the same
entity, **`DEVON ENERGY CORPORATION`** (LEI `54930042348RKR3ZPN35`, US-DE,
ACTIVE), each recorded as its own entity with the relationship kind GLEIF
reports (`IS_DIRECTLY_CONSOLIDATED_BY` / `IS_ULTIMATELY_CONSOLIDATED_BY`). Nine
of the eleven URLs answered `200` when the file was written; the
`direct-parent-reporting-exception` and `ultimate-parent-reporting-exception`
endpoints answered `404` because GLEIF publishes the *parent* side of that pair
for this entity, which the checker treats as a fact rather than a failure.

That parent is the finding of this record, and it is not what the identity table
above says. The table (from `blueprint.edn`, retrieved 2026-07-25) describes an
independently listed company, `NYSE:CTRA`; GLEIF now records the company as
consolidated by Devon. Read out of band when this landed, and not cited in
`facts.edn` because the generator does not record relationship periods: the
`direct-parent-relationship` resource dates the relationship period from
`2026-05-07`, was first registered with GLEIF on `2026-07-16`, and carries
corroboration level `ENTITY_SUPPLIED_ONLY` — weaker than the `FULLY_CORROBORATED`
entity record. Devon's own record, read the same way, lists this LEI among its
**3** direct children and reports `NO_KNOWN_PERSON` for its own ultimate parent,
which is why the chain stops there. Whether `CTRA` still trades, and what the
transaction behind the relationship was, are not things GLEIF records and were
not investigated here; the ticker in the table is left as it was recorded, not
re-verified.

GLEIF records **0 direct children** for this LEI (`meta.pagination.total` of the
cited `direct-children` page). That is a measured zero about what GLEIF's
relationship data holds, not a claim that the company has no subsidiaries: a
company appears as a child only if it holds an LEI and reports the relationship.
Cross-checked out of band: a GLEIF search on the legal name `coterra` returns
**2** records — this one and `COTERRA ENERGY OPERATING CO.`
(`11KYOFXPU1C4CQL1CL44`), which reports a `NON_CONSOLIDATING` reporting exception
rather than a parent, so it is not a child in GLEIF's data either. Neither search
is cited in `facts.edn`.

The cited LEI record also carries one `otherNames` entry — previous legal name
**`CABOT OIL & GAS CORPORATION`** — and one `CHANGE_LEGAL_NAME` event GLEIF dates
effective `2025-07-14`, `COMPLETED`. The generator records neither, so they are
named here and absent from `facts.edn`; that effective date is what GLEIF reports
and was not investigated. The record has no `otherAddresses`, no successor
entities, no `qcc` and no BIC, so nothing else on it is left out.

The check has three exit codes, not two: `0` when every cited URL answered and
every recorded value still matches, `1` when a citation broke or a value drifted
(each difference is named, with the recorded and live values side by side), and
`3` when the check could not be performed at all — `facts.edn` missing or empty,
or GLEIF unreachable at the transport level — because a check that could not run
must not look like a check that ran and found nothing. Before this landed, all
three were shown against the live API: unmodified → `0`; `:company/jurisdiction`
edited from `US-DE` to `FR` → `1`, naming `gleif-lei-record :company/jurisdiction`;
the `gleif-direct-parent` entity deleted → `1`, naming it as `ADDED`; one ISIN
altered → `1`, naming `gleif-isin-us127097an32 :securities/isin`; `fetch` made to
fail with `ENOTFOUND` → `3`; `facts.edn` absent → `3`. The first attempt at the
jurisdiction break used a `sed` form BSD `sed` ignores, and the resulting `0` was
caught only because the file was diffed against its backup before the run was
trusted.

## License

Repository structure: AGPL-3.0-or-later (see LICENSE). Archived third-party
ToS text: copyright Coterra Energy Inc. (see NOTICE).
