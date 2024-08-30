---
layout: page
title: Map
permalink: /map
---

<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" integrity="sha256-p4NxAoJBhIIN+hmNHrzRCf9tD/miZyoHS5obTRR9BMY=" crossorigin="" />
<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js" integrity="sha256-20nQCchB9co0qIjJZRGuk2/Z9VM+kNiyxNV1lvTlZBo=" crossorigin=""></script>

<link rel="stylesheet" href="https://unpkg.com/leaflet.markercluster@1.5.3/dist/MarkerCluster.css" />
<link rel="stylesheet" href="https://unpkg.com/leaflet.markercluster@1.5.3/dist/MarkerCluster.Default.css" />
<script src="https://unpkg.com/leaflet.markercluster@1.5.3/dist/leaflet.markercluster.js"></script>

<div id="map"></div>

<div id="display_hidden_markers"></div>

<script type="text/javascript">
	var map = L.map('map', { attributionControl: false }).setView([42.0587815,-73.9207478], 12);
	
	L.tileLayer('https://{s}.basemaps.cartocdn.com/rastertiles/voyager_labels_under/{z}/{x}/{y}{r}.png', {
		attribution: '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors &copy; <a href="https://carto.com/attributions">CARTO</a>',
		maxZoom: 20
	}).addTo(map);

	L.control.attribution({
	  position: 'bottomleft'
	}).addTo(map);

	var birdSvg = `<?xml version="1.0" encoding="UTF-8"?><svg enable-background="new 0 0 164 223.2" version="1.1" viewBox="0 0 164 223.2" xml:space="preserve" xmlns="http://www.w3.org/2000/svg"><path fill="#55892e" d="m81.9 1.5c44.2-0.6 82.9 39 80.4 83.3-2.2 34.8-21.9 65.5-42 92.9-12 14.6-24.7 33.2-39.3 43.5-17.2-17.4-33-36.3-47-56.4-18.1-27.4-36.3-59.5-31.8-93.5 4.5-39.2 40.5-70 79.7-69.8z"/><path fill="#ffffff" d="m37.3 146.4c-1.4 0.8-2.1 0.5-2.1 0.5s-1.9-0.7-1.5-1.5c0.7-1.6 15.6-11.4 19.6-15.3s3.3-5.4 7.5-13.8c1.1-2.3 2.5-4.3 3.7-6.5 1.7-3.1 2.6-4 6.9-2.4 1.9 0.6 3.7-0.9 4.5-2.6-8.6-1.3-14-7.1-19.4-13.1-12.7-14.3-33.8-32.3-33.8-32.3s44 1.8 62.8 18.4c2.6 2.3 4.7 5.6 6.8 8.4 3.4-2.1-0.6-6.1-2.4-9.1 2.9-5.9 5-10.7 8.2-16.2 3-5.4 8.2-7.4 14-6.5 3.7 0.6 7.2 2.5 10.9 3.7 1.7 0.6 3.5 1.2 5.3 1.1 10.5-0.5 22 0.9 24.2 1.6 2.1 0.5 2.4 1.1 0.9 1.1s-8.8-0.7-22.3 2.7c-8.8 2.3-8.5 3-10.4 9.1-2 6.2-4 12.5-5.5 18.8-4.8 21-18.3 32.8-38.6 36.5-12.4 2.2-13.8 0.6-21.5 7.3-6.7 7.1-7.9 8.8-9.5 9.7-1 0.6-2.2 0-2.7-0.3-0.3-0.2-1.7 0.8-2 1-0.9 0.4-3.5 0.5-3.6-0.3z"/></svg>`;

	var bird = L.divIcon({
		className: 'marker marker-wildlife',
		html: birdSvg,
		iconSize: [26, 26],
		iconAnchor: [13, 36],
		popupAnchor: [0, -36]
	});

	var museumSvg = `<?xml version="1.0" encoding="UTF-8"?><svg enable-background="new 0 0 164 223.2" version="1.1" viewBox="0 0 164 223.2" xml:space="preserve" xmlns="http://www.w3.org/2000/svg"><path fill="#55892e" d="m81.9 1.5c44.2-0.6 82.9 39 80.4 83.3-2.2 34.8-21.9 65.5-42 92.9-12 14.6-24.7 33.2-39.3 43.5-17.2-17.4-33-36.3-47-56.4-18.1-27.4-36.3-59.5-31.8-93.5 4.5-39.2 40.5-70 79.7-69.8z"/><path fill="#ffffff" d="m38 60.4v-2.3c0-1 0.4-1.7 1.4-2 5.2-2 10.5-4 15.7-5.9 8.4-3.2 16.8-6.4 25.2-9.5 0.6-0.2 1.5-0.2 2.1 0 14 5.1 27.9 10.2 41.8 15.3 1.5 0.5 1.7 0.9 1.7 2.5v4.5c0 1.5-0.9 2.1-2.3 1.8-0.9-0.2-1.8-0.5-2.8-0.7-1.2-0.2-1.8 0.2-2.1 1.4-0.4 1.6-0.7 1.9-2.4 1.9h-65.2-4c-1.3 0-1.8-0.4-2-1.7-0.3-1.4-0.9-1.8-2.3-1.5-1 0.2-1.9 0.5-2.8 0.7-1.1 0.3-2-0.4-2.1-1.5 0.1-1.1 0.1-2 0.1-3z"/><path fill="#ffffff" d="m82 130.1h-41.5c-0.9 0-1.7 0-2.2-0.8-0.6-1-0.3-2.2 0.9-2.8 1.5-0.8 3.1-1.7 4.6-2.4 1-0.5 1.4-1.2 1.4-2.2 0.1-1.7 0.7-2.3 2.4-2.3h68.5c1.9 0 2.5 0.5 2.5 2.4 0 1.1 0.5 1.8 1.5 2.2 1.5 0.8 3.1 1.6 4.6 2.4 1.2 0.6 1.6 2 0.8 3-0.3 0.3-0.8 0.6-1.3 0.7-0.9 0.1-1.8 0.1-2.8 0.1-13.1-0.3-26.2-0.3-39.4-0.3z"/><path fill="#ffffff" d="m66 93.6v-17.8c0-2.4 1.2-3.6 3.5-3.6 1.2 0 2.4 0.1 3.6 0 2.9-0.2 4 1.5 4 3.9-0.1 7.8 0 15.5 0 23.3v12c0 2.2-1.2 3.4-3.3 3.4h-4.5c-1.9 0-3.2-1.3-3.2-3.2-0.1-6-0.1-12-0.1-18z"/><path fill="#ffffff" d="m117.4 93.6v17.7c0 2.5-1.1 3.6-3.6 3.6h-4.2c-2 0-3.2-1.2-3.2-3.2v-5.7-30c0-2.7 1.1-3.8 3.8-3.8h3.8c2.2 0 3.4 1.2 3.4 3.4v18z"/><path fill="#ffffff" d="m56.9 93.6v17.9c0 2.1-1.2 3.4-3.3 3.4h-4.4c-2 0-3.3-1.3-3.3-3.4v-20-15.6c0-2.5 1.1-3.7 3.6-3.7h4c2.2 0 3.4 1.2 3.4 3.5v17.9z"/><path fill="#ffffff" d="m86.2 93.5v-17.7c0-2.4 1.1-3.6 3.5-3.6h4.3c1.9 0 3.2 1.2 3.2 3.1v36.3c0 2-1.2 3.2-3.2 3.2h-4.6c-1.9 0-3.1-1.2-3.1-3.1-0.1-6-0.1-12.1-0.1-18.2z"/></svg>`;

	var museum = L.divIcon({
		className: 'marker marker-museum',
		html: museumSvg,
		iconSize: [26, 26],
		iconAnchor: [13, 13],
		popupAnchor: [0, -13]
	});

	var leafSvg = `<?xml version="1.0" encoding="utf-8"?>
<!-- Generator: Adobe Illustrator 28.6.0, SVG Export Plug-In . SVG Version: 9.03 Build 54939)  -->
<svg version="1.1" id="Layer_1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" x="0px" y="0px"
	 viewBox="0 0 50 40.8" style="enable-background:new 0 0 50 40.8;" xml:space="preserve">
<g>
	<path d="M3.8,26.6c1.1,0.1,2.3-1,3.1-1.8c7.5-7.1,16.9-14.4,24.8-16.2c0.6,0.2-0.4,0.8-0.7,1c-11.2,6.5-23,15.5-30,26.7
		c-2.1,3-0.9,6,2.3,2.4c2.4-3.4,3.8-5.1,8.2-3.3c7.1,2.6,15.5,0.4,20.4-5.3c3.1-3.5,5-7.9,7.3-11.8c1.8-3.2,3.8-6.3,6.6-8.6
		c1.3-1.3,4.4-2.5,4-4.6C48.1,2,37.1,0.3,33.2-0.1C23.4-1,12.2,0.1,5.4,8.2c-3.7,4.3-5.7,9.9-4.2,15.3c0.4,1.3,1,3,2.4,3.2L3.8,26.6
		z"/>
</g>
</svg>`;

	var leaf = L.divIcon({
		className: 'marker marker-nature',
		html: leafSvg,
		iconSize: [26, 26],
		iconAnchor: [13, 13],
		popupAnchor: [0, -13]
	});

	var markers = L.markerClusterGroup({
		spiderfyOnMaxZoom: false,
		showCoverageOnHover: false
	});

	// tivoli bays wetlands
	markers.addLayer(L.marker([42.03907, -73.915076], {icon: museum}).bindPopup(`
		<dl>
			<dt>Recorder/settings:</dt>
				<dd>Sony PCM-M10, gain 4.5, limiter on</dd>
		</dl>
	`));

	// kaatsbaan culture park pond
	markers.addLayer(L.marker([42.0570125,-73.9178896], {icon: bird}).bindPopup('<div class="report-notes">Field report notes to go here</div>'));

	map.addLayer(markers);

	// hidden markers
	var hidden = L.divIcon({
		className: 'marker-hidden',
		html: '<svg viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><circle cx="12" cy="12" r="12" fill="#0000ff" /></svg>',
		iconSize: [12, 12],
		iconAnchor: [6, 6],
		popupAnchor: [0, -10]
	});

	L.marker([42.035889, -73.909928], {icon: hidden}).addTo(map).bindPopup('Turnoff for Tivoli Meadows trail');
	L.marker([42.039349, -73.905931], {icon: hidden}).addTo(map).bindPopup('Mic position?');
	L.marker([42.037182, -73.905587], {icon: hidden}).addTo(map).bindPopup('Mic position?');

	var triggerElement = document.getElementById("display_hidden_markers");

	triggerElement.addEventListener('click', function (e) {
		if (e.detail === 2) {
	    	var hiddenMarkers = document.getElementsByClassName("marker-hidden"),
	        len = hiddenMarkers !== null ? hiddenMarkers.length : 0, i = 0;
		    for (i; i < len; i++) {
		    	hiddenMarkers[i].classList.add("marker-show");
		    }

		    triggerElement.classList.add("visible");
	    }
	});

	// give coordinates of clicked position
	function getCoordinates(e) {
		if (triggerElement.classList.contains("visible")) {
	    	document.getElementById("display_hidden_markers").innerHTML = e.latlng.lat.toFixed(6) + ", " + e.latlng.lng.toFixed(6);
	    }
	}

	map.on('click', getCoordinates);
</script>
