# Chapter 4: Conditionals and Cat Moods

## The Opening Tail

Luna sits at the kitchen entrance, performing the complex calculations that only a cat can truly understand. It's 5:47 PM. Dinner is usually at 6:00 PM. Her human is in the kitchen. The can opener is on the counter. The refrigerator just opened.

IF human is in kitchen AND time is close to dinner AND refrigerator opened THEN commence operation "Starving Cat Performance."

She releases a pitiful meow, perfectly calibrated for maximum sympathy.

But wait - her human is getting vegetables, not cat food. New calculation required.

IF human has vegetables AND NOT cat food THEN switch to operation "Ankle Weaving Obstacle Course."

Luna's decision tree continues throughout the day. IF sunny spot available AND energy less than 50% THEN nap. IF mysterious sound detected AND threat level unknown THEN investigate cautiously. IF human has laptop open AND human looks busy THEN definitely sit on keyboard.

Every action is preceded by conditions. Every behavior depends on the current state of Luna's world.

## The Technical Purr-spective

Conditionals are the decision-makers of programming. They allow code to choose different paths based on current conditions, just like cats constantly evaluate their environment and adjust their behavior accordingly.

### The Basic If Statement: Simple Decisions

```javascript
let hoursSinceMeal = 3;

if (hoursSinceMeal > 2) {
    console.log("Time to act hungry!");
}
```

### If-Else: Two-Way Decisions

```javascript
let isHumanHome = true;

if (isHumanHome) {
    console.log("Demand immediate attention");
} else {
    console.log("Nap until human returns");
}
```

## Code in the Wild

### The Complete Decision Tree

```javascript
// Luna's mood determination system
let hoursSinceBreakfast = 4;
let energyLevel = 30;
let humanAttentionAvailable = true;
let sunnySpotAvailable = true;

if (hoursSinceBreakfast > 3) {
    console.log("Status: HUNGRY");
    console.log("Action: Meow plaintively near food bowl");
} else if (energyLevel < 20) {
    console.log("Status: EXHAUSTED");
    console.log("Action: Find nearest soft surface and collapse");
} else if (energyLevel > 80) {
    console.log("Status: ZOOMIES IMMINENT");
    console.log("Action: Sprint randomly through house at maximum speed");
} else if (sunnySpotAvailable && energyLevel < 50) {
    console.log("Status: SOLAR POWERED CHARGING MODE");
    console.log("Action: Occupy sunny spot for next 3 hours");
} else if (humanAttentionAvailable) {
    console.log("Status: AFFECTIONATE");
    console.log("Action: Purr and headbutt human");
} else {
    console.log("Status: CONTEMPLATIVE");
    console.log("Action: Sit in window and judge passersby");
}
```

### Comparison Operators: The Tools of Decision

```javascript
let catWeight = 12;
let treatsToday = 5;
let currentHour = 15;
let catAge = 7;

// Equal to
if (treatsToday === 5) {
    console.log("Exactly 5 treats - the perfect amount!");
}

// Not equal to
if (currentHour !== 3) {
    console.log("It's not 3 AM zoomies time... yet");
}

// Greater than / Less than
if (catWeight > 15) {
    console.log("Perhaps fewer treats tomorrow...");
}

if (catAge < 2) {
    console.log("Still a kitten!");
}

// Greater than or equal / Less than or equal
if (treatsToday >= 10) {
    console.log("Maximum treat limit reached!");
}

if (currentHour <= 6) {
    console.log("Too early - let the humans sleep");
}
```

### Logical Operators: Complex Cat Logic

```javascript
let isWeekend = true;
let humanAwake = false;
let foodBowlEmpty = true;
let weatherNice = true;
let doorOpen = false;

// AND operator (&&) - Both must be true
if (isWeekend && humanAwake) {
    console.log("Perfect time for extended play session!");
}

// OR operator (||) - At least one must be true
if (foodBowlEmpty || treatsToday < 3) {
    console.log("Food situation is UNACCEPTABLE");
}

// NOT operator (!) - Inverts the boolean
if (!humanAwake) {
    console.log("Time to wake the human with song of my people");
}

// Combining operators
if (weatherNice && doorOpen && !isRaining) {
    console.log("Outdoor adventure time!");
} else if (weatherNice && !doorOpen) {
    console.log("Stare longingly out window");
}
```

### The Ternary Operator: Quick Decisions

```javascript
// Syntax: condition ? ifTrue : ifFalse
let currentTime = 14;
let activity = currentTime < 12 ? "morning nap" : "afternoon nap";
console.log("Current activity: " + activity);

// Can be chained for multiple conditions
let hunger = 8;
let mood = hunger > 7 ? "hangry" : 
          hunger > 4 ? "peckish" : 
          "satisfied";
console.log("Current mood: " + mood);
```

### Switch Statements: Multiple Choices

