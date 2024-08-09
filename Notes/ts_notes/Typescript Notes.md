  

![Untitled](Typescript%20Notes%2038072016e2724a65a6bb080f15073e8a/Untitled.png)

In TypeScript, you work with types like `string` or `number` all the times.

**Important**: It is `string` and `number` (etc.), **NOT** `String`, `Number` etc.

**The core primitive types in TypeScript are all lowercase!**

![Untitled](Typescript%20Notes%2038072016e2724a65a6bb080f15073e8a/Untitled%201.png)

## Type Inference(automatically identifying the type of let/const
 based on the value assigned to it)

![Untitled](Typescript%20Notes%2038072016e2724a65a6bb080f15073e8a/Untitled%202.png)

- Typescript is capable enough to identify which type of value is assigned to the variable, providing a type to a variable/constant(that already has a value) is not a good practice.
    - `eg: const number2: number = 2.8; NOT A GOOD PRACTICE`
- It is IMPORTANT to assign a type to var whose values are not provided at the time of their creation.
    - `eg: const number1: number; //GOOD PRACTICE`
    - `number1 = 5;`
- What's the difference between JavaScript types (e.g. `typeof 'Max' => 'string'`) and TypeScript types (e.g. `const name: string = '...'`)?
    - ANS: JS has no compilation step but at runtime, you can check for certain types (e.g. in if conditions). TS on the other hand allows you to catch certain errors during development since it checks types during compilation as well.

## Object Type

![Untitled](Typescript%20Notes%2038072016e2724a65a6bb080f15073e8a/Untitled%203.png)

![Untitled](Typescript%20Notes%2038072016e2724a65a6bb080f15073e8a/Untitled%204.png)

### Nested Objects

Of course object types can also be created for **nested objects**.

Let's say you have this JavaScript **object**:

```jsx
1. const product = {
2.   id: 'abc1',
3.   price: 12.99,
4.   tags: ['great-offer', 'hot-and-new'],
5.   details: {
6.     title: 'Red Carpet',
7.     description: 'A great carpet - almost brand-new!'
8.   }
9. }
```

This would be the **type** of such an object:

```jsx
1. {
2.   id: string;
3.   price: number;
4.   tags: string[];
5.   details: {
6.     title: string;
7.     description: string;
8.   }
9. }
```

So you have an object type in an object type so to say.

## Array Type

![Untitled](Typescript%20Notes%2038072016e2724a65a6bb080f15073e8a/Untitled%205.png)

![Untitled](Typescript%20Notes%2038072016e2724a65a6bb080f15073e8a/Untitled%206.png)

On hobby we can access anything, we can access on any string for eg. toUpperCase() and TS doesnt not complain but Why? Bcz TS knows hobbies is of string type array so when we access person.hobbies TS inference will be an array of strings.

## Tuple Type

![Untitled](Typescript%20Notes%2038072016e2724a65a6bb080f15073e8a/Untitled%207.png)

Tuple are not fixed length and fixed type array.

![Untitled](Typescript%20Notes%2038072016e2724a65a6bb080f15073e8a/Untitled%208.png)

its a special array with exactly two types 1st one is number and 2nd one is string

```jsx
person.role[0] = 10; //ERROR bcz we have string as second value

person.roles.push("hello"); //NOT ERROR its an exception in TS as it cant catch this error

person.roles = [0,"hello","hi"]; //ERROR bcz we have assigned tuple to be of fixed length
```

## Enum Type-

![Untitled](Typescript%20Notes%2038072016e2724a65a6bb080f15073e8a/Untitled%209.png)

![Untitled](Typescript%20Notes%2038072016e2724a65a6bb080f15073e8a/Untitled%2010.png)

![Untitled](Typescript%20Notes%2038072016e2724a65a6bb080f15073e8a/Untitled%2011.png)

- By default enum values starts from ADMIN = 0, READ_ONLY = 1, AUTHOR = 2
- we can assign a custom values (string or number) to these enums

```jsx
enum Role {ADMIN = 1, READ_ONLY = 'read only', AUTHOR = 1.2}
```

## Any Type

![Untitled](Typescript%20Notes%2038072016e2724a65a6bb080f15073e8a/Untitled%2012.png)

NEVER USE ANY TYPE.

```jsx
let name: any;
name = 1;
name = 'ritik'

let names : any[]; //array of any type
names = ['ritik',1,{name: 'ritik'}]
```

## Union Type

