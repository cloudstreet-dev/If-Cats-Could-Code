# Chapter 13: Design Patterns in Cat Behavior

## The Opening Tail

The Neighborhood Cat Society had been operating successfully for decades, and Dr. Sarah, the local veterinarian and cat behaviorist, noticed something fascinating: despite each cat's unique personality, they all seemed to follow remarkably similar behavioral patterns in certain situations.

Take the "Treat Request Protocol," for instance. Every cat - from the sophisticated Persian down the street to the scrappy alley cat behind the deli - followed the same basic pattern when they wanted treats: establish eye contact, perform the "cute behavior" (purring, head rubbing, or strategic positioning), wait for human response, and escalate gradually if ignored. The specific implementations varied - Duchess used elegant sitting and patient staring, while Rascal employed dramatic meowing and ankle rubbing - but the underlying pattern was universal.

Dr. Sarah observed similar patterns everywhere: the "Territory Patrol Routine" (systematic perimeter checking with scent marking), the "Hunting Strategy Framework" (stalk, pounce, secure, present), and the "Social Hierarchy Protocol" (initial assessment, positioning display, conflict resolution if needed).

These weren't just random behaviors - they were proven solutions to common feline problems, refined through millions of years of evolution and passed down through generations. Each pattern was a template that individual cats could adapt to their specific circumstances while maintaining the core successful strategy.

What fascinated Dr. Sarah most was how these patterns could be combined: a cat might use the Territory Patrol pattern to identify potential hunting spots, then switch to the Hunting Strategy pattern when prey was detected, and finally employ the Social Display pattern to show off their catch. The patterns were like a toolkit of reliable solutions that cats could mix and match as needed.

## The Technical Purr-spective

Design patterns in programming are like the behavioral patterns Dr. Sarah observed in cats - they're proven solutions to common problems that occur repeatedly in software development. Just as cats have evolved effective strategies for hunting, socializing, and surviving, programmers have identified recurring design challenges and developed time-tested solutions.

### What Are Design Patterns?

Design patterns are:
- **Reusable solutions** to commonly occurring problems
- **Templates** that can be adapted to specific situations
- **Communication tools** - they provide a shared vocabulary for developers
- **Best practices** refined through years of experience

### The Three Categories

**Creational Patterns**: How objects are created (like how cats establish territory)
**Structural Patterns**: How objects relate to each other (like pack hierarchies)
**Behavioral Patterns**: How objects communicate and interact (like cat social protocols)

### Why Patterns Matter

- **Reduce complexity**: Proven solutions instead of reinventing the wheel
- **Improve communication**: Shared language among developers
- **Increase maintainability**: Well-understood structures are easier to modify
- **Enable flexibility**: Patterns often make systems more adaptable

## Code in the Wild

### Singleton Pattern: The Alpha Cat

```javascript
// Singleton - ensures only one instance of a class exists
// Like how there can only be one alpha cat in a territory

class AlphaCat {
    constructor() {
        if (AlphaCat.instance) {
            console.log("There can only be one Alpha Cat! Returning existing instance.");
            return AlphaCat.instance;
        }
        
        this.name = "Supreme Leader";
        this.territory = "Entire Neighborhood";
        this.subordinates = [];
        this.establishedDate = new Date();
        
        AlphaCat.instance = this;
        console.log(`${this.name} has established dominance over ${this.territory}`);
    }
    
    addSubordinate(cat) {
        this.subordinates.push(cat);
        console.log(`${cat.name} now recognizes ${this.name} as alpha`);
    }
    
    issueCommand(command) {
        console.log(`Alpha ${this.name} commands: ${command}`);
        this.subordinates.forEach(cat => {
            console.log(`  ${cat.name} obeys: ${command}`);
        });
    }
    
    static getInstance() {
        if (!AlphaCat.instance) {
            new AlphaCat();
        }
        return AlphaCat.instance;
    }
}

console.log("=== SINGLETON PATTERN DEMO ===");

// Try to create multiple alpha cats
let alpha1 = new AlphaCat();
let alpha2 = new AlphaCat(); // Will return the same instance
let alpha3 = AlphaCat.getInstance(); // Static method access

console.log("Are they the same instance?", alpha1 === alpha2); // true
console.log("Are they the same instance?", alpha1 === alpha3); // true

alpha1.addSubordinate({ name: "Whiskers" });
alpha1.addSubordinate({ name: "Mittens" });
alpha1.issueCommand("All cats to the food bowl!");

// Even alpha2 has the same subordinates because it's the same instance
console.log(`Alpha2 subordinates: ${alpha2.subordinates.map(c => c.name).join(", ")}`);
```

