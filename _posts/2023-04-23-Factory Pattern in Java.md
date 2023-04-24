---
layout:     post
title:      "Factory Pattern in Java"
subtitle:   "Design Pattern"
date:       2023-04-23 08:00:00
author:     "kap"
header-img: "img/2023-04/cat.jpg"
tags:
- Design Pattern
---
# Factory Pattern in Java - Payment System Example

The Factory Pattern can be used to create different payment method objects and execute payments accordingly.

## Example

Consider a payment processing system that supports multiple payment methods, such as Credit Card and PayPal.

### Payment.java (Interface)

```java
public interface Payment {
    void pay(double amount);
}
```

### CreditCard.java (Concrete class)
```java
public class CreditCard implements Payment {
    @Override
    public void pay(double amount) {
        System.out.println("Paying " + amount + " using Credit Card.");
    }
}
```

### PayPal.java (Concrete class)
```java
public class PayPal implements Payment {
    @Override
    public void pay(double amount) {
        System.out.println("Paying " + amount + " using PayPal.");
    }
}
```

### Factory class
```java
public class PaymentFactory {
    public Payment getPayment(String paymentType) {
        if (paymentType == null) {
            return null;
        }
        if (paymentType.equalsIgnoreCase("CREDITCARD")) {
            return new CreditCard();
        } else if (paymentType.equalsIgnoreCase("PAYPAL")) {
            return new PayPal();
        }
        return null;
    }
}
```

### Main.java (Client code)
```java
public class Main {
    public static void main(String[] args) {
        PaymentFactory paymentFactory = new PaymentFactory();
        
        // Create a CreditCard object and call its pay method
        Payment creditCard = paymentFactory.getPayment("CREDITCARD");
        creditCard.pay(100.0);
        
        // Create a PayPal object and call its pay method
        Payment payPal = paymentFactory.getPayment("PAYPAL");
        payPal.pay(200.0);
    }
}
```


# Abstract Factory Pattern in Java - Payment System Example

The Abstract Factory Pattern is a creational design pattern that provides an interface for creating families of related or dependent objects without specifying their concrete classes.

## Example

Consider a payment processing system that supports multiple payment methods and different payment processors, such as Stripe and Braintree.

### Payment.java (Interface)
```java
public interface Payment {
    void pay(double amount);
}


```java
public class CreditCard implements Payment {
    @Override
    public void pay(double amount) {
        System.out.println("Paying " + amount + " using Credit Card.");
    }
}
```
```java
public class PayPal implements Payment {
    @Override
    public void pay(double amount) {
        System.out.println("Paying " + amount + " using PayPal.");
    }
}

```


### Abstract Factory Interface

```java
public interface PaymentProcessor {
    Payment getPaymentMethod(String paymentType);
}

```


### Concrete Factory
```java
public class StripePaymentProcessor implements PaymentProcessor {
    @Override
    public Payment getPaymentMethod(String paymentType) {
        if (paymentType == null) {
            return null;
        }
        if (paymentType.equalsIgnoreCase("CREDITCARD")) {
            return new CreditCard();
        } else if (paymentType.equalsIgnoreCase("PAYPAL")) {
            return new PayPal();
        }
        return null;
    }
}

...............

public class BraintreePaymentProcessor implements PaymentProcessor {
    @Override
    public Payment getPaymentMethod(String paymentType) {
        if (paymentType == null) {
            return null;
        }
        if (paymentType.equalsIgnoreCase("CREDITCARD")) {
            return new CreditCard();
        } else if (paymentType.equalsIgnoreCase("PAYPAL")) {
            return new PayPal();
        }
        return null;
    }
}


```

### Client 
```java
public class Main {
    public static void main(String[] args) {
        PaymentProcessor stripeProcessor = new StripePaymentProcessor();
        PaymentProcessor braintreeProcessor = new BraintreePaymentProcessor();
        
        // Create a CreditCard object using Stripe processor and call its pay method
        Payment stripeCreditCard = stripeProcessor.getPaymentMethod("CREDITCARD");
        stripeCreditCard.pay(100.0);
        
        // Create a PayPal object using Braintree processor and call its pay method
        Payment braintreePayPal = braintreeProcessor.getPaymentMethod("PAYPAL");
        braintreePayPal.pay(200.0);
    }
}
```




# Factory Pattern vs Abstract Factory Pattern

The main difference between the Factory Pattern and the Abstract Factory Pattern is the level of abstraction they provide when creating objects.

## Factory Pattern

The Factory Pattern is a creational design pattern that provides an interface for creating objects in a superclass but allows subclasses to alter the type of objects that will be created. This pattern is used when you want to create objects of a single family or category.

### Example

In the Factory Pattern example, we have a single `PaymentFactory` class responsible for creating different payment method objects (i.e., `CreditCard` and `PayPal`). The client code uses this factory to create payment method objects without knowing the concrete classes involved.

```java
PaymentFactory paymentFactory = new PaymentFactory();

Payment creditCard = paymentFactory.getPayment("CREDITCARD");
creditCard.pay(100.0);

Payment payPal = paymentFactory.getPayment("PAYPAL");
payPal.pay(200.0);
```



## Abstract Factory Pattern
The Abstract Factory Pattern is a creational design pattern that provides an interface for creating families of related or dependent objects without specifying their concrete classes. This pattern is used when you want to create objects belonging to multiple families or categories.

### Example
In the Abstract Factory Pattern example, we have two concrete factories, StripePaymentProcessor and BraintreePaymentProcessor, responsible for creating different payment method objects (i.e., CreditCard and PayPal). The client code uses these factories to create payment method objects without knowing the concrete classes involved but also allows for different processing systems.

```java
PaymentProcessor stripeProcessor = new StripePaymentProcessor();
PaymentProcessor braintreeProcessor = new BraintreePaymentProcessor();

Payment stripeCreditCard = stripeProcessor.getPaymentMethod("CREDITCARD");
stripeCreditCard.pay(100.0);

Payment braintreePayPal = braintreeProcessor.getPaymentMethod("PAYPAL");
braintreePayPal.pay(200.0);
```

In summary, the Factory Pattern is focused on creating objects of a single family or category, whereas the Abstract Factory Pattern deals with creating objects belonging to multiple families or categories.

