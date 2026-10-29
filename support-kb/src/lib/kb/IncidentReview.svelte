<script lang="ts">
  import reports from '#lib/data/reports-4471.json';
  import notes from '#lib/data/desk-notes.json';
  import { groupByCause, uniqueById, type Cause, type Report } from '#lib/kb/causes';
  import { tenantOf } from '#lib/kb/locales';

  const unique = uniqueById(reports as Report[]);
  const merged = unique.filter((r) => r.mergedInto === 4471);
  const groups = groupByCause(merged, tenantOf);
  const tickets: Record<Cause, { url: string; label: string }> = {
    parser: { url: 'https://github.com/level44/mega.dev.challenge/issues/1', label: 'Shared month-first parser' },
    hint: { url: 'https://github.com/level44/mega.dev.challenge/issues/2', label: 'booking-v3 date hint' },
  };
  const flagged = merged.filter((r) => notes.oneDayMove.includes(r.id));
  const duringMergeWeek = (iso: string | null) => iso !== null && iso.slice(0, 10) >= '2026-06-29' && iso.slice(0, 10) < '2026-07-06';
  const received = merged.filter((r) => duringMergeWeek(r.receivedAt)).length;
  const filed = merged.filter((r) => duringMergeWeek(r.mergedAt)).length;
</script>

<p><strong>{merged.length}</strong> unique reports in #{4471}, across <strong>{groups.length}</strong> classified causes.
  The export has {reports.length} rows; {reports.length - unique.length} overlapping rows are not additional reports.
  Counts describe the export dated 2026-10-04, not today's live support queue.</p>

<table>
  <thead><tr><th>Cause and separate ticket</th><th>Reports</th><th>Tenants</th><th>Received</th><th>Intake tags, not causes</th></tr></thead>
  <tbody>
    {#each groups as group (group.cause)}
      <tr>
        <td>{#if group.cause !== 'unplaced'}<a href={tickets[group.cause].url}>{tickets[group.cause].label}</a>{:else}Unplaced: investigate{/if}</td>
        <td>{group.reports.length}</td>
        <td>{group.tenants}</td>
        <td>{group.firstSeen} → {group.lastSeen}</td>
        <td>{Object.entries(group.taggedAs).map(([tag, count]) => `${tag}: ${count}`).join(' · ')}</td>
      </tr>
    {/each}
  </tbody>
</table>

<h3>Coverage audit</h3>
<p>Deduplicate by report ID before filtering to #{4471}; resolve the tenant from its directory entry,
  then run <code>groupByCause</code>. Browser locale and <code>meta.intakeTag</code> are intake guesses.</p>
{#each groups as group (group.cause)}
  <details>
    <summary>{group.cause}: {group.reports.length} report IDs</summary>
    <p>{group.reports.map((r) => r.id).join(', ')}</p>
  </details>
{/each}

<h3>Unresolved individual symptoms</h3>
<p>{flagged.length} reports are flagged in <code>desk-notes.json</code> as reading like a one-day move.
  The exported classifier assigns them to parser using tenant and channel; that is an accounting assignment,
  not proof that every described symptom is caused by month/day parsing. Keep them in that total while
  investigating timestamps, time zones and the original inputs. The export does not establish a separate third cause.</p>
<ul>{#each flagged as report (report.id)}<li><code>{report.id}</code>: {report.body}</li>{/each}</ul>

<h3>Receipt time is not merge time</h3>
<p>During the week beginning 2026-06-29, the desk received {received} reports but merged {filed} into #{4471}.
  The merge spike includes backlog; it is not evidence of a matching spike in newly reported failures.
  Reports continue through {groups.map((g) => g.lastSeen).sort().at(-1)}. Neither a queued fix nor a
  declining merge bar demonstrates recovery.</p>
