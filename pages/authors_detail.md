---
layout: pagedefault
title: Profile
permalink: /stakeholder/
---
<head>
    <link rel="stylesheet" type="text/css" href="{{site.baseurl}}/assets/css/includes-style/browse-author-style.css">
    </head>

<div id="detail-root" class="detail-root container-fluid">
  <p>Loading…</p>
</div>

<!-- same data source -->
<script id="DATA" type="application/json">
  {{ site.data["biography"] | jsonify }}
</script>



<script>
(function(){
  const root = document.getElementById("detail-root");
  const safe = (x) => (x == null ? "" : String(x));
  const normalizeHeader = (h) => String(h||"").trim().toLowerCase()
    .replace(/&/g,"and").replace(/[@]/g,"at")
    .replace(/\s+/g,"_").replace(/[^\w]+/g,"_")
    .replace(/_+/g,"_").replace(/^_|_$/g,"");
  const slugify = (s) => String(s||"").trim().toLowerCase()
    .normalize("NFKD").replace(/[\u0300-\u036f]/g,"")
    .replace(/[^a-z0-9]+/g,"-").replace(/^-+|-+$/g,"");

  function getParam(name){
    const url = new URL(window.location.href);
    return url.searchParams.get(name);
  }

  function readData() {
    try { return JSON.parse(document.getElementById("DATA").textContent) || []; }
    catch(e){ console.error(e); return []; }
  }

  // Prepare data
  const RAW = readData();
  if (!RAW.length) { root.innerHTML = "<p>No data available.</p>"; return; }

  const HEADERS = Object.keys(RAW[0]).map(orig => {
    const label = String(orig||"").trim() || "(Unnamed column)";
    return { original: orig, key: normalizeHeader(label), label };
  });

  const DATA = RAW.map(row => {
    const out = {};
    for (const h of HEADERS) out[h.key] = safe(row[h.original]);
    out.slug = slugify(out.name  || out.title || out.name_ || out["name_"] );
    return out;
  });

  // locate by slug
  const id = getParam("id");
  if (!id) { root.innerHTML = "<p>Missing id.</p>"; return; }
  const item = DATA.find(r => r.slug === id);
  if (!item) { root.innerHTML = "<p>Not  found.</p>"; return; }

  // fields (use your actual column set; these match the earlier script)
  const name       = item.name || "(No Name)";
  const country    = item.country;
  const typeCat    = item.type_category;
  const lifespan   = item.lifespan;
  const genreType  = item.genre_type;
  const wiki       = item.wikipedia_url_if_applicable;
  const imageUrl   = item.image_url || item.image || "{{site.baseurl}}/authorphotos/" + item.authorid + ".jpg" ||"{{site.baseurl}}/assets/img/unknown_person.jpg";
  const dlocUrl    = item.dloc_items_url;
  const awards     = item.awards;
  const mediaAbout = item.media_about_them;
  const website    = item.website || item.site || ""; // just in case






//DATA

  console.log(item);
  // render detail view
  root.innerHTML = `<p>Hello World</p>`;
});
</script>

<style>
.detail-root { margin-top: 1rem; }
.detail-wrap { 
  display:grid; grid-template-columns: 1fr 2fr; gap: 1.25rem; 
  align-items:start;
}
@media (max-width: 900px) {
  .detail-wrap { grid-template-columns: 1fr; }
}
.detail-media { background:#f3f3f3; border:1px solid #ddd; border-radius:10px; overflow:hidden; }
.detail-media img { width:100%; height:auto; display:block; }
.detail-placeholder { width:100%; aspect-ratio: 4 / 3; background:#e5e5e5; }

.detail-info { 
  border:1px solid #ddd; border-radius:10px; padding:1rem; background:#fff;
}
.detail-title { margin:0 0 .25rem 0; }
.pill { display:inline-block; padding:.2rem .5rem; border-radius:999px; font-size:.9rem; background:#f6f6f6; border:1px solid #eee; }

.detail-grid { display:grid; grid-template-columns: repeat(auto-fit, minmax(220px, 1fr)); gap:.5rem .75rem; margin:.75rem 0; }
.detail-links { display:flex; flex-wrap:wrap; gap:.5rem .75rem; margin: .5rem 0 1rem; }
.detail-links a { text-decoration:none; border:1px solid #ddd; border-radius:8px; padding:.35rem .6rem; background:#fafafa; }
.detail-links a:hover { border-color:#c5c5c5; }

.backlink a { text-decoration:none; }
</style>
