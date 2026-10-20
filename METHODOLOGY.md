# The Compute Index: methodology

How every number in The Compute Index is collected and calculated. The full price history behind
the index is published next to this page as [`data/index_history.csv`](data/index_history.csv), so
anyone can check or rebuild it.

## What we track

Six flagship data-center and workstation GPUs: **H100 SXM, H200, B200 HGX, A100 PCIE, L40S and
RTX 4090.**

For each GPU we record the **on-demand hourly rental price per GPU in US dollars** listed by
cloud and marketplace providers. On-demand means pay-as-you-go list prices. Spot, interruptible,
reserved and negotiated contract prices are not included.

## Source

Prices come from the public GPU pricing board at [mlq.ai/gpu-pricing](https://mlq.ai/gpu-pricing/),
which lists the cheapest provider for each GPU model and the prices from other providers. We
collect a snapshot of the board at least once a week, usually on Sunday (US Eastern time), and keep
every snapshot.

The hyperscaler comparison ("vs hyperscaler") uses a public roundup of on-demand prices from 28
clouds. It is updated by hand when the roundup refreshes, and each issue shows its as-of month.

## Figures for each GPU

| Figure | Meaning |
|---|---|
| **Cheapest on-demand (floor)** | The lowest hourly price listed for that GPU in the snapshot, and the provider listing it |
| **Median listing** | The middle price of all listings for that GPU, including the floor. Shows what a typical provider charges |
| **Ceiling** | The highest listing |
| **Listings** | How many provider prices the board showed for that GPU |
| **Change** | Percent change in the floor since an earlier snapshot, with that snapshot's date ("+2.0% vs Oct 4") |

We only show a weekly change when we have a snapshot from 6 to 10 days earlier, and a monthly
change when we have one from 27 to 35 days earlier. If there isn't one, the change is left out
rather than compared with a snapshot from a different interval. We calculate changes from our own
snapshots; we don't quote the board's own change column.

## Likely data errors

A floor price below **15% of the median listing**, when a GPU has at least 4 listings, is flagged as
a likely data error (for example, an H200 listed at $0.01/hour). A flagged floor is still shown in
the table, marked as excluded, but it is not used for changes or for the index.

## The Compute Index (one number)

The Compute Index summarizes rental price movement across the six GPUs in a single number.

- **Base:** the first snapshot, October 1, 2026, equals 100.
- **Each step:** from one snapshot to the next, the index moves by the **average percent change in
  the floor price** of the GPUs that have a valid floor in both snapshots. Each GPU counts equally.
- **Chain-linked:** the steps are multiplied together. A GPU that is missing or flagged in a
  snapshot sits out that step, so a data error or a newly listed GPU can't move the index on its own.

An index above 100 means renting the cheapest available capacity costs more than at launch; below
100, it costs less. Example: if three GPUs' floors are unchanged and the other three rise 10%, 20%
and 30%, the index rises 10% (the average of 0, 0, 0, 10, 20 and 30).

## Corrections and revisions

We don't silently rewrite history. If we find an error in a past snapshot, we flag it in the CSV's
`anomaly` column and recalculate; the issue that next mentions the index says what changed. Changes
to this methodology are dated below.

## Limitations

- One public aggregator is the source for rental prices, so a provider missing from the board is
  missing from the index.
- Listed prices are not always available to buy at that moment, and minimum terms or regions can
  differ between providers.
- Floors move when one provider changes a single listing; the median is the steadier figure.

## History of this page

- 2026-10-18: First published. Chain-linked index, anomaly rule, dated changes, CSV history.
