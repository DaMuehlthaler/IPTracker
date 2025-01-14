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
			console.log(ipad, location, timezone, isp);
		} catch (error) {
			console.log(error);
		}
	}
</script>

<div class="flex flex-col items-center justify-center gap-4">
	<h1>IP Adress Tracker</h1>
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
</div>
