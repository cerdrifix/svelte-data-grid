<script>
	import DataGridHeaderFilter from './SvDataGridHeaderFilter.svelte';
    
    export let config, rows, filteredRows
	
	let headers = [], filters = {};
	let lastSortField, lastSortOrder;
	
	function composeHeaders() {
		headers.push(config.columns.filter(c => c.row == 0 || !c.row));
		for(let i=1; i<3; i++) {
			let _t = config.columns.filter(c => c.row == i);
			if(_t.length) {
				headers.push(_t);
			}
		}
	}
	
	function getNextSortOrder(sortField) {
		if(lastSortField == sortField) {
			return revertSortOrder(lastSortOrder);
		}
		lastSortField = sortField;
		return revertSortOrder("desc");
	}
	
	function revertSortOrder(sortOrder) {
		lastSortOrder = sortOrder == 'asc' ? 'desc' : 'asc';
		return lastSortOrder;
	}
	
	function sortColumn(sortField) {
		let sortOrder = getNextSortOrder(sortField); 
		let isDesc = sortOrder == 'desc';
		filteredRows = filteredRows.sort((a,b) => {
			let _a = a[sortField],
					_b = b[sortField];
			if( typeof _a === "string") {
				return isDesc ? _b.localeCompare(_a) : _a.localeCompare(_b);
			} else {
				return isDesc ? _b - _a : _a - _b;
			}
			return isDesc ? b[sortField].localeCompare(a[sortField]) : a[sortField].localeCompare(b[sortField]);
		});
	}
	
	composeHeaders();
	console.log(headers);
</script>

<style>
	th {
		padding: .75em;
		vertical-align: middle;
	}
	
	.fa {
    letter-spacing: -5px;
    font-size: 18px;
    vertical-align: sub;
	}
	
	.fa.disabled {
		color: #aaa;
	}
</style>

<thead class="dg-header">
	{#each headers as row, i}
	<tr>
		{#each row as {dataField, filter, description, type, dataSort, hidden, rowSpan, colSpan, width}, i}
		<th on:click={() => { !!dataSort && sortColumn(dataField)} }
			rowSpan={rowSpan}
			colSpan={colSpan}
			style="{!!width ? `width:${width}px;min-width:${width}px;` : ''}{!!hidden ? `display:none;`: ''}"
			>
			{description}
			{#if !!dataField && lastSortField == dataField}
			<span>
				<i class="fa fa-angle-up" class:disabled="{lastSortOrder === 'desc'}">&nbsp;</i>
				<i class="fa fa-angle-down" class:disabled="{lastSortOrder === 'asc'}">&nbsp;</i>
			</span>
			{/if}
			{#if !!dataField && !!filter}
			<DataGridHeaderFilter   bind:filteredRows={filteredRows} 
                                    rows={rows}
                                    filter={filter}
                                    dataField={dataField}
                                    bind:filters={filters} />
			{/if}
		</th>
		{/each}
	</tr>
	{/each}
</thead>