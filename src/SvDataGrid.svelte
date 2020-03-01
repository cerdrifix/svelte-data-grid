<script>
	import { onMount } from 'svelte';
	import DataGridHeader from './SvDataGridHeader.svelte';
	import DataGridBody from './SvDataGridBody.svelte';
	import moment from 'moment';
	import clone from 'lodash.clone';
	
	export let config;

	let promise, rows = [], filteredRows = [];

	export function cleanData() {
		setData([]);
	}
	export function refreshData(o) {
		switch(getObjectType(o)) {
			case "promise": {
				console.log("called as promise")
				promise = o;
				setData(promise.then(async (response) => {
					setData(await response.json())
				}));
				break;
			}
			case "function": {	
				console.log("called as function")
				promise = callServer(o);
				break;
			}
			case "object": {
				console.log("called as object")
				setData(o);
			}
			default: {
				console.log("called as ", getObjectType(o))
				if(!config.dataURL) return;
				promise = callServer(() => fetch(config.dataURL));
			}
		}
	}

	function getObjectType(obj) {
		if(!obj) return undefined;
		if(typeof obj.then === "function") return "promise";
		return typeof obj;
	}

	async function callServer(fn) {
		if(!fn) return;
		
		console.log("Fetching data...");

		let t1 = (new Date()).getTime();
		let data = await fn();
		let t2 = (new Date()).getTime();
		
		if(!!data) {
			setData(await data.json());
			console.log(`Fetched ${rows.length} rows in ${t2 - t1} millis`);
		} else {
			console.error("Error fetching data...");
		}
	}
	
	function setData(data) {
		if(!data)
			return;
		rows = clone(data);
		filteredRows = clone(data);
	}
	
	onMount(refreshData);
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
		{#await promise}
			<tr><td colspan={config.columns.length}>Waiting for data...</td></tr>
		{:then rows}
			<DataGridBody config={config} bind:filteredRows={filteredRows} />
		{:catch error}
			<tr><td colspan={config.columns.length}>Error retrieving data</td></tr>
		{/await}
</table>