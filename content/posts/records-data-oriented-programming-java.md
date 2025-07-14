---
title: "Java Records and Data Oriented Programming"
date: 2025-07-15T14:15:00+01:00
description: "An overview of how Project Amber enables a shift toward Data-Oriented Programming, offering clearer and more maintainable ways to model domain data in enterprise applications."
tags: [Java]
---

With a long history, Java has seen various programming styles. As Java evolves, the way we write our software evolves too. In particular, Project Amber has introduced a few new language features that simplify how we design applications. The Data-Oriented Programming (DOP) paradigm plays a significant role in this transformation.

## Languages Features

In short, Project Amber development efforts consist of three main language features:
- Java Records
- Sealed Classes
- Pattern Matching

This allows us to write the following code example:

```java
public sealed interface Result {
    record Success(String data) implements Result {}
    record Failure(String reason) implements Result {}
}

static void handleResult(Result result) {
    String message = switch (result) {
        case Success(var data) -> "✅ Success: " + data;
        case Failure(var reason) -> "❌ Failure: " + reason;
    };
    System.out.println(message);
}
```

## What Records Are Good For

Java records allow us to write immutable value objects. Contrary to traditional OOP principles, where data is coupled with behavior, Java records are pure data. Most importantly, they are immutable (assuming developers avoid placing mutable data structures inside them).

This helps eliminate boilerplate code typically found in Java Beans and enables efficient modeling of domain entities. Kotlin has had data classes for some time, with a similar concept in mind.

Algebraic Data Types (ADTs) are a particularly expressive use case when combining records with sealed classes. However, ADTs are not yet very common in enterprise software development, so I will not explore sealed class features deeply in this article.

That said, value objects are a core concept in Domain-Driven Design (DDD). Java records make it much easier to implement such concepts in a clean and concise way.

## Modeling Data in a New Way

Now we can leverage Java records to model data more effectively. No more Lombok annotations. Instead of a single model reused in multiple contexts, we can define dedicated records for each use case. This leads to clear and readable data structures that better reflect the business logic.

Consider the following example of an e-commerce order using a traditional approach:

```java
public class Order {
    private String id;
    private String customerName;
    private List<String> items;
    private BigDecimal totalPrice;
    private boolean isPaid;
    private boolean isShipped;

    public Order(String id, String customerName, List<String> items, BigDecimal totalPrice) {
        this.id = id;
        this.customerName = customerName;
        this.items = items;
        this.totalPrice = totalPrice;
        this.isPaid = false;
        this.isShipped = false;
    }

    // getters and setters for all fields
    public String getId() { return id; }
    public void setId(String id) { this.id = id; }
    
    public String getCustomerName() { return customerName; }
    public void setCustomerName(String customerName) { this.customerName = customerName; }

    public List<String> getItems() { return items; }
    public void setItems(List<String> items) { this.items = items; }

    public BigDecimal getTotalPrice() { return totalPrice; }
    public void setTotalPrice(BigDecimal totalPrice) { this.totalPrice = totalPrice; }

    public boolean isPaid() { return isPaid; }
    public void setPaid(boolean paid) { isPaid = paid; }

    public boolean isShipped() { return isShipped; }
    public void setShipped(boolean shipped) { isShipped = shipped; }

    @Override
    public String toString() {
        return "Order{" + "id='" + id + '\'' + ", customerName='" + customerName + '\'' +
                ", items=" + items + ", totalPrice=" + totalPrice +
                ", isPaid=" + isPaid + ", isShipped=" + isShipped + '}';
    }
}
```

As you can see, the same structure is used for all use cases. Additionally, the boilerplate code harms readability. Thankfully, we can now rewrite it more elegantly:

```java
// Algebraic Data Type for Order Status
public sealed interface OrderStatus permits Paid, Unpaid {}

public record Paid(String paymentId) implements OrderStatus {}
public record Unpaid() implements OrderStatus {}

// Immutable Order model for business logic
public record Order(
        String id,
        String customerName,
        List<String> items,
        BigDecimal totalPrice,
        OrderStatus status
) {}

// A record just for presenting Order Summary in the UI
public record OrderSummary(
        String id,
        String customerName,
        BigDecimal totalPrice
) {}

// A record used specifically for creating new orders (e.g., from a request body)
public record CreateOrderRequest(
        String customerName,
        List<String> items
) {}
```

These clean definitions of Java records significantly improve readability. In addition, we can embed useful validations:

```java
public record CreateOrderRequest(
        String customerName,
        List<String> items
) {
    public CreateOrderRequest {
        if (customerName.isBlank()) {
            throw new IllegalArgumentException("Customer name cannot be blank");
        }
    }
}
```

## Data-Oriented Programming

The work done in Project Amber aligns closely with the Data-Oriented Programming concept. Its goal is not to replace object-oriented programming, but to complement it when appropriate. Brian Goetz has a brilliant article on this topic, which sheds light on this paradigm and the direction in which Java is heading: ["Data Oriented Programming in Java" by Brian Goetz](https://www.infoq.com/articles/data-oriented-programming-java/)

Key principles of Data-Oriented Programming, as outlined by Brian Goetz, include making data immutable, separating data from behavior, and designing data aggregates with clear, predictable structures. This means focusing on simple, transparent representations of data that are easy to reason about, rather than encapsulating data behind abstractions. Such an approach promotes composition over inheritance, favors structural over nominal typing, and encourages modeling programs around the data they manipulate rather than the actions they perform.

This approach to data modeling also resonates with several principles from Domain-Driven Design. DOP does not contradict DDD or OOP, which means we can integrate it seamlessly into our existing architectures. Ultimately, it empowers developers to write clearer, more maintainable, and more domain-focused code with fewer compromises.