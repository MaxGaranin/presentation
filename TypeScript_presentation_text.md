# TypeScript

## Intro
TypeScript is a compiled language that's default compilation target is JavaScript. It's an open source project started and overseen by Microsoft. It provides built-in support for modern ES-Next features and **static type** system.  

### Why use types?
* Self-documenting
* Avoid common pitfalls
* Wide IDE support

(Self-documenting) Instead of adding comments to your code, with a type system you are annotating your code and it will always be in sync with the actual code.
(Avoid common pitfalls) TypeScript is going to spit out any compilation error unlike JavaScript which is an interpreted language. You can catch errors at the early stage during development, and less bugs
(Wide IDE support) TypeScript has rich support of any modern IDE, for example, VS Code, JetBrains WebStorm etc.

------------------------------------------------------------

## Basics
Every feature provided in TS is non-invading, meaning that it has syntax that doesn't overlap with any part of the JS code. This makes porting your JS app back and forth relatively easily.
You can specify your variable's type using the colon (:) followed by the actual name of the type:
```
const myStringVariable: string = "str";
```
There are 6 basic, primitive types to remember:
* number - represents any kind of numeric value - integer or float, hex, decimal, binary etc.
* string - represents any kind of string value;
* boolean - represents any boolean value, i.e. true or false;
* symbol - represents symbol values;
* null - represents null value only;
* undefined - represents undefined value only;

##### Any
Any type, as the name suggests, indicates any possible value. It serves as a kind-of fallback, allowing you to omit type checking. It's really helpful at the beginning when porting from JS. But, it shouldn't be overused, or even better - it shouldn't be used at all.
```
let myAnyVariable: any = "str";
myAnyVariable = 10;
myAnyVariable = true;
```

------------------------------------------------------------

## Composition types
Now it's time to explore some even more interesting types. Let call them composition types because they're composed of some smaller parts.

### Unions
Unions allow you to specify variable's type that you can assign different types of values to. They work as a list of possible and assignable types. They can be specified by writing your types, divided by the pipe symbol (|).
```
let myUnionVariable: string | number = "str";
myUnionVariable = 10;
myUnionVariable = false; // error
```
Union types have incredible potential. You can use them to handle different types of parameters in functions or replace your any types with these, truly type-safe alternatives.

### Arrays
Here, I'll introduce you to the first way of typing an array value. To denote an array type, you have to write the type for actual values of your array and proceed it by the square brackets symbol ([]).
```
const myStringArrayVariable: string[] = ["str", "str"]; 
```
You can join and use together many of previously met types. You can create a type for an array of strings and numbers with union types, or create a type for an array of literal values.
```
const myUnionArrayVariable: (string | number)[] = ["str", 10];
const myLiteralArrayVariable: ("str")[] = ["str","str"];
```

### Tuples
Structures closely related to arrays, so-called tuples can be utilized to specify a type of an array with a fixed number of elements, with all of them having strictly specified type.
```
const myTupleVariable: [number, string] = [10, "str"];
const myTupleVariable2: [string, number] = [10, "str"]; // error
```
It explains mostly everything. To define a tuple type, you start with square brackets ([]) that are really characteristic for arrays of any kind, any include types for your tuple one by one, separated by commas. It is pretty rationale stuff.

### Enums
Enums are commonly known among statically programming languages communities. They are used to simply provide more friendly names to numeric values. For example, there's a common pattern for requiring different numbers in configuration objects or etc. That's where enums find their use-cases.
```
enum Color {Red, Green, Blue};
```
### Functions
Typing a function is similar to other TS code we've written before. We still have the colons and common type-name syntax but in a different place.
```
function myFunction(myStringArg: string, myNumberArg: number): void
{
	// code
}
```
As you can see, the arguments section of the function is followed by our standard type annotation. It informs the compiler about function's return value type. In the example above it's void. It, in fact, indicates the absence of any type at all. This means that our function above doesn't return anything.  

But what if you want to take an additional, not-required argument? How to note that something is just optional? Easy - by proceeding your argument's name with the question mark (?)!
```
function myFunction(myArg: number, myOptionalArg?: string): void {
	// code
}
```

------------------------------------------------------------

## Classes
As we already know, in TS everything has to have a type. This includes class members. Before accessing any member using this. syntax, you need to first declare our member.
```
class MyClass {
    myStringMember: string = 'str';
    myBooleanMember?: boolean;
    
    constructor() {
        this.myStringMember; // 'str'
        this.myNumberMember = 10; // error
    }
}
```
If you won't declare a property earlier, you'll get an access error. Another thing you can use is the optional sign (?), effectively making your member not-required. Both of these methods make it not needed to assign any value to a particular member in the constructor.

