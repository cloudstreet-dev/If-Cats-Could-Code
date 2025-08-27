# Chapter 7: Stacks, Queues, and Cat Towers

## The Opening Tail

The Great Cat Tower Competition had begun. Five cats - Patches, Whiskers, Mittens, Shadow, and Cream - had discovered that the humans had left a pile of sturdy cardboard boxes in the living room. Like any self-respecting cats, they immediately saw the potential: the ultimate climbing structure.

Patches, being the boldest, jumped onto the first box. Whiskers, not to be outdone, leaped onto Patches' box, creating a two-level tower. Then Mittens climbed up, followed by Shadow, and finally little Cream scrambled to the very top. They had created a magnificent five-cat tower, with Patches at the bottom supporting everyone else, and Cream proudly surveying her domain from the peak.

But towers, as cats know, are temporary things. When Cream decided she wanted down (spotting a tasty treat), she had to climb down first - she couldn't exactly leap through Shadow, Mittens, and Whiskers to reach the ground. One by one, from top to bottom, they dismounted: Cream, then Shadow, then Mittens, Whiskers, and finally Patches.

Meanwhile, across the room, the food bowl situation was quite different. When dinner time arrived, the cats formed an orderly line (well, as orderly as cats can manage). Patches, having arrived first, got to eat first. Whiskers, who arrived second, waited patiently behind Patches. The others lined up in the order they arrived. This was fair: first come, first served. As each cat finished eating, they left from the front, making room for the next hungry feline.

## The Technical Purr-spective

In programming, we have two fundamental data structures that work exactly like our cat scenarios: **stacks** and **queues**. These structures control the order in which we store and retrieve data, just like cats naturally organize themselves in towers and lines.

### Stacks: Last In, First Out (LIFO)

A stack works like the cat tower. The last cat to climb up (Cream) was the first to climb down. In programming terms:
- **Push**: Add an item to the top (like a cat climbing onto the tower)  
- **Pop**: Remove the top item (like the top cat climbing down)
- **LIFO**: Last In, First Out

### Queues: First In, First Out (FIFO)

A queue works like the dinner line. The first cat to arrive (Patches) is the first to eat. In programming terms:
- **Enqueue**: Add an item to the back of the line
- **Dequeue**: Remove an item from the front of the line  
- **FIFO**: First In, First Out

Both structures are incredibly useful for managing data in specific orders, just like cats naturally organize themselves for different activities.

## Code in the Wild

### Building a Cat Stack

```javascript
// JavaScript arrays can work as stacks using push() and pop()
let catTower = [];

// Cats climb onto the tower (push to top)
console.log("Building the cat tower...");
catTower.push("Patches");   // Bottom cat
console.log("Tower: " + catTower);

catTower.push("Whiskers");  // Second level
console.log("Tower: " + catTower);

catTower.push("Mittens");   // Third level
console.log("Tower: " + catTower);

catTower.push("Shadow");    // Fourth level
console.log("Tower: " + catTower);

catTower.push("Cream");     // Top cat
console.log("Final tower: " + catTower);
console.log("Tower height: " + catTower.length + " cats");

// Cats climb down (pop from top)
console.log("\nTower collapsing...");
let topCat = catTower.pop();    // Cream gets down first
console.log(topCat + " climbs down. Remaining: " + catTower);

let secondCat = catTower.pop(); // Shadow is next
console.log(secondCat + " climbs down. Remaining: " + catTower);

// Check who's still on top without removing them
if (catTower.length > 0) {
    let currentTop = catTower[catTower.length - 1];
    console.log("Current top cat: " + currentTop);
}

// Everyone else comes down
while (catTower.length > 0) {
    let nextCat = catTower.pop();
    console.log(nextCat + " climbs down. Remaining: " + catTower.length + " cats");
}

console.log("Tower demolished! All cats safely down.");
```

### Creating a Custom Stack Class

```javascript
class CatTower {
    constructor() {
        this.cats = [];
    }
    
    // Add a cat to the top
    push(catName) {
        this.cats.push(catName);
        console.log(`${catName} climbed onto the tower. Height: ${this.cats.length}`);
    }
    
    // Remove the top cat
    pop() {
        if (this.isEmpty()) {
            console.log("No cats on the tower!");
            return null;
        }
        let topCat = this.cats.pop();
        console.log(`${topCat} climbed down. Height: ${this.cats.length}`);
        return topCat;
    }
    
    // Look at the top cat without removing them
    peek() {
        if (this.isEmpty()) {
            return null;
        }
        return this.cats[this.cats.length - 1];
    }
    
    // Check if tower is empty
    isEmpty() {
        return this.cats.length === 0;
    }
    
    // Get tower height
    size() {
        return this.cats.length;
    }
    
    // Show the whole tower
    display() {
        if (this.isEmpty()) {
            console.log("Empty tower");
            return;
        }
        
        console.log("Cat Tower (top to bottom):");
        for (let i = this.cats.length - 1; i >= 0; i--) {
            let level = this.cats.length - i;
            console.log(`  Level ${level}: ${this.cats[i]}`);
        }
    }
}

// Using our custom cat tower
let tower = new CatTower();

tower.push("Patches");
tower.push("Whiskers");
tower.push("Mittens");
tower.display();

console.log("Top cat is: " + tower.peek());
tower.pop();
tower.display();
```

