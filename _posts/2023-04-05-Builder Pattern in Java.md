---
layout:     post
title:      "Builder Pattern in Java"
subtitle:   "Design Pattern"
date:       2023-04-05 08:00:00
author:     "kap"
header-img: ""
tags:
- Design Pattern
---
# Business Examples Using Builder Pattern

The Builder pattern is a design pattern used in software development to separate the construction of a complex object from its representation. It is particularly useful when the construction process involves multiple steps or when the object's configuration can vary. Here are some business examples that often use the Builder pattern:

## 1. Report Generation Systems

In a business intelligence or analytics software, generating complex reports with varying data sources, filters, and display formats is a common use case for the Builder pattern. The report builder can construct different types of reports based on user specifications, while maintaining a clean separation between the report construction process and the report object itself.

## 2. E-commerce Platforms

In e-commerce systems, building complex product configurations (such as customizable PCs or furniture sets) often requires a step-by-step process that collects user preferences before finalizing the product. The Builder pattern can help manage this process, allowing for easy modification or extension of product attributes and configurations.

## 3. Customer Relationship Management (CRM) Systems

CRM systems may use the Builder pattern to construct detailed customer profiles, which can include various data points such as contact information, purchase history, and preferences. By using a builder, the system can create customer profiles with different levels of detail based on specific use cases or user roles.

## 4. Restaurant Management Systems

A restaurant management system may use the Builder pattern to create customized meal plans or recipes. For example, the system could offer a meal builder that allows users to select ingredients, portion sizes, and cooking methods, resulting in a custom meal plan or recipe tailored to specific dietary needs or preferences.

## 5. Document Processing Systems

In a document processing system, the Builder pattern can be used to generate complex documents, such as contracts or proposals, by combining various pre-defined templates and user inputs. The builder helps create documents with different structures and content based on the specific requirements of each use case.


# Builder Pattern in Java

## Overview

The Builder pattern is a popular design pattern in Java programming that simplifies the process of creating complex objects by breaking their construction into smaller, more manageable steps. It falls under the category of creational design patterns and is particularly useful when constructing objects with many optional parameters.

The primary goal of the Builder pattern is to separate the construction of an object from its representation, allowing for the creation of different representations using the same construction process.



## Components

The Builder pattern typically consists of the following components:

1. **Product**: The complex object being constructed.
2. **Builder**: An interface that defines the methods for constructing the various parts of the Product.
3. **Concrete Builder**: A class that implements the Builder interface and provides the actual construction process for the Product.
4. **Director**: A class that controls the construction process, using the Builder interface to assemble the Product.

## Advantages

- Encapsulates the construction process, making it easier to manage and maintain.
- Allows for the creation of different representations using the same construction process.
- Provides better control over the construction process, especially when dealing with optional parameters.

## Example

Consider a scenario where we need to create a `House` object with multiple optional parameters such as `walls`, `doors`, `windows`, `roof`, and `garden`.

```java
// Product
class House {
    private String walls;
    private String doors;
    private String windows;
    private String roof;
    private String garden;

    // Getters and setters
    // ...
}

// Builder
interface HouseBuilder {
    void buildWalls(String walls);
    void buildDoors(String doors);
    void buildWindows(String windows);
    void buildRoof(String roof);
    void buildGarden(String garden);
    House getHouse();
}

// Concrete Builder
class ConcreteHouseBuilder implements HouseBuilder {
    private House house;

    public ConcreteHouseBuilder() {
        this.house = new House();
    }

    @Override
    public void buildWalls(String walls) {
        house.setWalls(walls);
    }

    @Override
    public void buildDoors(String doors) {
        house.setDoors(doors);
    }

    @Override
    public void buildWindows(String windows) {
        house.setWindows(windows);
    }

    @Override
    public void buildRoof(String roof) {
        house.setRoof(roof);
    }

    @Override
    public void buildGarden(String garden) {
        house.setGarden(garden);
    }

    @Override
    public House getHouse() {
        return house;
    }
}

// Director
class HouseDirector {
    private HouseBuilder builder;

    public HouseDirector(HouseBuilder builder) {
        this.builder = builder;
    }

    public void constructHouse() {
        builder.buildWalls("Brick walls");
        builder.buildDoors("Wooden doors");
        builder.buildWindows("Double-glazed windows");
        builder.buildRoof("Slate roof");
        builder.buildGarden("Green lawn");
    }

    public House getHouse() {
        return builder.getHouse();
    }
}

// Client code
public class Main {
    public static void main(String[] args) {
        HouseBuilder builder = new ConcreteHouseBuilder();
        HouseDirector director = new HouseDirector(builder);
        director.constructHouse();
        House house = director.getHouse();

        System.out.println("House built with: " + house.getWalls() + ", " + house.getDoors() + ", " + house.getWindows() + ", " + house.getRoof() + ", " + house.getGarden());
    }
}
```


