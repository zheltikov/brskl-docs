# Item Movements (demand+supply)

<!-- TOC -->

- [Item Movements (demand+supply)](#item-movements-demandsupply)
    - [List of operations](#list-of-operations)
    - [Getting the data for an operation](#getting-the-data-for-an-operation)
    - [Creating an operation](#creating-an-operation)
    - [Updating an operation](#updating-an-operation)

<!-- /TOC -->

An Item Movement operation consists of two operations tied together: one demand and one supply. These operations are
coupled like so: the demand is the parent object, and the supply is the child object.

## List of operations

To get the list of movement operations you will need to call this endpoint:

```http
GET /api/company/v2/dashboard/item/movement/get-list
```

with these parameters:

```ts
const request_params = {
    fields: {
        hash_id: 'hash_id',
        id: 'id',
        date: 'timestamp_inserting',
        tradepoint_id: 'store.id',
        tradepoint_name: 'store.name',
        item_count: 'item_count',
        total_amount: 'total_amount',
        type: 'type',
        status: 'status',
        source: 'source',
    },
    filters: {
        '!source': 'order',
        type: 'demand',
        '!child_count': 0,
    }
};
```

Doing so, we get a list of the parent demand objects.

## Getting the data for an operation

Here we need to both the parent demand operation and the child supply operation.

To do this, we only need the ID of the parent operation from the user.

First, we need to get the parent demand operation:

```yaml
GET /api/company/v2/dashboard/item/movement/get-list

fields: {
          // all the necessary fields...
},
filters: {
  '!source': 'order',
  type: 'demand',
  '!child_count': 0,
  id: // the id of the demand that the user supplied
},
limit: 1, // only select one operation
```

Then, we need to get the child supply operation:

```yaml
GET /api/company/v2/dashboard/item/movement/get-list

fields: {
          // all the necessary fields...
},
filters: {
  '!source': 'order',
  type: 'supply',
  parent_id: // the id of the demand that the user supplied
},
limit: 1, // only select one operation
```

These two operations can be done in parallel, to speed things up:

```ts
const promises = [];

// ----

const demand_params = { /* the params described above... */};
promises.push(companyAPI.getMovementById(demand_params)); // Note: no await here!

// ----

const supply_params = { /* the params described above... */};
promises.push(companyAPI.getMovementById(supply_params)); // Note: no await here!

// ----

const [demand_data, supply_data] = await Promise.allSettled(promises);
```

## Creating an operation

To create a movement operation we must first create the demand operation; then, we need to create the supply operation
and link it with the demand:

1. Create the demand operation, with negative values.
2. Create the supply operation, with positive values. And add another parameter to the create
   endpoint: `parent_id: {{ the ID of the demand in step 1. }}`

The endpoints for the creates are the same as the ones previously used.

We have successfully created a movement operation.

## Updating an operation

We can update a movement operation only if both the parent demand operation and the child supply operation are open.

Then, to perform the actual update, do:

1. Update the demand operation, with negative values.
2. Update the supply operation, with positive values.

The endpoints for the updates are the same as the ones previously used.
