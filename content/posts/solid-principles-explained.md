---
title: "SOLID Principles Explained"
date: 2020-11-25T19:00:00+02:00
description: "Explaining SOLID principles with examples written in Java"
tags: [Java]
---

During the lifespan of software engineering, professionals created a lot of best practices and principles. Some of them were expressed as patterns, and some of them have evolved into abbreviations like DRY (Don't Repeat Yourself). In this article, I will focus on SOLID: a set of 5 principles (mainly applicable to object-oriented programming) for articulating design issues and solutions.

**S** - Single Responsibility Principle  
**O** - Open/Closed Principle  
**L** - Liskov’s Substitution Principle  
**I** - Interface Segregation Principle  
**D** - Dependency Inversion Principle  

These principles are independent, and there are no requirements to go through them in a particular order. I will start from the simplest first.

# Dependency Inversion Principle

Classes should not depend on implementations. Both low and high-level entities should depend on abstractions. This protects our code from changes made to dependencies. As long as those changes keep the public contract (interface), our code continues to behave correctly. Dependency Inversion also makes our code simpler for testing since we can substitute our dependencies with mocks without changing the main code.

Let's have a look at this with an example of an article service that saves it into the database. 

```java
class ArticleService {
	private ArticleMongoDBRepository repository;

	public void persistArticle(Article article) {
        repository.persist(article);
	}
}
```

The problem with this code is that it directly depends on MongoDB implementation of the repository, thus making it impossible to migrate to another type of storage, such as DynamoDB, without having to modify `ArticleService`. A better design is to depend on the abstract `ArticleRepository` so we can modify on the fly our concrete implementation without having to modify `ArticleService`.

# Liskov's Substitution Principle

The application should not blow up if any class is going to be replaced with its subclass. This is a requirement for subclasses which extend some class and should be implemented to be compatible with a parent class. The subclass should behave in a similar way for all operations mentioned in a parent class. It may extend that class by adding more methods but should not break inherited operations.

For example, let's assume that we have an article formatter which creates an HTML string based on an article. 

```java
class ArticleFormatter {
	public String format(Article article) {
		return "<p>"  + article.getBody() + "</p>";
	}
}
```

We would like to create another formatter with some coloring.

```java
class ColoredArticleFormatter extends ArticleFormatter {
	private String color = "black";

	@Override
	public String format(Article article) {
		return "<p style=\"color:" + color + ";\">"  + article.getBody() + "</p>";
	}

	public void setColor(String color) {
		this.color = color;
	}
}
```

It would be safe for an application to replace `ArticleFormatter` with `ColoredArticleFormatter` because semantically `ColoredArticleFormatter` fulfills all functionality that ArticleFormatter declares. But application would definitely break if we replace `ArticleFormatter` with the subclass which does not format the article into HTML string as expected.

```java
class BrokenArticleFormatter extends ArticleFormatter {
	@Override
	public String format(Article article) {
		return "?";
	}	
}
```

# Single Responsibility Principle

As the name suggests, the class should have only one responsibility. It is a well-understood principle that makes the code base less coupled and easier testable.

Back to our formatter example. What if we add one more method to it for fixing spelling?

```java
class ArticleFormatter {
	public String format(Article article) {
		return "<p>"  + article.getBody() + "</p>";
	}

	public String fixSpelling(Article article) {
		String fixedSpelling = // do spelling checks and fix mistakes
		return fixedSpelling;
	}
}
```

Now `ArticleFormatter` has 2 reasons to change: when we are making changes to HTML formatting or introducing new spelling rules. And this violates the Single Responsibility Principle and increases testing complexity. By modifying `ArticleFormatter` class to fix one thing we may break the other. Instead, a new `ArticleSpellChecker` class should be created with the `fixSpelling` method.

# Interface Segregation Principle

Interfaces should be as small as possible, so clients should not be required to implement methods they don't need to. This principle also helps to achieve Liskov's Substitution Principle by ensuring that all subclasses will more likely be capable of substituting parent class due to a scoped functionality.

For example, we have a sharing functionality for article. And we have such interface:

```java
interface Sharable {
	void sendViaEmail(Article article);
	void postOnFacebook(Article article);
	void postOnInstagram(Article article);
}
```

But only entities with an image or video can be posted on Instagram, so not all implementations will implement this method properly. This may lead to a misbehaving application when some client code will attempt to invoke the `postOnInstagram` method on a plain text article that does not support it.

Interface Segregation Principle suggests that we should have 3 different interfaces so our classes can decide how many of them they will implement: `EmailSharable`, `FacebookSharable`, or `InstagramSharable`.

# Open/Closed Principle

Classes should be open to extension but closed to modification. A good design will allow implementing changes by class extension. In that way, we don't have to modify the underlying class risking introducing bugs. The new class will be used by all newly added code that requires modification.

If we would like image support for our `ArticleFormatter` class, we can do it by extension:

```java
class ArticleFormatterWithImage extends ArticleFormatter {
	private Image image;
	
	@Override
	public String format(Article article) {
		return formatImage(image) + "<p>"  + article.getBody() + "</p>";
	}

	public void setImage(Image image) {
		this.image = image;
	}
}
```

This shows that `ArticleFormatter` is well designed to not require direct modifications. All additional features can be implemented by extending it.

SOLID should not be followed blindly. Please reason about your code and decide what would be the best solution for your case, keeping SOLID in mind.