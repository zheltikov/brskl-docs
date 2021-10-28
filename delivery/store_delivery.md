# Delivery for Stores

The main schema used for all data exchange in the delivery endpoints looks as follows:

```Hack
shape(
    // The Store ID whose delivery settings these are
    'store_id' => int,
    
    // The settings for the External delivery (managed by partners)
    'external' => shape(
        'enabled' => bool | 0 | 1,
        
        // Specifies where this delivery works (describing a circle on a map)
        'coords' => shape(
            'lat' => num,
            'lon' => num,
            ...,
        ),
        'radius' => 0 | (int & positive),
        
        // Specifies the rates for this delivery.
        // This way you can specify different delivery tariffs based on
        // different order total amounts.
        // There must be at least one rate specified.
        // The rates must not overlap, and the distance between the lower bound
        // of the one rate and the upper bound of the previous rate must be
        // exactly 1 monetary unit.
        'rates' => array<
            shape(
                // The upper and lower price bounds where this rate applies,
                // respectively.
                'order_amount_max' => num & positive,
                'order_amount_min' => num & positive,
                
                // The type of calculation to be applied for this rate.
                // May be one of the following values:
                // - 0: The delivery is paid by the customer
                // - 1: A Percentage Discount applied to the delivery price
                // - 2: A Fixed Discount applied to the delivery price
                // - 3: The delivery has a fixed price
                // - 4: Free delivery
                'calculation_type' => int & (0 | 1 | 2 | 3 | 4),
                
                // An additional value needed to calculate the delivery price.
                // Depends on the value of the `calculation_type` field.
                // This value can be zero if not additional value is needed.
                'calculation_val' => num & !negative,
                
                ...,
            )
        >,
        
        ...,
    ),
    
    // The settings for the Internal delivery (managed by the company itself)
    'internal' => shape(
        'enabled' => bool | 0 | 1,
        
        // Specifies where this delivery works (describing a circle on a map)
        'coords' => shape(
            'lat' => num,
            'lon' => num,
            ...,
        ),
        'radius' => 0 | (int & positive),
        
        // Specifies the rates for this delivery.
        // This way you can specify different delivery tariffs based on
        // different order total amounts.
        // There must be at least one rate specified.
        // The rates must not overlap, and the distance between the lower bound
        // of the one rate and the upper bound of the previous rate must be
        // exactly 1 monetary unit.
        'rates' => array<
            shape(
                // The upper and lower price bounds where this rate applies,
                // respectively.
                'order_amount_max' => num & positive,
                'order_amount_min' => num & positive,
                
                // The type of calculation to be applied for this rate.
                // May be one of the following values:
                // - 0: The delivery is paid by the customer
                // - 1: A Percentage Discount applied to the delivery price
                // - 2: A Fixed Discount applied to the delivery price
                // - 3: The delivery has a fixed price
                // - 4: Free delivery
                'calculation_type' => int & (0 | 1 | 2 | 3 | 4),
                
                // An additional value needed to calculate the delivery price.
                // Depends on the value of the `calculation_type` field.
                // This value can be zero if not additional value is needed.
                'calculation_val' => num & !negative,
                
                ...,
            )
        >,
        
        ...,
    ),
    
    // This controls the customer pickup availability
    'pickup' => shape(
        'enabled' => bool | 0 | 1,
        ...,
    ),
    
    ...,
)
```