### Modifiers
Being a statically-typed language, TS borrows many ideas from other similar languages. One of which being access modifiers. To use them, you need to specify particular modifier's respective keyword before your class member.
```
class MyClass {
    private myStringMember: string;
    protected myNumberMember: number;
    
    public constructor() {
        this.myStringMember = 'str';
        this.myNumberMember = 10;
    }
}
```
You can use these with properties, methods and even the constructor (with some limits).

##### Public
The default modifier, if there's no directly specified one. Indicates that given member can be accessed publicly, meaning both outside and inside of given class.
```
class MyClass {
    public myStringMember: string = 'str';
    
    public constructor() {
        this.myStringMember; // 'str'
    }
}

new MyClass().myStringMember; // 'str'
```
It's also one of the two modifiers that can be applied to the constructor (and is by default). Public constructor allows your class to be instantiated anywhere in your code.

##### Private
Private modifier limits the accessibility of class member to only inside of the class. Accessing it outside will throw an error. It follows the OOP principle of encapsulation allowing you to hide information that isn't required outside of given scope.
```
class MyClass {
    private myStringMember: string = 'str';
    
    constructor() {
        this.myStringMember; // 'str'
    }
}

new MyClass().myStringMember; // error
```
In general, this technique is very useful. Too bad that there's no direct equivalent of it in JS. And although there is a proposal for that, for now, closures seems like the only alternative. That's why in the output of TS compiler, all members are publicly accessible anyway.

##### Protected
Protected modifier serves as a middle-ground between the private and public one. Protected members are accessible inside the class and all of its derivatives (unlike private).
```
class MyClass {
    protected myStringMember: string = 'str';
    
    protected constructor() {
        this.myStringMember; // 'str'
    }
}

class MyInheritedClass extends MyClass {
    public constructor() {
        super();
        this.myStringMember; // 'str'
    }
}

new MyClass(); // error

const instance = new MyInheritedClass();
instance.myStringMember; // error
```

##### Read-only
Beyond accessibility modifiers, TS provides us 2 more: readonly and static. Although static is a part of JS, readonly is not. As the name suggests, it allows indicating a particular member as, obviously, read-only. Thus, making it assignable only when declaring and in the constructor.
```
class MyClass {
    readonly myStringMember: string = 'str';
    
    constructor() {
        this.myStringMember = 'string'
    }
    
    myMethod(): void {
        this.myStringMember = 'str'; // error
    }
}
```

### Abstract classes
Abstract classes are nothing more than classes that cannot be instantiated by themselves and thus, serve only as a reference for other, inherited classes. As for the syntax, everything new that comes with abstract classes is the abstract keyword. It's used to define the class itself and its particular members.

abstract class MyAbstractClass {
    abstract myAbstractMethod(): void;
    abstract myAbstractStringMember: string;
    
    constructor() {
        this.myMethod();
    }
    
    myMethod() {
        this.myAbstractMethod();
    }
}

-----------------------------------------------------------

## Interfaces
Interfaces allow you to define and work with the shape of value, rather than the value itself.

Interfaces are commonly used to describe the shape of complex structures, like objects and classes. They indicate what publicly available properties/members the end structure need to have. To define one you must use interface keyword and proper syntax:
```
interface MyInterface {
    readonly myStringProperty: string;
    myNumberProperty?: number;
    
    myMethodProperty(): void
}
```
Inside the interface declaration, we can use most of the previously learned TS syntax - more specifically read-only and optional properties. Interfaces can also include methods that our future structures will need to implement.
One of the main use-cases of the interfaces is as a type.

### Inheritance of interfaces
Interfaces just like classes can extend each other and classes' properties too! You can make your interface extend one, or even more (not possible in classes) interfaces with simple extends keyword syntax. In this case, properties shared by extended interfaces are combined into single ones.
```
interface MyCombinedInterface extends MyInterface, MyHybridInterface {
    myBooleanProperty: boolean;
}
```

### Inheritance of classes
Interfaces and classes share a special relation. From their declaration syntax alone you can see the similarities. That's because classes can implement interfaces.
```
class MyClass implements MyInterface {
    myStringProperty: string = 'str';
    myNumberProperty: number = 10;
}
```
By using the implements keyword, you indicate that given class must have all properties implemented as described in a particular interface. This allows you to later define your variables more swiftly.

---------------------------------------------------------

## Conclusion
TypeScript is a language that helps us write better, safer, and more concise code, improving our experience and removing some dumb errors that we can make along the way. 