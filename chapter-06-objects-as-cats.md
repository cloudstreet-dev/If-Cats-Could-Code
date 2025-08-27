# Chapter 6: Objects as Individual Cats

## The Opening Tail

Professor Whiskers wasn't just any cat. He was a distinguished, 7-year-old Maine Coon with luxurious silver fur, piercing green eyes, and an attitude that suggested he owned the entire university where he resided. Unlike his simpler days as a kitten, when he was just "that gray fuzzy thing," Professor Whiskers had developed into a complex individual with specific characteristics and behaviors.

He had a morning routine: stretch, groom whiskers, inspect his food bowl, judge the graduate students. He could purr at varying frequencies, meow in different tones for different demands, and had mastered the art of opening doors with his massive paws. His favorite sleeping spot was the sunny windowsill in the philosophy department, and his least favorite thing was dogs - particularly loud ones.

Professor Whiskers was more than just a name in a list. He was a complete entity with properties (age, breed, color) and abilities (purr, meow, open doors). He was, in programming terms, an object - a self-contained bundle of related information and capabilities.

## The Technical Purr-spective

In programming, an object is like Professor Whiskers - a complex entity that bundles related properties (data) and methods (functions) together. While an array might store a list of cat names, an object represents a complete cat with all its characteristics and behaviors in one organized package.

### What Makes an Object?

Objects have two main components:

1. **Properties**: Characteristics or attributes (like age, color, name)
2. **Methods**: Actions or behaviors the object can perform (like meow, sleep, hunt)

Think of properties as describing what the cat IS, and methods as describing what the cat CAN DO.

### Object Syntax: The Cat Blueprint

In JavaScript, we create objects using curly braces `{}` and define properties and methods inside:

```javascript
let objectName = {
    property1: value1,
    property2: value2,
    methodName: function() {
        // what the method does
    }
};
```

## Code in the Wild

```javascript
// Professor Whiskers as an object
let professorWhiskers = {
    // Properties (what he IS)
    name: "Professor Whiskers",
    age: 7,
    breed: "Maine Coon",
    color: "silver",
    weight: 15.5,
    favoriteSpot: "philosophy department windowsill",
    isIndoor: true,
    energyLevel: "dignified",
    
    // Methods (what he CAN DO)
    meow: function() {
        console.log(this.name + " says: Meow! (in a distinguished manner)");
    },
    
    purr: function() {
        console.log(this.name + " purrs contentedly at " + this.age + " years old");
    },
    
    sleep: function(hours) {
        console.log(this.name + " naps for " + hours + " hours in the " + this.favoriteSpot);
    },
    
    introduce: function() {
        return `Hello, I am ${this.name}, a ${this.age}-year-old ${this.color} ${this.breed}.`;
    }
};

// Using the object
console.log(professorWhiskers.introduce());
professorWhiskers.meow();
professorWhiskers.sleep(3);
```

### Accessing Object Properties: Getting to Know the Cat

```javascript
let mittens = {
    name: "Mittens",
    age: 2,
    color: "gray",
    isHungry: true
};

// Dot notation (most common)
console.log("Cat name: " + mittens.name);        // "Mittens"
console.log("Cat age: " + mittens.age);          // 2
console.log("Is hungry: " + mittens.isHungry);   // true

// Bracket notation (useful for dynamic property names)
console.log("Cat color: " + mittens["color"]);   // "gray"

// Dynamic property access
let propertyToCheck = "age";
console.log("Dynamic access: " + mittens[propertyToCheck]);  // 2

// Checking if property exists
if ("name" in mittens) {
    console.log("This cat has a name: " + mittens.name);
}

// Getting all property names
let properties = Object.keys(mittens);
console.log("Cat properties: " + properties);  // ["name", "age", "color", "isHungry"]
```

### Modifying Objects: Cats Change Over Time

