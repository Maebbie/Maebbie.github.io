<script src="https://cdn.jsdelivr.net/npm/marked/marked.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/dompurify@3/dist/purify.min.js"></script>

<article id="content">Loading…</article>

<script>
async function loadFile() {
  const res = await fetch('https://github.com/BasisVR/Basis/blob/long-term-support-20251102/Basis/ProjectSettings/ProjectVersion.txt');
  if (!res.ok) throw new Error(res.status);
  document.getElementById('content').innerHTML =
    DOMPurify.sanitize(marked.parse(await res.text()));
}
loadFile();
setInterval(loadFile, 5 * 60 * 1000); // optional auto-refresh
</script>
