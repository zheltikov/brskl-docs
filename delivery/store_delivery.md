# Delivery for Stores

The main schema used for all data exchange in the delivery endpoints looks as follows:

```Hack
shape(
    'store_id' => int,
    'external' => shape(
        'enabled' => bool | 0 | 1,
        'coords' => shape(
            'lat' => num,
            'lon' => num,
        ),
        'radius' => 0 | (int & positive),
        'rates' => array<
            shape(
                'order_amount_max' => num & positive,
                'order_amount_min' => num & positive,
                'calculation_type' => int & (0 | 1 | 2 | 3),
                'calculation_val' => num & positive,
                ...,
            )
        >,
        ...,
    ),
    'internal' => shape(
        'enabled' => bool | 0 | 1,
        'coords' => shape(
            'lat' => num,
            'lon' => num,
        ),
        'radius' => 0 | (int & positive),
        'rates' => array<
            shape(
                'order_amount_max' => num & positive,
                'order_amount_min' => num & positive,
                'calculation_type' => int & (0 | 1 | 2 | 3),
                'calculation_val' => num & positive,
                ...,
            )
        >,
        ...,
    ),
    'pickup' => shape(
        'enabled' => bool | 0 | 1,
        ...,
    ),
    ...,
)
```
