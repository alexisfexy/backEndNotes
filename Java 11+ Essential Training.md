# Java 11+ Essential Training

## Basic Java Syntax

```java
package HELLOWORLD; // package: tells you where the file is stored in terms of directory location  

public class Main {
  // this is the Main method: in non object oriented languages you would call it a function
  // public: access modifier
  // static: you dont need to create an instance of the main class to run it
  // void: doesnt return anything
  // nameofmethod(parameters)
  // main method: starts AUTOMATICALLY when you start up the class -- always recieves an array of string values 
  public static void main(String[] args) {
        System.out.println("Hello World");
    }
}
```



- Java is CASE SENSITIVE 

  - but NOT sensitive to white space

- All statments must end in a semicolon 

  - semicolons are not nee

- Conventions: 

  - Class, method, feld, and other names are identifiers

  - Identifiers must start with alpha character or underscore (cannot start with a number)

  - Keywords cant be an identifier either (duh)

  - **Identifier Conventions:**

    - Classes start with uppercase character

      - ```JAVA
        public class MyClass
        ```

      - 

    - Methods & Variables alwasy start with a lowercase character

    - Constants are all uppercase

      - ```java
        public static final String FIRSTNAME = "David";
          // public static final is basically the same as saying "const"
        ```

  - **Code Blocks**:

    - Defined by pairs of braces

      - ```java
        { a code block }
        ```

    - Method implementations:

      - ```java
        void doSomething(String withThis) { a code block} 
        ```

    - Loops:

      - ```java
        for (int i=0; i < 10; i++) { a code block}
        ```

## Memory Manamgent

- Memory for objects allocated *automatically*
- Local variables + function calls are in the stack
- Object + member variables are stored in heap 
- Objects are retained in memory unitl dereferenced 

- Garbage Collection (automatic)

  - allocated + deallocated meory
  - destroy dereferenced object 
  - managed by the virutual machine 

- **Minimizing Memory**

  - minimize the number of objects you create

  - system methods to report memory usage:

    - ```java
      Runtime.maxMemory()
      Runtime.totalMemory()
      ```



<u>Major IDEs used today:</u> 

- NetBeans
- Eclipse
- Intellij IDEA (free vs. paid ultimate)



<u>Jshell Basics:</u> 

/list: listing of current cold

/save: save it to text fiel 

/reset

/exit 

.class = bytecode file , .java = text file 



## Declare + Manage Variables:

statically typed language- the type is set right when declared 

**Primative Data Types**

- numbers, characters (not full strings), booleans

- can contain a single value-- not objects (so no members, or fields)

- primitive type names are all lowercase:

  - ```java
    int, 
    char,
    short, long, // for inte
    double, float, // for floating values
    boolean
    ```

- a String is an OBJECT and is NOT a primitive (notice it is not all lowercase). It is an instance of a class (see below)

- **Delcaring Primitive Variables:**

  - Explicit type delcarations:

  - ```java
    int myVar = 5; // datatype identifier assignment value
    ```

  - Inferred Data Types (starting in Java 10)

    - only works on local variables within methods:

      - ```java
        var myVar = 5;
        // keyword identifier (variablename) assignment value
        
        // in this case we infer int 
        ```

    - Explicit Types in Literal Values:

      - Types can be set in numeric literals with alpha notation 

      - force teh value to haev a particular type

      - ```java
        var myInt = 5;
        var myFloat = 5f;
        var myDouble = 5d;
        var myLong = 5L; // uppercase L because it distinguishes it from the number 1 versus l 
        ```

![image-20200807164200121](/Users/alexisfexy/Library/Application Support/typora-user-images/image-20200807164200121.png)

- double is usually used for currency  

**Primitive Wrapper Classes**

- Class library includes wrapper classes for each primitive
- Conversion and formatting of numeric values  

![image-20200807164557830](/Users/alexisfexy/Library/Application Support/typora-user-images/image-20200807164557830.png)

They do a lot of the work for you!!