![Untitled](Typescript%20Notes%2038072016e2724a65a6bb080f15073e8a/Untitled%2013.png)

## Literal Type

- Literal types are the types which are based on core type string or number and so on but then we have the specific version of the type.
- In the below code we allow two strings NOT ANY STRING just the below mentioned strings

![Untitled](Typescript%20Notes%2038072016e2724a65a6bb080f15073e8a/Untitled%2014.png)

## Type Aliases / Custom Types

![Untitled](Typescript%20Notes%2038072016e2724a65a6bb080f15073e8a/Untitled%2015.png)

## ***Type Aliases & Object Types***

Type aliases can be used to "create" your own types. You're not limited to storing union types though - you can also provide an alias to a (possibly complex) object type.

For example:

```jsx

1. type User = { name: string; age: number };
2. const u1: User = { name: 'Max', age: 30 }; // this works!
```

This allows you to avoid unnecessary repetition and manage types centrally.

For example, you can simplify this code:

```jsx

1. function greet(user: { name: string; age: number }) {
2.   console.log('Hi, I am ' + user.name);
3. }
4.  
5. function isOlder(user: { name: string; age: number }, checkAge: number) {
6.   return checkAge > user.age;
7. }
```

To:

```jsx

1. type User = { name: string; age: number };
2.  
3. function greet(user: User) {
4.   console.log('Hi, I am ' + user.name);
5. }
6.  
7. function isOlder(user: User, checkAge: number) {
8.   return checkAge > user.age;
9. }
```

- Which of the following snippets could be simplified by using an enum type?
    
    **A)**
    
    **
    `1. const users = ['Max', 'Michael', 'Julie'];`**
    
    **B)**
    
    **
    `1. const userA = { name: 'Max' };
    2. const userB = { name: 'Michael' };`**
    
    **C)**
    
    **
    `1. const ROLE_ADMIN = 0;
    2. const ROLE_AUTHOR = 1;`**
    
    ANS:  C
    

- Will the following code throw a compilation error?
    
    **
    `1. type User = {name: string; age: number};
    2. const u1: User = ['Max', 29];`**
    
    ANS: YES (The "User" type clearly wants an object with a "name" and an "age" property. NOT an array.)
    

- Will this code make it through compilation?
    
    **
    `1. type Product = {title: string; price: number;};
    2. const p1: Product = { title: 'A Book', price: 12.99, isListed: true }`**
    
    ANS: No, isListed is not part of Product type
    
