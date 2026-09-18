<p id="txt-status">[Dev Project Version](https://github.com/BasisVR/Basis/blob/long-term-support-20251102/Basis/ProjectSettings/ProjectVersion.txt)</p>

<script>
(function () {
  const status = document.getElementById("txt-status");

  fetch("https://raw.githubusercontent.com/BasisVR/Basis/refs/heads/developer/Basis/ProjectSettings/ProjectVersion.txt")
    .then(r => { if (!r.ok) throw new Error(r.status); return r.text(); })
    .then(text => {
      const pre = document.createElement("pre");
      const code = document.createElement("code");
      code.textContent = text;
      pre.appendChild(code);
      status.replaceWith(pre);
    })
    .catch(() => {
      status.textContent = "[Dev Project Version](https://github.com/BasisVR/Basis/blob/long-term-support-20251102/Basis/ProjectSettings/ProjectVersion.txt)";
    });
})();
</script>