```java
String doubleValue = "156.5";
// want to turn the string into a true numeric 
// use class Double 
Double doubleObj = Double.parseDouble(doubleValue);
// parseDouble is a static method: static means you call the method from the class itself (not from an instance of the class)
// returns it as a double. Once you have this double, you can use it to convert to other types --- the double object (the instance of the Double class) has member methods (byteValue, intValue, floatValue, toString)
// notice in this case: we are using inferred datatyping
var byteValue = doubleObj.byteValue(); 
var intValue = doubleObj.intValue();
```



**Unsigned Integers **

- Primitive Integers are alwasy signed (positive & negative values are supported)

- Long and integer wrapper classes have methods to support unsigned operations:

  - ```java
    var unsigned = Integer.parseUnsignedInt("300");
    System.out.println("Unsigned value is " + unsigned);
    var result = Integer.divideUnsigned(unsigned,2);
    System.out.println(
    "The result is" + result)
    ```



```java
// Example Explicit Typing:

byte 1 = 1;
short sh = 1;
int sh = 1;

// all contain same value but take up different amounts of memory: 
// byte < short < int in terms of memory


// Example Inferred Typing: 
var v = 1;
var longValue = 3_000_000_000L;
var floatValue = 30000000.333442f; 

System.out.println(variableName);

// the wrapper classes for all the primitives have constant values for the Max Value.

byte b = 127; 
if (b < Byte.MAX_VALUE) {
  	b++;
}

```





**Object Variables **

If not a primitive, then it is an object!

When you are writing in java you are creating *classes:* a class is essentially a temoplate for creating objects 

- <u>An object is an instance of a class</u>
- Classes can define fields (data values) & methods (actions)
- When you create an object it has the fields & methods!
- <u>**Field definition:**</u> variable that is a *member* of a class, after the calss delcaration 
- Local Variable: variable defined inside a method 



*Declaring a variable:*

Explicit or Inferring

```java
ClothingItem item; // not intializing it
// the default value will be NULL 


// TRADITIONAL INITIALIZATION SYNTAX: 
ClothingItem item = new clothingItem();
// constructuor method & class name are always the SAME 
// new creates an instance of the object
// explicit data typing & still required in some contexts 



// INFERRED TYPING: Type Inference 
Var item = new clothingItem();
// same result as explicit typing but only works for local variables inside methods or in Jshell

```

![image-20200807181915894](/Users/alexisfexy/Library/Application Support/typora-user-images/image-20200807181915894.png)



Objects have access to members 

- after initializing you can set data values & call methods

  ``` java
  var item = new ClothingItem();
  item.setType("Hat");
  item.displayItem();
  ```

  

**Manage Currency with BigDecimial**

```java
double value = .012;
double psum = value + value + value; // this becomes .03600000000000000004

// when you are dealing with currency this becomes a problem because adding enough values together gets you are seeing some rouding errors

// BigDecimal Class:
// BigInteger & BigDecimal -- big decimal is best for 
// convert number to string, then to a big decimal objec

var stringValue = Double.toString(value);
var bigValue = new BigDecimal(stringValue)
  // creating an object with new & calling the constructor 
 
 
var bigSum = bigValue.add(bigValue).add(bigValue); // calling add methond
  // bigSum = 0.036
  
// turing back into primitive
  
 var sum = bigSum.doubleValue();

```



**Convert values between numeric types:**

- more precises to less precise: widening

```java
short sh = 100; 
int i = sh;
// int takes up more memory than the short
  // widening so you dont need any special syntax

long longValue = i;
short shortValue = longValue; // you will get an ERROR
// "lossy" conversion because you might lose some of your data 

short shortValue = (short) longValue;
// syntax acknowledging that you know you could lose some data 
// sometimes caleld "casting syntax": 
// casting is usually when you have obejcts that are instances of classes 
// CONVERSION is the best term. Making a copy of the value, setting the new variable's type explicitly 
```



**Math operators using math class** 

```java
double doubleValue = -3.99999;
long rounded = Math.round(doubleValue);
```



**Managing True and False**

- not 1 & 0 in Java. It is literally only true & false

  ```java
  boolean aValue = true;
  var b1 = true;
  // will default to false if you don't 
  boolean b3; // b3 = false
  
  // can switch it by negating
  var b4 = !b1 
    
  var i = 0; 
  var b5 = (i != 0) // boolean expression
  
    
  var s = "true";
  var b6 = Boolean.parseBoolean(s);
  ```

  

