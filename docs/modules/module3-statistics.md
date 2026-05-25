---
title: "Module 3 — Core statistical methods for environmental data"
hide:
  - toc
---

<style>
  /* The page uses the standalone HTML in an iframe as the header — hide the auto-generated H1 */
  .md-content__inner > h1:first-of-type { display: none; }
</style>

<p style="margin: 1.2rem 0 0.6rem 0;">
  <a href="../../assets/module3-statistics.html" target="_blank" rel="noopener" style="display:inline-block;padding:10px 18px;border-radius:8px;background:#06151a;color:#5fc9bb;font-weight:600;text-decoration:none;border:1px solid #1c3e48;">
    Open the module in a new tab ↗
  </a>
  <span style="margin-left:0.6rem;color:var(--md-default-fg-color--light);font-size:0.9rem;">recommended for the full experience (full-width, custom fonts).</span>
</p>

<iframe
  id="m3-iframe"
  src="../../assets/module3-statistics.html"
  title="Module 3 — Statistical methods (interactive version)"
  scrolling="no"
  style="width:100%; height:800px; border:1px solid var(--md-default-fg-color--lightest); border-radius:10px; background:#06151a; margin-top:0.5rem; display:block;">
</iframe>

<script>
(function() {
  var f = document.getElementById('m3-iframe');
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
