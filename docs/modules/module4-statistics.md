# Modul 4 — Metode statistice de bază pentru date de mediu

**Vineri 22 mai · 15:15-16:00**

<p style="margin: 1.2rem 0 0.6rem 0;">
  <a href="../../assets/module4-statistics.html" target="_blank" rel="noopener" style="display:inline-block;padding:10px 18px;border-radius:8px;background:#06151a;color:#5fc9bb;font-weight:600;text-decoration:none;border:1px solid #1c3e48;">
    Deschide modulul în filă nouă ↗
  </a>
  <span style="margin-left:0.6rem;color:var(--md-default-fg-color--light);font-size:0.9rem;">recomandat pentru experiența completă (full-width, fonturi custom).</span>
</p>

<iframe
  id="m4-iframe"
  src="../../assets/module4-statistics.html"
  title="Modul 4 — Metode statistice (versiune interactivă)"
  scrolling="no"
  style="width:100%; height:800px; border:1px solid var(--md-default-fg-color--lightest); border-radius:10px; background:#06151a; margin-top:0.5rem; display:block;">
</iframe>

<script>
(function() {
  var f = document.getElementById('m4-iframe');
  if (!f) return;
  function resize() {
    try {
      var doc = f.contentDocument || f.contentWindow.document;
      var h = Math.max(
        doc.body.scrollHeight,
        doc.documentElement.scrollHeight,
        doc.body.offsetHeight,
        doc.documentElement.offsetHeight
      );
      f.style.height = (h + 20) + 'px';
    } catch (e) {}
  }
  f.addEventListener('load', function() {
    resize();
    setTimeout(resize, 200);
    setTimeout(resize, 800);
  });
  window.addEventListener('resize', resize);
})();
</script>
