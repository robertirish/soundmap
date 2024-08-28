---
layout: page
title: Map
permalink: /map
---

<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" integrity="sha256-p4NxAoJBhIIN+hmNHrzRCf9tD/miZyoHS5obTRR9BMY=" crossorigin="" />
<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js" integrity="sha256-20nQCchB9co0qIjJZRGuk2/Z9VM+kNiyxNV1lvTlZBo=" crossorigin=""></script>

<div id="map"></div>

<div id="display_hidden_markers"></div>

<script type="text/javascript">
	var map = L.map('map').setView([42.0587815,-73.9207478], 12);
	
	L.tileLayer('https://{s}.basemaps.cartocdn.com/rastertiles/voyager_labels_under/{z}/{x}/{y}{r}.png', {
		attribution: '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors &copy; <a href="https://carto.com/attributions">CARTO</a>',
		subdomains: 'abcd',
		maxZoom: 20
	}).addTo(map);

	var red = L.divIcon({
		className: 'marker',
		html: '<svg viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><circle cx="12" cy="12" r="12" fill="#ff0000" /></svg>',
		iconSize: [12, 12],
		iconAnchor: [6, 6],
		popupAnchor: [0, 0]
	});

	var hidden = L.divIcon({
		className: 'marker-hidden',
		html: '<svg viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><circle cx="12" cy="12" r="12" fill="#0000ff" /></svg>',
		iconSize: [12, 12],
		iconAnchor: [6, 6],
		popupAnchor: [0, -10]
	});	

	// tivoli bays wetlands
	L.marker([42.03907, -73.915076], {icon: red}).addTo(map).bindPopup('<div class="report-notes">Field report notes to go here</div>');

	// kaatsbaan culture park pond
	L.marker([42.0570125,-73.9178896], {icon: red}).addTo(map).bindPopup('<div class="report-notes">Field report notes to go here</div>');

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
