# Chapter 9: Searching for Hidden Mice

## The Opening Tail

Detective Whiskers had a reputation throughout the neighborhood for being the finest mouse-hunting cat around. His success wasn't just due to sharp reflexes or keen senses - it was his methodical approach to searching that set him apart from other cats who relied solely on instinct.

When Mrs. Henderson called about strange scratching sounds in her walls, Detective Whiskers arrived with a systematic plan. The house had twelve rooms, and he knew mice could be anywhere. A less experienced cat might just run around randomly, hoping to stumble upon the culprits. But Detective Whiskers had developed several proven strategies over his years of investigation.

For a thorough search of an unfamiliar house, he used his "Room-by-Room Method" - methodically checking every room from left to right, top to bottom. When he had a hunch about which area was most likely (like the kitchen where crumbs gathered), he used his "Best-Guess First" approach, starting with the most promising locations.

If he was tracking a particularly clever mouse that liked to hide in sorted locations - say, bookshelves arranged alphabetically - he employed his famous "Divide and Conquer" technique. He'd start in the middle of the bookshelf. If he smelled mouse scent, but it seemed to come from earlier in the alphabet, he'd focus on the first half. If it seemed to come from later in the alphabet, he'd search the second half. By repeatedly dividing his search space in half, he could locate even the craftiest mouse in record time.

Detective Whiskers understood that the best search strategy depended on the situation: the size of the area, whether it was organized or chaotic, and what information he already had about his target.

## The Technical Purr-spective

In programming, searching is the process of finding specific information within a collection of data. Just like Detective Whiskers has different strategies for different hunting scenarios, programmers have various search algorithms, each suited for different types of data organization and search requirements.

### Types of Search Problems

**Linear Search**: Like checking every room in the house one by one
**Binary Search**: Like Detective Whiskers' divide-and-conquer method (only works on sorted data)
**Depth-First Search**: Like exploring every corner of one room before moving to the next  
**Breadth-First Search**: Like checking one room on each floor before going deeper
**Hash Table Search**: Like knowing exactly which room a mouse prefers (constant-time lookup)

### Why Search Algorithms Matter

The difference between good and bad search algorithms can be dramatic:
- Linear search through 1,000,000 items: might need 1,000,000 steps
- Binary search through 1,000,000 items: needs at most 20 steps
- Hash table lookup: needs just 1 step

Choosing the right search strategy can mean the difference between a program that responds instantly and one that makes users wait.

## Code in the Wild

### Linear Search: The Thorough Room-by-Room Method

```javascript
// Detective Whiskers' basic room-by-room search
function findMouseLinear(rooms, targetMouse) {
    console.log(`Starting linear search for ${targetMouse}...`);
    let stepsCounter = 0;
    
    for (let i = 0; i < rooms.length; i++) {
        stepsCounter++;
        console.log(`Step ${stepsCounter}: Checking ${rooms[i]}`);
        
        if (rooms[i] === targetMouse) {
            console.log(`Found ${targetMouse} in room ${i + 1} after ${stepsCounter} steps!`);
            return i; // Return the index where found
        }
    }
    
    console.log(`${targetMouse} not found after checking all ${stepsCounter} rooms.`);
    return -1; // Not found
}

// The case of the missing mice
let houseRooms = [
    "Living Room", "Kitchen", "Bedroom", "Bathroom", "Basement", 
    "Attic", "Dining Room", "Study", "Laundry Room", "Pantry"
];

let suspiciousSounds = [
    "scurrying", "squeaking", "scratching", "nibbling", "Mickey Mouse", 
    "silence", "purring", "barking", "meowing", "snoring"
];

console.log("=== LINEAR SEARCH DEMONSTRATION ===");
findMouseLinear(suspiciousSounds, "Mickey Mouse");
console.log();
findMouseLinear(suspiciousSounds, "elephant");

// Linear search with objects (more realistic)
function findCatByName(cats, targetName) {
    let comparisons = 0;
    
    for (let cat of cats) {
        comparisons++;
        if (cat.name === targetName) {
            console.log(`Found ${targetName} after ${comparisons} comparisons`);
            return cat;
        }
    }
    
    console.log(`${targetName} not found after ${comparisons} comparisons`);
    return null;
}

let catCollection = [
    { name: "Whiskers", age: 5, job: "detective" },
    { name: "Mittens", age: 3, job: "napper" },
    { name: "Shadow", age: 7, job: "hunter" },
    { name: "Patches", age: 2, job: "toy destroyer" },
    { name: "Cream", age: 4, job: "food critic" }
];

console.log("\n=== SEARCHING CAT COLLECTION ===");
findCatByName(catCollection, "Shadow");
findCatByName(catCollection, "Fluffy");
```