### Observer Pattern: The Neighborhood Watch

```javascript
// Observer Pattern - one-to-many dependency where observers are notified of changes
// Like how all cats in the neighborhood watch for interesting events

class NeighborhoodEventBroadcaster {
    constructor() {
        this.observers = [];
        this.events = [];
    }
    
    subscribe(cat) {
        this.observers.push(cat);
        console.log(`${cat.name} is now watching neighborhood events`);
    }
    
    unsubscribe(cat) {
        this.observers = this.observers.filter(observer => observer !== cat);
        console.log(`${cat.name} stopped watching neighborhood events`);
    }
    
    notify(event) {
        console.log(`\nBROADCASTING: ${event.type} - ${event.description}`);
        this.events.push(event);
        
        this.observers.forEach(cat => {
            cat.handleEvent(event);
        });
    }
}

class ObserverCat {
    constructor(name, interests = []) {
        this.name = name;
        this.interests = interests;
        this.eventHistory = [];
    }
    
    handleEvent(event) {
        this.eventHistory.push(event);
        
        if (this.interests.includes(event.type)) {
            console.log(`  ${this.name}: *perks up* This ${event.type} is interesting!`);
            this.reactToEvent(event);
        } else {
            console.log(`  ${this.name}: *yawns* Not interested in ${event.type}`);
        }
    }
    
    reactToEvent(event) {
        switch (event.type) {
            case "food":
                console.log(`    ${this.name} heads toward the sound of food`);
                break;
            case "intruder":
                console.log(`    ${this.name} goes on high alert`);
                break;
            case "weather":
                console.log(`    ${this.name} seeks appropriate shelter`);
                break;
            case "play":
                console.log(`    ${this.name} gets excited and ready to play`);
                break;
        }
    }
}

console.log("\n=== OBSERVER PATTERN DEMO ===");

let broadcaster = new NeighborhoodEventBroadcaster();

// Create cats with different interests
let whiskers = new ObserverCat("Whiskers", ["food", "play"]);
let mittens = new ObserverCat("Mittens", ["intruder", "weather"]);
let shadow = new ObserverCat("Shadow", ["intruder", "play"]);
let patches = new ObserverCat("Patches", ["food", "weather"]);

// Cats subscribe to neighborhood watch
broadcaster.subscribe(whiskers);
broadcaster.subscribe(mittens);
broadcaster.subscribe(shadow);
broadcaster.subscribe(patches);

// Broadcast various events
broadcaster.notify({
    type: "food", 
    description: "Someone opened a tuna can at 123 Main Street"
});

broadcaster.notify({
    type: "intruder", 
    description: "Unknown cat spotted near the oak tree"
});

broadcaster.notify({
    type: "weather", 
    description: "Rain clouds gathering - time to find shelter"
});

// One cat unsubscribes
broadcaster.unsubscribe(mittens);

broadcaster.notify({
    type: "play", 
    description: "Kids brought out the laser pointer in the backyard"
});
```

### Strategy Pattern: Hunting Techniques

```javascript
// Strategy Pattern - encapsulate algorithms and make them interchangeable
// Like how cats have different hunting strategies for different prey

class HuntingContext {
    constructor(cat) {
        this.cat = cat;
        this.strategy = null;
    }
    
    setStrategy(strategy) {
        this.strategy = strategy;
        console.log(`${this.cat.name} switches to ${strategy.name} hunting strategy`);
    }
    
    hunt(prey) {
        if (!this.strategy) {
            console.log(`${this.cat.name} doesn't know how to hunt yet!`);
            return false;
        }
        
        return this.strategy.execute(this.cat, prey);
    }
}

