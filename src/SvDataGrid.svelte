<script>
	import { onMount } from 'svelte';
	import DataGridHeader from './SvDataGridHeader.svelte';
	import DataGridBody from './SvDataGridBody.svelte';
	import moment from 'moment';
	import clone from 'lodash.clone';
	
	export let config;
	export function cleanData() {
		setData([]);
	}
	export function refreshData(data) {
		setData(data);
	};
	let rows = [], filteredRows = [];
	
	async function fetchData() {
		console.log("Fetching data...")
		let t1 = (new Date()).getTime();
		const res = await fetch(config.dataURL);
    rows = await res.json();
		filteredRows = clone(rows);
		let t2 = (new Date()).getTime();
		console.log("Fetched " + rows.length + " rows in " + (t2 - t1) + " ms");
	}
	
	onMount(() => {
		if(!!config.data)
			setData(config.data);

		if(!!config.dataURL)
			fetchData();
	});

	function setData(data) {
		rows = clone(data);
		filteredRows = clone(data);
	}
</script>

<style>
	@import url(https://fonts.googleapis.com/icon?family=Material+Icons);
	
	.dg-table-container {
		width: 100%;
		color: #333;
		font-size: 0.8em;
	}
</style>
<table class="dg-table-container table">
		<DataGridHeader config={config} bind:filteredRows={filteredRows} rows={rows} />
		<DataGridBody config={config} bind:filteredRows={filteredRows} />
</table>