### Binary Search: The Divide and Conquer Method

```javascript
// Binary search only works on SORTED data
function findMouseBinary(sortedRooms, targetMouse) {
    console.log(`Starting binary search for ${targetMouse} in sorted list...`);
    
    let left = 0;
    let right = sortedRooms.length - 1;
    let steps = 0;
    
    while (left <= right) {
        steps++;
        let middle = Math.floor((left + right) / 2);
        let currentRoom = sortedRooms[middle];
        
        console.log(`Step ${steps}: Checking middle position ${middle}: "${currentRoom}"`);
        
        if (currentRoom === targetMouse) {
            console.log(`Found ${targetMouse} at position ${middle} in ${steps} steps!`);
            return middle;
        }
        
        if (currentRoom < targetMouse) {
            console.log(`"${currentRoom}" comes before "${targetMouse}" alphabetically, searching right half`);
            left = middle + 1;
        } else {
            console.log(`"${currentRoom}" comes after "${targetMouse}" alphabetically, searching left half`);
            right = middle - 1;
        }
    }
    
    console.log(`${targetMouse} not found after ${steps} steps.`);
    return -1;
}

// Sorted list of locations where mice might hide
let sortedHidingSpots = [
    "Attic", "Basement", "Bathroom", "Bedroom", "Closet", 
    "Dining Room", "Garage", "Kitchen", "Pantry", "Study"
];

console.log("\n=== BINARY SEARCH DEMONSTRATION ===");
console.log("Searching in:", sortedHidingSpots);
console.log();

findMouseBinary(sortedHidingSpots, "Kitchen");
console.log();
findMouseBinary(sortedHidingSpots, "Attic");
console.log();
findMouseBinary(sortedHidingSpots, "Library"); // Not in list

// Binary search with numbers (mouse trap IDs)
function findTrapById(sortedTrapIds, targetId) {
    let left = 0;
    let right = sortedTrapIds.length - 1;
    let steps = 0;
    
    while (left <= right) {
        steps++;
        let middle = Math.floor((left + right) / 2);
        let currentId = sortedTrapIds[middle];
        
        console.log(`Step ${steps}: Checking trap ID ${currentId} at position ${middle}`);
        
        if (currentId === targetId) {
            console.log(`Found trap ${targetId} in ${steps} steps!`);
            return middle;
        }
        
        if (currentId < targetId) {
            left = middle + 1;
        } else {
            right = middle - 1;
        }
    }
    
    console.log(`Trap ${targetId} not found after ${steps} steps.`);
    return -1;
}

let mouseTrapIds = [101, 105, 112, 118, 125, 133, 140, 147, 156, 162, 171, 189, 195];

console.log("\n=== SEARCHING MOUSE TRAP IDS ===");
console.log("Available traps:", mouseTrapIds);
findTrapById(mouseTrapIds, 140);
console.log();
findTrapById(mouseTrapIds, 200);
```

### Advanced Search: Finding Multiple Matches