// Different hunting strategies
class MouseHuntingStrategy {
    constructor() {
        this.name = "Mouse Hunting";
    }
    
    execute(cat, prey) {
        console.log(`${cat.name} employs mouse hunting strategy:`);
        console.log(`  1. Crouches low and stays very still`);
        console.log(`  2. Watches prey carefully for movement patterns`);
        console.log(`  3. Slowly stalks closer using furniture as cover`);
        console.log(`  4. Pounces with quick, precise movement`);
        console.log(`  5. Secures prey with paws`);
        
        let success = Math.random() > 0.3; // 70% success rate
        console.log(`  Result: ${success ? "Success!" : "Prey escaped!"}`);
        return success;
    }
}

class BirdHuntingStrategy {
    constructor() {
        this.name = "Bird Hunting";
    }
    
    execute(cat, prey) {
        console.log(`${cat.name} employs bird hunting strategy:`);
        console.log(`  1. Positions under bird's perch`);
        console.log(`  2. Makes chattering sounds to get bird's attention`);
        console.log(`  3. Waits for bird to come lower`);
        console.log(`  4. Leaps upward with maximum height`);
        console.log(`  5. Uses claws to secure prey in mid-air`);
        
        let success = Math.random() > 0.6; // 40% success rate (birds are harder)
        console.log(`  Result: ${success ? "Success!" : "Bird flew away!"}`);
        return success;
    }
}

class FishHuntingStrategy {
    constructor() {
        this.name = "Fish Hunting";
    }
    
    execute(cat, prey) {
        console.log(`${cat.name} employs fish hunting strategy:`);
        console.log(`  1. Sits patiently at water's edge`);
        console.log(`  2. Watches for fish movement in shallow water`);
        console.log(`  3. Dips paw into water when fish approaches`);
        console.log(`  4. Attempts to scoop fish onto shore`);
        console.log(`  5. Quickly secures slippery prey`);
        
        let success = Math.random() > 0.7; // 30% success rate (very difficult)
        console.log(`  Result: ${success ? "Success!" : "Fish swam away!"}`);
        return success;
    }
}

console.log("\n=== STRATEGY PATTERN DEMO ===");

let hunter = new HuntingContext({ name: "Shadow" });

// Available strategies
let mouseStrategy = new MouseHuntingStrategy();
let birdStrategy = new BirdHuntingStrategy();
let fishStrategy = new FishHuntingStrategy();

// Hunt different prey with appropriate strategies
console.log("\nHunting a mouse:");
hunter.setStrategy(mouseStrategy);
hunter.hunt("field mouse");

console.log("\nHunting a bird:");
hunter.setStrategy(birdStrategy);
hunter.hunt("sparrow");

console.log("\nHunting fish:");
hunter.setStrategy(fishStrategy);
hunter.hunt("goldfish");

// Demonstrate strategy switching based on conditions
console.log("\nAdaptive hunting based on environment:");
function adaptiveHunt(environment, prey) {
    switch (environment) {
        case "indoor":
            hunter.setStrategy(mouseStrategy);
            break;
        case "garden":
            hunter.setStrategy(birdStrategy);
            break;
        case "pond":
            hunter.setStrategy(fishStrategy);
            break;
    }
    
    return hunter.hunt(prey);
}

adaptiveHunt("indoor", "toy mouse");
adaptiveHunt("garden", "robin");
adaptiveHunt("pond", "koi fish");
```

### Factory Pattern: Cat Breed Creation

```javascript
// Factory Pattern - create objects without specifying exact classes
// Like how different cat breeds are created with specific characteristics

class CatFactory {
    static createCat(breed, name) {
        console.log(`Creating a ${breed} cat named ${name}...`);
        
        switch (breed.toLowerCase()) {
            case 'persian':
                return new PersianCat(name);
            case 'siamese':
                return new SiameseCat(name);
            case 'maine coon':
                return new MaineCoonCat(name);
            case 'ragdoll':
                return new RagdollCat(name);
            default:
                return new GenericCat(name, breed);
        }
    }
    
    static getSupportedBreeds() {
        return ['persian', 'siamese', 'maine coon', 'ragdoll'];
    }
}

