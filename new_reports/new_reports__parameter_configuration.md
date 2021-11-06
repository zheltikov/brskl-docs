# Report Parameter Configuration

After the user selects the report type, he needs to configure the report parameters. For example, in a set of inputs
like these:

![Report Parameters](images/new_reports_003.jpg "Report Parameters")

Each report type may have its own, different, parameters.  
To get the parameters for a specific report, you will need to fetch this endpoint:

Method: `GET`  
Endpoint: `/api/company/v2/dashboard/reports/orders/get-report-config`

You will get a response structure that looks like this:

```Hack
shape(
    // A dictionary with the parameters needed for each report type
    'parameters' => array<
        // The report type as the key
        string,
        
        // An array of parameter identifiers as the value
        array<string>,
    >,
    
    // A dictionary with options for several parameters (like dropdowns)
    'options' => array<
        // The parameter identifier as the key
        string,
        
        // A list of available options as key-value pairs as the value
        array<string, string | num>,
    >,
    
    ...,
)
```

For example, we may get a response like this one:

```json5
{
  parameters: {
    deliveries_report: [
      'store',
      'period',
      'customer',
      'payment_service',
    ],
  },
  options: {
    payment_service: {
      APP: 1,
      TERMINAL: 2,
    }
  }
}
```

This means that the report type `deliveries_report`, has those specified four parameters. The
parameter `payment_service` has some options available, so it's a dropdown with two options: `APP` and `TERMINAL`, shown
to the user; when the report will be sent, you will need to send the values `1` and `2` respectively.

## Notes

In this first iteration of this functionality, these are the only parameter types available:

- `customer`: The Customer ID search part used to filter the report.
- `grouping`: The time grouping to apply to the report (days, months, ...). The options are available in this endpoint.
- `payment_service`: Used to filter the report, to see only orders with a specific payment service. The options are
  available in this endpoint.
- `period`: The period to filter the report by. It may be a closed period (with both start and end timestamps) or an
  open report (with only the start or end timestamp).
- `status`: Filter the report to contain only orders with a specific status. The options are available in this endpoint.
- `store`: Filter the report to contain only data from the specified stores.
- `type`:  Filter the report to contain only orders with of specific type. The options are available in this endpoint.