```javascript
// Find all mice that match certain criteria
function findAllMatches(cats, predicate, description) {
    console.log(`\nSearching for all cats where ${description}:`);
    let matches = [];
    let comparisons = 0;
    
    for (let cat of cats) {
        comparisons++;
        if (predicate(cat)) {
            matches.push(cat);
        }
    }
    
    console.log(`Found ${matches.length} matches after ${comparisons} comparisons:`);
    matches.forEach(cat => console.log(`  - ${cat.name} (age: ${cat.age}, skill: ${cat.huntingSkill})`));
    
    return matches;
}

let huntingCats = [
    { name: "Detective Whiskers", age: 6, huntingSkill: 95, territory: "Downtown" },
    { name: "Shadow Hunter", age: 4, huntingSkill: 88, territory: "Suburbs" },
    { name: "Mittens", age: 2, huntingSkill: 45, territory: "Downtown" },
    { name: "Ninja Cat", age: 5, huntingSkill: 92, territory: "Industrial" },
    { name: "Lazy Tom", age: 8, huntingSkill: 30, territory: "Suburbs" },
    { name: "Quick Paws", age: 3, huntingSkill: 76, territory: "Downtown" }
];

// Find experienced hunters
findAllMatches(
    huntingCats, 
    cat => cat.huntingSkill >= 80, 
    "hunting skill >= 80"
);

// Find young cats in downtown
findAllMatches(
    huntingCats, 
    cat => cat.age <= 4 && cat.territory === "Downtown", 
    "age <= 4 AND territory is Downtown"
);

// Find cats whose names contain "Cat"
findAllMatches(
    huntingCats, 
    cat => cat.name.toLowerCase().includes("cat"), 
    "name contains 'cat'"
);
```

### Search with Early Termination

```javascript
// Sometimes we want to stop searching as soon as we find what we need
function findFirstExpertHunter(cats, minSkillLevel) {
    console.log(`\nLooking for first cat with hunting skill >= ${minSkillLevel}:`);
    
    for (let i = 0; i < cats.length; i++) {
        let cat = cats[i];
        console.log(`Checking ${cat.name} (skill: ${cat.huntingSkill})`);
        
        if (cat.huntingSkill >= minSkillLevel) {
            console.log(`Found! ${cat.name} meets our requirements.`);
            return cat; // Early termination - stop searching
        }
    }
    
    console.log("No cat found with required skill level.");
    return null;
}

// Find the first cat that can handle the tough mouse case
let availableCats = [
    { name: "Lazy Tom", huntingSkill: 30 },
    { name: "Mittens", huntingSkill: 45 },
    { name: "Quick Paws", huntingSkill: 76 },
    { name: "Shadow Hunter", huntingSkill: 88 }, // We'll stop here
    { name: "Detective Whiskers", huntingSkill: 95 } // Won't check this one
];

findFirstExpertHunter(availableCats, 80);

// Demonstrate the efficiency gain
console.log("\n=== EFFICIENCY COMPARISON ===");
console.log("Linear search (check all):", availableCats.length, "comparisons needed for full list");
console.log("Early termination search:", "4 comparisons needed to find first match");
```

### Specialized Search: Using Indices and Hash Maps

