---
title: "Single Responsibility Principle"
date: 2019-02-22T10:25:49Z
weight: 2
draft: false
---


Single Responsibility Principle (SRP) is about how we separate or modularise code in object oriented design. This principle says that **a class should have only one reason to change**. 

If we naively apply this principle, we will end up with several classes where each class contains just one method that just does one small thing. This leads to unnecessary complexity. This is clearly not what we want to achieve with this principle.

Robert C. Martin who introduced this principle clarified that this principle is "about people". Any changes to a class should originate from only single person or a group of people representing a department or business division or similar within the company/organisation that uses the application.

If changes to a class is originating from more than one person/department/division then the class is said to have multiple responsibilities and it violates single responsibility principle.

For example, say you have a class `Student` that calculates grade for students and also tracks the books they borrowed from library. 

``` csharp

public class Student
{
    public string CalculateGrade()
    {
        // calculate and return grade
    }

    public void BorrowBook(int bookId)
    {
        // record the book borrowed
    }

    public void ReturnBook(int bookId)
    {
        // record that the book is returned
    }
}

```

Any changes to this class can be requested by one of two departments

* teaching department may ask to change the way we calculate grading
* library department may ask for changes to the way we record borrowed and returned books

So, two different departments are responsible for this class. In other words, this class fulfils different requirements of two different departments.

{{%notice info %}}
In the context of this principle, we can think of responsibility as "reason to change". What we perceive as responsibility or reason to change may be different based on the domain and the problem we are solving.
{{%/notice%}}

If you are wondering why having a class that changes for more than one reason is not a good idea, then say library department asks for some changes to the way books are borrowed. While making the changes for library department, there is very high risk of breaking `CalculateGrade()` method.

This is because we don't know how the code is intertwined within the class. 

Even if you are very careful and made sure the functionality of the `CalculateGrade()` method is unaffected, at the very least you need to ask teaching department to test grade calculation and this is not ideal.

So, separating **unrelated** methods into different classes is a good idea, but at the same time, we need to make sure we don't end up with atomised classes.

Robert C. Martin also provided the following complementing definition to clear some of the misinterpretation of this principle

> **Gather together the things that change for the same reasons and separate those things that change for different reason**

This complementing definition sort of says that we need to increase cohesion and decrease coupling of classes. This will help us to avoid atomised classes.

{{% notice note %}}
In the context of a class, **cohesion** means the methods and properties must be closed related and **coupling** means the level of interdependencies between classes.
{{% /notice %}}


With all these in mind, we can refactor the `Student` class as follow

``` csharp

public class StudentGrade
{
    public string CalculateGrade()
    {
        // calculate and return grade
    }
}

```

``` csharp
public class StudentLibraryTracker
{
        public void BorrowBook(int bookId)
    {
        // record the book borrowed
    }

    public void ReturnBook(int bookId)
    {
        // record that the book is returned
    }
}
```

Now, `StudentGrade` class only fulfils the requirements of teaching department so it will only change for the requests from teaching department and `StudentLibraryTracker` class only fulfils the requirements of library department and any changes needed will only originate from library department.

Note here, we have not split the original class into three different class each with one method. We have `StudentLibraryTracker` class with two methods in it and `StudentGrade` class with one method. 

This is because we have taken the complementary principle into account and **gathered `BorrowBook()` and `ReturnBook()` into one class because they changes for the same reason and separated `CalculateGrade()` to different class because it is not changing for the same reason as the other two methods**.


### References
1. https://en.wikipedia.org/wiki/Single_responsibility_principle

2. https://8thlight.com/blog/uncle-bob/2014/05/08/SingleReponsibilityPrinciple.html

3. https://hackernoon.com/you-dont-understand-the-single-responsibility-principle-abfdd005b137

4. http://www.butunclebob.com/ArticleS.UncleBob.PrinciplesOfOod