// Base cat class
class Cat {
    constructor(name, breed) {
        this.name = name;
        this.breed = breed;
    }
    
    introduce() {
        return `Hello, I'm ${this.name}, a ${this.breed}`;
    }
    
    makeSound() {
        return "meow";
    }
}

// Specific cat breeds with unique characteristics
class PersianCat extends Cat {
    constructor(name) {
        super(name, "Persian");
        this.furLength = "long";
        this.personalityTraits = ["calm", "gentle", "quiet"];
        this.groomingNeeds = "high";
    }
    
    makeSound() {
        return "soft purr";
    }
    
    groom() {
        return `${this.name} requires daily brushing due to ${this.furLength} ${this.breed} fur`;
    }
}

class SiameseCat extends Cat {
    constructor(name) {
        super(name, "Siamese");
        this.vocalness = "very high";
        this.personalityTraits = ["vocal", "intelligent", "social"];
        this.eyeColor = "blue";
    }
    
    makeSound() {
        return "loud, distinctive meow";
    }
    
    talk() {
        return `${this.name} has a lot to say with their ${this.vocalness} vocalness`;
    }
}

class MaineCoonCat extends Cat {
    constructor(name) {
        super(name, "Maine Coon");
        this.size = "large";
        this.personalityTraits = ["gentle", "friendly", "dog-like"];
        this.specialAbilities = ["excellent mouser", "water-tolerant"];
    }
    
    hunt() {
        return `${this.name} is an ${this.specialAbilities[0]} due to Maine Coon instincts`;
    }
}

class RagdollCat extends Cat {
    constructor(name) {
        super(name, "Ragdoll");
        this.temperament = "docile";
        this.personalityTraits = ["relaxed", "affectionate", "trusting"];
        this.specialTrait = "goes limp when picked up";
    }
    
    relax() {
        return `${this.name} demonstrates the famous Ragdoll trait: ${this.specialTrait}`;
    }
}

class GenericCat extends Cat {
    constructor(name, breed) {
        super(name, breed);
        this.personalityTraits = ["independent", "curious"];
    }
}

console.log("\n=== FACTORY PATTERN DEMO ===");

// Create different cats using the factory
let cats = [
    CatFactory.createCat("persian", "Fluffy"),
    CatFactory.createCat("siamese", "Chatty"),
    CatFactory.createCat("maine coon", "Bigfoot"),
    CatFactory.createCat("ragdoll", "Flopsy"),
    CatFactory.createCat("tabby", "Generic") // Not specifically supported
];

// Demonstrate polymorphism - same interface, different behaviors
console.log("\nCat introductions:");
cats.forEach(cat => {
    console.log(cat.introduce());
    console.log(`  Sound: ${cat.makeSound()}`);
    console.log(`  Traits: ${cat.personalityTraits.join(", ")}`);
    
    // Demonstrate breed-specific abilities
    if (cat instanceof PersianCat) {
        console.log(`  Special: ${cat.groom()}`);
    } else if (cat instanceof SiameseCat) {
        console.log(`  Special: ${cat.talk()}`);
    } else if (cat instanceof MaineCoonCat) {
        console.log(`  Special: ${cat.hunt()}`);
    } else if (cat instanceof RagdollCat) {
        console.log(`  Special: ${cat.relax()}`);
    }
    
    console.log("");
});

console.log("Supported breeds:", CatFactory.getSupportedBreeds().join(", "));
```

### Decorator Pattern: Cat Accessories

```javascript
// Decorator Pattern - add new functionality to objects dynamically
// Like how cats can wear different accessories to enhance their abilities

class BasicCat {
    constructor(name) {
        this.name = name;
        this.abilities = ["meow", "purr", "sleep"];
        this.cuteness = 7;
        this.protection = 0;
        this.visibility = 5;
    }
    
    describe() {
        return `${this.name}: Abilities [${this.abilities.join(", ")}], Cuteness: ${this.cuteness}, Protection: ${this.protection}, Visibility: ${this.visibility}`;
    }
    
    performAction(action) {
        if (this.abilities.includes(action)) {
            return `${this.name} performs ${action}`;
        }
        return `${this.name} doesn't know how to ${action}`;
    }
}

