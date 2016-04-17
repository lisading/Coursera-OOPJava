# Week 3 Inheritance

##### Those contents are learning notes from Coursera [Object Oriented Programming in Java](https://www.coursera.org/learn/object-oriented-java), offered by UC San Diego.   
##### Instructors: Mia Minners, Leo Porter, Christine Alvarado.    
##### Author: Lisa Ding

<br>

This weeks' lecture mainly discusses **Inheritance** and **Polymorphism**. 

### 1. Inheritance in Java

What is inherited?  
- public instance variables  
- public methods  
- private instance variables  
  
 **Reference**   ————  **Object**   
 Person p =         new Person( );  
 Student s =        new Student( );  

`Person p = new Student( );`   √   
A **Student** “is-a” **Person**

`Student s = new Person( );`   ×

`Person[ ] p = new Person[3]`  
`p[0] = new Person( );`  
`p[1] = new Student( );`  
`p[2] = new Faculty( );`   √   
A **Person** array CAN store **Student** and **Faculty** objects

### 2. Visibility modifiers
Use appropriate visibility modifiers when writing classes.

- public: can access from **any class**
- protected: can access from **same class**, **same package**, **any subclass**
- package: can access from **same class**, **same package**(Lose access by any subclass)
- private: can access from **same class**

**Rule of thumb 1**: Make member variables private 
(and methods either public or private)

**Rule of thumb 2**: Always use either public or private

### 3. Object Construction in Java

All classes inherit from Object class;
Java object construction occurs **from the inside out**.

	Student s = new Student( );
	
**new** is an operator that allocates space;  
**this** is passed to the constructor **Student( )**;  

Objects are created from the **inside out**

Student —\> Person —\> Object  
(Subclass) —\> (Superclass) —\> (Indirect Superclass)

The compiler change the code:  
Your code —\> Java Compiler —\> Bytecode  
Your code: Human readable Java  
Java Compiler: processes code and inserts new commands  
Bytecode: runs on JVM  

An example:
	public class Person
	{
	   private String name;
	}

The compiler gets the code, and will **change the code** in 3 steps:

#### **Compiler Rules**

##### Rule #1: No superclass? Compiler inserts: **extends Object**

	public class Person extends Object
	   //see how Person inherit from Object
	{
	   private String name;
	}

##### Rule #2: No constructor? Java gives one for you.

	public class Person extends Object  
	{
	   private String name;
	   public Person( ){ 
	      //Java gives you a default constructor with no arguments
	   }
	}

##### Rule #3: 1st Line must be:  
**this(args)**       //call other constructor in the same class    
**Or** 
**super(args)**      //or call constructors from super class    
**Otherwise, Java inserts:**    
**“super( ); ”**     //otherwise, call the default constructor of the super     

```
class
	public class Person extends Object  
	{
	   private String name;
	   public Person( ){ 
	         super( );
	   }
	}	
```

- Exercise:

```
	public class Student extends Person
	{
	}
```
—\>


	public class Student extends Person
	{
		public Student( )
		{
			super( );
		}
	}


### 4. Initialising Variables in a Class Hierarchy

Use same-class and super class constructors in class creation

In the last chapter there remains a question: But how do we initialise `name`? 

- Code from last chapter:

```
	public class Person extends Object  
	{
	   private String name;
	   public Person( ){ 
	         super( );
	   }
	}
```

- Initialise `name` variable:

```
	public class Person extends Object  
	{
	   private String name;
	   public Person( String n ){ 
	         super( );   // super( ) has to be the first line
	         this.name = n;
	   }
	}
```

Another Example:

A False one:

```
	public class Student extends Person
	{
		public Student( String n )
		{
			super( );
	               this.name = n;  
	               // False! name is a provate variable from the Person Class
	               //How to initialise name without a public setter?
		}
	}
```

 A Correct one:
 
 
	public class Student extends Person
	{
		public Student( String n )
		{
			super( n );
		}
	}