```javascript
// Create an index for faster searching (like a phone book)
class CatDatabase {
    constructor() {
        this.cats = [];
        this.nameIndex = new Map();    // Name -> cat object
        this.territoryIndex = new Map(); // Territory -> array of cats
        this.skillIndex = new Map();     // Skill level -> array of cats
    }
    
    addCat(cat) {
        this.cats.push(cat);
        
        // Update name index (one cat per name)
        this.nameIndex.set(cat.name, cat);
        
        // Update territory index (multiple cats per territory)
        if (!this.territoryIndex.has(cat.territory)) {
            this.territoryIndex.set(cat.territory, []);
        }
        this.territoryIndex.get(cat.territory).push(cat);
        
        // Update skill index (group by skill ranges)
        let skillRange = Math.floor(cat.huntingSkill / 10) * 10; // 0-9, 10-19, 20-29, etc.
        if (!this.skillIndex.has(skillRange)) {
            this.skillIndex.set(skillRange, []);
        }
        this.skillIndex.get(skillRange).push(cat);
        
        console.log(`Added ${cat.name} to database with indices`);
    }
    
    // O(1) search by name - instant!
    findByName(name) {
        console.log(`Hash lookup for "${name}"`);
        let cat = this.nameIndex.get(name);
        if (cat) {
            console.log(`Found instantly: ${cat.name}`);
            return cat;
        } else {
            console.log(`"${name}" not found in database`);
            return null;
        }
    }
    
    // Fast search by territory
    findByTerritory(territory) {
        console.log(`Index lookup for territory "${territory}"`);
        let cats = this.territoryIndex.get(territory) || [];
        console.log(`Found ${cats.length} cats in ${territory}:`);
        cats.forEach(cat => console.log(`  - ${cat.name}`));
        return cats;
    }
    
    // Fast search by skill range
    findBySkillRange(minSkill, maxSkill) {
        console.log(`Index lookup for skill range ${minSkill}-${maxSkill}`);
        let results = [];
        
        for (let skillLevel = Math.floor(minSkill / 10) * 10; 
             skillLevel <= Math.floor(maxSkill / 10) * 10; 
             skillLevel += 10) {
            
            let catsInRange = this.skillIndex.get(skillLevel) || [];
            for (let cat of catsInRange) {
                if (cat.huntingSkill >= minSkill && cat.huntingSkill <= maxSkill) {
                    results.push(cat);
                }
            }
        }
        
        console.log(`Found ${results.length} cats with skill ${minSkill}-${maxSkill}:`);
        results.forEach(cat => console.log(`  - ${cat.name} (${cat.huntingSkill})`));
        return results;
    }
    
    // Show database statistics
    getStats() {
        console.log("\n=== DATABASE STATISTICS ===");
        console.log(`Total cats: ${this.cats.length}`);
        console.log(`Territories: ${this.territoryIndex.size}`);
        console.log(`Skill ranges: ${this.skillIndex.size}`);
        
        console.log("\nCats per territory:");
        for (let [territory, cats] of this.territoryIndex) {
            console.log(`  ${territory}: ${cats.length} cats`);
        }
    }
}

// Build a database with indices
let catDB = new CatDatabase();

let expertCats = [
    { name: "Detective Whiskers", huntingSkill: 95, territory: "Downtown" },
    { name: "Shadow Hunter", huntingSkill: 88, territory: "Suburbs" },
    { name: "Ninja Cat", huntingSkill: 92, territory: "Industrial" },
    { name: "Quick Paws", huntingSkill: 76, territory: "Downtown" },
    { name: "Silent Stalker", huntingSkill: 84, territory: "Rural" },
    { name: "Mouse Nemesis", huntingSkill: 91, territory: "Downtown" }
];

expertCats.forEach(cat => catDB.addCat(cat));

console.log("\n=== INSTANT SEARCH DEMONSTRATIONS ===");
catDB.findByName("Detective Whiskers");
catDB.findByName("Unknown Cat");

catDB.findByTerritory("Downtown");
catDB.findBySkillRange(85, 95);

catDB.getStats();
```

### Performance Comparison: The Great Search Race

```javascript
// Demonstrate the dramatic difference in search performance
function performanceComparison() {
    // Create large datasets
    let smallList = [];
    let mediumList = [];
    let largeList = [];
    
    // Fill with dummy data
    for (let i = 1; i <= 10; i++) {
        smallList.push(`Mouse${i}`);
    }
    
    for (let i = 1; i <= 1000; i++) {
        mediumList.push(`Mouse${i}`);
    }
    
    for (let i = 1; i <= 100000; i++) {
        largeList.push(`Mouse${i}`);
    }
    
    console.log("\n=== PERFORMANCE COMPARISON ===");
    
    function timeSearch(searchFunction, list, target, description) {
        let start = Date.now();
        let result = searchFunction(list, target);
        let end = Date.now();
        let time = end - start;
        
        let found = result !== -1 ? "Found" : "Not found";
        console.log(`${description}: ${found} in ${time}ms`);
        return time;
    }
    
    // Test with worst-case scenario (target at end or not found)
    let worstCaseTarget = "Mouse999999"; // Not in any list
    
    console.log("\nSearching for 'Mouse999999' (worst case):");
    console.log("Small list (10 items):");
    timeSearch(findMouseLinearSimple, smallList, worstCaseTarget, "  Linear Search");
    
    console.log("Medium list (1,000 items):");
    timeSearch(findMouseLinearSimple, mediumList, worstCaseTarget, "  Linear Search");
    
    console.log("Large list (100,000 items):");
    timeSearch(findMouseLinearSimple, largeList, worstCaseTarget, "  Linear Search");
    
    // For binary search, we need sorted lists
    let sortedMedium = [...mediumList].sort();
    let sortedLarge = [...largeList].sort();
    
    console.log("\nBinary Search on same data (sorted):");
    console.log("Medium list (1,000 items):");
    timeSearch(findMouseBinarySimple, sortedMedium, worstCaseTarget, "  Binary Search");
    
    console.log("Large list (100,000 items):");
    timeSearch(findMouseBinarySimple, sortedLarge, worstCaseTarget, "  Binary Search");
}

// Simplified search functions for performance testing
function findMouseLinearSimple(list, target) {
    for (let i = 0; i < list.length; i++) {
        if (list[i] === target) return i;
    }
    return -1;
}

function findMouseBinarySimple(sortedList, target) {
    let left = 0;
    let right = sortedList.length - 1;
    
    while (left <= right) {
        let middle = Math.floor((left + right) / 2);
        if (sortedList[middle] === target) return middle;
        if (sortedList[middle] < target) {
            left = middle + 1;
        } else {
            right = middle - 1;
        }
    }
    return -1;
}

performanceComparison();
```

