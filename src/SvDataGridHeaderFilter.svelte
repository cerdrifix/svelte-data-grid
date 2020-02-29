<script>
	import clone from 'lodash.clone';
	export let filter, filters = {}, dataField, rows, filteredRows;
	
	let timerId;
	function doFilter(event) {
		if(!!timerId)
			clearTimeout(timerId);
		
		timerId = setTimeout(() => {
			filters[dataField] = event.target.value.toLowerCase();
			filteredRows = clone(rows);
			for(let key in filters) {
				filteredRows = filteredRows.filter(r => r[key].toLowerCase().indexOf(filters[key]) >= 0);
			}
		}, filter.delay);
	}
	 
</script>

<style>
	input {
		height: 22px !important;
		margin-top: 10px;
		padding-left: 10px;
	}
	
	input.text-filter {
		font-weight: 400;
		padding: .375rem .75rem;
		line-height: 1.5;
		color: #495057;
		background-color: #fff;
    background-clip: padding-box;
    border: 1px solid #ced4da;
    border-radius: .25rem;
    transition: border-color .15s ease-in-out,box-shadow .15s ease-in-out;
	}
</style>

<div>
	{#if filter.type == "TextFilter"}
		<input type="text" 
					 class="filter text-filter form-control" 
					 placeholder="{ !!filter.placeholder ? filter.placeholder : ' '}" 
					 on:keydown={doFilter}
					 on:click|stopPropagation=""/>
	{/if}
</div>