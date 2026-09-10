# Little Snitch telemetry rule groups

## Setup (two layers, use both)

**Layer 1 - auto-updating, does the heavy lifting.** Open Little Snitch Configuration → Rules window → click "Add Blocklists…" in the sidebar (or the "+" next to the Blocklists section header). Filter by "Tracking" category, add one or two lists. Little Snitch re-downloads these on a schedule from the publisher, so this is what actually keeps pace with Apple and Google shuffling hostnames — a static list, including this one, cannot.

**Layer 2 - hand-picked, gives you certainty on specific vendors.** For each `.lsrules` file here: File → Import Rules… → select the file. Each imports as its own named rule group so you can toggle vendors independently instead of one undifferentiated blob. If you want auto-updates on these too, host them somewhere (a gist's raw URL works) and use File → New Rule Group Subscription instead of Import — same JSON, Little Snitch just re-fetches it on an interval you set.

## Do not add these to a deny list

These look like telemetry because of the `googleapis.com`/`apple.com` pattern-matching instinct, but blocking them breaks real functionality, not just data collection:

| Domain | Breaks |
|---|---|
| `mtalk.google.com` | Firebase Cloud Messaging - push notifications for every app using it, not just Google's |
| `android.clients.google.com` | Play Store / Play Services updates |
| `update.googleapis.com` | Chrome auto-update |
| `clientservices.googleapis.com` | Device management/sync |
| `safebrowsing.googleapis.com` | Chrome/Safari malicious-site warnings - arguably worth keeping regardless |
| `gs.apple.com`, `appleid.apple.com` | Apple ID sign-in |
| `gsp-ssl.ls.apple.com`, `gsp64-ssl.ls.apple.com` | Find My network |
| `mesu.apple.com`, `gdmf.apple.com` | macOS/iOS software updates |
| `guzzoni.apple.com`, `api-glb-*.smoot.apple.com` | Siri |

## Honesty about coverage

The four vendor files here are deliberately conservative — every domain in them has years of cross-referenced documentation behind it. What they are *not* is exhaustive: Apple alone has been documented (by projects like cedws/apple-telemetry, now archived and unmaintained) as contacting hundreds of loosely-named hostnames with no consistent scheme. Hand-typing that full surface would mean either guessing at hostnames I can't verify are still live, or copying a stale list wholesale — neither is actually more useful to you than Layer 1's auto-updating picker. Use these files for the vendors you specifically care about; lean on the blocklist subscription for everything else.
