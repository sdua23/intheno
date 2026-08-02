# In The N.O. — Chart Hosting & Publishing Guide

How interactive charts and email images work on this site. All assets live in this repo and are
served by GitHub Pages at `https://sdua23.github.io/intheno/<filename>`.

## The system in one paragraph
Each chart exists twice: an **interactive HTML page** (`chart_*.html`) shown on the website via an
iframe, and a **static PNG** (`email_*.png`) shown in the email newsletter (email clients strip
scripts and SVG, so interactive charts cannot render there). Ghost's per-card Visibility toggles
route each version to the right audience.

## Adding a chart to an article
1. **Upload** the `chart_*.html` and `email_*.png` files to this repo (Add file → Upload files →
   Commit). The Pages deploy runs automatically (~1 min; check the Actions tab).
2. **Listener card** — once per article, paste this in its own HTML card:
   ```html
   <script>
   window.addEventListener("message",function(e){
    if(!e.data||e.data.sc!=="chart-resize")return;
    var fr=document.querySelectorAll("iframe.sc-chart");
    for(var i=0;i<fr.length;i++)if(fr[i].contentWindow===e.source)fr[i].style.height=e.data.h+"px";
   });
   </script>
   ```
3. **Web chart** — HTML card per chart, Visibility: Web ON / **Email OFF**:
   ```html
   <iframe class="sc-chart" src="https://sdua23.github.io/intheno/chart_NAME.html?v=1"
    style="width:100%;height:640px;border:0;display:block" scrolling="no" loading="lazy"></iframe>
   ```
4. **Email image** — HTML card per chart, Visibility: **Web OFF** / Email ON:
   ```html
   <img src="https://sdua23.github.io/intheno/email_NAME.png" alt="Chart title" style="width:100%">
   ```
   (Must be an HTML card, not a native image card — only HTML cards expose Visibility toggles.)

## Updating an existing chart
1. Upload the new file **at the same filename** and commit — every article referencing it updates.
2. **Bump the version** on its iframe (`?v=1` → `?v=2`) in each article that uses it. This defeats
   browser/CDN caching; without it the old chart can linger for hours.
3. Email PNGs: already-sent newsletters load images at open time, so overwriting a PNG silently
   updates old emails too. If a sent email must stay frozen, use a new filename instead
   (`email_NAME_v2.png`).

## Requirements for new chart HTML files
- Full standalone page (`<!DOCTYPE html>` … ), transparent background, fonts loaded via Google
  Fonts link (Cardo + Inter) — iframes do not inherit the site's fonts.
- Must include the **resize reporter** before `</body>` (measures true content height and reports
  it to the parent; do not use `scrollHeight`, which can never shrink below the iframe height):
  ```html
  <script>
  (function(){function h(){var el=document.body.firstElementChild;
   return el?Math.ceil(el.getBoundingClientRect().bottom+(parseFloat(getComputedStyle(el).marginBottom)||0))+8:document.body.scrollHeight}
  function send(){parent.postMessage({sc:"chart-resize",h:h()},"*")}
  window.addEventListener("load",send);window.addEventListener("resize",send);
  if(document.fonts&&document.fonts.ready)document.fonts.ready.then(send);
  var n=0,t=setInterval(function(){send();if(++n>8)clearInterval(t)},500);
  })();
  </script>
  ```
- Keep all CSS scoped/inlined in the file; no external stylesheets besides fonts.
- Filenames: lowercase, no spaces (URLs are case-sensitive).

## Why it's built this way
- **Iframes instead of pasted HTML cards**: Ghost counts every code token in HTML cards toward the
  read-time estimate; a pasted chart adds 5–15 fake minutes. An iframe is ~10 words.
- **GitHub Pages instead of Ghost file uploads**: Ghost(Pro) serves uploaded .html as plain text
  (you'd see source code in the iframe).
- **PNG fallbacks**: email clients strip `<script>`/`<svg>`; without a fallback, charts vanish
  from newsletters. PNGs are exported from the real chart pages so web and email always match.

## Troubleshooting
- **Chart shows source code** → file is not on GitHub Pages (check the URL) or Pages not deployed.
- **Big whitespace under a chart** → stale cached page in the iframe; bump `?v=`.
- **Chart missing in email** → the HTML iframe card was left Email-ON with no PNG card, or the PNG
  card's Visibility is wrong.
- **Article content escapes the column after a card** → unbalanced `</div>` in a pasted card;
  count opens vs closes.
- **Wrong fonts inside a chart** → the chart page is missing its Google Fonts link.