### Building a Cat Queue (Dinner Line)

```javascript
// JavaScript arrays can work as queues using push() and shift()
let dinnerLine = [];

// Cats join the line (enqueue - add to back)
console.log("Dinner time! Cats forming a line...");
dinnerLine.push("Patches");     // First in line
console.log("Line: " + dinnerLine);

dinnerLine.push("Whiskers");    // Second in line
console.log("Line: " + dinnerLine);

dinnerLine.push("Mittens");     // Third in line
console.log("Line: " + dinnerLine);

dinnerLine.push("Shadow");      // Fourth in line
console.log("Line: " + dinnerLine);

dinnerLine.push("Cream");       // Last in line
console.log("Final line: " + dinnerLine);
console.log("Cats waiting: " + dinnerLine.length);

// Cats eat and leave (dequeue - remove from front)
console.log("\nServing dinner...");
let firstCat = dinnerLine.shift();  // Patches eats first
console.log(firstCat + " is eating. Still waiting: " + dinnerLine);

let secondCat = dinnerLine.shift(); // Whiskers is next
console.log(secondCat + " is eating. Still waiting: " + dinnerLine);

// Check who's next without removing them
if (dinnerLine.length > 0) {
    let nextCat = dinnerLine[0];
    console.log("Next to eat: " + nextCat);
}

// Everyone else gets fed
while (dinnerLine.length > 0) {
    let nextCat = dinnerLine.shift();
    console.log(nextCat + " is eating. " + dinnerLine.length + " cats still waiting");
}

console.log("All cats fed! Dinner time over.");
```

### Creating a Custom Queue Class

```javascript
class CatDinnerLine {
    constructor() {
        this.cats = [];
    }
    
    // Add a cat to the back of the line
    enqueue(catName) {
        this.cats.push(catName);
        console.log(`${catName} joined the dinner line. Position: ${this.cats.length}`);
    }
    
    // Remove the first cat from the line
    dequeue() {
        if (this.isEmpty()) {
            console.log("No cats in line!");
            return null;
        }
        let firstCat = this.cats.shift();
        console.log(`${firstCat} is now eating. ${this.cats.length} cats still waiting`);
        return firstCat;
    }
    
    // Look at the first cat without removing them
    front() {
        if (this.isEmpty()) {
            return null;
        }
        return this.cats[0];
    }
    
    // Look at the last cat in line
    back() {
        if (this.isEmpty()) {
            return null;
        }
        return this.cats[this.cats.length - 1];
    }
    
    // Check if line is empty
    isEmpty() {
        return this.cats.length === 0;
    }
    
    // Get line length
    size() {
        return this.cats.length;
    }
    
    // Show the whole line
    display() {
        if (this.isEmpty()) {
            console.log("No cats in line");
            return;
        }
        
        console.log("Dinner Line (front to back):");
        this.cats.forEach((cat, index) => {
            let position = index + 1;
            let status = index === 0 ? "(next to eat)" : `(waiting, position ${position})`;
            console.log(`  ${position}. ${cat} ${status}`);
        });
    }
}

// Using our custom dinner line
let dinnerQueue = new CatDinnerLine();

dinnerQueue.enqueue("Patches");
dinnerQueue.enqueue("Whiskers");  
dinnerQueue.enqueue("Mittens");
dinnerQueue.display();

console.log("Next to eat: " + dinnerQueue.front());
console.log("Last in line: " + dinnerQueue.back());

dinnerQueue.dequeue();
dinnerQueue.display();
```

### Real-World Applications: When to Use Each

