<script>

	import { createEventDispatcher } from 'svelte';
	import moment from 'moment';
	
	const dispatch = createEventDispatcher();

	export let filteredRows = [], config;
	
	function dataFormatDefaultFunc(value, type) {
		let returnValue;
		switch(type) {
			case "datetime": {
				let d = moment(value, "YYYY-MM-DDTHH:mm:ssZ")
				returnValue = d.isValid() ? d.format("DD/MM/YYYY HH:mm") : value;
				break;
			}
			default: {
				returnValue = value;
			}
		}
		return returnValue;
	}

	function rowClicked(row, index) {
		dispatch('rowClicked', {
			row,
			index
		});
	}
</script>

<style>
	td {
		padding: .75em;
		vertical-align: middle;
	}
</style>

<tbody>
	{#each filteredRows as row, ridx}
	<tr on:click={rowClicked(row, ridx)}>
		{#each config.columns.filter(c => !!c.dataField) as {dataField, type, hidden, width, dataFormat, dataAlign}, cidx}
		<td style="{!!width ? `width:${width}px;min-width:${width}px;` : ''}
							 {!!hidden ? `display:none;`: ''}
							 {!!dataAlign ? `text-align: ${dataAlign};` : ''}">
			{!!dataFormat ? dataFormat(row[dataField], row) : dataFormatDefaultFunc(row[dataField], type)}
		</td>
		{/each}
	</tr>
	{/each}
</tbody>