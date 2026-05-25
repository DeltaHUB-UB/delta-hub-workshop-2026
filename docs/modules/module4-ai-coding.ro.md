---
title: "Modul 4 — AI-assisted coding cu Python (hands-on)"
hide:
  - toc
---

<style>
  /* Pagina folosește HTML-ul standalone din iframe ca header — ascundem H1-ul auto-generat de Material */
  .md-content__inner > h1:first-of-type { display: none; }
</style>

<p style="margin: 1.2rem 0 0.6rem 0;">
  <a href="../../assets/modul4-Ai_assisted.ro.html" target="_blank" rel="noopener" style="display:inline-block;padding:10px 18px;border-radius:8px;background:#0c1719;color:#6FA68F;font-weight:600;text-decoration:none;border:1px solid #2a3a3e;">
    Deschide modulul în filă nouă ↗
  </a>
  <span style="margin-left:0.6rem;color:var(--md-default-fg-color--light);font-size:0.9rem;">recomandat pentru experiența completă (full-width, fonturi custom).</span>
</p>

<iframe
  id="m4-iframe"
  src="../../assets/modul4-Ai_assisted.ro.html"
  title="Modul 4 — AI-assisted coding cu Python (versiune interactivă)"
  scrolling="no"
  style="width:100%; height:800px; border:1px solid var(--md-default-fg-color--lightest); border-radius:10px; background:#0c1719; margin-top:0.5rem; display:block;">
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