// Base decorator
class CatDecorator {
    constructor(cat) {
        this.cat = cat;
    }
    
    get name() { return this.cat.name; }
    get abilities() { return this.cat.abilities; }
    get cuteness() { return this.cat.cuteness; }
    get protection() { return this.cat.protection; }
    get visibility() { return this.cat.visibility; }
    
    describe() {
        return this.cat.describe();
    }
    
    performAction(action) {
        return this.cat.performAction(action);
    }
}

// Specific decorators
class CollarDecorator extends CatDecorator {
    constructor(cat, collarType = "basic") {
        super(cat);
        this.collarType = collarType;
    }
    
    get visibility() {
        return this.cat.visibility + 2; // Easier to spot with collar
    }
    
    describe() {
        return this.cat.describe().replace(
            this.cat.name, 
            `${this.cat.name} (wearing ${this.collarType} collar)`
        );
    }
    
    performAction(action) {
        if (action === "identify") {
            return `${this.name}'s ${this.collarType} collar shows identification info`;
        }
        return this.cat.performAction(action);
    }
}

class BellCollarDecorator extends CollarDecorator {
    constructor(cat) {
        super(cat, "bell");
        this.originalAbilities = [...cat.abilities];
    }
    
    get abilities() {
        let abilities = [...this.originalAbilities];
        if (abilities.includes("hunt")) {
            abilities = abilities.filter(a => a !== "hunt");
            abilities.push("failed hunt (bell warning)");
        }
        abilities.push("jingle");
        return abilities;
    }
    
    get visibility() {
        return this.cat.visibility + 4; // Bell makes cat very noticeable
    }
    
    performAction(action) {
        if (action === "jingle") {
            return `${this.name} makes jingling sounds while moving`;
        }
        if (action === "hunt") {
            return `${this.name} tries to hunt but the bell warns prey`;
        }
        return super.performAction(action);
    }
}

class SweaterDecorator extends CatDecorator {
    constructor(cat, sweaterColor = "red") {
        super(cat);
        this.sweaterColor = sweaterColor;
    }
    
    get cuteness() {
        return this.cat.cuteness + 3; // Sweaters are adorable
    }
    
    get protection() {
        return this.cat.protection + 2; // Warmth protection
    }
    
    describe() {
        return this.cat.describe().replace(
            this.cat.name, 
            `${this.cat.name} (wearing ${this.sweaterColor} sweater)`
        );
    }
    
    performAction(action) {
        if (action === "stay warm") {
            return `${this.name}'s ${this.sweaterColor} sweater keeps them cozy`;
        }
        return this.cat.performAction(action);
    }
}

class SuperheroCapDecorator extends CatDecorator {
    constructor(cat, heroName = "Captain Whiskers") {
        super(cat);
        this.heroName = heroName;
    }
    
    get abilities() {
        return [...this.cat.abilities, "save the day", "look heroic"];
    }
    
    get cuteness() {
        return this.cat.cuteness + 2;
    }
    
    get protection() {
        return this.cat.protection + 1;
    }
    
    describe() {
        return this.cat.describe().replace(
            this.cat.name, 
            `${this.heroName} (${this.cat.name} in superhero cape)`
        );
    }
    
    performAction(action) {
        if (action === "save the day") {
            return `${this.heroName} strikes a heroic pose and saves everyone!`;
        }
        if (action === "look heroic") {
            return `${this.heroName}'s cape flows majestically in the breeze`;
        }
        return this.cat.performAction(action);
    }
}

console.log("\n=== DECORATOR PATTERN DEMO ===");

// Start with a basic cat
let mittens = new BasicCat("Mittens");
console.log("Basic cat:");
console.log(mittens.describe());
console.log(mittens.performAction("meow"));

// Add a collar
console.log("\nAdding collar:");
mittens = new CollarDecorator(mittens);
console.log(mittens.describe());
console.log(mittens.performAction("identify"));

// Add a bell to the collar
console.log("\nUpgrading to bell collar:");
mittens = new BellCollarDecorator(new BasicCat("Mittens")); // Start fresh for bell
console.log(mittens.describe());
console.log(mittens.performAction("jingle"));
console.log(mittens.performAction("hunt"));

