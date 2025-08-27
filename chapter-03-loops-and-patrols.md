# Chapter 3: Loops and Territorial Patrols

## The Opening Tail

Every evening at dusk, Whiskers begins her patrol. Starting at the back door, she follows the exact same path she's followed for three years: along the fence line, pause at the corner to sniff, continue to the garden shed, investigate the space underneath, circle the birdbath twice (always clockwise), check each of the four flower beds, and finally return to the back door.

Then she does it again. And again. Three complete circuits, every single evening.

Her indoor patrol is equally methodical. Check the food bowl. Walk to the water fountain. Inspect the litter box. Survey the living room from the cat tree. Look out the front window. Return to food bowl. Repeat until satisfied that all is well in her domain.

Sometimes the patrol finds something interesting - a new smell by the fence on round two, or a suspicious leaf by the shed on round three. Sometimes she patrols extra times when she's feeling particularly vigilant. But the pattern remains: repeat the same actions while conditions are met.

Whiskers has discovered the power of loops.

## The Technical Purr-spective

Loops are one of the most powerful concepts in programming. They allow us to repeat actions without writing the same code over and over. Just as cats patrol their territory in cycles, loops cycle through code until certain conditions are met.

### Types of Loops: Different Patrol Patterns

JavaScript offers several types of loops, each suited for different situations:

1. **for loop**: When you know how many times to repeat (like circling the bed exactly 3 times)
2. **while loop**: When you repeat until something happens (like meowing until fed)
3. **do...while loop**: When you must do something at least once (like checking the food bowl)
4. **for...of loop**: When you need to check each item in a collection (like inspecting each toy)

## Code in the Wild

### The Basic For Loop: Counting Patrols

```javascript
// Whiskers patrols the yard 3 times
for (let patrol = 1; patrol <= 3; patrol++) {
    console.log("Patrol #" + patrol + " starting...");
    console.log("Checking fence line");
    console.log("Investigating garden shed");
    console.log("Circling birdbath");
    console.log("Patrol #" + patrol + " complete!\n");
}
```

Let's break down this loop:
- `let patrol = 1`: Start counting from patrol 1
- `patrol <= 3`: Keep going while patrol number is 3 or less
- `patrol++`: After each patrol, increase the count by 1

### The While Loop: Persistent Behavior

```javascript
// Meow until human wakes up
let humanAwake = false;
let meowCount = 0;

while (!humanAwake) {
    meowCount++;
    console.log("Meow #" + meowCount + ": MEOW!");
    
    // After 5 meows, human wakes up
    if (meowCount >= 5) {
        humanAwake = true;
        console.log("Human is finally awake!");
    }
}
```

### The Do...While Loop: Always Check At Least Once

```javascript
// Always check the food bowl at least once
let foodLevel = 0;  // Empty bowl
let checksPerformed = 0;

do {
    checksPerformed++;
    console.log("Check #" + checksPerformed + ": Inspecting food bowl...");
    
    // Imagine food might appear
    if (Math.random() > 0.7) {
        foodLevel = 100;
        console.log("Food detected! Eating...");
    } else {
        console.log("Still empty. Will check again soon.");
    }
} while (foodLevel === 0 && checksPerformed < 5);
```

### Nested Loops: Patrols Within Patrols

```javascript
// Complete daily routine: 3 patrols, each with 4 checkpoints
for (let patrol = 1; patrol <= 3; patrol++) {
    console.log("\n=== Starting Patrol " + patrol + " ===");
    
    for (let checkpoint = 1; checkpoint <= 4; checkpoint++) {
        console.log("  Checkpoint " + checkpoint + " inspected");
        
        // Random chance of finding something interesting
        if (Math.random() > 0.8) {
            console.log("  -> Found something suspicious!");
        }
    }
    
    console.log("Patrol " + patrol + " complete");
}
```

### Looping Through Collections

```javascript
// Check each toy in the toy basket
const toyBasket = ["red ball", "feather wand", "catnip mouse", "laser pointer", "crinkly fish"];

console.log("Inspecting toy collection:");
for (let i = 0; i < toyBasket.length; i++) {
    console.log("Toy #" + (i + 1) + ": " + toyBasket[i]);
    
    if (toyBasket[i] === "catnip mouse") {
        console.log("  *goes absolutely wild*");
    }
}

// Modern way using for...of
console.log("\nReinspecting with for...of:");
for (const toy of toyBasket) {
    console.log("Playing with: " + toy);
}
```

### Breaking Out of Loops: Mission Accomplished

```javascript
// Search for the red dot (laser pointer)
const rooms = ["kitchen", "living room", "bedroom", "bathroom", "office"];
let dotFound = false;

for (let i = 0; i < rooms.length; i++) {
    console.log("Searching " + rooms[i] + "...");
    
    // Random chance of finding the dot
    if (Math.random() > 0.6) {
        console.log("RED DOT SPOTTED IN " + rooms[i].toUpperCase() + "!");
        dotFound = true;
        break;  // Stop searching once found
    }
    
    console.log("Not here, moving on...");
}

if (!dotFound) {
    console.log("The red dot has vanished again. How mysterious.");
}
```

### Continue: Skip This Round

```javascript
// Morning routine, but skip certain activities if tired
const activities = ["stretch", "eat", "groom", "play", "patrol", "nap"];
let energyLevel = 3;  // Low energy

for (const activity of activities) {
    if (activity === "play" && energyLevel < 5) {
        console.log("Too tired for " + activity + ", skipping...");
        continue;  // Skip to next activity
    }
    
    if (activity === "patrol" && energyLevel < 4) {
        console.log("Too tired for " + activity + ", skipping...");
        continue;
    }
    
    console.log("Performing: " + activity);
}
```