```javascript
// Stack Example: Undo functionality (like cats retracing steps)
class CatActionHistory {
    constructor() {
        this.actions = [];
    }
    
    performAction(action) {
        this.actions.push(action);
        console.log(`Cat performed: ${action}`);
    }
    
    undo() {
        if (this.actions.length === 0) {
            console.log("No actions to undo");
            return;
        }
        let lastAction = this.actions.pop();
        console.log(`Undoing: ${lastAction}`);
    }
    
    showHistory() {
        console.log("Action history:", this.actions);
    }
}

let catHistory = new CatActionHistory();
catHistory.performAction("knocked over water bowl");
catHistory.performAction("climbed curtains");
catHistory.performAction("meowed loudly");
catHistory.showHistory();

catHistory.undo();  // Un-meows
catHistory.undo();  // Un-climbs curtains
catHistory.showHistory();

// Queue Example: Task scheduling (like cat chores)
class CatTaskScheduler {
    constructor() {
        this.tasks = [];
    }
    
    addTask(task, priority = "normal") {
        let taskObj = { task, priority, timestamp: Date.now() };
        
        if (priority === "urgent") {
            // Urgent tasks go to front (but this breaks pure queue behavior)
            this.tasks.unshift(taskObj);
            console.log(`URGENT task added: ${task}`);
        } else {
            this.tasks.push(taskObj);
            console.log(`Task scheduled: ${task}`);
        }
    }
    
    completeNextTask() {
        if (this.tasks.length === 0) {
            console.log("No tasks to complete");
            return;
        }
        
        let nextTask = this.tasks.shift();
        console.log(`Completing task: ${nextTask.task}`);
        return nextTask;
    }
    
    showTasks() {
        if (this.tasks.length === 0) {
            console.log("No tasks scheduled");
            return;
        }
        
        console.log("Scheduled tasks:");
        this.tasks.forEach((task, index) => {
            console.log(`  ${index + 1}. ${task.task} (${task.priority})`);
        });
    }
}

let scheduler = new CatTaskScheduler();
scheduler.addTask("clean litter box");
scheduler.addTask("refill water bowl");
scheduler.addTask("find missing toy mouse", "urgent");
scheduler.showTasks();

scheduler.completeNextTask();
scheduler.completeNextTask();
scheduler.showTasks();
```

### Advanced Operations: Stack and Queue Combinations

```javascript
// Cat Playground: Multiple structures working together
class CatPlayground {
    constructor() {
        this.towerQueue = [];      // Queue of cats waiting to climb tower
        this.currentTower = [];    // Stack of cats on the tower
        this.playedCats = [];      // Cats who have finished playing
    }
    
    // Cat joins the line to play
    joinQueue(catName) {
        this.towerQueue.push(catName);
        console.log(`${catName} is waiting to climb the tower. Position: ${this.towerQueue.length}`);
        this.displayStatus();
    }
    
    // Next cat in line climbs the tower
    climbTower() {
        if (this.towerQueue.length === 0) {
            console.log("No cats waiting to climb");
            return;
        }
        
        let nextCat = this.towerQueue.shift();  // Remove from queue (FIFO)
        this.currentTower.push(nextCat);        // Add to tower (stack)
        console.log(`${nextCat} climbed onto the tower!`);
        this.displayStatus();
    }
    
    // Top cat gets down from tower
    descendTower() {
        if (this.currentTower.length === 0) {
            console.log("No cats on the tower");
            return;
        }
        
        let topCat = this.currentTower.pop();   // Remove from tower (LIFO)
        this.playedCats.push(topCat);           // Add to finished group
        console.log(`${topCat} climbed down and finished playing`);
        this.displayStatus();
    }
    
    // Show current state of playground
    displayStatus() {
        console.log("\n--- PLAYGROUND STATUS ---");
        console.log("Waiting in line:", this.towerQueue.length > 0 ? this.towerQueue.join(", ") : "none");
        console.log("On tower (bottom to top):", this.currentTower.length > 0 ? this.currentTower.join(" -> ") : "empty");
        console.log("Finished playing:", this.playedCats.length > 0 ? this.playedCats.join(", ") : "none");
        console.log("-------------------------\n");
    }
}

// Simulate playground activity
let playground = new CatPlayground();

// Cats arrive and want to play
playground.joinQueue("Patches");
playground.joinQueue("Whiskers");
playground.joinQueue("Mittens");

// Some cats climb up
playground.climbTower();
playground.climbTower();

// More cats join
playground.joinQueue("Shadow");
playground.joinQueue("Cream");

// Tower activity
playground.climbTower();  // Mittens climbs up
playground.descendTower(); // Mittens comes down (LIFO)
playground.climbTower();   // Shadow climbs up
playground.descendTower(); // Shadow comes down
playground.descendTower(); // Whiskers comes down
playground.descendTower(); // Patches comes down
playground.climbTower();   // Cream finally gets to climb
```

## Paw-sible Problems

### Exercise 1: Cat Treat Dispenser (Stack)
Create a treat dispenser that works like a stack - treats are added to the top and dispensed from the top.

```javascript
class TreatDispenser {
    // Implement push (add treat), pop (dispense treat), and peek (see next treat)
    // Track different treat types and how many of each
}

// Test your dispenser
let dispenser = new TreatDispenser();
// Add treats, dispense them, check status
```

### Exercise 2: Vet Appointment Queue
Create a queue system for a veterinary clinic where cats wait their turn.

