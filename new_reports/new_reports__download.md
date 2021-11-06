# Report Download

After the report is successfully generated, the user may download it using these buttons:

![Download Buttons](images/new_reports_006.jpg "Download Buttons")

To download a generated report, use the following endpoint:

Method: `GET`  
Endpoint: `/api/company/v2/dashboard/reports/orders/download-report`

Supply to it the following query parameters:

```Hack
shape(
    // The access token
    'token' => string,

    // The `report_key` you got on the generation stage of the report
    'report_key' => string,
    
    // The export format that the user wants to download
    'export_format' => 'csv' | 'xlsx',
    
    // A list of columns that the resulting report should have, with localized names
    'dictionary' => array<
        shape(
            // The name of the source field for the column
            'name' => string,
            
            // The localized column name
            'label' => string,
            
            // An optional dictionary of value mappings with translations.
            // For example, for the `payment_service` field:
            // { 1: 'Приложение ', 2: 'Терминал' }
            ?'dictionary' => array<string, string>,
            
            ...,
        )
    >,
    
    ...,
)
```

You should not call this endpoint as an XHR request, but rather open the resulting URL via `window.open`.  
When called, you should get a properly generated report file as a download.
