<script>
	let ip = '';
	let fetchlink =
		'https://geo.ipify.org/api/v2/country,city?apiKey=at_P2k1SJ7wtx0ZDGYh8UMH2PPI6Xz0w&ipAddress=';

	function combineLink() {
		let finishfetchlink = fetchlink + ip;
		console.log(finishfetchlink);
		console.log(ip);
	}
</script>
<div class="flex flex-col items-center justify-center gap-4">
<h1>IP Adress Tracker</h1>
<input type="text" bind:value={ip} placeholder="Enter your IP" />
<button class="btn" onclick={combineLink}>Hallihalo</button>
<div class="flex flex-row items-center justify-center gap-4">
	<p>IP Address: </p>
	<p>Location: </p>
	<p>Timezone: </p>
	<p>ISP: </p>
</div>
</div>