```javascript
let growingKitten = {
    name: "Patches",
    age: 0.5,
    weight: 2,
    skills: ["sleep", "eat"]
};

// Changing existing properties
growingKitten.age = 1.0;           // Happy birthday!
growingKitten.weight = 4;          // Growing strong
console.log(`${growingKitten.name} is now ${growingKitten.age} years old`);

// Adding new properties (learning new skills)
growingKitten.favoriteFood = "tuna";
growingKitten.canOpenDoors = false;
growingKitten.skills.push("play", "climb");

// Adding methods later
growingKitten.celebrate = function() {
    console.log(`${this.name} does happy zoomies!`);
};

console.log(growingKitten);
growingKitten.celebrate();

// Removing properties (cat loses interest in something)
delete growingKitten.canOpenDoors;
console.log("Can open doors: " + growingKitten.canOpenDoors);  // undefined
```

### The Magic of `this`: Understanding Cat Self-Awareness

```javascript
let selfAwareCat = {
    name: "Socrates",
    age: 5,
    philosophy: "I think, therefore I am... hungry",
    
    contemplate: function() {
        // 'this' refers to the current object (selfAwareCat)
        console.log(`${this.name} thinks: "${this.philosophy}"`);
        console.log(`At ${this.age} years old, I have achieved enlightenment.`);
    },
    
    hasAnotherBirthday: function() {
        this.age += 1;  // Modify this cat's age
        console.log(`${this.name} is now ${this.age} years old and wiser.`);
    },
    
    compareAge: function(otherCat) {
        if (this.age > otherCat.age) {
            console.log(`${this.name} is older than ${otherCat.name}`);
        } else if (this.age < otherCat.age) {
            console.log(`${this.name} is younger than ${otherCat.name}`);
        } else {
            console.log(`${this.name} and ${otherCat.name} are the same age`);
        }
    }
};

selfAwareCat.contemplate();
selfAwareCat.hasAnotherBirthday();

// Using one cat to compare with another
let youngCat = { name: "Kitten", age: 1 };
selfAwareCat.compareAge(youngCat);
```

### Object Constructors: Creating Many Similar Cats

```javascript
// Constructor function (traditional way)
function Cat(name, age, breed, color) {
    this.name = name;
    this.age = age;
    this.breed = breed;
    this.color = color;
    this.energy = 100;
    
    this.meow = function() {
        console.log(`${this.name} meows!`);
    };
    
    this.sleep = function(hours) {
        this.energy = Math.min(100, this.energy + (hours * 10));
        console.log(`${this.name} slept for ${hours} hours. Energy: ${this.energy}`);
    };
    
    this.play = function(minutes) {
        this.energy = Math.max(0, this.energy - (minutes * 2));
        console.log(`${this.name} played for ${minutes} minutes. Energy: ${this.energy}`);
    };
}

// Creating cats using the constructor
let cat1 = new Cat("Whiskers", 3, "Tabby", "brown");
let cat2 = new Cat("Shadow", 2, "Black Cat", "black");
let cat3 = new Cat("Cream", 1, "Persian", "white");

cat1.meow();
cat2.play(15);
cat3.sleep(2);

// Modern ES6 Class syntax (cleaner way)
class ModernCat {
    constructor(name, age, breed, color) {
        this.name = name;
        this.age = age;
        this.breed = breed;
        this.color = color;
        this.energy = 100;
        this.mood = "content";
    }
    
    meow() {
        console.log(`${this.name} says meow in a ${this.mood} way!`);
    }
    
    sleep(hours) {
        this.energy = Math.min(100, this.energy + (hours * 10));
        this.mood = "relaxed";
        console.log(`${this.name} had a nice ${hours}-hour nap.`);
    }
    
    play(minutes) {
        this.energy = Math.max(0, this.energy - (minutes * 2));
        this.mood = this.energy > 50 ? "playful" : "tired";
        console.log(`${this.name} played and is now ${this.mood}.`);
    }
    
    introduce() {
        return `Hi! I'm ${this.name}, a ${this.age}-year-old ${this.color} ${this.breed}. I'm feeling ${this.mood}.`;
    }
}

// Creating modern cats
let modernCat = new ModernCat("Luna", 4, "Siamese", "cream");
console.log(modernCat.introduce());
modernCat.play(30);
modernCat.meow();
```

### Complex Objects: Cats with Rich Inner Lives

