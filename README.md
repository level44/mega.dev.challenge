# mega.dev.challenge

## S01E03 · what it shows

The original 2026-10-04 knowledge-base export is preserved in [`support-kb/`](support-kb/).
Accepted historical decisions have not been rewritten.

- [Signed incident review](support-kb/src/routes/kb/incidents/4471-review/+page.svx)
- [Rendered review, standalone HTML](support-kb/rendered/4471-review.html) — download and open locally; includes styles, report data and interactive examples without external dependencies.
- [Computed coverage component](support-kb/src/lib/kb/IncidentReview.svelte)
- [Shared parser issue: 379 unique reports](https://github.com/level44/mega.dev.challenge/issues/1)
- [booking-v3 hint issue: 33 unique reports](https://github.com/level44/mega.dev.challenge/issues/2)

The export contains 427 rows but 412 unique report IDs. Coverage uses the exported
`uniqueById`, `groupByCause`, tenant directory and locale rules, not intake tags.
Five one-day-like reports remain counted under parser by the exported model, with
their individual mechanism explicitly unresolved. All counts describe the export,
not today's live queue.

The course accepted both tickets, then disclosed its engineering update: upstream
#4471 closed 5 October; parser **fixed**, hint **won't fix** because booking-v3 was
retired. Accordingly, issue #1 is closed as completed and #2 as not planned.
Both the signed source and rendered page record these distinct outcomes.

The new page follows the existing `.svx` conventions, imports its computed numbers
from a Svelte component, is automatically discovered by `KbIndex`, and renders its
frontmatter signature through `RenderStamp`. GitHub's source view does not execute
those components.

### Verification performed

Rendered the original incident, triage explorer, KB index and signed review using
Svelte 5, mdsvex and Vite in a temporary local harness. Observed the two cause
groups, exercised the same-tenant form/app counterexample, expanded the parser
report-ID audit, followed the index link to the review and verified its rendered
desk reference. The new review rendered without browser errors. The standalone
HTML was also served independently: both explorer verdicts worked, the rendered
desk reference matched, and it loaded no external scripts or stylesheets.

No production application fix or deployment is claimed. The mounted course MCP
accepted this submission on 2026-10-29 and confirmed that its debrief was saved:
https://hq.mega.dev/challenges/what-it-shows#debrief

The separately signed-in course browser still displayed "Open · not solved yet"
after that confirmation. The MCP/browser account or state discrepancy remains
unresolved; the MCP acceptance must not be mistaken for verified browser progress.
