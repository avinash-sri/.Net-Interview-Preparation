# OOPS (Object-Oriented Programming)

## OOPS (Object-Oriented Programming)

The main aim of OOP is to bind together the data and the functions that operate on them so that no other part of the code can access this data except that function.

Object-Oriented Programming is a methodology or paradigm to design a program using classes and objects.

### Class

It is a user-defined data type, which holds its own data members and member functions, which can be accessed by and used by creating an instance of that class.

A class is like a blueprint for an object.

### Object

It is an instance of the class, when a class is defined, no memory is allocated but when it is instantiated (i.e., an object is created) memory is allocated.

### Encapsulation

It is the process of combining data and functions into a single unit called class.

In Encapsulation, the data is not accessed directly, it is accessed through the functions present inside the class.

In simpler words, attributes of the class are kept private and public getter and setter methods are provided to manipulate these attributes. Thus, encapsulation makes the concept of data hiding possible.

### Abstraction

Abstraction means displaying only essential information and hiding the details.

Data abstraction refers to providing only essential information about the data to the outside world, hiding the background details or implementation.

### Polymorphism

The word polymorphism means having many forms. In simpler words, we can define polymorphism as the ability of a message to be displayed in more than one form.

There are 2 types of polymorphism:

1. Compile Time Polymorphism (Static)
2. Runtime Polymorphism (Dynamic)

### Compile Time Polymorphism

The polymorphism which is implemented at the compile time is known as compile-time polymorphism.

**Example:** Function overloading

### Function Overloading

It is a feature in which allows you to have more than one function with the same function name but with different functionality.

### Runtime Polymorphism

The polymorphism which is implemented at the runtime is known as runtime polymorphism, also known as dynamic polymorphism.

**Example:** Function overriding

### Function Overriding

Function overriding means when the child class contains the method which is already present in the parent (base) class. Hence, the child class overrides the method of the parent class.

In case of function overriding, parent and child classes both contain the same function with a different definition.

---

## Constructor

Constructor is a special method which is invoked automatically at the time of object creation.

It is used to initialize the data members of new objects generally.

The constructor has the same name as the class name.

It can either accept the arguments or not.

### Types of Constructor

1. **Default Constructor** - A constructor which has no argument is known as default constructor. It is invoked at the time of creating an object.
2. **Parameterized Constructor** - Constructor which has parameters is called a parameterized constructor. It is used to initialize objects with a different set of values.
3. **Copy Constructor** - A copy constructor is an overloaded constructor used to declare and initialize an object from another object.

---

## Destructor

A destructor works opposite to the constructor. It removes and destroys the memory of an object, which constructor allocated during the creation of an object.

It also has the same name as the class name preceded by a tilde (`~`) operator.

It does not have any parameters.

In a class, there is always a single destructor.

---

## Inheritance

The capability of a class to derive properties and characteristics from another class is called inheritance.

The class which inherits the members of another class is called derived class and the class whose members are inherited is called base class.

### C++ Syntax

```cpp
class derived-class: visibility-mode base-class
```

Inheritance supports the concept of "reusability", i.e., when we want to create a new class and there is already a class that includes some of the code that we want, we can derive our new class from the existing class.

### Sub Class

The class that inherits properties from another class is called sub class or derived class.

### Super Class

The class whose properties are inherited by sub class is called base class or super class.

---

## Access Modifiers

Access modifiers or Access specifiers in a class are used to assign the accessibility to the class members.

That is, it sets some restrictions on the class members not to get directly accessed by the outside functions.

There are 3 types of access modifiers:

1. Public
2. Private
3. Protected

### Note

If we do not specify any access modifiers for the members inside the class then by default the access modifier for the members will be private.

---

# Indexing

An index in a database is a data structure that improves the speed of data retrieval operations on database table.

It functions similar to an index in a book, helping you quickly locate specific information without reading the entire content.

In databases, indexes are used to optimize query performance by reducing the amount of data that needs to be scanned or searched.

## Clustered Index

A clustered index is a type of database index that defines the physical order of data rows in a table based on the values of one or more columns.

A table can have only one clustered index.

### Syntax

```sql
CREATE CLUSTERED INDEX INDEX_NAME
ON TableName (column_name);
```

## Non-clustered Index

A non-clustered index is a type of database index that improves the performance of data retrieval operations by creating a separate data structure that maps index key values to the corresponding row identifiers in a table.

A table can have more than one non-clustered index.

Since the non-clustered index is stored separately from the table, additional storage space is required.

### Syntax

```sql
CREATE NONCLUSTERED INDEX Index_Name
ON TableName (column_name);
```