```javascript
let complexCat = {
    // Basic info
    name: "Einstein",
    age: 6,
    breed: "Russian Blue",
    
    // Complex properties
    personality: {
        intelligence: 9,
        friendliness: 6,
        independence: 8,
        playfulness: 4
    },
    
    favorites: {
        foods: ["salmon", "chicken", "catnip"],
        toys: ["feather wand", "laser pointer", "cardboard box"],
        sleepingSpots: ["sunny window", "human's bed", "top of bookshelf"]
    },
    
    skills: {
        canOpenDoors: true,
        canUseToilet: false,
        canCatch: ["mice", "birds", "laser dots"],
        tricks: ["sit", "high five", "come when called"]
    },
    
    // Complex methods
    displayPersonality: function() {
        console.log(`${this.name}'s personality:`);
        for (let trait in this.personality) {
            console.log(`  ${trait}: ${this.personality[trait]}/10`);
        }
    },
    
    randomFavorite: function(category) {
        let items = this.favorites[category];
        let random = Math.floor(Math.random() * items.length);
        return items[random];
    },
    
    learnTrick: function(newTrick) {
        if (!this.skills.tricks.includes(newTrick)) {
            this.skills.tricks.push(newTrick);
            console.log(`${this.name} learned "${newTrick}"! Current tricks: ${this.skills.tricks.join(", ")}`);
        } else {
            console.log(`${this.name} already knows "${newTrick}"`);
        }
    },
    
    getFullProfile: function() {
        return {
            name: this.name,
            age: this.age,
            breed: this.breed,
            personalityScore: Object.values(this.personality).reduce((sum, val) => sum + val, 0),
            totalTricks: this.skills.tricks.length,
            favoriteFood: this.randomFavorite("foods")
        };
    }
};

complexCat.displayPersonality();
console.log(`${complexCat.name} wants: ` + complexCat.randomFavorite("foods"));
complexCat.learnTrick("roll over");
console.log(complexCat.getFullProfile());
```

### Object Relationships: Cats and Their World

```javascript
// Objects can contain other objects
let household = {
    address: "123 Cat Street",
    humans: [
        { name: "Alice", role: "primary caretaker" },
        { name: "Bob", role: "treat dispenser" }
    ],
    
    cats: [
        {
            name: "Whiskers",
            age: 3,
            favoriteHuman: "Alice",
            relationship: "bonded pair"
        },
        {
            name: "Shadow", 
            age: 2,
            favoriteHuman: "Bob",
            relationship: "playful companion"
        }
    ],
    
    routine: {
        morning: ["feed cats", "clean litter", "play time"],
        evening: ["dinner", "grooming", "cuddle time"]
    },
    
    introduceCats: function() {
        console.log(`Welcome to ${this.address}! We have ${this.cats.length} cats:`);
        this.cats.forEach(cat => {
            console.log(`- ${cat.name}, age ${cat.age}, loves ${cat.favoriteHuman}`);
        });
    },
    
    findCat: function(name) {
        return this.cats.find(cat => cat.name === name);
    },
    
    addCat: function(newCat) {
        this.cats.push(newCat);
        console.log(`${newCat.name} has joined the household!`);
    }
};

household.introduceCats();

let whiskers = household.findCat("Whiskers");
console.log(`Found: ${whiskers.name}, who is ${whiskers.age} years old`);

household.addCat({ name: "Mittens", age: 1, favoriteHuman: "Alice", relationship: "new kitten" });
```

## Paw-sible Problems

### Exercise 1: Create Your Dream Cat
Create an object representing your ideal cat with at least 5 properties and 3 methods.

```javascript
let dreamCat = {
    // Add properties here
    
    // Add methods here
};