```javascript
class VetClinic {
    // Implement appointment scheduling with:
    // - Regular appointments (normal queue)
    // - Emergency cases (jump to front)
    // - Show waiting times
}

// Test your clinic system
```

### Exercise 3: Cat Browser History
Implement a browser history system using a stack, where cats can visit pages and use "back" button.

```javascript
class CatBrowserHistory {
    // Implement:
    // - visit(page) - go to new page
    // - back() - go to previous page  
    // - canGoBack() - check if back is possible
    // - showHistory() - display current stack
}

// Test browsing behavior
```

### Exercise 4: Multi-Stack Cat Hotel
Create a cat hotel with multiple towers (stacks) where cats can check in and out.

```javascript
class CatHotel {
    // Multiple stacks representing different towers
    // Methods:
    // - checkIn(catName, towerNumber)
    // - checkOut(towerNumber) - top cat leaves
    // - findCat(catName) - which tower is the cat in?
    // - showAllTowers()
}
```

### Exercise 5: Priority Cat Queue
Create a queue that handles both regular cats and VIP cats (who get priority).

```javascript
class PriorityCatQueue {
    // Two internal queues: regular and VIP
    // VIP cats are always served first
    // Within each group, maintain FIFO order
}
```

## Cat-ch the Pattern

### Key Takeaways

1. **Stacks are LIFO**: Last in, first out - like cats climbing up and down towers
2. **Queues are FIFO**: First in, first out - like fair waiting lines
3. **Choose the right structure**: Use stacks for reversible operations, queues for fair ordering
4. **JavaScript arrays support both**: push/pop for stacks, push/shift for queues
5. **Real applications everywhere**: Undo systems, task scheduling, browser history, etc.

### When to Use Each Structure

**Use Stacks for:**
- Undo/Redo functionality
- Browser history
- Function call management
- Backtracking algorithms
- Reversing operations
- Temporary storage where latest item is most important

**Use Queues for:**
- Task scheduling
- Print job management  
- Breadth-first search
- Buffer for streaming data
- Fair resource allocation
- Any "first come, first served" scenario

### Stack Operations Summary
```javascript
// Basic stack operations
let stack = [];
stack.push(item);    // Add to top
let item = stack.pop();     // Remove from top
let top = stack[stack.length - 1];  // Peek at top
let isEmpty = stack.length === 0;   // Check if empty
```

### Queue Operations Summary
```javascript
// Basic queue operations  
let queue = [];
queue.push(item);    // Add to back (enqueue)
let item = queue.shift();   // Remove from front (dequeue)
let front = queue[0];       // Peek at front
let back = queue[queue.length - 1];  // Peek at back
let isEmpty = queue.length === 0;    // Check if empty
```

### Performance Considerations

```javascript
// For large datasets, be aware of performance:

// Array.shift() is slow for large arrays (O(n))
// because it has to move all elements
let slowQueue = [];
for (let i = 0; i < 10000; i++) {
    slowQueue.push(`cat${i}`);
}
// This will be slow:
// slowQueue.shift();  // Has to move 9999 elements

// For high-performance queues, consider using:
// - Linked lists
// - Circular buffers  
// - Libraries like collections.deque in other languages

// Stacks with push/pop are fast (O(1)) because they work at array end
let fastStack = [];
fastStack.push("cat");  // Fast
fastStack.pop();        // Fast
```

### Common Mistakes to Avoid

```javascript
// Don't mix stack and queue operations carelessly
let confused = [];
confused.push("first");   // Add to end
confused.push("second");  // Add to end  
confused.shift();         // Remove from front - this breaks stack behavior!

// Be consistent with your data structure choice
// If you need a stack, use push/pop
// If you need a queue, use push/shift

// Don't forget to check for empty structures
let emptyStack = [];
// console.log(emptyStack.pop());  // undefined - but no error
// Better:
if (emptyStack.length > 0) {
    console.log(emptyStack.pop());
} else {
    console.log("Stack is empty!");
}

// Remember array.shift() and array.unshift() can be confusing
let array = [1, 2, 3];
array.unshift(0);  // Adds to FRONT: [0, 1, 2, 3]
array.shift();     // Removes from FRONT: [1, 2, 3]
array.push(4);     // Adds to END: [1, 2, 3, 4]
array.pop();       // Removes from END: [1, 2, 3]
```

### Looking Ahead

Stacks and queues are linear structures - they organize data in a single line or column. In the next chapter, we'll explore trees and hierarchies, which allow us to organize data in more complex, branching structures, much like how cats organize themselves in family hierarchies and social structures.

Trees let us represent relationships where one item can have multiple "children," like a cat family with parents, siblings, and multiple generations - perfect for organizing complex, related data.

---

*Next Chapter: [Trees and Cat Family Hierarchies](chapter-08-trees-and-hierarchies.md)*

*Previous: [Objects as Individual Cats](chapter-06-objects-as-cats.md)*