**Characters as primitives**

```java
char c1 = 'a';
// single quotes for characters, double quotes for strings 
var c2 = '2';
char c3 = '3'
  
// can also use unicode: 
char dollar = '\u0024'
  
var upper = Character.toUpperCase(c1); 

char[] chars = {'h', 'e', 'y'};
String s = new String(chars)
var charArray = s.toCharArray();
```



**Operators:**

- note: for % (modulo) operator you will alwasy get an integer back 

- ++, --, +=

  - *Postfix v. Prefix Incrementing:* 
    - Postfix: `System.out.println(intValue ++)`
      - Output: 10, New value: 11 -- changes the value AFTER you output it 
    - Prefix: `System.out.println(++ inValue)`
      - Output: 10, New value: 11

- != sometimes called "bang"

- instanceof: is this an instance of the class 

- equality of strings: 

  - do not use the == instead do the equals method 

    ```java
    String s1 = "Hello";
    String s2 = "Hello";
    if (s1.equals(s2)) {
      System.out.println("They Match");
    }
    else {
      System.out.println("No Match!")
    }
    ```

- Logical Operators: 

  - && AND

  - || OR

  - ?= Ternary (shorthand if-then) 

    - ```java
      var message = (i == 1)
        ? "There is 1" // result is true
        : "There are " + i; // result is false
      System.out.println(message)
        
      ```

## Manage String Values

String has a reference to an array of characters (as shown above)

```java
String s1 = "this is a string";
// double quotes!!

var s3 = new String("string here");
var chars = s1.toCharArray();
// char[17] {'t', 'h', 'i', 's'}

// concactination: + operator!
String s4 = "Shirt size: "; 
String s4 += "medium"; 

// strings are imputable. When you append or replace a strings value you are really just discarding it and making an new one in memory. You want to reduce the number of objects you are making so there are better methods for this (see below )

// remeber string is an OBJECT so we can use their methods!

var upper = s6.toUppercase();
var charAt = s6.charAt(4);
var bytes = s6.getBytes() // review this 
```



To create complex strings from scratch use the **builder class**:

- a builder is a software pattern to use to create or build an object
- first: creat the builder itself, then call series of methods to create target object

```java
var sb = new StringBuilder("Welcome");
sb.append(" to California");
// to turn it into an actual string you need to use the string builders toString method:
var s = sb.toString();

// can all append and other methods multiple times & can chain them together

StringBuilder b = new StringBuilder();
b.append("Shirt size: ").append("M").append("Qty: 4"); 

var s = b.toString(); 
System.out.println(s);
```



Note: when you are converting primitive values to strings - as long as you have at least one string in the + then it will convert the rest to a string 

``` java
// example: 
String howMany = 20 + " things"; 
System.out.println(howMany);

int intValue = 10; 
var fromInt = Integer.toString(intValue)
  
boolean boolValue = true; 
var fromBool = Boolean.toString(boolvalue)
```



Format numeric values as strings:

You can use a set of formatter classes:

```java
package com.company; 

import java.text.NumberFormat;
import java.util.Locale;

public static void main(String[] args){
  var doubleValue = 10_000_000.53; 
  
  var numF = NumberFormat.getNumberInstance();
  System.out.println("Number " + numF.format(doubleValue))
    
    // force number to interger: rounds value down to whole number before processing or displaying it
	var intF = NumberFromat.getIntegerInstance();
  System.out.println("Number " + intF.format(doubleValue))
    // command D to copy code on IntelliJ
  
  intF.setGroupingUsed(false); // if you want to suppress the separators between thousand groupings (the commas) 
   System.out.println("Number " + intF.format(doubleValue))
     
  // control Locale: an object 
     var locale = Locale.getDefault();
     var localeFormatter = NumberFormat.getNumberInstance(locale);
  System.out.println("Number: "+ localFormatter.format(doubleValue)); 
 
  var locale = new Locale(language: "de", country: "DE"); // format using the language for Germany
  var currencyFormatter = NumberFormat.getCurrentyInstance(locale);
  System.out.println(currencyFormatter.format(doubleValue));
  
  // decimal format
  var df = new DecimalFormat("$00.00");
  System.out.println(df.format(1));
    
}



```