// Add a sweater
console.log("\nAdding sweater:");
mittens = new SweaterDecorator(mittens, "blue");
console.log(mittens.describe());
console.log(mittens.performAction("stay warm"));

// Add superhero cape
console.log("\nAdding superhero cape:");
mittens = new SuperheroCapDecorator(mittens, "Captain Mittens");
console.log(mittens.describe());
console.log(mittens.performAction("save the day"));
console.log(mittens.performAction("look heroic"));

// Show how decorators stack
console.log("\nAll abilities:");
mittens.abilities.forEach(ability => {
    console.log(`- ${mittens.performAction(ability)}`);
});
```

### Command Pattern: Cat Trick Training

```javascript
// Command Pattern - encapsulate requests as objects
// Like teaching cats different tricks that can be executed on command

class CatTrickCommand {
    execute() {
        throw new Error("Subclasses must implement execute()");
    }
    
    undo() {
        throw new Error("Subclasses must implement undo()");
    }
}

class SitCommand extends CatTrickCommand {
    constructor(cat) {
        super();
        this.cat = cat;
        this.previousPosition = null;
    }
    
    execute() {
        this.previousPosition = this.cat.position;
        this.cat.position = "sitting";
        return `${this.cat.name} sits down obediently`;
    }
    
    undo() {
        this.cat.position = this.previousPosition;
        return `${this.cat.name} returns to ${this.previousPosition} position`;
    }
}

class RollOverCommand extends CatTrickCommand {
    constructor(cat) {
        super();
        this.cat = cat;
        this.hasRolled = false;
    }
    
    execute() {
        if (!this.hasRolled) {
            this.hasRolled = true;
            return `${this.cat.name} rolls over gracefully`;
        }
        return `${this.cat.name} is already rolled over`;
    }
    
    undo() {
        if (this.hasRolled) {
            this.hasRolled = false;
            return `${this.cat.name} rolls back to normal position`;
        }
        return `${this.cat.name} was not rolled over`;
    }
}

class HighFiveCommand extends CatTrickCommand {
    constructor(cat) {
        super();
        this.cat = cat;
        this.pawsRaised = false;
    }
    
    execute() {
        this.pawsRaised = true;
        return `${this.cat.name} raises paw for high five!`;
    }
    
    undo() {
        this.pawsRaised = false;
        return `${this.cat.name} lowers paw`;
    }
}

class SpeakCommand extends CatTrickCommand {
    constructor(cat, sound = "meow") {
        super();
        this.cat = cat;
        this.sound = sound;
        this.timesMeowed = 0;
    }
    
    execute() {
        this.timesMeowed++;
        return `${this.cat.name} says "${this.sound}" (${this.timesMeowed} times today)`;
    }
    
    undo() {
        if (this.timesMeowed > 0) {
            this.timesMeowed--;
            return `${this.cat.name} takes back their "${this.sound}"`;
        }
        return `${this.cat.name} hasn't spoken to undo`;
    }
}

// Trainer (Invoker) class
class CatTrainer {
    constructor() {
        this.commandHistory = [];
        this.currentPosition = -1;
    }
    
    executeCommand(command) {
        // Remove any commands after current position (for new branch)
        this.commandHistory = this.commandHistory.slice(0, this.currentPosition + 1);
        
        // Execute and store the command
        let result = command.execute();
        this.commandHistory.push(command);
        this.currentPosition++;
        
        console.log(result);
        return result;
    }
    
    undo() {
        if (this.currentPosition >= 0) {
            let command = this.commandHistory[this.currentPosition];
            let result = command.undo();
            this.currentPosition--;
            console.log(`Undo: ${result}`);
            return result;
        } else {
            console.log("Nothing to undo");
            return "Nothing to undo";
        }
    }
    
    redo() {
        if (this.currentPosition < this.commandHistory.length - 1) {
            this.currentPosition++;
            let command = this.commandHistory[this.currentPosition];
            let result = command.execute();
            console.log(`Redo: ${result}`);
            return result;
        } else {
            console.log("Nothing to redo");
            return "Nothing to redo";
        }
    }
    
