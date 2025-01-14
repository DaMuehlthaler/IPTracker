<script>
	import { onMount } from 'svelte';
	import 'leaflet/dist/leaflet.css';
	import 'leaflet.markercluster/dist/MarkerCluster.css';
	import 'leaflet.markercluster/dist/MarkerCluster.Default.css';

	let map;
	let L;

	onMount(async () => {
		L = await import('leaflet');
		map = L.map('map').setView([51.505, -0.09], 13);

		L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
			attribution: '© OpenStreetMap contributors'
		}).addTo(map);
	});

	let ip = '';
	let fetchlink =
		'https://geo.ipify.org/api/v2/country,city?apiKey=at_P2k1SJ7wtx0ZDGYh8UMH2PPI6Xz0w&ipAddress=';

	function combineLink() {
		if (checkIfValidIp(ip)) {
			console.log('valid');
			fetchIpData();
		} else {
			console.log('invalid ip');
		}
	}

	function checkIfValidIp(ip) {
		const ipRegex =
			/^(25[0-5]|2[0-4][0-9]|1[0-9]{2}|[1-9]?[0-9])(\.(25[0-5]|2[0-4][0-9]|1[0-9]{2}|[1-9]?[0-9])){3}$/;
		return ipRegex.test(ip);
	}

	let ipad = '';
	let location = '';
	let timezone = '';
	let isp = '';

	async function fetchIpData() {
		try {
			const response = await fetch(fetchlink + ip);
			const data = await response.json();
			console.log(data);
			ipad = data.ip;
			location = data.location.city + ', ' + data.location.country;
			timezone = 'UTC' + data.location.timezone;
			isp = data.isp;

			const lat = data.location.lat;
			const lng = data.location.lng;
			map.setView([lat, lng], 13);

			L.marker([lat, lng]).addTo(map).bindPopup(`Location: ${location}`).openPopup();

			console.log(ipad, location, timezone, isp);
		} catch (error) {
			console.log(error);
		}
	}
</script>

<div class="flex flex-col items-center justify-center gap-4">
	<h1>IP Address Tracker</h1>
	<input type="text" bind:value={ip} placeholder="Enter your IP" />
	<button class="btn" onclick={combineLink}>Submit</button>
	<div
		class="flex h-[80%] w-[80%] flex-row items-start justify-center gap-2 rounded-lg bg-white p-4 text-black shadow-lg"
	>
		<div>
			<p class="font-bold">IP Address:</p>
			<p>{ipad}</p>
		</div>
		<div>
			<p class="font-bold">Location:</p>
			<p>{location}</p>
		</div>
		<div>
			<p class="font-bold">Timezone:</p>
			<p>{timezone}</p>
		</div>
		<div>
			<p class="font-bold">ISP:</p>
			<p>{isp}</p>
		</div>
	</div>
	<div id="map" class="h-80 w-80"></div>
</div>
