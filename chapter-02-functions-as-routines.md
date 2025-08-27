# Chapter 2: Functions as Daily Routines

## The Opening Tail

Every morning at 6:47 AM sharp, Shadow begins his routine. Not 6:45. Not 6:50. Precisely 6:47.

First, he stretches - front paws forward, back arched high, tail quivering. Then, he pads softly to his human's bedroom door and releases a singular, piercing meow. If no response comes within 30 seconds, he repeats the meow, slightly louder. This continues with increasing volume until the door opens.

Once successful, Shadow leads his bleary-eyed human to the kitchen, where the feeding ritual begins. Circle the food bowl three times. Sniff suspiciously. Look at human with disappointment (regardless of the food quality). Finally, eat exactly half the portion, saving the rest for later.

This routine never varies. Give Shadow the same inputs (morning time, closed bedroom door, empty food bowl), and you'll get the same outputs (loud cat, awakened human, half-eaten breakfast). It's as predictable as code itself.

Shadow has other routines too: the afternoon window patrol, the evening zoomies, the midnight water fountain investigation. Each one is a specific set of actions that produces a predictable result. Each one is, in essence, a function.

## The Technical Purr-spective

Functions are the building blocks of organized code. Like a cat's daily routines, they're reusable sets of instructions that perform specific tasks. You define them once and can use them whenever needed.

### What Makes a Function?

A function consists of:

1. **A Name**: How we call upon it (like "morningRoutine")
2. **Parameters**: What information it needs (time of day, hunger level)
3. **Actions**: What it does (the actual routine)
4. **Return Value**: What it gives back (a fed cat, an annoyed human)

Think of it this way: A function is like teaching your cat a trick. Once they learn "sit" or "high five," you can ask for it anytime, and they'll (hopefully) perform the same action.

## Code in the Wild

### Creating Your First Function

```javascript
// Shadow's morning meow routine
function morningMeow() {
    console.log("Meow!");
    console.log("Meow!!");
    console.log("MEOW!!!");
}

// Execute the routine
morningMeow();  // This "calls" or "invokes" the function
```

### Functions with Parameters: Customizable Routines

Just as cats adjust their behavior based on circumstances, functions can accept inputs:

```javascript
// A more flexible meowing function
function meowWithVolume(volume) {
    if (volume === "quiet") {
        console.log("mew");
    } else if (volume === "normal") {
        console.log("Meow");
    } else if (volume === "loud") {
        console.log("MEOOOOOW!!!");
    }
}

// Different situations call for different volumes
meowWithVolume("quiet");   // Asking nicely
meowWithVolume("normal");  // Standard request
meowWithVolume("loud");    // It's been 5 whole minutes since breakfast
```

### Multiple Parameters: Complex Behaviors

```javascript
// Feeding routine with multiple inputs
function feedCat(catName, foodType, amount) {
    console.log(catName + " is approaching the food bowl");
    console.log("Sniffing the " + foodType);
    console.log("Eating " + amount + " of the food");
    console.log(catName + " walks away, leaving half for later");
}

feedCat("Shadow", "salmon pate", "most");
feedCat("Mittens", "chicken chunks", "a little");
feedCat("Tiger", "tuna surprise", "everything");
```

### Return Values: Getting Something Back

Functions can give us information back, like a cat bringing you a "present":

```javascript
// Calculate how many treats a cat deserves
function calculateTreats(goodBehaviors, badBehaviors) {
    let treatCount = goodBehaviors * 2 - badBehaviors;
    return treatCount;  // Send this value back
}

let shadowTreats = calculateTreats(5, 2);  // 5 good things, 2 bad things
console.log("Shadow gets " + shadowTreats + " treats");  // Shadow gets 8 treats

let mittensТreats = calculateTreats(3, 0);  // Perfect angel
console.log("Mittens gets " + mittensТreats + " treats");  // Mittens gets 6 treats
```

### Functions Calling Functions: Routine Chains

Just as one cat behavior triggers another, functions can call other functions:

```javascript
// Individual routines
function stretchRoutine() {
    console.log("Streeeeetch front paws");
    console.log("Arch back hiiiigh");
    console.log("Wiggle tail");
}

function demandAttention() {
    console.log("Meow!");
    console.log("Paw at human");
    console.log("MEOW LOUDER!");
}

function eatBreakfast() {
    console.log("Sniff food suspiciously");
    console.log("Eat exactly half");
    console.log("Walk away dramatically");
}

// The complete morning routine
function completeMorningRoutine() {
    console.log("=== Morning Routine Starting ===");
    stretchRoutine();
    demandAttention();
    eatBreakfast();
    console.log("=== Morning Routine Complete ===");
}

// One function triggers the entire sequence
completeMorningRoutine();
```

### Arrow Functions: Modern Cat Style

JavaScript has a shorter way to write functions, like a cat's efficient movements:

```javascript
// Traditional function
function purr(happiness) {
    return "purr".repeat(happiness);
}

// Arrow function (same result, sleeker syntax)
const modernPurr = (happiness) => "purr".repeat(happiness);

console.log(purr(3));        // "purrpurrpurr"
console.log(modernPurr(3));  // "purrpurrpurr"

// Even shorter for simple operations
const quickMeow = () => console.log("Meow!");
const doubleТreats = treats => treats * 2;

quickMeow();  // "Meow!"
console.log(doubleТreats(3));  // 6
```

### Default Parameters: Standard Cat Settings

Sometimes cats have default behaviors when no specific instruction is given:

```javascript
// Nap function with default duration
function takeNap(duration = 2) {
    console.log("Napping for " + duration + " hours");
    console.log("ZZZ".repeat(duration));
}

takeNap();     // Uses default: "Napping for 2 hours"
takeNap(4);    // "Napping for 4 hours"
takeNap(0.5);  // "Napping for 0.5 hours" (quick catnap)
```

### Real-World Cat Functions

```javascript
// Check if it's feeding time
function isFeedingTime(currentHour) {
    if (currentHour === 6 || currentHour === 12 || currentHour === 18) {
        return true;
    }
    return false;
}

// Calculate cat age in human years
function catToHumanYears(catAge) {
    if (catAge === 1) {
        return 15;
    } else if (catAge === 2) {
        return 24;
    } else {
        return 24 + (catAge - 2) * 4;
    }
}

// Determine cat mood based on various factors
function determineMood(hoursSinceFood, hoursSlept, toysAvailable) {
    let moodScore = 10;
    
    moodScore -= hoursSinceFood * 2;  // Hungrier = grumpier
    moodScore += hoursSlept;          // More sleep = happier
    moodScore += toysAvailable * 0.5; // Toys help a little
    
    if (moodScore > 8) {
        return "playful";
    } else if (moodScore > 5) {
        return "content";
    } else if (moodScore > 2) {
        return "grumpy";
    } else {
        return "plotting your demise";
    }
}

// Test the mood function
let currentMood = determineMood(3, 14, 5);
console.log("Cat mood: " + currentMood);
```

## Paw-sible Problems

### Exercise 1: The Greeting Function
Create a function that greets a cat by name and mentions their favorite activity.

```javascript
// Create a function called greetCat that takes a name and activity
// It should print a personalized greeting
// Example: greetCat("Fluffy", "chasing lasers") 
// Output: "Hello Fluffy! Time for chasing lasers!"
```

### Exercise 2: The Treat Calculator
Write a function that determines how many treats to give based on the time of day.

```javascript
// Morning (6-12): 2 treats
// Afternoon (12-17): 1 treat  
// Evening (17-22): 3 treats
// Night (22-6): 0 treats (sleeping time!)
```

### Exercise 3: The Playtime Scheduler
Create a function that returns whether it's a good time to play based on the cat's energy level (1-10) and the time since last meal.

```javascript
// If energy > 7 and time since meal > 1 hour: "Great time to play!"
// If energy > 5 and time since meal > 2 hours: "Could play a little"
// Otherwise: "Better let them rest"
```

### Exercise 4: The Multi-Cat Household
Create functions to manage multiple cats' feeding times, ensuring no cat is forgotten.

```javascript
// Create a function for each cat's feeding routine
// Create a master function that calls all individual routines
// Add a function to check if all cats have been fed
```

## Cat-ch the Pattern

### Key Takeaways

1. **Functions are reusable routines**: Write once, use many times (like teaching a trick)
2. **Parameters make functions flexible**: Same routine, different details (like meowing at different volumes)
3. **Return values provide feedback**: Functions can send information back (like a cat bringing you a toy)
4. **Functions organize code**: Break complex behaviors into simple, manageable pieces
5. **Functions can call other functions**: Build complex behaviors from simple ones

### Function Best Practices (The Well-Trained Cat)

```javascript
// Good: Clear, single purpose
function calculateCatAge(birthYear) {
    const currentYear = new Date().getFullYear();
    return currentYear - birthYear;
}

// Bad: Trying to do too much
function doEverything(cat, food, toy, time) {
    // Feed cat, play with cat, groom cat, put cat to bed...
    // Too many responsibilities!
}

// Good: Descriptive names
function checkIfHungry(lastFeedingTime) {
    // Clear what this does
}

// Bad: Unclear names
function doThing(x) {
    // What thing? What's x?
}
```

### Common Function Patterns

```javascript
// The Guard Pattern (Like a cat checking if it's safe)
function safeToPounce(targetSize, distanceAway) {
    if (targetSize > 10) {
        return false;  // Too big!
    }
    if (distanceAway > 5) {
        return false;  // Too far!
    }
    return true;  // Perfect pouncing opportunity
}

// The Accumulator Pattern (Like collecting toys)
function countTotalToys(toyBox) {
    let total = 0;
    // Imagine going through each toy
    total = toyBox.mice + toyBox.balls + toyBox.strings;
    return total;
}

// The Transformer Pattern (Like mood changes)
function adjustMoodAfterTreats(currentMood) {
    if (currentMood === "grumpy") {
        return "content";
    } else if (currentMood === "content") {
        return "happy";
    } else {
        return "ecstatic";
    }
}
```

### Debugging Functions (When the Cat Won't Cat)

```javascript
// Use console.log to understand what's happening
function mysteriousCatBehavior(input) {
    console.log("Input received:", input);
    
    let result = input * 2;
    console.log("After doubling:", result);
    
    result = result + 10;
    console.log("After adding 10:", result);
    
    return result;
}

// Test with different inputs to understand the function
console.log(mysteriousCatBehavior(5));
```

### Looking Ahead

Now that you understand functions as the daily routines that bring order to chaos, you're ready to explore loops - the repetitive behaviors that cats (and programs) perform over and over. 

In the next chapter, we'll follow cats on their territorial patrols, watching them check the same spots repeatedly, just like loops check conditions and repeat actions. You'll learn why cats walk the same path every day and how this behavior mirrors the way computers handle repetitive tasks.

After all, whether it's a cat circling their bed exactly three times before lying down or a program processing each item in a list, repetition is a fundamental part of both feline and programming life.

---

*Next Chapter: [Loops and Territorial Patrols](chapter-03-loops-and-patrols.md)*

*Previous: [Variables as Toys](chapter-01-variables-as-toys.md)*