String & wrapper classes do not need to be explicitly imported. But all other classes need to be imported 



**String Interpolate wiht Placeholders**

Using format!

```java
var item = "Shirt";
var size = "Medium";
var price = 14.99; 
var color = "Red";

var template = "Clothing item: %s, size %s, color %s, $%.2f";
var itemString = String.format(template, item, size, color, price); 
System.out.println(itemString); 


  // %s for string, %d for integers, %f for floating values (%.2f means truncate to 2 decimal places)

```



**Compare Strings to Eachother**

Note: == asks if they point to the same place in memory. This is NOT what you want when comparing strings which is why you use .equals

```java
String s1 = "Hello!"
var s2 = "Hello!"
  
if (s1 == s2) {
  System.out.println("Matched");
}
else {
   System.out.println("Nope");
}

String s3 = new String("Hello")
  String s4 = new String("Hello")
  // THESE WILL NOT MATCH
  // Strings are objects, and refer to stuff in memory. In the first example, if you leave it up to Java, it will see if it is already in memory, and then just point to it. It will make the new variable a reverence to that object instead of creating a brand new one.  (this is called interning -- you can force it to ahppen by calling the intern method from the string class)
  
  // the double equal sign asks if 2 variables reerence the same object rather than comparing the variables. This is why instead you should use .equals methods for all string comparisons. You can also use other comparsion methods:
  
  var s5 = "HELLO!"; 
	s3.equalsIgnoreCase(s5) // would be true
  s3.equals(s5) // false
  
```



**Parse String Values**

```java
var s1 = "Welcome to California"; 
int l = s1.length();
int positon = s1.indexOf("California")
var sub = s1.substring(11); 

String s2 = "Welcome!     ";
var len = s2.length(); // 14

// remove whitespace from beginnign or end of string 
var trimmed = s2.trim(); 

```



**Get string from user input**

Using a class named Scanner

```java 
package com.company;
import java.util.Scanner

public class Main {
  public static void main(String[] args){
    var scanner = new Scanner(System.in) // need to pass in where the data is coming from
    System.out.print("Enter a value: ");
    // instead of println, you do just print because it doesnt put the cursor on a new line 
    
    var input = scanner.nextLine(); // wait until the user puts a new line then copy everything before that 
    
    System.out.println(input); 
    
    
    
    System.out.print("Enter number 1: ");
    var number1 = scanner.nextInt();
    	
    System.out.print("Enter number 2: ");
    var number2 = scanner.nextInt();
    
    var sum = number1 + nubmer2;
    System.out.println("The sum is: " + sum);
  }
}
```

The scanner class works if they are giving you the format you want. It needs ERROR HANDELING - see below for more details on this. 



**Calculator Challenge**

```java
package com.company;

import java.util.Scanner;

public class Main {
  public static void main(String[]  args){
    var sc = new Scanner(System.in);
    System.out.print("Enter a numeric value: ");
    var d1 = sc.nextDouble(); 
    
    System.out.print("Enter a numeric value: ");
    var d2 = sc.nextDouble();
    
    double result = d1/d2;
    System.out.println("The answer is " + result);
    
  }
}
```





## Manage Program Flow

**If-else**

```java
import java.time.LocalDateTime;

public class Main{
  public static void main(String[] args){
    var now = LocalDateTime.now(); 
  var monthNubmer = now.getMonthValue();

  String message;
  if (monthNumber < 1 || monthNumber > 12)
  {
    message = "This isnt a valid month";
  }
  else if (month <=3 ){
    message = "That is in quarter 2";
  }
  else { message = "another number"};
  System.out.println(message);
  }
}
} 
```



**Switch Case**