### Practical Loop Examples

```javascript
// Count treats given throughout the day
let treatsGiven = 0;
const maxTreats = 10;

while (treatsGiven < maxTreats) {
    console.log("Giving treat #" + (treatsGiven + 1));
    treatsGiven++;
    
    if (treatsGiven === 5) {
        console.log("Halfway through treat allowance!");
    }
}
console.log("That's enough treats for today!");

// Find the warmest spot in the house
const spots = [
    {name: "windowsill", temperature: 78},
    {name: "fireplace", temperature: 82},
    {name: "laptop keyboard", temperature: 85},
    {name: "sunny floor patch", temperature: 76},
    {name: "heated bed", temperature: 80}
];

let warmestSpot = spots[0];
for (const spot of spots) {
    if (spot.temperature > warmestSpot.temperature) {
        warmestSpot = spot;
    }
}
console.log("The warmest spot is: " + warmestSpot.name + 
            " at " + warmestSpot.temperature + "°F");

// The infamous "knocking things off table" loop
const tableItems = ["pen", "glasses", "phone", "cup", "keys"];
console.log("\nTime for chaos:");

for (let i = tableItems.length - 1; i >= 0; i--) {
    console.log("Examining " + tableItems[i] + "...");
    console.log("*paw paw paw*");
    console.log(tableItems[i] + " has been knocked off!");
    console.log("Items remaining on table: " + i);
    
    if (i === 0) {
        console.log("Mission accomplished. Table cleared.");
    }
}
```

### Infinite Loops: The Eternal Cat

```javascript
// WARNING: Don't actually run infinite loops!
// This is what NOT to do:

// while (true) {
//     console.log("Cat wants attention");
//     // This will run FOREVER because true is always true
// }

// Better: Always have an exit condition
let attentionReceived = 0;
while (attentionReceived < 10) {
    console.log("Cat wants attention");
    attentionReceived++;
}
console.log("Cat is satisfied... for now");
```

## Paw-sible Problems

### Exercise 1: The Breakfast Reminder
Create a loop that meows increasingly loudly until breakfast is served (after 5 meows).

```javascript
// Start with quiet meows, get louder each time
// After 5 meows, breakfast is served
// Print "Finally!" when done
```

### Exercise 2: The Toy Inspector
Write a loop that checks each toy and counts how many need replacing (use an array of toy conditions).

```javascript
const toys = ["good", "worn", "broken", "good", "worn", "good", "broken"];
// Count how many toys are "broken" and need replacing
```

### Exercise 3: The Window Watcher
Create a nested loop that represents watching windows throughout the day (3 time periods, 4 windows each).

```javascript
// Morning, afternoon, evening
// Living room, bedroom, kitchen, bathroom windows
// Random chance of seeing something interesting
```

### Exercise 4: The Treat Beggar
Write a loop that continues begging for treats but stops if either: given 3 treats OR ignored 10 times.

```javascript
// Use while loop with multiple conditions
// Track both treats received and times ignored
```

## Cat-ch the Pattern

### Key Takeaways

1. **Loops eliminate repetition**: Write once, execute many times (like a patrol route)
2. **Different loops for different needs**: for (counted), while (conditional), for...of (collections)
3. **Always need an exit**: Every patrol eventually ends (avoid infinite loops)
4. **Can control flow**: break (stop completely), continue (skip this round)
5. **Can nest loops**: Loops within loops (like checking multiple things at each patrol point)

### Common Loop Patterns

```javascript
// The Counter Pattern
for (let i = 0; i < 10; i++) {
    // Do something 10 times
}

// The Collection Iterator Pattern  
for (const item of collection) {
    // Process each item
}

// The Search Pattern
let found = false;
for (const item of items) {
    if (item === target) {
        found = true;
        break;
    }
}

// The Accumulator Pattern
let total = 0;
for (const number of numbers) {
    total += number;
}

// The Filter Pattern
const goodToys = [];
for (const toy of allToys) {
    if (toy.condition === "good") {
        goodToys.push(toy);
    }
}
```

### Loop Debugging Tips

```javascript
// Add console.logs to understand what's happening
for (let i = 0; i < 5; i++) {
    console.log("Loop iteration: " + i);
    // Your code here
    console.log("Ending iteration: " + i);
}

// Check your conditions
let counter = 0;
while (counter < 10) {
    console.log("Counter is: " + counter);
    counter++;  // Don't forget to update!
}
```

### Performance Considerations

```javascript
// Good: Calculate length once
const toys = ["ball", "mouse", "string"];
const toyCount = toys.length;
for (let i = 0; i < toyCount; i++) {
    // Process toys
}

// Less efficient: Calculates length every iteration
for (let i = 0; i < toys.length; i++) {
    // Process toys
}
```

### Looking Ahead

Now that you've mastered the art of repetition through loops, you're ready to explore decision-making with conditionals. In the next chapter, we'll decode the complex decision tree that governs a cat's behavior.

Is it time to eat? Should I nap or play? Is that a threat or a friend? Cats make hundreds of decisions daily, and each one follows logical conditions - just like the if/else statements that control program flow.

Just as a cat decides whether to come when called based on multiple factors (am I comfortable? is it dinner time? do I feel like it?), programs use conditionals to choose different paths based on current conditions.

---

*Next Chapter: [Conditionals and Cat Moods](chapter-04-conditionals-and-moods.md)*

*Previous: [Functions as Daily Routines](chapter-02-functions-as-routines.md)*