// Test your cat
```

### Exercise 2: Cat Aging System
Create a cat object that can age, with methods to have birthdays and check if it's a senior cat (7+ years).

```javascript
// Create a cat that can age and tell you its life stage
// Implement methods for: birthday(), getLifeStage(), and introduce()
```

### Exercise 3: Cat Interaction
Create two cat objects that can interact with each other.

```javascript
// Create two cats that can:
// - Greet each other
// - Play together (affects both cats' energy)
// - Compare ages
```

### Exercise 4: Cat Shelter Management
Create a shelter object that manages multiple cats.

```javascript
// Create a shelter that can:
// - Add new cats
// - Find cats by name or characteristics  
// - List available cats
// - Handle adoptions
```

### Exercise 5: Advanced Cat Personality
Create a cat with a complex personality system that affects its behavior.

```javascript
// Create a cat with:
// - Multiple personality traits
// - Mood that changes based on interactions
// - Different responses based on current mood
// - Methods that modify personality over time
```

## Cat-ch the Pattern

### Key Takeaways

1. **Objects bundle related data and behavior**: Like real cats, they combine characteristics and abilities
2. **Properties describe, methods act**: Properties are what the object IS, methods are what it CAN DO
3. **`this` provides self-reference**: Allows objects to work with their own properties
4. **Objects can be complex and nested**: Real-world entities often have multiple layers of information
5. **Constructors create similar objects**: Useful when you need many objects with the same structure

### Object Creation Patterns

**Object Literal** (single, unique object):
```javascript
let uniqueCat = { name: "Special", age: 5 };
```

**Constructor Function** (multiple similar objects):
```javascript
function Cat(name, age) {
    this.name = name;
    this.age = age;
}
let cat1 = new Cat("Whiskers", 3);
```

**ES6 Class** (modern, clean syntax):
```javascript
class Cat {
    constructor(name, age) {
        this.name = name;
        this.age = age;
    }
}
```

### Common Object Patterns

**Property Access**:
```javascript
obj.property        // Dot notation (when you know the property name)
obj["property"]     // Bracket notation (when property name is dynamic)
obj[variable]       // Using variable as property name
```

**Property Checks**:
```javascript
"property" in obj           // Does property exist?
obj.hasOwnProperty("prop")  // Does object directly have this property?
Object.keys(obj)           // Get all property names
Object.values(obj)         // Get all property values
```

### Common Mistakes to Avoid

```javascript
// Forgetting 'this' in methods
let badCat = {
    name: "Confused",
    speak: function() {
        console.log(name + " says meow");  // Error! 'name' is undefined
        console.log(this.name + " says meow");  // Correct!
    }
};

// Arrow functions don't have 'this' context
let problematicCat = {
    name: "Problem",
    speak: () => {
        console.log(this.name);  // 'this' doesn't refer to the object!
    },
    speakCorrectly: function() {
        console.log(this.name);  // This works!
    }
};

// Modifying objects passed to functions (they're references!)
function changeCat(cat) {
    cat.name = "Changed";  // This modifies the original object!
}

let myCat = { name: "Original" };
changeCat(myCat);
console.log(myCat.name);  // "Changed" - original was modified!

// To avoid this, create a copy:
function safeCatChange(cat) {
    let catCopy = { ...cat };  // Create shallow copy
    catCopy.name = "Changed";
    return catCopy;
}
```

### Object Best Practices

```javascript
// Use meaningful property names
let goodCat = {
    name: "Whiskers",
    age: 3,
    energyLevel: 8,
    isHungry: true
};

// Not like this:
let badCat = {
    n: "Whiskers",
    a: 3,
    e: 8,
    h: true
};

// Group related properties
let wellOrganizedCat = {
    basicInfo: {
        name: "Organized",
        age: 4,
        breed: "Tabby"
    },
    status: {
        hunger: 6,
        energy: 8,
        mood: "playful"
    },
    preferences: {
        favoriteFood: "tuna",
        favoriteSpot: "windowsill"
    }
};

// Keep methods focused and single-purpose
let focusedCat = {
    name: "Focused",
    energy: 100,
    
    // Good: single responsibility
    eat: function() { /* just handle eating */ },
    sleep: function() { /* just handle sleeping */ },
    play: function() { /* just handle playing */ },
    
    // Avoid: methods that do too many things
    // doEverything: function() { /* eat, sleep, play, etc. - too much! */ }
};
```

### Looking Ahead

Objects are the building blocks of complex programs, just as individual cats are the foundation of understanding cat behavior. In the next chapter, we'll explore how objects can be organized and accessed in structured ways, much like cats organize themselves in towers, queues, and hierarchical social structures.

We'll learn about stacks and queues - fundamental data structures that help us manage objects in ordered, predictable ways, much like cats naturally form feeding lines and establish pecking orders.

---

*Next Chapter: [Stacks, Queues, and Cat Towers](chapter-07-stacks-and-queues.md)*

*Previous: [Arrays as a Litter of Kittens](chapter-05-arrays-as-litter.md)*