```java
package com.company;
import java.time.LocalDateTime;

public class Main{
  public static void main(String[] args){
    var now = LocalDateTime.now();
    var monthNumber = now.getMonthValue;
    
    switch (monthNumber){
      case 1:
        System.out.println("The month in January");
        break;
     	case 2:
        System.out.println("The month in February");
        break;
      case 9:
      case 10:
      case 11:
        System.out.println("This is 9,10, or 11")
       	break; 
      default: 
        System.out.println("You chose another month");
        
        
       
}
      // command shift return in Intellij completes the statment 
  }

}
```



**Create looping code blocks:**

```java
package com.company; 

public class Main{
  public static void main(String[] args){
    
    
    String[] months = 
    {"January", "February", "March"};
    
    // ITERATIVE LOOPING:
    for (int i = 0; i < months.length ; i++){
      System.out.println(months[i]);}
      // note it is months.length not months.lenght() because with arrays it is a property NOT a method
  }
  
  
  // FOR EACH LOOP:
  // in intellij do option return to see if you can replace with something 
  for (String month : months){
    System.out.println(month);
    // for each item in the array
  }
  //can also replace it with var and it will do inference typing
  for (var month : months){
    System.out.println(month);
  }
  
  
  // WHILE LOOPING
  
  var whileCounter = 0;
  while (whileCounter < months.length) {
    System.out.println(months);
    whileCounter++;
  }
  
  // DO LOOP: 
  
    var docounter = 0;
  // shift f6, "rename all occurances"
  do {
    System.out.println(months);
    doCounter++;
  } while (doCounter < months.length)
  
}
```



**Create reusable code with METHODS:**

functions / subroutines / METHODS: 

```java
public static void main(String[] args)
  
  // static means that it can be called from the main class without having to create an instance of the class 
```

