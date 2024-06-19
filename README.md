# Vanilla Table

A plain JavaScript table. You provide an endpoint and it displays the data.

This is a work in progress but it stable for my use. 

## Features

### Working

  * retrieve server-side data
  * overall search UI
  * pagination UI
  * arrow navigation between cells
  * custom column header html
  * custom body html

### Planned

  * editor
  * expand/collapse which inserts additional records or custom html
  * sort UI
  * filter UI
  * better doc w/ real examples

### Maybe

  * use client-side data

## How to use it 

1. Include the contents of `vanilla_table.html` in your page.
2. Add some placeholders wherever you want your table to appear.

```html
<div class="mb-1" id="table-search-placeholder"></div>
<div class="table-container mb-0" id="table-placeholder"></div>
<div class=" mt-4" id="table-pagination-placeholder"></div>
```

3. Link to the `vanilla_table.js` script in your page. `<script src="/path/to/vanilla_table.js"></script>`
4. Build out your options object. \ 
(Example shown w/ [Bulma 1.0](https://bulma.io/documentation/) CSS and [FontAwesome 6 free](https://docs.fontawesome.com/web/setup/host-yourself/svg-js) fonts.)

```javascript

let options = {
    'table_elem_id': 'table-placeholder',
    'pagination_elem_id': 'table-pagination-placeholder',
    'search_elem_id': 'table-search-placeholder',
    'data_url': 'http://localhost:5000/api/palm',
    'has_search': true,
    'has_pagination': true,
    'columns': [
        {
            'name': 'id',
            'label': ' ',
            'selectable': false,
            'custom_body': function (record) {
                return `<a href="/palm/${record['id']}" aria-label="Go to palm" title="Go to palm">
                            <i class="fa-solid fa-magnifying-glass"></i>
                        </a>`
            }
        },
        {
            'name': 'genus',
            'label': 'Genus'
        },
        {
            'name': 'species',
            'label': 'Species'
        },
        {
            'name': 'variety',
            'label': 'Variety'
        },
        {
            'name': 'common_name',
            'label': 'Common Name'
        },
        {
            'name': 'zone_name',
            'custom_label': `                   
            <div class="dropdown is-hoverable">
                <div class="dropdown-trigger">
                    Zone <i class="fa-solid fa-circle-info is-size-7" aria-haspopup="true"
                        aria-controls="zone-help"></i>
                </div>
                <div class="dropdown-menu" id="zone-help" role="menu">
                    <div class="dropdown-content">
                        <div class="dropdown-item is-size-7 has-text-weight-normal">
                            <p>USDA Hardiness Zones are based on the average annual minimum
                                temperature.</p>
                            <a target="_blank" href="https://planthardiness.ars.usda.gov/"> <i
                                    class="fa-solid fa-arrow-up-right-from-square"></i> More info at
                                usda.gov</a>
                        </div>
                    </div>
                </div>
            </div>`,
            'editable': true,
        },
        {
            'name': 'observation_count',
            'label': 'Observation Count'
        }
    ]
}
```

5. Initialize the table

```javascript
let vanilla = new VanillaTable(options)
vanilla.createTable()
vanilla.refreshTable()
```

## Data

### Outgoing

The table will GET from an endpoint and pass any optional parameters as query strings on the URL.

GET `YOURSITE?search=term&page=1&results_per_page=25`

### Incoming

Server side, take the optional parameters, use them to query from whatever data source.
Build the meta info to keep the JS fast then bundle it with the records in a JSON object to send to the client. 

```json
{
  "meta": {
    "has_next_page": false,
    "has_previous_page": true,
    "offset": 15,
    "page": 2,
    "results_on_this_page": 5,
    "results_per_page": 15,
    "search": "sabal",
    "total_pages": 2,
    "total_results": 20
  },
  "records": [
    {
      "common_name": "Royal Palmetto",
      "genus": "Sabal",
      "id": 2609,
      "last_modified": "2024-06-02 19:06:10.263856",
      "legacy_id": 2608,
      "observation_count": 6,
      "palm_name": "Sabal pumos",
      "species": "pumos",
      "variety": null,
      "who_modified": "Excel Importer",
      "zone_id": 17,
      "zone_name": "9a"
    },
    {
      "common_name": "Llanos Palmetto",
      "genus": "Sabal",
      "id": 2610,
      "last_modified": "2024-06-02 19:06:10.263859",
      "legacy_id": 2609,
      "observation_count": 4,
      "palm_name": "Sabal rosei",
      "species": "rosei",
      "variety": null,
      "who_modified": "Excel Importer",
      "zone_id": 17,
      "zone_name": "9a"
    },
    {
      "common_name": "Sonora Palmetto",
      "genus": "Sabal",
      "id": 2611,
      "last_modified": "2024-06-02 19:06:10.263862",
      "legacy_id": 2610,
      "observation_count": 22,
      "palm_name": "Sabal uresana",
      "species": "uresana",
      "variety": null,
      "who_modified": "Excel Importer",
      "zone_id": 14,
      "zone_name": "7b"
    },
    {
      "common_name": "Brazoria Palmetto",
      "genus": "Sabal",
      "id": 2612,
      "last_modified": "2024-06-02 19:06:10.263865",
      "legacy_id": 2611,
      "observation_count": 10,
      "palm_name": "Sabal x",
      "species": "x",
      "variety": null,
      "who_modified": "Excel Importer",
      "zone_id": 17,
      "zone_name": "9a"
    },
    {
      "common_name": "Thatch palm",
      "genus": "Sabal",
      "id": 2613,
      "last_modified": "2024-06-02 19:06:10.263868",
      "legacy_id": 2612,
      "observation_count": 10,
      "palm_name": "Sabal yapa",
      "species": "yapa",
      "variety": null,
      "who_modified": "Excel Importer",
      "zone_id": 17,
      "zone_name": "9a"
    }
  ]
}
```