    showHistory() {
        console.log("\nCommand History:");
        this.commandHistory.forEach((command, index) => {
            let marker = index === this.currentPosition ? " <-- Current" : "";
            console.log(`  ${index + 1}. ${command.constructor.name}${marker}`);
        });
    }
}

// Cat class for command pattern demo
class TrainableCat {
    constructor(name) {
        this.name = name;
        this.position = "standing";
        this.mood = "curious";
    }
    
    getStatus() {
        return `${this.name} is ${this.position} and feeling ${this.mood}`;
    }
}

console.log("\n=== COMMAND PATTERN DEMO ===");

let cat = new TrainableCat("Whiskers");
let trainer = new CatTrainer();

console.log("Initial state:", cat.getStatus());

// Create various commands
let sitCommand = new SitCommand(cat);
let rollCommand = new RollOverCommand(cat);
let highFiveCommand = new HighFiveCommand(cat);
let speakCommand = new SpeakCommand(cat, "purr");

// Execute a sequence of tricks
console.log("\nTraining session:");
trainer.executeCommand(sitCommand);
console.log(cat.getStatus());

trainer.executeCommand(speakCommand);
trainer.executeCommand(rollCommand);
trainer.executeCommand(highFiveCommand);
trainer.executeCommand(speakCommand);

// Show command history
trainer.showHistory();

// Demonstrate undo/redo
console.log("\nUndo last few commands:");
trainer.undo(); // Undo speak
trainer.undo(); // Undo high five
trainer.undo(); // Undo roll over

console.log("\nRedo some commands:");
trainer.redo(); // Redo roll over
trainer.redo(); // Redo high five

trainer.showHistory();

// Execute new command (creates new branch, discarding undone commands)
console.log("\nNew command (creates new branch):");
trainer.executeCommand(new SpeakCommand(cat, "meow loudly"));

trainer.showHistory();
```

## Paw-sible Problems

### Exercise 1: Implement the State Pattern
Create a cat mood system using the State pattern where cats behave differently based on their current mood.

```javascript
class CatMoodStateMachine {
    // Implement states: Happy, Hungry, Sleepy, Playful, Angry
    // Each state has different behaviors for common actions:
    // - petCat(), feedCat(), playCat(), ignoreCat()
    // States should transition based on actions
}
```

### Exercise 2: Build a Template Method Pattern
Create a daily routine template that different cat breeds can customize.

```javascript
class CatDailyRoutine {
    // Template method with steps:
    // wakeUp(), eat(), groom(), patrol(), play(), nap(), eat(), sleep()
    // Different breeds override specific steps
    // Persian cats might have longer grooming, Siamese might be more vocal
}
```

### Exercise 3: Implement the Adapter Pattern
Create an adapter that allows old cat toys to work with a new interactive play system.

```javascript
class LegacyToyAdapter {
    // Adapt old simple toys (string, ball, mouse) 
    // to work with new interactive system
    // that expects methods like: startInteraction(), respondToMovement(), endSession()
}
```

### Exercise 4: Create a Chain of Responsibility
Build a cat decision-making system where different factors influence behavior choices.

```javascript
class CatDecisionChain {
    // Chain: HungerHandler -> PlayfulnessHandler -> TirednessHandler -> DefaultHandler
    // Each handler decides if it can handle current cat state
    // or passes to next handler in chain
}
```

### Exercise 5: Design a Model-View-Controller (MVC)
Create an MVC system for a virtual cat pet application.

```javascript
class CatPetMVC {
    // Model: Cat data (hunger, happiness, health, energy)
    // View: Display cat status and handle user interaction
    // Controller: Process user actions and update model
    // Demonstrate separation of concerns
}
```

## Cat-ch the Pattern

### Key Takeaways

1. **Patterns solve recurring problems**: They provide tested solutions to common design challenges
2. **Patterns improve communication**: Shared vocabulary makes discussing designs easier
3. **Patterns enable flexibility**: Well-implemented patterns make code more adaptable
4. **Don't force patterns**: Use them when they naturally fit the problem
5. **Combine patterns thoughtfully**: Patterns can work together but avoid over-engineering

### Pattern Categories Summary

**Creational Patterns**:
- **Singleton**: Ensure only one instance exists
- **Factory**: Create objects without specifying exact classes
- **Builder**: Construct complex objects step by step

**Structural Patterns**:
- **Decorator**: Add functionality dynamically
- **Adapter**: Make incompatible interfaces work together
- **Facade**: Provide simplified interface to complex subsystem

**Behavioral Patterns**:
- **Observer**: Notify multiple objects of changes
- **Strategy**: Encapsulate algorithms and make them interchangeable  
- **Command**: Encapsulate requests as objects
- **State**: Change object behavior based on internal state

### When to Use Design Patterns

**Use patterns when:**
- You recognize a recurring problem
- The pattern fits naturally
- It improves code flexibility or maintainability
- It helps communicate design intent

**Avoid patterns when:**
- They add unnecessary complexity
- The problem is simple enough without them
- You're forcing a pattern to fit
- The team isn't familiar with the pattern

### Common Pattern Combinations

```javascript
// Observer + Strategy: Different notification strategies
class NotificationManager {
    constructor() {
        this.observers = [];
        this.strategy = new EmailNotification();
    }
    
