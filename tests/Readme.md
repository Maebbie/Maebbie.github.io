<iframe src="https://github.com/BasisVR/Basis/blob/long-term-support-20251102/Basis/ProjectSettings/ProjectVersion.txt"
        width="100%" height="600"></iframe>


<!DOCTYPE html>
<html>
<body>
  <h1>Live notes</h1>
  <pre id="content">Loading…</pre>

<script>
async function loadFile() {
  const res = await fetch('https://github.com/BasisVR/Basis/blob/long-term-support-20251102/Basis/ProjectSettings/ProjectVersion.txt');
  if (!res.ok) throw new Error(res.status);
  document.getElementById('content').textContent = await res.text();
}
loadFile();
setInterval(loadFile, 5 * 60 * 1000); // optional: auto-refresh every 5 min
</script>
</body>
</html>
