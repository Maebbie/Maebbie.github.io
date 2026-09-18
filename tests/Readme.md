


<pre><code id="txt-content">Refer to https://github.com/BasisVR/Basis/blob/long-term-support-20251102/Basis/ProjectSettings/ProjectVersion.txt</code></pre>

<script>
fetch("https://raw.githubusercontent.com/BasisVR/Basis/refs/heads/long-term-support-20251102/Basis/ProjectSettings/ProjectVersion.txt")
  .then(r => { if (!r.ok) throw new Error(r.status); return r.text(); })
  .then(text => document.getElementById("txt-content").textContent = text)
  .catch(() => document.getElementById("txt-content").textContent = "Could not load file.");
</script>





<iframe src="https://raw.githubusercontent.com/BasisVR/Basis/refs/heads/long-term-support-20251102/Basis/ProjectSettings/ProjectVersion.txt"
        width="100%" height="600"></iframe>