- Will this code make it through compilation?
    
    **
    `1. type User = { name: string } | string;
    2. let u1: User = {name: 'Max'};
    3. u1 = 'Michael';`**
    
    **ANS:**  YES, This code is fine. The union type allows either an object (with a "name" property) OR a string. You can switch values how often you want.
    
    ## Function Return Type
    
    - Though TS inference can identify the return type of the function we can also explicitly mention the return type of of the function
    
    ![Untitled](Typescript%20Notes%2038072016e2724a65a6bb080f15073e8a/Untitled%2016.png)
    
    ![Untitled](Typescript%20Notes%2038072016e2724a65a6bb080f15073e8a/Untitled%2017.png)
    
    ### void return type
    
    - if we dont want our function to return anything we can set the return type to “void”. We can either explicitly define the void type of leave it upto TS inference to identify it.
    - ************NOTE:************ there is no void type in JS its a TS thing
    
    ![Untitled](Typescript%20Notes%2038072016e2724a65a6bb080f15073e8a/Untitled%2018.png)
    
    **********IMPORTANT:********** 
    
    we can have undefined as a return type in TS but for that we have to return empty. And its better to choose void than undefined as its more conventional and you dont have to add empty return at the bottom .
    
    ![Untitled](Typescript%20Notes%2038072016e2724a65a6bb080f15073e8a/Untitled%2019.png)
    
    ## Function Types
    
    - So function types, allow us to describe which type of function specifically we want to use somewhere.
    
    ![Untitled](Typescript%20Notes%2038072016e2724a65a6bb080f15073e8a/Untitled%2020.png)
    
    - Here in the above code we have provided ****************Function**************** type to ******combineValues****** variable but that doesnt signify what type of function the combineValue variable can hold.
    - We change the assignment from add to printResult and we will get the error bcz printResult takes one argument and add takes 2, with TS we should be able to catch it at the time of compilation. How to do that?
    
    ![Untitled](Typescript%20Notes%2038072016e2724a65a6bb080f15073e8a/Untitled%2021.png)
    
    ## Function Types and Callbacks
    
    Adding a type to a callback function in the function itself.
    
    ![Untitled](Typescript%20Notes%2038072016e2724a65a6bb080f15073e8a/Untitled%2022.png)
    
    ![Untitled](Typescript%20Notes%2038072016e2724a65a6bb080f15073e8a/Untitled%2023.png)
    
    - The adv of defining the callback function definition is that inside the function we pass its the callback type that is able to infer that ************************************************result will be a number************************************************ and hence we could do anything with the number without explicitly stating the type in the anonymous function.
    - We will get error fi we add another value to the anonymous function definition
    - ************NOTE:************  the only thing TS wont pickup here is if we return something from the anonymous function. This however is NOT a mistake or a bug in TS. By specifying void here in the callback is to denote we will ignore any value returned from the cb function. Thats why we are able to return result inside our anonymous function without getting error
    
    Ques1: 
    
    Will this code compile?
    
    ```jsx
    
    **1. function sendRequest(data: string, cb: (response: any) => void) {
    2.   // ... sending a request with "data"
    3.   return cb({data: 'Hi there!'});
    4. }
    5.  
    6. sendRequest('Send this!', (response) => { 
    7.   console.log(response);
    8.   return true;
    9.  });**
    ```
    
    ANS: Yes, callback functions can return something, even if the argument on which they're passed does NOT expect a returned value.
    
    ## The “unknown” type
    
    - `unknown` is the type-safe counterpart of `any`. Anything is assignable to `unknown`, but `unknown` isn’t assignable to anything but itself and `any` without a type assertion or a control flow based narrowing. Likewise, no operations are permitted on an `unknown` without first asserting or narrowing to a more specific type.
    
    ![Untitled](Typescript%20Notes%2038072016e2724a65a6bb080f15073e8a/Untitled%2024.png)
    
    - if we dont perform a typecheck before assigning a **unknown** variable value to another variable then TS will show error. Whereas in the case on **any** we can assign an any value to another variable.
    
    ![Untitled](Typescript%20Notes%2038072016e2724a65a6bb080f15073e8a/Untitled%2025.png)
    
    ## “never” type
    
    - TypeScript introduced a new type `never`, which indicates the values that will never occur.
    - The never type is used when you are sure that something is never going to occur. For example, you write a function which will not return to its end point or always throws an exception.
    
    ```jsx
    function throwError(errorMsg: string): never { 
                throw new Error(errorMsg); 
    } 
    
    function keepProcessing(): never { 
                while (true) { 
             console.log('I always does something and never ends.')
         }
    }
    ```
    
    - In the above example, the `throwError()` function throws an error and `keepProcessing()` function is always executing and never reaches an end point because the while loop never ends. Thus, never type is used to indicate the value that will never occur or return from a function.
    
    ### Difference between never and void
    
    - The void type can have undefined or null as a value where as never cannot have any value.
    
    ```jsx
    let something: void = null;
    let nothing: never = null; // Error: Type 'null' is not assignable to type 'never'
    ```
    
    ## Compile the entire project /multiple files
    
    ```jsx
    tsc app.ts //the cmd to compile specific ts file
    
    tsc --init //the cmd will create a tsconfig.json file.
    
    tsc //cmd to compile the ts code all at once
    
    tsc --w //cmd to activate watch mode in the code and whenever we make changes in any file and save them it will automatically be compiled 
    ```
    
    ## Include/exclude files in TS compilation
    
    ![Untitled](Typescript%20Notes%2038072016e2724a65a6bb080f15073e8a/Untitled%2026.png)
    
    - there is also “file” option it is similar include BUT it takes ONLY files NOT folders
    
    ```jsx
    "file":[
    	"app.ts"
    ]
    ```
    
    ## Compilation target
    
    - target helps TS in identifying to which JS version the code needs to be compiled.
    
    ```jsx
    tsconfig.json
    {
    	"compilationOptions":{
    		"target": "es5"
    	}
    }
    ```
    
    ## Understanding TS Core libs
    
    - libs is an option that allows which default options and features TS knows.
    - for eg: working with the DOM. You wrote you as below and it still works TS doesnt find error in addEventListener.

![Untitled](Typescript%20Notes%2038072016e2724a65a6bb080f15073e8a/Untitled%2027.png)

