<script>
	import { onMount } from 'svelte';
	import 'leaflet/dist/leaflet.css';
	import 'leaflet.markercluster/dist/MarkerCluster.css';
	import 'leaflet.markercluster/dist/MarkerCluster.Default.css';

	let map;
	let L;

	onMount(async () => {
		L = await import('leaflet');
		map = L.map('map').setView([47.340791, 13.190148], 13);

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

<!-- 333533 -->

<div class="flex h-screen flex-col items-center justify-center gap-4 bg-[#34403a]">
	<p
		class="m-0 mt-8 box-border text-center text-4xl font-bold tracking-wide text-white no-underline outline-none"
	>
		IP Address Tracker
	</p>
<div class="flex items-center bg-base-100 mt-3 mb-10 rounded shadow-lg">
  <input
    type="text"
    placeholder="Search for any IP address or domain"
    class="input input-bordered w-[500px] rounded-r-none"
	bind:value={ip}
  />
  <button class="btn btn-error rounded-l-none" onclick={combineLink}>
    ➤
  </button>
</div>
	
	<div
		class="flex h-[20%] w-[80%] flex-row items-start justify-center gap-2 space-x-56 rounded-lg bg-white p-4 text-black shadow-lg"
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
	<div id="map" class="h-screen w-screen z-0"></div>
</div>
