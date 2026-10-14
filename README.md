# Vernacular

**Every niche has its own language.** Watch fans say "Pepsi" for a Rolex GMT-Master II ref. 126710BLRO. Sneaker fans say "Pandas" for a Nike Dunk Low in white and black. Card collectors say "RDPA" for a Rookie Debut Patch Auto.

Product catalogs only know the official names, so this language is invisible to anything built on a catalog. Vernacular is a small personal study of that gap. It asks how often fans' own words can be mapped back to one exact product, and what it takes to do that accurately.

> **Status:** early and under construction. The numbers will be published once they've been measured on a hand-labeled test set.

## What it does

1. **Reads** a small number of public comments from a few hobby communities. It's read-only and never posts, comments, votes or messages anyone.
2. **Finds** each phrase that refers to a product ("my GMT," "the Pandas," "the OG Fleer Jordan").
3. **Matches** each phrase to one item in a hand-built catalog. If it can't tell which item is meant, it says so instead of guessing.
4. **Learns** from the cases a person confirms, so each run needs less review than the last.
5. **Publishes** aggregate findings and a public glossary of community shorthand. Every example links back to its original thread.

## Scope

| Niche | Communities | Products in scope (to start) |
|---|---|---|
| Watches | r/Watches, r/rolex | Rolex, Tudor, Omega, Seiko |
| Sneakers | r/Sneakers, r/Jordans | Nike Dunk, Air Jordan 1 |
| Trading cards | r/sportscards, r/basketballcards | A handful of recent basketball sets plus classic rookies |

The catalog is built **from the mentions outward**: it covers the items people actually talk about. Mentions of anything outside it are counted and reported as "out of catalog," never dropped.

## How accuracy is measured

- **Canonical catalog:** every entry is checked against a primary source (brand reference pages, manufacturer style codes, official set checklists) and records that source.
- **Held-out test set:** about 300 hand-labeled mentions, each tagged as an exact item, a model family only, ambiguous, or not a product. It's never used for tuning.
- **Strong baseline:** fuzzy and identifier matching on official catalog names, so the comparison is fair.
- **Metrics:** precision when the system commits to an answer, coverage, abstain rate, and exact vs. family accuracy, with confidence intervals.
- **Model use:** an LLM is used only to choose among catalog candidates or to abstain. It can't invent items. **No model is trained on Reddit content.**

## Data handling

- **Access:** read-only access through Reddit's Data API, under Reddit's terms and with approval. Low volume: a few hundred comments a month, with requests capped well below the rate limit.
- **What's stored:** short excerpts and permalinks only. **No usernames are stored or published.**
- **Deletions:** content deleted or removed on Reddit is purged from storage within 48 hours, and a deletion sweep runs before every analysis and every publish.
- **What's published:** aggregate statistics and short, attributed excerpts that link back to Reddit, on a personal, ad-free site.
- **Not commercial:** nothing is sold or redistributed.

## Planned Repo layout

```
catalog/     hand-verified canonical items per niche (with source URLs)
glossary/    community names → canonical items, with when each was first seen
gold/        labeling guide (the labeled test set itself is not published)
prompts/     versioned prompts for mention extraction and resolution
src/         collection, extraction, matching, review, evaluation, site build
runs/        per-run metrics and manifests (no raw comment text)
```

## Planned Usage

Requires Python 3.11+, Reddit API credentials (approved, script app) and an Anthropic API key.

```bash
cp .env.example .env        # add your credentials
pip install -r requirements.txt
python src/sweep_deletions.py
python src/collect_api.py --config config/communities.yaml
python src/extract.py && python src/resolve.py
python src/evaluate.py
```

## Contact

Nico Staple · [nicostaple.com](https://nicostaple.com)
