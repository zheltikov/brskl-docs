# Refunds

`{POST} /api/payments/v1/dashboard/transaction/refund/refund`

The general form of the Request parameters is as follows:

```ts
interface Request {
    uuid: string;            // The UUID of the Transaction to refund

    action: string;          // The action to perform:
                             // - 'check': Check the input data, and report if the refund can be made
                             // - 'execute': Perform the refund itself

    mode: string;            // The mode in which to perform the refund:
                             // - 'full': The refund is made with an exact amount specified, in the `amount` parameter
                             // - 'by_items': The refund is made item-by-item, based on the data in the `items` parameter

    amount: number;          // The amount which to refund

    items: Array<{           // The item data which to refund
        code: string;        // The Item identifier
        amount: number;      // The total amount to refund for this item, calculated as `price` * `quantity`
        price: number;       // The price of this item
        quantity: number;    // The quantity of this item to refund
    }>;
}
```

To perform a Refund you must first check if it can be done (`action: check`), and then perform the refund
itself (`action: execute`).

The refund can be made in two modes:

- **by items**: specifying the exact items, and in which quantity they need to be refunded.
- **by amount**: specifying the total amount to refund, without detailed info about the items.

## Refund _by items_

To perform this type of refund you will need to set the `mode` parameter to `by_items`. The parameter `items` must be
provided, and the `amount` parameter is not needed, it will be ignored.

## Refund _by amount_

To perform this type of refund you will need to set the `mode` parameter to `full`. The parameter `amount` must be
provided, and the `items` parameter is not needed, it will be ignored.
