---
layout: single
title: "Buscar"
permalink: /search/
author_profile: false
toc: false
---

<input type="search" id="search-input" placeholder="Escribe para buscar" style="width:100%;max-width:560px;height:2.25rem;padding:0 0.75rem;border:1px solid #ddd;border-radius:6px;">

<div id="search-results" style="margin-top:1rem;"></div>

<script>
(function() {
  function escapeHtml(s) { return s.replace(/[&<>"']/g, function(c){ return ({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;','\'':'&#39;'}[c]); }); }

  function buildIndex() {
    var pages = [];
    {% assign pages = site.pages | where_exp: "p", "p.search_exclude != true" %}
    {% for p in pages %}
      pages.push({
        title: {{ p.title | jsonify }},
        url: {{ p.url | absolute_url | jsonify }},
        content: {{ p.content | strip_html | normalize_whitespace | jsonify }}
      });
    {% endfor %}
    {% for coll in site.collections %}
      {% for doc in coll.docs %}
        pages.push({
          title: {{ doc.title | jsonify }},
          url: {{ doc.url | absolute_url | jsonify }},
          content: {{ doc.content | strip_html | normalize_whitespace | jsonify }}
        });
      {% endfor %}
    {% endfor %}
    return pages;
  }

  var INDEX = buildIndex();
  var input = document.getElementById('search-input');
  var results = document.getElementById('search-results');

  function search(q) {
    q = q.trim().toLowerCase();
    if (!q) { results.innerHTML = ''; return; }
    var tokens = q.split(/\s+/).filter(Boolean);
    var matches = INDEX.map(function(page){
      var text = (page.title + ' ' + page.content).toLowerCase();
      var score = 0;
      tokens.forEach(function(t){ if (text.indexOf(t) !== -1) score++; });
      return { page: page, score: score };
    }).filter(function(r){ return r.score > 0; }).sort(function(a,b){ return b.score - a.score; }).slice(0, 30);

    results.innerHTML = matches.map(function(r){
      return '<div><a href="' + r.page.url + '"><strong>' + escapeHtml(r.page.title || r.page.url) + '</strong></a></div>';
    }).join('');
  }

  var params = new URLSearchParams(window.location.search);
  var q = params.get('q') || '';
  input.value = q;
  input.addEventListener('input', function(){ search(this.value); });
  if (q) search(q);
})();
</script>


