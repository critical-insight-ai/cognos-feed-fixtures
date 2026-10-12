# cognos-feed-fixtures

Static feeds that CognOS live tests poll. A test changes a feed by committing to this repository, so it can
observe an edited or removed item on demand. A real publisher cannot produce that.

**No personal data.** Every title, link and description here is invented. Item links point at
`example.org` (reserved for documentation, RFC 2606) and are never fetched; only the feed files are.

## Feeds

Served by GitHub Pages from the `main` branch:

| Feed | URL | Used by |
|---|---|---|
| Newswire (RSS 2.0) | `https://critical-insight-ai.github.io/cognos-feed-fixtures/newswire/rss.xml` | The newswire projection live proof (CognOS `add-declarative-event-projections`, task 6.2) |

## How a live test edits a feed

1. Commit a change to the feed file: edit an item's title or description, add an item, or remove one.
2. Wait for the Pages deployment to finish (about a minute), then for the CDN copy to expire (`Cache-Control: max-age=600`).
3. Poll the feed from CognOS and assert on what changed.
4. Afterwards, restore the feed's baseline (the file as tagged `baseline`), so the next test starts from the same document.

Keep each item's `guid` stable across edits: the poller recognises an edited item by its guid, and a new guid
is a new item.

## Who may change it

The CognOS team. A CognOS tenant that polls these feeds needs an egress grant for
`critical-insight-ai.github.io` only, never a wildcard.