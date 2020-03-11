# Changelogs

## 0.5.0

Row click management

## 0.3.0

Improved fetch management. README still missing

Exporting two functions:

- cleanData(): used to reset data to []
- refreshData(object | promise | function): used to refresh grid data. Promises or functions must return json array. Object must be json array. If nothing is passed then fetches data using config.dataURL;

### 0.3.1
Fixed "waiting for data" await

## 0.2.0

Minor fixes

## 0.1.0

First release. Trying to push it as a library

```html
<!-- App.html -->

<script>
    import SvDataGrid from 'svelte-data-grid';

    let config = {
        dataURL: "https://url-to-data-service/",
        columns: [
            { row: "0", dataField: "Id", rowSpan: "2", hidden: true, isKey: true, description: "ID" },
            { row: "0", dataField: "Date", width: 120, rowSpan: "2", dataSort: true, description: "Date", type: "datetime" },
            { row: "0", dataField: "Country", width: "150", rowSpan: "2", description: "Country", dataSort: true, filter: { type: 'TextFilter', delay: 600, placeholder: ' ' } },
            { row: "0", dataField: "Medals", colSpan: "3", description: "Medals" },
            { row: "1", dataField: "GoldMedals", width: 120, dataSort: true, description: "Gold" },
            { row: "1", dataField: "SilverMedals", width: 120, dataSort: true, description: "Silver" },
            { row: "1", dataField: "BronzeMedals", width: 120, dataSort: true, description: "Bronze" }
        ]
    }

</script>

<SvDataGrid config={dataGridConfig} />

```