- Static: can be called directly from the CLASS (you don't need to make an instance )
  - when you make your own methods you will typically them static 
- NonStatic (take out the word static): then you need an instance of the class 



TO MAKE METHOD in intellij: menu, refactor, extract, method: 

Select Refractor > Extract > Method…. TO MAKE RESUABLE CODE IN INTELLIJ

```java
double num1 = getInput("Enter a number: ");

private static double getInput(Scanner sc,  String prompt){
  System.out.print(prompt);
  return sc.nextDouble();
}
```



**Create Overload Methods: **

- method signature: name, number+ types of inputs 
- you can have mulitple methods with the same name but they need different signatures (differnt number of parameters)

```java
private static double addValues(int... values) //... means you can recieve any number of values 
{
  int result = 0;
  for (var value : values) {
    result += value;
}
  return result;
}
```



**Pass Arguments by reference v. value:**

- by copy or by reference:

  - COPY: passing argument in and recieving a copy of the argument, so not changing the original
  - REFERENCE: if you change the original refrence, it will change the actual object 

- Arguments are <u>always</u> passed by COPY

  ```java
  // PASSING PRIMITIVE VALUES
  void incrementValue (int inMethod) {
    inMethod ++;
    System.out.println("In method: " + inMethod);
  }
  
  var original = 10;
  System.out.println(original); // 10
  incrementValue(original); // 11
  System.out.println(original); // 10
  
  // you are not referecing the ACTUAL value in the method so the original value will NOT change
  
  
  
  // PRIMITIVES WRAPPED IN OBJECTS
  
  void incrementValue (int[] inMethod) {
    inMethod[0] ++;
    System.out.println("In method: " + inMethod[0]);
  }
  
  int[] original = {10, 20, 30};
  System.out.println(original); // 10
  incrementValue(original); // 11
  System.out.println(original); // 11
  
  ```

- **Object Variables are REFERENCES,** not the actual value in memory!!! 
- reference variable points to location in memory
  - when reference passed, a new reference is created
  - both array elements point to original objects or values / same meory location
    - ONE object in memory but mulitple references
- EXCEPTION: passing immutable values:
  - <u>String class!! is IMMUTABLE</u>! 
  - you are not modifying, just creating new string 



## Debugging & Exception Handling



**Exceptions with Try / Catch**

```java
String s = null;
var sub = s.substring(1); 
// automatically add try catch in Intellij: codd > surround with > try catch 

try {
  var sub = s.substring(1);
} catch (Exception e) {
  e.printStackTrace();
}
System.out.println("Not dead yet");
```



**Multiple Catch Blocks**

```java
// mulit catch: distinct catch blocks for each kind of exception 

String s = "";
var sub = s.substring(1); 

try {
  var sub = s.substring(1);
} catch (NullPointerException e) {
  e.printStackTrace();
  System.out.println("Null pointer: " + e.getMessage); // getMessage = message associated with exception
}
catch (StringIndexOutOfBoundsException e) {
  e.printStackTrace();
  System.out.println("Out of Bounds: " + e.getMessage);
}
catch (Exception e) // general Exception is a way to make sure you catch ALL exceptions
{
  e.printStackTrace();
  System.out.println("I don't know what happened: " + e.getMessage);
  
}
System.out.println("Not dead yet");
```



**Close Objects**

- Objects you open, but then need to close to make sure you are not leaking memory
- Usually when you are working with files!!



```java
package com.company;
import java.io.*;

public class Main {
  public static void main(String[] args){
    var file = new File("hello.txt");
    System.out.println("File exists: " + file.exists()); // tells you if you can find the file successfully
    
    // read the contents of file
    FileReader reader = new FileReader(file); 
    BufferedReader br = new BufferedReader(reader);
    var text = br.readLine();
    System.out.println();
    
    // handle all the exceptions by TRY CATCH BLOCK!!
  }
}
```

- finally block: gets excuted last, no matter what happens in the try-catch block

  - not ideal because you have to make sure that the readers are visible within all code blocks, you might have nested expressions, etc. it is NOT the way to go.

- try with resources block: declare closable objects so they are automatically closed when you are done with them: `try(Object name = thing){}` (see below)

  - you know an object needs to be closed this way if you go to the documentation, then under "implemented interfaces" it says Closeable

- ```java
  package com.company;
  import java.io.*; // * gets all of the classes from this io 
  
  public class Main {
    public static void main(String[] args){
      var file = new File("hello.txt");
      System.out.println("File exists: " + file.exists()); // tells you if you can find the file successfully
      
      try (FileReader reader = new FileReader(file); 
      BufferedReader br = new BufferedReader(reader)) 
        // because these objects are in the try parentheses they will be closed automatically after 
           {
             var text = br.readLine();
             System.out.println(text);
             
           } catch (IOException e) {
             e.printStackTrace();
      }
    }
  }
  ```



## Create Custom Classes

**Declare & use custom classes:**

new java class 

```java
package com.company; 

public class CalcHelper {
  
  // STATIC: can be called from class itself not an instance of the calss
 // PUBLIC: access modifier, can be called from anywhere in the application 
  public static double addValues(double d1, double d2) {
    return d1 + d2;
  }
  
  // command D to duplicate code
   public static double subtractValues(double d1, double d2) {
    return d1 - d2;
  }
   public static double mulitplyValues(double d1, double d2) {
    return d1 * d2;
  }
  public static double divideValues(double d1, double d2) {
    return d1 / d2;
  } 
}


// if we called this in the main class that is also in package com.company, we would not need to import it since we are a part of the same package 

```

**Abstraction**: taking the functionality that you have at the top level of the application, and hiding it in another part of the application. Better to organize your code!



**Organize Code with Packages**

- Package matches a directory structure 

- You can create classes that are part of the "default package"

  - aka no package at all

  - make DefaultPackageClass:

  - NOTE: no package delcaration in this. The absence of a package delcaration means it is in the default package

    ```java
    public class {}
    ```

    

- Note: many classes including system class, string, and wrapper classes for primitive values are members of the package "java.lang"

  - these do NOT need to be imported because java.lang is always imported.

    

**Create instance fields and methods:**

- store variables as members of class instances, these are called *instance fields*
  - usually delcared as private to classes, but you can access them through methods of class (called *instance methods*)

```java
package com.company.model;

public class ClothingItem {
  private String type; 
  private String size;
  private double price; // coudl use BigDecimal
  private int quantity; 
  // PRIVATE means they are only accessible from within the current class. to make them accessible to the rest of the application: CODE > GENERATE > GETTER & SETTER  
  
  public String getType(){
    return type;
  } // GETTER: also known as the "accessor" method. Returns 
  
  public void setType(String type) {
    this.type = type;
  }
  // SETTER: known as modifier
  // this.type, to distinguish name of field versus name of class item 
  
}
```

Accesing in Main: 

```java
package com.company; 
import com.company.model.ClothingItem; 

public class Main {
  public static void main(String[] args){
    var item = new ClothingItem(); 
    /// calling constructor method to create an instance of the item
    
    // BUT note that our clothing item class doesnt actually this method. But it is okay because ** JAVA RULE: if there is no explicity constructor is declared, then a no arguments constructor is made for you 
  //  ** if there IS an explicity it will require you to use it & will not allow or no argument constructors 
    
    item.setType("Shirt");
    item.setSize("M");
    item.setPrice(19.99);
    item.setQuantity(3);
    var totalPrice = item.getPrice() * item.getQuantity; 
    var formatter = NumberFormatter.getCurrencyInstance();
    var output = String.format("Your %s will cost %s",
                              item.getType(),
                              formatter.format(totalPrice));
    System.out.println(output);
                               
  }
}
```



**Declare mulitple constructor methods: **

NOTE:

- most methods require a type delcaration for the return value - either void, String, etc. BUT constructors do not require this. The create the 
- names always is the same 
- public so the class can be constructed or made anywhere in the application 

```java
// CODE > GENERATE > CONSTRUCTOR

public ClothingItem(){};
public ClothingItem(String type, String size, double price, int quantity) {
  this.type = type;
  this.size = size;
  this.quanitity = quantity;
  this.price = price;
}
```





**Use static fields as constants: **

const is in many languages but there is none here: 

- instead you use STATIC and FINAL:

  - static: member of class, not class instance

  - Final: it cannot be chagned 

  - ```java
    public static final String SHIRT = "Shirt"; 
    ```

  - **psfs tab in IntelliJ to get this**

  - some people make a class call Constants and put all their constants there 



**Declare and use Enum Types**

<u>Enumerator</u>: special class that contains possible values

- great to set up a finite amount of choices 
- either use the identifiers for the Enumerator values or you can add an associated string usign a private field, constuctor, and override of the toString method

```java
package com.company.model;
public enum ClothingSize {
  S("Small"), M("Medium"), L("Large"); // set possible values
  private String description; 
  
  ClothingSize(String description){
    this.description = description;
  }
  
  // override method name toString 
  public String toString()
  {
    return description;
  }}


// in main function:
private String size; 
// change to:
private ClothingSize size; 

// instead of saying M you would say ClothingSize.M
```





## Work with Inheritance 

**About Inheritance**

- object oriented so supports inheritance! 
- defines relationships betweeen classes, when they need to inherit functionality from another
- only SINGLE inheritance in Java
  - each class can only extend only one class directly
  - aka only have one direct super class 
  - can be SUPER deep inheritance but since single you know where the inheritance is coming from

Vocabulary: 

- parent/child
- base/derived
- superclass/subclass (usually this in java!!)



<u>Superclass</u>: 

- all classes derived from the superclass: Object
  - this is where the default toString method & other methods 
- Superclasses don't need any special notation
- If a class is not FINAL then they can be extended 
- members are inherited into subclasses unless (fields, method, etc.) they are marked private

```java
private class ClothingItem {
  private String size;
  public String getType(){
    return "Clothing Item";
  }
  public String getType(){
    return size; // Getter (Accessor Method)
  }
  public void setSize(String size) {
    this.size = size; // Setter (Modifier Method)
  }
}

public class Hat extends ClothingItem {
  @Override 
  public String getType(){
    return "Hat";
  }
}
```



To inherit functionality:

- add `extends SuperClassName` in your class definition
- optional: @Override to override any methods from superclass
  - name, # arguments, same return type 
  - not required but improves readibility 

<u>Polymorphism</u>

- ability to address an object as eitehr it super or subtype

- call inhertied w/o excat type 

  - you can pass an Object somehwere that expects the super-type 
  - the recieving context can call methos w/0 knowing whether it has a super or sub type

  

  In code: 

  - use supertype in declaration
  - use subtype in intialzation

  ```java
  ClothingItem item = new Hat();
  System.out.println("This is a" + item.getType()) // This is a Hat
  ```

  

**Extend Classes + Override Methods**

```java
public class Shirt extends ClothingItem {
  
  public Shirt(ClothingItem size, double price, int quantity){
    super(ClothingItem.SHIRT, size,price, quantity)
      // super() calls the super constructor 
  }
}
```



Code > Optimize Imports

- gets rid of imports you are not using & also alphabetizes them / groups them 



**Use Objects as their Super Types **

polymorphism: treat an object as its own type or its super type

```java
// super class: clothingItem
// sub class: shirt 
// sub class: hat

public class Hat extends ClothingItem {
  
  public Hat(ClothingItem size, double price, int quantity){
    super(ClothingItem.HAT, size,price, quantity)
      // super() calls the super constructor 
  }
}



/// MAIN CLASS: 
// even if your method calls for a ClothingItem, you can pass it a Hat


```

to create a method: 

- highlight all, refactor > extract > method

to rename everything:

- refactor > rename 
  - changes the name everywhere it is written



## Manage Data Collections

**Store Values in Simple Arrays**

- array: 
  - NOT RESIZABLE, cannot add or remove values of it 

```java
String[] colors = new String[3]; 
color[0] = "Blue";
color[1] = "Pink";
color[2] = "Yellow
  
for (int i = 0; i < colors.length; i++ ){
  System.out.println(colors[i])
}

// chcek 


ClothingItem[] items =
{
  new Shirt(ClothingSize.L, 
           19.99,
           10),
  
  new Hat(ClothingSize.L, 
           10.00,
           4),  
}

for (ClothingItem item : items){
  displayItemDetails(item);
}
```



Because you **cannot** resize arrays in Java:

- if you need to use *classes and interfaces* from the Java Collections Framework 



**Manage Resizable Arrays with Lists**

- since arrays are not resizable they arent the best way to store a collections

- Java Collections Framework:

  - collection: a collection of data
    - ordered
    - unordered
  - **lists**: interface. interface in OO programming is a contract - any particular class that implements this interface is guaranteed to also have a specific set of  methods
    - when you declare a list, you are saying the object fufills that contract
    - but you also have to specify which concrete class taht impletes that interface youre using 

  LISTS: 

  - sizing: `.size()` aka NOT .length()
  - get an item: `.get(index)`
  - add item: `.add(item)`

  

  ADDITIONAL LISTS: 

  - LinkedList, ArrayList, Vector, etc.

  ```java
  List<String> colors = new ArrayList<>();
  colors.add("Blue");
  colors.add("Pink");
  colors.add("Yellow");
  
  for (int i = 0; i < colors.size(); i++){
    System.out.print.colors.
  }
  
    
    // <Type of the stuff you will store in the list
    
  
  // WITH COMPLEX OBJECTS
  
  List<ClothingItem> items = new ArrayList<>();
  items.add(new Shirt(ClothingSize.L, 19.99, 10)); 
  items.add(new Hat(ClothingSize.L, 10.00, 04)))
    
  for (ClothingItem item : items){ displayItemDetails(item);} 
  
  ```

  

  **Manage Key-Value Pairs with Maps**

- UNORDERED collection

- Map is an interface (just like lists)

- `.put` instead of .add !!! 

- `.get(anItem)`

- to loop through: 

  - get a list of key items
    - using `.keySet()` (a set is anotehr kind of collection )
  - Then use a for each loop 

   NOTE: does not store data in any particular order!! 

  ```java
  Map<String, ClothingItem>  items = new HashMap<>();
  // you need to choose a map class- just like with list interface, you only need to declare it once
  
  items.put("shirt", new Shirt(ClothingSize.L, 19.99,3));
  
  items.put("hat", new Hat(ClothingSize.L, 19.99,3));
    
    
  var keys = items.keySet();
  for (String key : keys){
    var item = items.get(key);
    displayItemDetails(item);
  }
  ```

  

NEXT STEPS:

- Advanced Java Programming
- Applying Java:
  - Enterprise class web applications:
    - Spring
    - Java EE (Jakarta EE)
  - Databases:
    - Hibernate ORM System
  - Microservices 

