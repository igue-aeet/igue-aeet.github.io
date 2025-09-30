---
layout: single
title: "Miembros"
permalink: /miembros/
author_profile: false
toc: false
---

<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" integrity="sha256-p4NxAoJBhIIN+hmNHrzRCf9tD/miZyoHS5obTRR9BMY=" crossorigin=""/>
<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js" integrity="sha256-20nQCchB9co0qIjJZRGuk2/Z9VM+kNiyxNV1lvTlZBo=" crossorigin=""></script>

<style>
#members-map { height: 420px; margin-bottom: 1.5rem; }
.members-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(160px, 1fr)); gap: 1rem; }
.member-card { text-align: center; }
.member-card img { width: 120px; height: 120px; object-fit: cover; border-radius: 50%; display: block; margin: 0 auto 0.5rem; }
</style>

<div id="members-map"></div>

<script>
document.addEventListener('DOMContentLoaded', function () {
  var map = L.map('members-map');
  var basemap = L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
    maxZoom: 18,
    attribution: '&copy; OpenStreetMap contributors'
  }).addTo(map);

  var markers = L.layerGroup().addTo(map);

  var members = [
    // Example seed entries; replace/extend with real members
    { name: 'Juan Antonio Hernández Agüero', lat: 52.354, lon: 4.955, org: 'Vrije Universiteit Amsterdam' },
    { name: 'Joan Casanelles Abella', lat: 48.402, lon: 11.722, org: 'TUM' },
    { name: 'Jéssica Jiménez Peñuela', lat: 37.188, lon: -3.606, org: 'Universidad de Granada' }
  ];

  var bounds = [];
  members.forEach(function (m) {
    var marker = L.marker([m.lat, m.lon]).bindPopup('<b>' + m.name + '</b><br/>' + (m.org || ''));
    markers.addLayer(marker);
    bounds.push([m.lat, m.lon]);
  });

  if (bounds.length > 0) {
    map.fitBounds(bounds, { padding: [20, 20] });
  } else {
    map.setView([40.0, -3.7], 5);
  }
});
</script>

<div class="members-grid">
  <div class="member-card">
    <img src="/images/profile.png" alt="Juan Antonio Hernández Agüero"/>
    <div>Juan Antonio Hernández Agüero</div>
  </div>
  <div class="member-card">
    <img src="/images/profile.png" alt="Joan Casanelles Abella"/>
    <div>Joan Casanelles Abella</div>
  </div>
  <div class="member-card">
    <img src="/images/profile.png" alt="Jéssica Jiménez Peñuela"/>
    <div>Jéssica Jiménez Peñuela</div>
  </div>
</div>

<p style="margin-top:1rem;">
Para unirte al directorio, completa el formulario: <a href="https://forms.gle/ESYtu19jNd2Gmibk7" target="_blank" rel="noopener">Formulario de alta</a>.
</p>