## Paw-sible Problems

### Exercise 1: Mouse Hunt Simulator
Create a search simulator that lets Detective Whiskers hunt for mice using different search strategies.

```javascript
class MouseHuntSimulator {
    // Implement different search strategies:
    // - linearHunt(mice, target)
    // - binaryHunt(sortedMice, target)  
    // - randomHunt(mice, target)
    // Compare their efficiency and success rates
}

// Test with different scenarios
```

### Exercise 2: Cat Finder Service
Build a service that can find cats based on various criteria.

```javascript
class CatFinderService {
    // Implement search methods:
    // - findByAge(minAge, maxAge)
    // - findByName(namePattern)
    // - findByLocation(location)
    // - findBestMatch(criteria) - complex multi-criteria search
}
```

### Exercise 3: Optimize the Pet Store Database
Given a pet store database, implement efficient search with indices.

```javascript
class PetStoreDatabase {
    // Create indices for common search patterns:
    // - By species (cat, dog, bird, etc.)
    // - By price range
    // - By age
    // - By availability status
    // Measure search performance before and after indices
}
```

### Exercise 4: Smart Mouse Trap Network
Create a system that searches for the best trap placement using different algorithms.

```javascript
class TrapNetwork {
    // Search algorithms to find:
    // - Closest trap to a mouse sighting
    // - Best trap based on success rate
    // - Optimal trap placement for coverage
    // - Path finding between trap locations
}
```

### Exercise 5: Missing Cat Search System
Implement a search system for finding missing cats in a neighborhood.

```javascript
class MissingCatTracker {
    // Search strategies:
    // - breadthFirstSearch() - search expanding outward from last known location
    // - depthFirstSearch() - thoroughly search specific areas
    // - probabilitySearch() - focus on high-probability locations first
    // Track search progress and effectiveness
}
```

## Cat-ch the Pattern

### Key Takeaways

1. **Linear search is simple but slow**: Checks every item one by one - O(n) time complexity
2. **Binary search is fast but requires sorted data**: Eliminates half the possibilities each step - O(log n) time complexity  
3. **Right algorithm for the right data**: Use binary search on sorted data, hash tables for instant lookup
4. **Early termination can save time**: Stop searching when you find what you need
5. **Preprocessing can speed up repeated searches**: Building indices takes time upfront but makes future searches much faster

### Search Algorithm Summary

**Linear Search**:
```javascript
// Best for: unsorted data, small datasets, one-time searches
for (let i = 0; i < array.length; i++) {
    if (array[i] === target) return i;
}
```

**Binary Search**:
```javascript
// Best for: sorted data, large datasets, repeated searches
let left = 0, right = array.length - 1;
while (left <= right) {
    let mid = Math.floor((left + right) / 2);
    if (array[mid] === target) return mid;
    if (array[mid] < target) left = mid + 1;
    else right = mid - 1;
}
```

