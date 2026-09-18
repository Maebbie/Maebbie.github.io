
<pre><code id="txt-content">Loading file…</code></pre>

<script>
fetch("https://raw.githubusercontent.com/BasisVR/Basis/refs/heads/developer/Basis/ProjectSettings/ProjectVersion.txt")
  .then(r => { if (!r.ok) throw new Error(r.status); return r.text(); })
  .then(text => document.getElementById("txt-content").textContent = text)
  .catch(() => document.getElementById("txt-content").textContent = "Could not load file.");
</script>