- This is so bcz of ********lib******** compilation option present in the ****************************tsconfig.json****************************  file. and its not even set in the tsconfig file by default BUT if its not set in the file some default is been assumed and it depends on the js target value and for es6 it includes all features that are globally available in JS.

```jsx
tsconfig.json
{
	"compilationOptions":{
		"target": "es5",
		"lib": [] //since we have set lib to empty array the above code will throw error as we have overridden the default features
 	}
}
```

## Other Compilation Options

```jsx
tsconfig.json
{
	"compilationOptions":{
		"target": "es5",
		"lib": [], //since we have set lib to empty array the above code will throw error as we have overridden the default features
	 	"allowJS": true, //Allow JavaScript files to be a part of your program. Use the checkJS option to get errors from these files
		"checkJS": true,
		"jsx": "preserve" //helps with reactjs codebase
		"sourceMap": true, //help us in generating TS files in the developer console Source tab
		"outDir": "./dist", //provide the directory where the compiled JS files should be stored. By default the files are created in the root directory 
		"rootDir": "./src", //provide the directory whose TS files are need to be compiled. By default select root files
		"noEmitOnError": true, //will not generate any .js file if the one TS file code has errors.
	}
}
```

## strict compilation option

- The idea is that you opt into a **strict-by-default mode** so that you enjoy all the benefits of better type safety without having to enable each compiler option separately. You can either set the `--strict` flag on the command line or specify the `strict` option within your project's *tsconfig.json* file to opt into this mode.
- As of TypeScript 4.3 in August 2021, the `--strict` flag enables the following eight [compiler options](https://www.typescriptlang.org/tsconfig/#strict):
    - `-alwaysStrict`
    - `-strictBindCallApply`
    - `-strictFunctionTypes`
    - `-strictNullChecks`
    - `-strictPropertyInitialization`
    - `-noImplicitAny`
    - `-noImplicitThis`
    - `-useUnknownInCatchVariables`
    
    ## Classes
    
    ![Untitled](Typescript%20Notes%2038072016e2724a65a6bb080f15073e8a/Untitled%2028.png)
    
    - TS class code is compiled in JS based on the target JS version mentioned in the tsconfig.json.
    - ******ES6******
    
    ![Untitled](Typescript%20Notes%2038072016e2724a65a6bb080f15073e8a/Untitled%2029.png)
    
    - ****************************************************ES5 (constructor function)****************************************************
    
    ![Untitled](Typescript%20Notes%2038072016e2724a65a6bb080f15073e8a/Untitled%2030.png)
    
    ## Constructor Function and ‘this’ keyword
    
    ![Untitled](Typescript%20Notes%2038072016e2724a65a6bb080f15073e8a/Untitled%2031.png)
    
    ![Untitled](Typescript%20Notes%2038072016e2724a65a6bb080f15073e8a/Untitled%2032.png)
    
    - In the above code we have provided **this** with **Department** type so now
    
    ```jsx
    const accountingCopy = { describe: accounting.describe}; 
    acountingCopy.describe(); 
    //TS will throw error bcz the describe expect 'this' to be of Department 
    //type i.e. it should have name present in the scope of this which is not 
    //present in the accountingCopy scope.
    ```
    
    ## “private” and “public” access modifier
    
    - private varibles.
    
    ```jsx
    class Person {
      // declare our property types
      firstName: string;
      lastName: string;
      private _age: number;
    
      // when accessing the age property return the private _age
      // getters and setters are part of the JavaScript Class syntax
      get age() {
        return this._age;
      }
    
      constructor(firstName: string, lastName: string, age: number) {
        this.firstName = firstName;
        this.lastName = lastName;
        this._age = age;
      }
    
      sayHello() {
        console.log(`Hello, my name is ${this.firstName} ${this.lastName}!`);
      }
    
      // Only this method can update the private _age
      addOneYear() {
        this._age = this._age + 1;
      }
    }
    
    const cory = new Person('Cory', 'Rylan', 100);
    cory.addOneYear();
    console.log(cory.age); // 101
    
    cory._age = 200; // error: Property '_age' is private and only accessible within class 'Person'.
    console.log(cory._age); // error: Property '_age' is private and only accessible within class 'Person'.
    cory.age = 200; // error: Cannot assign to 'age' because it is a constant or a read-only property.
    ```
    
    - private methods
    
    ```jsx
    class Person {
      private _age: number;
    
      get age() {
        return this._age;
      }
    
      constructor(public firstName: string, public lastName: string, age: number) { //shorthand to define variables in class
        this._age = age;
      }
    
      sayHello() {
        this.log(`Hello, my name is ${this.firstName} ${this.lastName}!`);
      }
    
      addOneYear() {
        this._age = this._age + 1;
      }
    
      private log(message) {
        console.log(message);
      }
    }
    
    const cory = new Person('Cory', 'Rylan', 100);
    cory.sayHello(); // Hello, my name is Cory Rylan!
    cory.log('hi'); // error: Property 'log' is private and only accessible within class 'Person'.
    ```
    
    ## “readonly”
    
    - Prefix `readonly` is used to make a property as read-only. Read-only members can be accessed outside the class, but their value cannot be changed. Since read-only members cannot be changed outside the class, they either need to be initialized at declaration or initialized inside the class constructor.
    
    ```jsx
    class Employee {
        readonly empCode: number;
        empName: string;
        
        constructor(code: number, name: string)     {
            this.empCode = code;
            this.empName = name;
        }
    }
    let emp = new Employee(10, "John");
    emp.empCode = 20; //Compiler Error
    emp.empName = 'Bill';
    ```
    
    ## “protected”
    
    - The **protected** access modifier is similar to the private access modifier, except that protected members can be accessed using their deriving classes.
    
    ```jsx
    class Employee {
        public empName: string;
        protected empCode: number;
    
        constructor(name: string, code: number){
            this.empName = name;
            this.empCode = code;
        }
    }
    
    class SalesEmployee extends Employee{
        private department: string;
        
        constructor(name: string, code: number, department: string) {
            super(name, code);
            this.department = department;
        }
    }
    
    let emp = new SalesEmployee("John Smith", 123, "Sales");
    emp.empCode; //Compiler Error
    ```
    
    ## getter and setter in TS
    
    ```jsx
    class Person {
        private _age: number;
        private _firstName: string;
        private _lastName: string;
    
     
        public get age() {
            return this._age;
        }
    
        public set age(theAge: number) {
            if (theAge <= 0 || theAge >= 200) {
                throw new Error('The age is invalid');
            }
            this._age = theAge;
        }
    
        public getFullName(): string {
            return `${this._firstName} ${this._lastName}`;
        }
    }
    ```
    
    - how it works
        - First, change the access modifiers of the `age`, `firstName`, and `lastName` properties from `public` to `private`.
        - Second, change the property `age` to `_age`.
        - Third, create getter and setter methods for the `_age` property. In the setter method, check the validity of the input age before assigning it to the `_age` property.
    
    ```jsx
    // Now, you can access the age setter method as follows:
    let person = new Person();
    person.age = 10;
    ```
    
    - Notice that the call to the setter doesn’t have parentheses like a regular method. When you call `person.age`, the `age` setter method is invoked. If you assign an invalid `age` value, the setter will throw an error:
    
    ```jsx
    person.age = 0;
    
    //ERROR
    Error: The age is invalid
    ```
    
    - When you access the `person.age`, the `age` getter is invoked.
    
    ```jsx
    console.log(person.age);
    ```
    
	 ## static properties
    
    - Unlike an instance property, a static property is shared among all instances of a class.
    - To declare a static property, you use the `static` keyword. To access a static property, you use the `className.propertyName` syntax. For example:
    
    ```jsx
    class Employee {
        static headcount: number = 0;
    
        constructor(
            private firstName: string,
            private lastName: string,
            private jobTitle: string) {
    
            Employee.headcount++;
        }
    }
    ```
    
    - In this example, the `headcount` is a static property that is initialized to zero. Its value is increased by 1 whenever a new object is created.
    - The following creates two `Employee` objects and shows the value of the `headcount` property. It returns two as expected.
    
    ```jsx
    let john = new Employee('John', 'Doe', 'Front-end Developer');
    let jane = new Employee('Jane', 'Doe', 'Back-end Developer');
    
    console.log(Employee.headcount); // 2
    ```
    
    ## static methods
    
    - Similar to the static property, a static method is also shared across instances of the class. To declare a static method, you use the `static` keyword before the method name. For example:
    
    ```jsx
    class Employee {
        private static headcount: number = 0;
    
        constructor(
            private firstName: string,
            private lastName: string,
            private jobTitle: string) {
    
            Employee.headcount++;
        }
    
        public static getHeadcount() {
            return Employee.headcount;
        }
    }
    ```
    
    - First, change the access modifier of the `headcount` static property from `public` to `private` so that its value cannot be changed outside of the class without creating a new `Employee` object.
    - Second, add the `getHeadcount()` static method that returns the value of the `headcount` static property.
    - To call a static method, you use the `className.staticMethod()` syntax. For example:
    
    ```jsx
    let john = new Employee('John', 'Doe', 'Front-end Developer');
    let jane = new Employee('Jane', 'Doe', 'Back-end Developer');
    
    console.log(Employee.getHeadcount); // 2
    ```
    
    ## Abstract class
    
    - Typically, an abstract class contains one or more abstract methods.
    - An abstract method does not contain implementation. It only defines the signature of the method without including the method body. An abstract method must be implemented in the derived class.
    
    ```jsx
    abstract class Employee {
        constructor(private firstName: string, private lastName: string) {
        }
        abstract getSalary(): number
        get fullName(): string {
            return `${this.firstName} ${this.lastName}`;
        }
        compensationStatement(): string {
            return `${this.fullName} makes ${this.getSalary()} a month.`;
        }
    }
    ```
    
    - In the `Employee` class:
        - The constructor declares the `firstName` and `lastName` properties.
        - The `getSalary()` method is an abstract method. The derived class will implement the logic based on the type of employee.
        - The `getFullName()` and `compensationStatement()` methods contain detailed implementation. Note that the `compensationStatement()` method calls the `getSalary()` method.
        
        ```jsx
        let employee = new Employee('John','Doe');
        
        //error TS2511: Cannot create an instance of an abstract class.
        ```
        
    - The following `FullTimeEmployee` class inherits from the `Employee` class:
    
    ```jsx
    class FullTimeEmployee extends Employee {
        constructor(firstName: string, lastName: string, private salary: number) {
            super(firstName, lastName);
        }
        getSalary(): number {
            return this.salary;
        }
    }
    ```
    
    - In this `FullTimeEmployee` class, the salary is set in the constructor. Because the `getSalary()` is an abstract method of the `Employee` class, the `FullTimeEmployee` class needs to implement this method. In this example, it just returns the salary without any calculation.
    - The following shows the `Contractor` class that also inherits from the `Employee` class:
    
    ```jsx
    class Contractor extends Employee {
        constructor(firstName: string, lastName: string, private rate: number, private hours: number) {
            super(firstName, lastName);
        }
        getSalary(): number {
            return this.rate * this.hours;
        }
    }
    ```
    
    - In the `Contractor` class, the constructor initializes the rate and hours.
    - The `getSalary()` method calculates the salary by multiplying the rate by the hours.
    - The following first creates a `FullTimeEmployee` object and a `Contractor` object and then shows the compensation statements to the console:
    
    ```jsx
    let john = new FullTimeEmployee('John', 'Doe', 12000);
    let jane = new Contractor('Jane', 'Doe', 100, 160);
    
    console.log(john.compensationStatement());
    console.log(jane.compensationStatement());
    ```
    
    ```jsx
    //OUTPUT
    John Doe makes 12000 a month.
    Jane Doe makes 16000 a month.
    ```


## Singleton class

```jsx
/**
 * The Singleton class defines the `getInstance` method that lets clients access
 * the unique singleton instance.
 */
class Singleton {
    private static instance: Singleton;

    /**
     * The Singleton's constructor should always be private to prevent direct
     * construction calls with the `new` operator.
     */
    private constructor() { }

    /**
     * The static method that controls the access to the singleton instance.
     *
     * This implementation let you subclass the Singleton class while keeping
     * just one instance of each subclass around.
     */
    public static getInstance(): Singleton {
        if (!Singleton.instance) {
            Singleton.instance = new Singleton();
        }

        return Singleton.instance;
    }

    /**
     * Finally, any singleton should define some business logic, which can be
     * executed on its instance.
     */
    public someBusinessLogic() {
        // ...
    }
}

/**
 * The client code.
 */
function clientCode() {
    const s1 = Singleton.getInstance();
    const s2 = Singleton.getInstance();

    if (s1 === s2) {
        console.log('Singleton works, both variables contain the same instance.');
    } else {
        console.log('Singleton failed, variables contain different instances.');
    }
}

clientCode();
```

## Interfaces

- An interface describe the structure of an object. We can use it to describe how an object should look like.
![[image.png]]

## Interfaces as Types

```jsx
interface KeyPair {
    key: number;
    value: string;
}

let kv1: KeyPair = { key:1, value:"Steve" }; // OK

let kv2: KeyPair = { key:1, val:"Steve" }; // Compiler Error: 'val' doesn't exist in type 'KeyPair'

let kv3: KeyPair = { key:1, value:100 }; // Compiler Error:
```

## Interfaces as Function Types

```jsx
interface KeyValueProcessor
{
    (key: number, value: string): void;
};

function addKeyValue(key:number, value:string):void {
    console.log('addKeyValue: key = ' + key + ', value = ' + value)
}

function updateKeyValue(key: number, value:string):void {
    console.log('updateKeyValue: key = '+ key + ', value = ' + value)
}

let kvp: KeyValueProcessor = addKeyValue;
kvp(1, 'Bill'); //Output: addKeyValue: key = 1, value = Bill

kvp = updateKeyValue;
kvp(2, 'Steve'); //Output: updateKeyValue: key = 2, value = Steve
```

- In the above example, an interface `KeyValueProcessor` includes a method signature. This defines the function type. Now, we can define a variable of type `KeyValueProcessor` which can only point to functions with the same signature as defined in the `KeyValueProcessor` interface. So, `addKeyValue` or `updateKeyValue` function is assigned to `kvp`. So, `kvp` can be called like a function

```jsx
function delete(key:number):void { 
    console.log('Key deleted.')
}
    
let kvp: KeyValueProcessor = delete; //Compiler Error
```

## Interface for Array Type

```jsx
interface NumList {
    [index:number]:number
}

let numArr: NumList = [1, 2, 3];
numArr[0];
numArr[1];

interface IStringList {
    [index:string]:string
}

let strArr : IStringList = {};
strArr["TS"] = "TypeScript";
strArr["JS"] = "JavaScript";
```

- In the above example, interface `NumList` defines a type of array with index as number and value as number type. In the same way, `IStringList` defines a string array with index as string and value as string.

## Optional Property

- we may declare an interface with excess properties but may not expect all objects to define all the given interface properties. We can have optional properties, marked with a "?". In such cases, objects of the interface may or may not define these properties.

```jsx
interface IEmployee {
    empCode: number;
    empName: string;
    empDept?:string;
}

let empObj1:IEmployee = {   // OK
    empCode:1,
    empName:"Steve"
}

let empObj2:IEmployee = {    // OK
    empCode:1,
    empName:"Bill",
    empDept:"IT"
}
```

## Readonly Property

```jsx
interface Citizen {
    name: string;
    readonly SSN: number;
}

let personObj: Citizen  = { SSN: 110555444, name: 'James Bond' }

personObj.name = 'Steve Smith'; // OK
personObj.SSN = '333666888'; // Compiler Error
```

## Extending Interface

- Interfaces can extend one or more interfaces. This makes writing interfaces flexible and reusable.

```jsx
interface IPerson {
    name: string;
    gender: string;
}

interface IEmployee extends IPerson {
    empCode: number;
}

let empObj:IEmployee = {
    empCode:1,
    name:"Bill",
    gender:"Male"
}
```

## Implementing Interface

```jsx
interface IEmployee {
    empCode: number;
    name: string;
    getSalary:(empCode: number) => number;
}

class Employee implements IEmployee {
    empCode: number;
    name: string;

    constructor(code: number, name: string) {
        this.empCode = code;
        this.name = name;
    }

    getSalary(empCode:number):number {
        return 20000;
    }
}

let emp = new Employee(1, "Steve");
```

- the `IEmployee` interface is implemented in the Employee class using the the implement keyword. The implementing class should strictly define the properties and the function with the same name and data type. If the implementing class does not follow the structure, then the compiler will show an error.

# Advance Types in TS

- ## Intersection Types
	- In case of object types the intersection types combines the object properties.
	
```jsx
type Student = { 
student_id: number, 
name: string
} 

type Teacher = { 
Teacher_Id: number,
teacher_name: string
} 

type intersected_type = Student & Teacher; 

let obj1: intersected_type = { 
student_id: 3232, 
name: "rita", 
Teacher_Id: 7873, 
teacher_name: "seema", 
}; 

console.log(obj1.Teacher_Id); 
console.log(obj1.name);
``` 

- In case of union types it takes the type which is common in both of them.
```jsx
type Combinable = string | number;
type Numeric = number | boolean;

type Universal = Combinable & Numeric // HERE THE TYPE OF UNIVERSAL IS number
```

- ## Type Guards

	- Helps us with UNION types whilst it is good to have flexibility but we often need to know the exact types.
```jsx
type Combinable = string | number;
type Numeric = number | boolean;

function add(a: Combinable, b:Combinale){
	if(typeof a === 'string' || typeof b === 'string'){ //TYPE GUARD
		return a.toString() + b.toString();
	}
	return a+b;
}
```

```jsx
type Student = { 
id: number, 
name: string
} 

type Teacher = { 
id: number,
teacher_name: string
} 

type unknownStudent = Student | Taacher

function printStudentInfo(std: UnknownStudent){
	console.log(std.id);
	if(teacher_name in std){
		console.log("teacher name: ",std.teacher_name)
	}
	if(name in std){
		console.log("student name: ",std.name)
	}
}
```

```jsx
class Car{
	drive(){
		console.log("driving...");
	}
}

class Truck{
	drive(){
		console.log("driving truck");
	}
	loadCargo(){
		console.log("loading cargo");
	}
}

type Vehicle = Car | Truck

const v1 = new Car();
const v2 = new Truck();

function usedVehicle(vehicle: Vehicle){
	vehicle.drive();
	if(vehicle instanceOf Truck){ //TYPE GUARD
		vehicle.loadCargo();
	}
}
useVehicle(v1);
useVehicle(v2)
```

- ## Discriminated Unions
	- Discriminated Unions enables the creation of a type that can represent several different possibilities or variants. By attaching discriminators to each variant, TypeScript's type system can help ensure that we handle all possible cases gracefully. Discriminators can be string literals, numeric literals, or even symbols.
	- ##### NOTE: we cant use instance of with interface

```jsx
interface Bird{
	type: "bird",
	flyingSpeed: number
}

interface Horse{
	type: 'horse',
	runningSpeed: number 
}

type Animal = Bird | Horse;

function moveAnimal(animal:Animal){
	let speed;
	switch(animal.type){
		case 'bird':
			speed = animal.flyingSpeed
		case 'horse':
			speed = animal.runningSpeed
	}
	console.log("Speed: ",speed);
}

moveAnimal({type: 'bird', flyingSpeed: 10});
```

- ## Typecasting
	- ##### NOTE: by using ! after the value we are telling TS that the value will never be null
```jsx
const userInputElement = document.getElementById('user-input-field')! as HTMLInputElement;

userInputElement.value = 'Hi there'
```

- ## Index Properties
	- Index types allows us to create which are more flexible regarding the properties they might hold.
```jsx
interface ErrorContainer{
	[prop: string]: string;
}

const errorBag: ErrorContainer = {
	email: "NOT A VALID EMAIL ID",
	username: "USERNAME MUST START WE A CAPITAL CHARACTER"
}
```

- ## Function Overloading
	- We use function overloading in place where TS would not be able to correctly infer the return types on its own.
	-  **Problem**
```jsx
type Combinable = string | number;

function add(a: Combinable, b: Combinable){
	if(typeof a === 'string' || typeof b === 'string'){
		return a.toString() + b.toString();
	}
	return a+b;
}

// HERE WE HAVE TO EXPLICITELY PERFORM TYPECAST ON ADD() AS TS WONT BE ABLE TO IDENLTIFY THE RETURNED VALUE TYPE AND SHOWING ERROR ON split();
const result = add('Ritik','Rawal') as string;
result.split(' ');
```

- **Solution**
```jsx

function add(a: number, b: number): number;
function add(a: string, b: string): string;
function add(a: string, b: number): string;
function add(a: number, b: string): string;
function add(a: Combinable, b: Combinable){
	if(typeof a === 'string' || typeof b === 'string'){
		return a.toString() + b.toString();
	}
	return a+b;
}

const result = add('Ritik','Rawal');
result.split(' ');
```


# Generics
- TypeScript Generics is a tool which provides a way to create **reusable** components. It creates a component that can work with a **variety of data types** rather than a single data type. It allows users to consume these components and use their own types. Generics ensures that the program is flexible as well as scalable in the long term.
```jsx
EXAMPLE 1: (CUSTOM GENERIC)
function identity<T>(arg: T): T {    
     return arg;    
}    
let output1 = identity<string>("myString");    
let output2 = identity<number>( 100 );  
console.log(output1);  
console.log(output2);

EXAMPLE 2: 
const promise: Promise<string> = new Promise((resolve,reject)=>{
	setTimeout(()=>{
		resolve(20);
		
	},2000);
})
promise.then(data => data.split(" "));

```

##### Note: whats the difference between any and <T> ?