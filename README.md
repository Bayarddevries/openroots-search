# OpenRoots Search

One search box, every open genealogy source. Free, no login, no tracking, no backend.

**Live:** https://bayarddevries.github.io/openroots-search/

## What it does

Type a name (plus optional birth year and place) and get:

- **Live inline results** from official free public APIs that allow cross-origin requests:
  Internet Archive, Wikipedia, Wikidata (SPARQL), Open Library, Google Books, Wikimedia Commons.
- **One-tap prefilled links** into the sources that need their own site (login walls or bot
  protection): Find a Grave, FamilySearch (free account), BillionGraves, Geneanet, WikiTree,
  the Metis National Historical Online Database, LAC Collection Search, Chronicling America,
  HathiTrust, Automated Genealogy, Red River Ancestry, Saskatchewan vital stats, Cyndi's List.
- **Manitoba vital stats** via cross-origin POST forms (births / marriages / deaths) that open
  the official gov.mb.ca results in a new tab.

## Design rules

- Static single file (`index.html`), vanilla JS, no build step, mobile-first. Hosted on GitHub Pages.
- Only official free APIs are queried inline. Everything else is an outbound link. Nothing is
  scraped behind a login, no credentials are ever requested or handled (the private
  `genealogy-cli` FamilySearch session-cookie module stays local by design).
- No cookies, no analytics, no storage. Queries go from your browser straight to the source.
- Guardrail printed on the page: do not use this to search for living people.
- Volunteer-site data (Find a Grave family links, hobbyist trees) is labelled as leads to
  verify against primary records (scrip, census, parish registers, vital stats).

## Run locally

```
python3 -m http.server 8000
# open http://localhost:8000/index.html
```

## Extending

Add an inline source: append an object to the `inline` array in `index.html` with a `run(q)`
that returns `[{title, sub, url}]` (it must be a CORS-enabled endpoint, verified with curl:
`curl -s -D - -o /dev/null -H "Origin: https://bayarddevries.github.io" <url>` and look for
`access-control-allow-origin`). Add a deep-link source: append to the `chips` array.
