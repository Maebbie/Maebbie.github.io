
<pre><code id="txt-content">[Dev Project Version](https://github.com/BasisVR/Basis/blob/long-term-support-20251102/Basis/ProjectSettings/ProjectVersion.txt)</code></pre>

<script>
fetch("https://raw.githubusercontent.com/BasisVR/Basis/refs/heads/developer/Basis/ProjectSettings/ProjectVersion.txt")
  .then(r => { if (!r.ok) throw new Error(r.status); return r.text(); })
  .then(text => document.getElementById("txt-content").textContent = text)
  .catch(() => document.getElementById("txt-content").textContent = "[Dev Project Version](https://github.com/BasisVR/Basis/blob/long-term-support-20251102/Basis/ProjectSettings/ProjectVersion.txt)");
</script>
