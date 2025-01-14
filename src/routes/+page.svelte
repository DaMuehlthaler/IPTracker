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
		} else {
			console.log('invalid ip');
		}
	}
	function checkIfValidIp(ip) {
		const ipRegex =
			/^(25[0-5]|2[0-4][0-9]|1[0-9]{2}|[1-9]?[0-9])(\.(25[0-5]|2[0-4][0-9]|1[0-9]{2}|[1-9]?[0-9])){3}$/;
		return ipRegex.test(ip);
	}
</script>

<h1>IP Adress Tracker</h1>
<input type="text" bind:value={ip} placeholder="Enter your IP" />
<button class="btn" onclick={combineLink}>Hallihalo</button>
