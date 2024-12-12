# Episode TI 1 - Currency Conversion

⬅️ [Back to index](README.md)

## The task

It is necessary to implement a component that is responsible for converting the currencies. The component should use 3rd party service that provides information about conversion rates and available via `RateService` interface. Calls to the `RateService` are expensive, the service is available only 98% of time. 

```java
interface RateService {
    TODO getRate(TODO... input);
}

class CurrencyConverter {
    private final RateService rateService; 

    public CurrencyConverter(final RateService rateService) {
        this.rateService = rateService; 
    }

    public TODO convert(TODO... input) {
        // put your implementation here. 
    }
}
```

## The solution

First of all, it is necessary to come up with the idea on what should be put instead of `TODO` everywhere. The `RateService` should accept a pair of instances of a `Currency` type and next return a rate value. For the sake of precision, it is better to use `BigDecimal` rather than `dobule` or `float`. So, the interface should be updated to: 

```java
interface RateService {
    BigDecimal getRate(Currency from, Currency to);
}
```

The second part is about implementation of the `CurrencyConverter`. It should accept an instance of a type that has both amount and the currency to represent the value like `10$`. The structure might be like this: 

```java
record Amount(
    BigDecimal amount, 
    Currency currency
) {}
```

So, the component now has the following interface: 

```java
class CurrencyConverter {
    public Amount convert(final Amount amount, final Currency target) {}
}
```

To be continued...