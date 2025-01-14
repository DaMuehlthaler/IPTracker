<script>
	let ip = '';
	let fetchlink =
		'https://geo.ipify.org/api/v2/country,city?apiKey=at_P2k1SJ7wtx0ZDGYh8UMH2PPI6Xz0w&ipAddress=';

	function combineLink() {
		if (checkIfValidIp(ip) === true) {
			console.log('valid');
			let finishfetchlink = fetchlink + ip;
			console.log(finishfetchlink);
			console.log(ip);
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


	 async function fetchIpData() {
		try {
			const response = await fetch(fetchlink + ip);
			const data = await response.json();
			console.log(data);
			let ipad = data.ip;
			let location = data.location.city + ', ' + data.location.country;
			let timezone = data.location.timezone;
			let isp = data.isp;
			console.log(ipad, location, timezone, isp);
		} catch (error) {
			console.log(error);
		}

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
