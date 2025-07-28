Classes in JS:

   Before the 'class' keyword (ES5), constructor functions were used:

// Constructor function
function Person(name, age) {
    this.name = name;
    this.age = age;
}

Person.prototype.greet = function() {
    console.log(`Hello, my name is ${this.name} and I am ${this.age} years old.`);
};

const john = new Person('John', 30);
john.greet(); // Output: Hello, my name is John and I am 30 years old.


    the 'class' keyword (ES6):
// Class syntax
class Person {
    constructor(name, age) {
        this.name = name;
        this.age = age;
    }

    greet() {
        console.log(`Hello, my name is ${this.name} and I am ${this.age} years old.`);
    }
}

const jane = new Person('Jane', 25);
jane.greet(); // Output: Hello, my name is Jane and I am 25 years old.



Prototypes:

In JavaScript, every object has a hidden internal property called [[Prototype]] that points to another object.
This is known as the prototype chain.
When you try to access a property on an object, JavaScript will look up the prototype chain until it finds the property or reaches the end of the chain.


const animal = {
    eats: true
};

const rabbit = {
    jumps: true,
    __proto__: animal
};

console.log(rabbit.eats); // true
console.log(rabbit.jumps); // true





Real-Life Code Samples

Using Classes

class Animal {
    constructor(name) {
        this.name = name;
    }

    speak() {
        console.log(`${this.name} makes a noise.`);
    }
}

class Dog extends Animal {
    speak() {
        console.log(`${this.name} barks.`);
    }
}

const dog = new Dog('Rex');
dog.speak(); // Output: Rex barks.




Using Prototypes

function Animal(name) {
    this.name = name;
}

Animal.prototype.speak = function() {
    console.log(`${this.name} makes a noise.`);
};

function Dog(name) {
    Animal.call(this, name);
}

Dog.prototype = Object.create(Animal.prototype);
Dog.prototype.constructor = Dog;

Dog.prototype.speak = function() {
    console.log(`${this.name} barks.`);
};

const dog = new Dog('Rex');
dog.speak(); // Output: Rex barks.