Let’s add a no-arg constructor:

	public class Student extends Person
	{
		public Student( String n )
		{
			super( n );
		}
	}
	 
	public Student( ) {
	    {
	    super("Student");
	    //Use a default value for name
	    //No-arg constructor
	    }
	}

A better way to do this:

	public class Student extends Person
	{
		public Student( String n )
		{
			super( n );
		}
	}
	 
	public Student( ) {
	    {
	    this("Student");
	    //Use our same class constructor
	    }
	}

### 5. Method Overriding

- `Overloading`: **Same class** has same method name with **different** parameters.
- `Overriding`: **Subclass** has same method name with the **same parameters** as the superclass

Object class:   
Modifier and Type: `String`;   
Method and Description: `toString( )`:    
`toString( )`: Returns a string representation of the object   

1.

	public class Person{
	   private String name;
	   
	    public String toString( ){
	        return this.getName( );
	    }
	}
	
2.

	//assume ctor
	Person p = new Person("Tim");
	System.out.println( p.toString( ) );  
	//Calls Person's toString( ) instead of object's method because of overriding
	// .toString( ) there is unnecessary beacuse println automatically calls toString( )
	//So the code can be: System.out.println( p );
	
3.

	public class Student extends Person{
	   private int studentID;
	
	   public int getSID( ){
	        return studentID;
	    }
	public String toString( ){
	    return this.getSID( ) + ": " + super.getName( );
	    }
	}

4. 

	//assume ctor
	Student s = new Student ("Cara", 1234);
	System.out.println( s );    //Calls Student toString( )
	//Output: 1234: Cara
	
5.

	//assume ctor
	Person s = new Student ("Cara", 1234);
	System.out.println( s );    
	//Output: 1234: Cara  -- Because of Polymorphism!

### 6. Polymorphism: Introduction

Polymorphism: Superclass reference to subclass object.   
Polymorphism: The appropriate method gets called.

	Person s = new Student ("Cara", 1234);

	//assume appropriate ctors
	Person p [ ] = new Person [3];
	p [0] = new Person ("Tim");
	p[1] = new Student("Cara", 1234);
	p[2] = new Faculty("Mia", "ABCD")
	
	for(int i = 0; i < p.length; i++)
	{
	    System.out.println( p[i] );    //toString( ) method depends on dynamic type
	}


### 7. Polymorphism: Compile Time vs. Runtime

##### Two steps:   
Step 1: Compiler interprets code   
Step 2: Runtime environment executes interpreted code   

##### Compile Time Rules:
1. Compiler ONLY knows reference type   
2. Can only look in reference type class for method   
3. Outputs a method signature    

##### Run Time Rules:   
1. Follow exact runtime type of object to find method   
2. Must match compile time method signature to appropriate method in actual object’s class    

### 8. Polymorphism: Casting

##### Casting

- Automatic type promotion 自动类型转换 (like `int` to `double`)
	- Superclass ref = new Subclass( ) widening

- Explicit casting 强制类型转换 (like `double` to `int`)
	- Subclass ref = (Subclass) superRef;  narrowing

	Person s = new Student (“Cara”, 1234);
	( (Student)s ).getSID( );

##### Runtime type check

`instanceof`: Provides runtime check of is-a relationship

	if ( s instanceof Student)
	{
	   //only executes if s is-a Student at runtime
	  ( (Student)s ).getSID( );
	}


### 9. Abstract Classes and Interfaces

Use the keyword `abstract`

- Can make any class abstract with keyword abstract:

```
	public abstract class Person{
	//Cannot create objects of this type
```
- Class **must** be abstract if any methods are:

```
	public abstract void monthlyStatement( ){
	//Concrete subclasses must override this method
```

Abstract classes offer inheritance of both!

- **Implementation**: instance variables and methods while define common behaviours   
- **Interface**: method signatures which define required behaviours   