# Builder Pattern with Inner Class in Java

## Overview

Using an inner class for the Builder pattern is a common approach in Java programming. It allows for better encapsulation and helps to keep the code more organized. The inner class Builder is defined within the class of the complex object being constructed, making it easier to access its members and maintain the relationship between the Product and Builder.

## Example

Consider the same `House` object scenario, but this time we'll use an inner class for the Builder.

```java
// Product with Inner Class Builder
class House {
    private String walls;
    private String doors;
    private String windows;
    private String roof;
    private String garden;

    // Getters
    // ...

    // Constructor is private, forcing use of the Builder
    private House(Builder builder) {
        this.walls = builder.walls;
        this.doors = builder.doors;
        this.windows = builder.windows;
        this.roof = builder.roof;
        this.garden = builder.garden;
    }

    // Inner Class Builder
    public static class Builder {
        private String walls;
        private String doors;
        private String windows;
        private String roof;
        private String garden;

        public Builder walls(String walls) {
            this.walls = walls;
            return this;
        }

        public Builder doors(String doors) {
            this.doors = doors;
            return this;
        }

        public Builder windows(String windows) {
            this.windows = windows;
            return this;
        }

        public Builder roof(String roof) {
            this.roof = roof;
            return this;
        }

        public Builder garden(String garden) {
            this.garden = garden;
            return this;
        }

        public House build() {
            return new House(this);
        }
    }

        // toString() method
    @Override
    public String toString() {
        return "House{" +
                "walls='" + walls + '\'' +
                ", doors='" + doors + '\'' +
                ", windows='" + windows + '\'' +
                ", roof='" + roof + '\'' +
                ", garden='" + garden + '\'' +
                '}';
    }
}

// Client code
public class Main {
    public static void main(String[] args) {
        House house = new House.Builder()
            .walls("Brick walls")
            .doors("Wooden doors")
            .windows("Double-glazed windows")
            .roof("Slate roof")
            .garden("Green lawn")
            .build();

        System.out.println("House built with: " + house.getWalls() + ", " + house.getDoors() + ", " + house.getWindows() + ", " + house.getRoof() + ", " + house.getGarden());
    }
}
```

# Why the Inner Class Must Be Static in the Builder Pattern

When implementing the Builder pattern with an inner class, it's important to make the inner `Builder` class static. There are several reasons for this:

## 1. Independence from the Outer Class Instance

If the inner `Builder` class is non-static, it would require an instance of the outer class (in this case, `House`) to be created before you can create an instance of the `Builder`. This would defeat the purpose of the Builder pattern, which aims to simplify and control the construction of complex objects.

By making the inner `Builder` class static, it becomes independent of the outer class instance, allowing you to create a `Builder` instance without creating an instance of the outer class first. This is crucial for the Builder pattern to function correctly, as the main goal is to construct the outer class object through the `Builder`.

## 2. Encapsulation

Making the inner `Builder` class static ensures that the `Builder` is fully encapsulated within the context of the outer class. This helps to keep the code organized and maintain the relationship between the `Builder` and the outer class while preventing unintended access or modifications to the outer class instance.

## 3. Memory Efficiency

Using a static inner class for the `Builder` can lead to more efficient memory usage. Static inner classes do not hold an implicit reference to the outer class instance, so they do not add any additional memory overhead when instantiated. Non-static inner classes, on the other hand, hold a reference to their outer class instances, which could lead to unnecessary memory consumption if multiple `Builder` instances are created.

In summary, making the inner `Builder` class static in the Builder pattern is essential for maintaining independence from the outer class instance, ensuring proper encapsulation, and improving memory efficiency.


......

***life is not easy，still working smart***