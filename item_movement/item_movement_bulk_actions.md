# Item Movement Reports Bulk Actions

There are two bulk operations that can be done with Item Movement Reports: deletion and download.

Their usage is described below:

## Deletion of Item Movement Reports

To delete various Item Movement Reports at a time, use the following endpoint:

Method: `POST`  
Endpoint: `/api/company/v2/dashboard/reports/items/movement/history/delete-report`

The Request parameters for this endpoint look as follows:

```ts
interface Request {
    // If you need to delete a single report supply this parameter:
    id: int;

    // If you need to delete several reports, supply:
    ids: int[];
}
```

On success, you get a successful message, like: `{ message: 'Успешно удалено' }`

## Download of Item Movement Reports

To download several Item Movements reports at a time, use the following endpoint:

Method: `GET`  
Endpoint: `/api/company/v2/dashboard/reports/items/movement/history/bulk-download`

The necessary Query Parameters for this endpoint are:

```ts
interface Request {
    // If you need to download a single report supply this parameter:
    id: int;

    // If you need to download several reports, supply:
    ids: int[];

    // The format of the report files that you want
    export_format: 'csv' | 'xlsx';

    // The authorization token needed to download the file
    token: string;
}
```

On success, you get a zip file with the report files as a download.
