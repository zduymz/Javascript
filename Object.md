

# Object

In JS, the simple types are number, string, boleans, null and undefined.
All other values are objects.
There are 2 atomic mechanisms that are used to create objects:
1. Literal
2. Functional

## Create object from literal
```javascript
var person = {
	"Name": "Duy",
	"do_writing": function() { console.log("Duy is writing document");}
}
```

## Prototype Link
Every object has a __hidden link__ to another object called *prototype link*. Objects by default are linked to the object **Object.prototype**.
The prototype link is only used when retreiving property values from an object.
When retrieving a property value from an object, if it can not be found, it will look for it the prototype object that it is linked to.
```javascript
person.age; // undefined

Object.prototype.age = 10; //Every object is linked to Object.prototype by default
person.age; // return 10
```
Therefore the prototype linkage is used to __augment__ an object with addtional properties.

## Create Funtion Object
Funtion in JS are oject, and are *first class citizens*. A variable can be assigned a function object like:
```javascript
var add = function(a, b) {
	return a + b;
}
typeof add //function - which is an object in JS
```
Since function is object, they too have a prototype link. But where 
- object literals are default linked to **Object.prototype**
- function objects are linked to **Funtion.prototype**

Now, it starts get confusing here, function objects have in addition to a prototype link a property called **protype**. This property (again, this prototype property is not the prototype link) can be manipulated.

Just like *Object.prototype* is used to augment objects with properties, so *Function.prototype* is used to augment functions with properties.

```javascript
var result = add(2,3); // return 5

//Add a function to Function.prototype
Funtction.prototype.substract = function(a, b) {
	return a - b;
}

//Now, we can use substract on any funtion object
result = add.substract(5,2); // return 3
```
**Function.prototype** is __itself an object__, and therefore it is linked to **Object.prototype**.
```javascript
Object.prototype.denominator = 5;
Object.prototype.divide = function(a) {
	return a/denominator;
}

var result = add.divide(50); //return 10
```
The process described above is the mainstay of  **__Prototype Based Programming__**, and it is called *prototype chaining*.

## Create Objects From Functions
Above section we talked about creating function objects.
And here we create a new object using a function.

To recap from the last section, this is how *funtion object* is created:
```javascript
var Bird = function(name){
    this.name = name;
}
```
The variable Animal:
- is a function and an object (remember, everything in JS is object)
- it has a *prototype property*, and its prototype link is set to *Function.prototype* by default

Something special happens here if the function is invoked with *new* (called the __constructor invocation pattern__)
```javascript
var mocking = new Bird('Hunger'); //using new to invoke the function
```
What happens here is *new* creates an object from function object Bird.
Its power comes in because it will use Bird's *prototype property* as the *prototype link* for the new object.

### Problem with using "constructor invocation pattern"
Using *new* i one of four ways in which to invoke a function, the most popular way being call the function.
```javascript
var Vehicle = function(type) {
    this.type = type;
    return type;
}

var car = Vehicle('ToYota'); //Normal function call
var plane = new Vehicle('Boeing');
```
*car* was assigned the result of invoking Vehicle as a normal function, but *plane* on the other was assigned the result of using the *constructor invocation pattern* which is an object (the return is ignored). This is terrible, and it is very easy to make a mistake.

Objects cann now be created with a chosen prototype link by doing the following:
```javascript
var animal = {
    "type": "cat",
    "make_sound": function() {
        return 'prrrrrr';
    }
}

var cat = Object.create(animal);
cat.make_sound(); //prrrrrr
cat.type; //cat
```

# WrapUp
| | Object Literal | Function Object | Object from Function |
| --- | :---: | :---: | :---: |
|Construction| var x = {} | var x = function(){} | var x = new FunctionObject() |
| Prototype Link | Object.prototype | Function.prototype | Whatever the function's prototype property is set to (Object.prototype by default) |
| Prototype Property | not exist | exist | not exist |