    setNotificationStrategy(strategy) {
        this.strategy = strategy;
    }
    
    notify(message) {
        this.strategy.send(message);
        this.observers.forEach(obs => obs.update(message));
    }
}

// Factory + Decorator: Create objects and enhance them
class EnhancedCatFactory {
    static createCat(breed, enhancements = []) {
        let cat = CatFactory.createCat(breed);
        
        enhancements.forEach(enhancement => {
            cat = new enhancement(cat);
        });
        
        return cat;
    }
}

// Command + Observer: Commands notify observers of execution
class ObservableCommand extends Command {
    constructor() {
        super();
        this.observers = [];
    }
    
    execute() {
        let result = super.execute();
        this.notifyObservers('executed', result);
        return result;
    }
}
```

### Anti-Patterns to Avoid

```javascript
// God Object (too much responsibility)
class BadCatManager {
    // Does everything: feeding, grooming, playing, medical care, 
    // scheduling, reporting, communications, etc.
    // Better: Split into focused classes
}

// Spaghetti Code (no clear structure)
function badCatCare() {
    if (cat.hungry) {
        feed();
        if (cat.messy) {
            groom();
            if (tired) {
                sleep();
                // Deep nesting continues...
            }
        }
    }
    // Better: Use clear control structures and patterns
}

// Pattern Overuse (unnecessary complexity)
class OverEngineeredCatName {
    // Using 5 different patterns to store a simple string
    // Sometimes a simple property is better than a pattern
}
```

### Pattern Implementation Tips

```javascript
// Keep patterns simple and focused
class SimpleObserver {
    constructor() {
        this.observers = [];
    }
    
    addObserver(obs) { this.observers.push(obs); }
    removeObserver(obs) { /* remove logic */ }
    notify(data) { this.observers.forEach(obs => obs.update(data)); }
}

// Make patterns testable
class TestableFactory {
    constructor(dependencies = {}) {
        this.dependencies = dependencies; // Inject dependencies for testing
    }
    
    create(type) {
        // Use dependencies to create objects
        return new this.dependencies[type]();
    }
}

// Document pattern usage
/**
 * Implements Strategy pattern for cat feeding schedules.
 * Allows switching between different feeding algorithms:
 * - FixedTimeStrategy: Feed at specific times
 * - DemandFeedingStrategy: Feed when requested
 * - HealthBasedStrategy: Feed based on health metrics
 */
class FeedingScheduler {
    // Implementation...
}
```

### Looking Ahead

Design patterns help us structure individual components and their interactions, but what happens when we need to organize an entire system? In our next chapter, we'll explore software architecture - the art of designing the overall structure of applications, much like how cats establish and manage their territories on a larger scale.

We'll discover how architectural principles help create systems that are scalable, maintainable, and robust, just as cats create territorial organizations that can adapt and grow over time.

---

*Next Chapter: [Architecture as Territory Management](chapter-14-architecture-as-territory.md)*

*Previous: [Optimization and Strategic Laziness](chapter-12-optimization-and-laziness.md)*