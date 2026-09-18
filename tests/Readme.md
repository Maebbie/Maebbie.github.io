<p id="txt1"><a href="https://github.com/BasisVR/Basis/blob/long-term-support-20251102/Basis/ProjectSettings/ProjectVersion.txt">Dev Project Version</a></p>

<script>
(function () {
  const status = document.getElementById("txt1");

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
      status.querySelector("a").textContent = "Dev Project Version";
    });
})();
</script>

<p id="txt2"><a href="https://raw.githubusercontent.com/BasisVR/Basis/refs/heads/long-term-support-20251102/Basis/ProjectSettings/ProjectVersion.txt">LTS Project Version</a></p>

<script>
(function () {
  const status = document.getElementById("txt2");

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
      status.querySelector("a").textContent = "LTS Project Version";
    });
})();
</script>