**Hash Table Search**:
```javascript
// Best for: exact matches, maximum speed, when you can preprocess data
let map = new Map();
// ... populate map
return map.get(target); // O(1) lookup
```

### Time Complexity Comparison

| Algorithm | Best Case | Average Case | Worst Case | Space | Requirements |
|-----------|-----------|--------------|------------|-------|--------------|
| Linear Search | O(1) | O(n) | O(n) | O(1) | None |
| Binary Search | O(1) | O(log n) | O(log n) | O(1) | Sorted data |
| Hash Table | O(1) | O(1) | O(n) | O(n) | Preprocessing |

### When to Use Each Search Type

**Use Linear Search when:**
- Data is unsorted and you can't sort it
- Dataset is small (< 100 items)
- You're searching just once
- You need to find ALL matches, not just the first one
- Memory is extremely limited

**Use Binary Search when:**
- Data is sorted or you can afford to sort it
- Dataset is large (> 1000 items)
- You'll search many times
- You need to find insertion points
- Memory usage must be minimal

**Use Hash Tables/Maps when:**
- You need maximum search speed
- You can preprocess the data
- Memory usage is not a constraint
- You're doing many exact-match searches
- Data doesn't need to be sorted

### Common Search Patterns

```javascript
// Pattern 1: Find first match
function findFirst(array, predicate) {
    for (let item of array) {
        if (predicate(item)) return item;
    }
    return null;
}

// Pattern 2: Find all matches
function findAll(array, predicate) {
    return array.filter(predicate);
}

// Pattern 3: Find best match (minimum/maximum)
function findBest(array, compareFunction) {
    if (array.length === 0) return null;
    let best = array[0];
    for (let i = 1; i < array.length; i++) {
        if (compareFunction(array[i], best) < 0) {
            best = array[i];
        }
    }
    return best;
}

// Pattern 4: Find nearest match
function findNearest(sortedArray, target) {
    let left = 0, right = sortedArray.length - 1;
    let nearest = sortedArray[0];
    
    while (left <= right) {
        let mid = Math.floor((left + right) / 2);
        if (Math.abs(sortedArray[mid] - target) < Math.abs(nearest - target)) {
            nearest = sortedArray[mid];
        }
        
        if (sortedArray[mid] < target) left = mid + 1;
        else right = mid - 1;
    }
    
    return nearest;
}
```

### Search Optimization Tips

```javascript
// Tip 1: Index frequently searched fields
let catsByName = new Map(); // O(1) name lookups
let catsByAge = new Map();  // O(1) age lookups

// Tip 2: Use appropriate data structures
let ageRanges = new Map([
    ["kitten", cats.filter(c => c.age < 1)],
    ["young", cats.filter(c => c.age >= 1 && c.age < 3)],
    ["adult", cats.filter(c => c.age >= 3 && c.age < 7)],
    ["senior", cats.filter(c => c.age >= 7)]
]);

// Tip 3: Cache expensive searches
let searchCache = new Map();
function cachedSearch(query) {
    if (searchCache.has(query)) {
        return searchCache.get(query);
    }
    let result = expensiveSearch(query);
    searchCache.set(query, result);
    return result;
}

// Tip 4: Use early termination when possible
function hasAnySeniorCats(cats) {
    for (let cat of cats) {
        if (cat.age >= 7) return true; // Found one, stop searching
    }
    return false;
}
```

### Looking Ahead

Now that we've mastered finding mice (and data), what happens when we need to organize what we've found? In the next chapter, we'll explore sorting algorithms - the techniques Detective Whiskers uses to organize his case files and programmers use to arrange data in meaningful order.

Just as a well-organized evidence room makes future investigations more efficient, sorted data makes searching, analyzing, and processing information much faster and more effective.

---

*Next Chapter: [Sorting the Kibble Bowl](chapter-10-sorting-the-kibble.md)*

*Previous: [Trees and Cat Family Hierarchies](chapter-08-trees-and-hierarchies.md)*