```javascript
let timeOfDay = "evening";

switch(timeOfDay) {
    case "morning":
        console.log("Breakfast patrol");
        console.log("Morning window watching");
        break;
    case "afternoon":
        console.log("Extended nap period");
        console.log("Occasional stretch");
        break;
    case "evening":
        console.log("Dinner demands");
        console.log("Evening zoomies");
        break;
    case "night":
        console.log("Nocturnal hunting simulation");
        console.log("3 AM existential crisis");
        break;
    default:
        console.log("Time is a construct. Nap anyway.");
}
```

### Truthy and Falsy: Cat Philosophy

```javascript
// In JavaScript, some values are "truthy" or "falsy"
// Falsy values: false, 0, "", null, undefined, NaN
// Everything else is truthy

let treatsInPocket = 3;  // Truthy (non-zero number)
let favoriteBox = "";     // Falsy (empty string)
let napSpot = "couch";    // Truthy (non-empty string)

// Cats understand this intuitively
if (treatsInPocket) {
    console.log("Human has treats! Follow closely!");
}

if (!favoriteBox) {
    console.log("No box? This is unacceptable.");
}

// Practical use
let catName = "";
catName = catName || "Mystery Cat";  // Uses default if name is empty
console.log(catName);  // "Mystery Cat"
```

### Nested Conditions: Deep Cat Thoughts

```javascript
let location = "kitchen";
let humanActivity = "cooking";
let smellType = "fish";
let lastFedHours = 2;

if (location === "kitchen") {
    console.log("Kitchen detected. Engaging food mode.");
    
    if (humanActivity === "cooking") {
        console.log("Cooking in progress. High alert!");
        
        if (smellType === "fish" || smellType === "chicken") {
            console.log("ACCEPTABLE FOOD DETECTED");
            console.log("Deploy maximum cuteness");
            
            if (lastFedHours > 1) {
                console.log("Also mention it's been FOREVER since last meal");
            }
        } else {
            console.log("Boring human food. Mild supervision only.");
        }
    } else {
        console.log("No cooking. Check food bowl status.");
    }
}
```

## Paw-sible Problems

### Exercise 1: The Door Decision
Create a function that decides whether a cat wants to go outside based on weather and time of day.

```javascript
function shouldGoOutside(weather, temperature, timeOfDay) {
    // If weather is "sunny" and temperature is between 60-75
    // and it's not "night", return true
    // Otherwise return false
}
```

### Exercise 2: The Treat Calculator
Write conditions that determine how many treats a cat should get based on their behavior.

```javascript
let caughtMice = 0;
let knockedThingsOver = 3;
let usedLitterBox = true;
let scratchedFurniture = false;
// Calculate treat count based on behavior
```

### Exercise 3: The Nap Location Selector
Create a decision tree that picks the best nap spot based on multiple factors.

```javascript
let temperature = 72;
let sunnySpots = ["windowsill", "floor patch"];
let humanLocation = "couch";
let timeOfDay = "afternoon";
// Determine best nap location
```

### Exercise 4: The Attention Demand System
Write a complex conditional that determines how insistently a cat should demand attention.

```javascript
let minutesSinceAttention = 45;
let humanBusyness = "very";
let catMood = "needy";
// Determine attention demand level: "subtle", "moderate", "intense", or "ignore human"
```

## Cat-ch the Pattern

### Key Takeaways

1. **Conditionals control flow**: Like a cat choosing different paths through the house
2. **Multiple conditions can combine**: Using &&, ||, and ! for complex decisions
3. **Order matters**: Check most specific conditions first
4. **Always have a default**: Like a cat's default state (napping)
5. **Nest carefully**: Too much nesting makes code hard to follow

### Common Conditional Patterns

```javascript
// The Guard Clause
function feedCat(foodType) {
    if (!foodType) {
        return "No food? Unacceptable!";
    }
    if (foodType === "dry") {
        return "Acceptable, but disappointed";
    }
    return "Excellent choice, human";
}

// The State Machine
let catState = "sleeping";

if (catState === "sleeping" && loudNoise) {
    catState = "alert";
} else if (catState === "alert" && !threat) {
    catState = "curious";
} else if (catState === "curious" && foundToy) {
    catState = "playing";
}

// The Validation Pattern
function validateTreatRequest(treatsToday, catWeight, hasBeenGood) {
    if (treatsToday >= 10) {
        return false;  // Too many already
    }
    if (catWeight > 15 && treatsToday >= 5) {
        return false;  // Weight management
    }
    if (!hasBeenGood) {
        return false;  // Behavior matters
    }
    return true;  // Treat approved!
}
```

### Looking Ahead

Now that you understand how cats make decisions through conditionals, you're ready to explore how they organize collections - starting with arrays. In the next chapter, we'll see how a litter of kittens perfectly demonstrates the concept of arrays: ordered, indexed, and each one unique yet part of a group.

Just as mother cats keep track of each kitten by their position in the nest, arrays let us organize and access multiple values in a structured way.

---

*Next Chapter: [Arrays as a Litter of Kittens](chapter-05-arrays-as-litter.md)*

*Previous: [Loops and Territorial Patrols](chapter-03-loops-and-patrols.md)*