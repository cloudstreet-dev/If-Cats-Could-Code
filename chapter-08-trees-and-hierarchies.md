# Chapter 8: Trees and Cat Family Hierarchies

## The Opening Tail

In the grand estate of Whiskerworth Manor, the feline family tree was both complex and fascinating. At the top reigned Queen Duchess, a majestic Persian who had ruled the manor for twelve years. She had three children: Prince Whiskers (the eldest and heir), Princess Mittens (the diplomat), and Count Shadow (the adventurer).

Prince Whiskers had taken a mate and produced two kittens: Sir Patches and Lady Cream. Princess Mittens, not to be outdone, had her own offspring: Baron Smokey and Earl Tabby. Count Shadow, ever the free spirit, had adopted a stray kitten named Scout.

When the annual Catnip Festival approached, the family hierarchy became crucial. Queen Duchess made the high-level decisions about the festivities. She delegated different responsibilities to her children: Prince Whiskers managed the entertainment, Princess Mittens handled diplomacy with neighboring cat families, and Count Shadow organized the outdoor adventures.

Each of her children, in turn, could delegate to their own offspring. Prince Whiskers had Sir Patches organize the toy competitions while Lady Cream managed the treat distributions. This branching structure meant decisions flowed from the top down, but information could also bubble up from the bottom - if Scout discovered something interesting during patrol, the news would travel up through Count Shadow to Queen Duchess.

The family tree wasn't just about hierarchy; it was about relationships, inheritance of traits, and efficient organization. Each cat knew their place, their responsibilities, and whom to report to, creating a structure that was both flexible and orderly.

## The Technical Purr-spective

In programming, a tree is a data structure that mirrors the cat family hierarchy. Unlike arrays or stacks that organize data linearly, trees organize data in branching, hierarchical relationships. Each element (called a **node**) can have multiple children, but every node has exactly one parent (except the top node, called the **root**).

### Tree Terminology: The Family Vocabulary

Just like cat families have specific relationship terms, trees use family-related terminology:

- **Root**: The top node (Queen Duchess)
- **Parent**: A node that has children (Prince Whiskers)  
- **Child**: A node that has a parent (Sir Patches)
- **Siblings**: Nodes that share the same parent (Sir Patches and Lady Cream)
- **Leaf**: A node with no children (Scout)
- **Ancestor**: Any node above another node in the hierarchy
- **Descendant**: Any node below another node in the hierarchy
- **Depth**: How many levels down from the root
- **Height**: The longest path from a node to a leaf

### Why Trees Matter

Trees are everywhere in programming:
- File systems (folders contain files and other folders)
- Family genealogy
- Organization charts
- Decision-making processes  
- Website navigation structures
- Database hierarchies

They're perfect when data has natural parent-child relationships and when you need to search, organize, or traverse hierarchical information efficiently.

## Code in the Wild

### Creating a Basic Cat Family Tree

```javascript
// Simple tree node representing a cat
class CatNode {
    constructor(name, title = "", age = 0) {
        this.name = name;
        this.title = title;
        this.age = age;
        this.children = [];
        this.parent = null;
    }
    
    // Add a child cat
    addChild(childCat) {
        childCat.parent = this;
        this.children.push(childCat);
    }
    
    // Remove a child (if they move away)
    removeChild(childCat) {
        const index = this.children.indexOf(childCat);
        if (index > -1) {
            this.children.splice(index, 1);
            childCat.parent = null;
        }
    }
    
    // Get full name with title
    getFullName() {
        return this.title ? `${this.title} ${this.name}` : this.name;
    }
    
    // Check if this cat is a leaf (has no children)
    isLeaf() {
        return this.children.length === 0;
    }
    
    // Get all descendants
    getAllDescendants() {
        let descendants = [];
        for (let child of this.children) {
            descendants.push(child);
            descendants = descendants.concat(child.getAllDescendants());
        }
        return descendants;
    }
}

// Building the Whiskerworth Manor family tree
let queenDuchess = new CatNode("Duchess", "Queen", 12);

// First generation children
let princeWhiskers = new CatNode("Whiskers", "Prince", 8);
let princessMittens = new CatNode("Mittens", "Princess", 7);
let countShadow = new CatNode("Shadow", "Count", 6);

// Add children to Queen Duchess
queenDuchess.addChild(princeWhiskers);
queenDuchess.addChild(princessMittens);
queenDuchess.addChild(countShadow);

// Second generation (grandchildren)
let sirPatches = new CatNode("Patches", "Sir", 3);
let ladyCream = new CatNode("Cream", "Lady", 2);
let baronSmokey = new CatNode("Smokey", "Baron", 4);
let earlTabby = new CatNode("Tabby", "Earl", 3);
let scout = new CatNode("Scout", "", 1);

// Add grandchildren to their parents
princeWhiskers.addChild(sirPatches);
princeWhiskers.addChild(ladyCream);
princessMittens.addChild(baronSmokey);
princessMittens.addChild(earlTabby);
countShadow.addChild(scout);

// Display the family tree
console.log("Whiskerworth Manor Family Tree:");
console.log(`Root: ${queenDuchess.getFullName()}, Age: ${queenDuchess.age}`);
```

### Tree Traversal: Visiting the Whole Family

```javascript
class CatFamilyTree {
    constructor(root) {
        this.root = root;
    }
    
    // Depth-First Search (DFS) - visit children before siblings
    // Like a detailed family inspection, going deep into each branch
    depthFirstTraversal(node = this.root, depth = 0) {
        if (!node) return;
        
        // Visit current node
        let indent = "  ".repeat(depth);
        console.log(`${indent}${node.getFullName()} (Age: ${node.age})`);
        
        // Visit all children recursively
        for (let child of node.children) {
            this.depthFirstTraversal(child, depth + 1);
        }
    }
    
    // Breadth-First Search (BFS) - visit all cats at same level first
    // Like announcing news level by level through the family
    breadthFirstTraversal() {
        if (!this.root) return;
        
        let queue = [{ node: this.root, level: 0 }];
        let currentLevel = 0;
        
        console.log("Family Announcement (level by level):");
        
        while (queue.length > 0) {
            let { node, level } = queue.shift();
            
            // Print level header when we reach a new level
            if (level > currentLevel) {
                console.log(`\n--- Generation ${level} ---`);
                currentLevel = level;
            } else if (level === 0) {
                console.log(`\n--- Generation ${level} (Royal) ---`);
            }
            
            console.log(`  ${node.getFullName()} (Age: ${node.age})`);
            
            // Add all children to queue for next level
            for (let child of node.children) {
                queue.push({ node: child, level: level + 1 });
            }
        }
    }
    
    // Find a cat by name (returns first match)
    findCat(name, node = this.root) {
        if (!node) return null;
        
        if (node.name === name) {
            return node;
        }
        
        // Search all children
        for (let child of node.children) {
            let found = this.findCat(name, child);
            if (found) return found;
        }
        
        return null;
    }
    
    // Find all cats meeting certain criteria
    findCatsWhere(predicate, node = this.root, results = []) {
        if (!node) return results;
        
        if (predicate(node)) {
            results.push(node);
        }
        
        for (let child of node.children) {
            this.findCatsWhere(predicate, child, results);
        }
        
        return results;
    }
    
    // Get path from root to a specific cat
    getPathTo(targetName, node = this.root, path = []) {
        if (!node) return null;
        
        path.push(node.getFullName());
        
        if (node.name === targetName) {
            return path.slice(); // Return copy of path
        }
        
        for (let child of node.children) {
            let result = this.getPathTo(targetName, child, path);
            if (result) return result;
        }
        
        path.pop(); // Backtrack
        return null;
    }
    
    // Calculate tree statistics
    getTreeStats(node = this.root) {
        if (!node) return { totalCats: 0, maxDepth: 0, leaves: 0 };
        
        let stats = { totalCats: 1, maxDepth: 1, leaves: 0 };
        
        if (node.isLeaf()) {
            stats.leaves = 1;
        }
        
        for (let child of node.children) {
            let childStats = this.getTreeStats(child);
            stats.totalCats += childStats.totalCats;
            stats.maxDepth = Math.max(stats.maxDepth, childStats.maxDepth + 1);
            stats.leaves += childStats.leaves;
        }
        
        return stats;
    }
}

// Using the family tree
let familyTree = new CatFamilyTree(queenDuchess);

console.log("\n=== DEPTH-FIRST TRAVERSAL ===");
familyTree.depthFirstTraversal();

console.log("\n=== BREADTH-FIRST TRAVERSAL ===");
familyTree.breadthFirstTraversal();

console.log("\n=== FINDING SPECIFIC CATS ===");
let patches = familyTree.findCat("Patches");
if (patches) {
    console.log(`Found: ${patches.getFullName()}, parent is ${patches.parent.getFullName()}`);
}

console.log("\n=== FINDING YOUNG CATS (age <= 3) ===");
let youngCats = familyTree.findCatsWhere(cat => cat.age <= 3);
youngCats.forEach(cat => console.log(`  ${cat.getFullName()} (Age: ${cat.age})`));

console.log("\n=== PATH TO SCOUT ===");
let pathToScout = familyTree.getPathTo("Scout");
console.log("Path: " + pathToScout.join(" → "));

console.log("\n=== FAMILY STATISTICS ===");
let stats = familyTree.getTreeStats();
console.log(`Total cats: ${stats.totalCats}`);
console.log(`Family depth: ${stats.maxDepth} generations`);
console.log(`Cats with no children: ${stats.leaves}`);
```

### Binary Search Tree: The Organized Cat Registry

```javascript
// Special type of tree where each node has at most 2 children
// Left child is smaller, right child is larger (perfect for searching)
class CatRegistryNode {
    constructor(cat) {
        this.cat = cat;  // Store the whole cat object
        this.left = null;   // Cats with smaller IDs
        this.right = null;  // Cats with larger IDs
    }
}

class CatRegistry {
    constructor() {
        this.root = null;
    }
    
    // Add a cat to the registry (sorted by ID)
    register(cat) {
        this.root = this._insertRecursive(this.root, cat);
        console.log(`Registered: ${cat.name} (ID: ${cat.id})`);
    }
    
    _insertRecursive(node, cat) {
        // If empty spot, create new node
        if (!node) {
            return new CatRegistryNode(cat);
        }
        
        // Decide which direction to go
        if (cat.id < node.cat.id) {
            node.left = this._insertRecursive(node.left, cat);
        } else if (cat.id > node.cat.id) {
            node.right = this._insertRecursive(node.right, cat);
        } else {
            // ID already exists, update the cat
            console.log(`Updating existing cat with ID ${cat.id}`);
            node.cat = cat;
        }
        
        return node;
    }
    
    // Find a cat by ID (very efficient!)
    findById(id) {
        return this._searchRecursive(this.root, id);
    }
    
    _searchRecursive(node, id) {
        if (!node || node.cat.id === id) {
            return node ? node.cat : null;
        }
        
        if (id < node.cat.id) {
            return this._searchRecursive(node.left, id);
        } else {
            return this._searchRecursive(node.right, id);
        }
    }
    
    // Get all cats in sorted order (in-order traversal)
    getAllCatsSorted() {
        let cats = [];
        this._inOrderTraversal(this.root, cats);
        return cats;
    }
    
    _inOrderTraversal(node, result) {
        if (node) {
            this._inOrderTraversal(node.left, result);   // Visit left
            result.push(node.cat);                       // Visit current  
            this._inOrderTraversal(node.right, result);  // Visit right
        }
    }
    
    // Find all cats within an ID range
    findCatsInRange(minId, maxId) {
        let cats = [];
        this._rangeSearch(this.root, minId, maxId, cats);
        return cats;
    }
    
    _rangeSearch(node, minId, maxId, result) {
        if (!node) return;
        
        // If current cat is in range, add it
        if (node.cat.id >= minId && node.cat.id <= maxId) {
            result.push(node.cat);
        }
        
        // Search left if there might be cats in range
        if (node.cat.id > minId) {
            this._rangeSearch(node.left, minId, maxId, result);
        }
        
        // Search right if there might be cats in range
        if (node.cat.id < maxId) {
            this._rangeSearch(node.right, minId, maxId, result);
        }
    }
}

// Using the cat registry
let registry = new CatRegistry();

// Register cats (they'll be automatically sorted by ID)
registry.register({ id: 150, name: "Mittens", age: 3 });
registry.register({ id: 75, name: "Shadow", age: 5 });
registry.register({ id: 200, name: "Whiskers", age: 7 });
registry.register({ id: 25, name: "Patches", age: 2 });
registry.register({ id: 100, name: "Cream", age: 4 });
registry.register({ id: 300, name: "Scout", age: 1 });

console.log("\n=== FINDING CATS BY ID ===");
let foundCat = registry.findById(100);
if (foundCat) {
    console.log(`Found: ${foundCat.name}, age ${foundCat.age}`);
}

console.log("\n=== ALL CATS SORTED BY ID ===");
let allCats = registry.getAllCatsSorted();
allCats.forEach(cat => console.log(`ID: ${cat.id}, Name: ${cat.name}, Age: ${cat.age}`));

console.log("\n=== CATS WITH IDs BETWEEN 50 AND 150 ===");
let catsInRange = registry.findCatsInRange(50, 150);
catsInRange.forEach(cat => console.log(`ID: ${cat.id}, Name: ${cat.name}`));
```

### File System Tree: Cat Territory Organization

```javascript
// Trees are perfect for representing hierarchical structures like file systems
class TerritoryNode {
    constructor(name, type = "folder") {
        this.name = name;
        this.type = type; // "folder" or "file"
        this.children = [];
        this.parent = null;
        this.owner = null; // Which cat owns this territory
    }
    
    addChild(child) {
        child.parent = this;
        this.children.push(child);
        return child;
    }
    
    addFolder(name) {
        return this.addChild(new TerritoryNode(name, "folder"));
    }
    
    addFile(name, owner = null) {
        let file = this.addChild(new TerritoryNode(name, "file"));
        file.owner = owner;
        return file;
    }
    
    // Get full path from root
    getFullPath() {
        if (!this.parent) return this.name;
        return this.parent.getFullPath() + "/" + this.name;
    }
    
    // Find territory by path
    findByPath(path) {
        let parts = path.split("/").filter(part => part !== "");
        let current = this;
        
        for (let part of parts) {
            let found = current.children.find(child => child.name === part);
            if (!found) return null;
            current = found;
        }
        
        return current;
    }
    
    // List all territories owned by a specific cat
    findTerritoriesOwnedBy(catName, results = []) {
        if (this.owner === catName) {
            results.push(this);
        }
        
        for (let child of this.children) {
            child.findTerritoriesOwnedBy(catName, results);
        }
        
        return results;
    }
    
    // Display territory tree
    display(indent = 0) {
        let prefix = "  ".repeat(indent);
        let icon = this.type === "folder" ? "📁" : "📄";
        let ownerInfo = this.owner ? ` (owned by ${this.owner})` : "";
        console.log(`${prefix}${icon} ${this.name}${ownerInfo}`);
        
        for (let child of this.children) {
            child.display(indent + 1);
        }
    }
}

// Create cat house territory structure
let house = new TerritoryNode("Cat House");

// First floor
let firstFloor = house.addFolder("First Floor");
let kitchen = firstFloor.addFolder("Kitchen");
kitchen.addFile("food_bowl.txt", "Whiskers");
kitchen.addFile("water_bowl.txt", "Mittens");
kitchen.addFile("treat_jar.txt", "Shadow");

let livingRoom = firstFloor.addFolder("Living Room");
livingRoom.addFile("sunny_spot.txt", "Patches");
livingRoom.addFile("couch_corner.txt", "Cream");
livingRoom.addFolder("under_coffee_table").addFile("secret_hideout.txt", "Scout");

// Second floor
let secondFloor = house.addFolder("Second Floor");
let bedroom = secondFloor.addFolder("Master Bedroom");
bedroom.addFile("human_bed.txt", "Whiskers");
bedroom.addFile("windowsill.txt", "Mittens");

let bathroom = secondFloor.addFolder("Bathroom");
bathroom.addFile("sink.txt", "Shadow");
bathroom.addFile("bathtub.txt", null); // Unclaimed territory

console.log("\n=== CAT HOUSE TERRITORY MAP ===");
house.display();

console.log("\n=== WHISKERS' TERRITORIES ===");
let whiskersSpots = house.findTerritoriesOwnedBy("Whiskers");
whiskersSpots.forEach(spot => console.log(`  ${spot.getFullPath()}`));

console.log("\n=== FINDING SPECIFIC TERRITORY ===");
let secretSpot = house.findByPath("First Floor/Living Room/under_coffee_table/secret_hideout.txt");
if (secretSpot) {
    console.log(`Found: ${secretSpot.getFullPath()}, owned by ${secretSpot.owner}`);
}
```

## Paw-sible Problems

### Exercise 1: Build Your Own Cat Family Tree
Create a family tree for your own cats (real or imaginary) with at least 3 generations.

```javascript
class MyCatFamily {
    // Create your family tree structure
    // Include methods to:
    // - Add new family members
    // - Find cats by name
    // - Get family statistics
    // - Display the tree structure
}

// Test your family tree
```

### Exercise 2: Cat Shelter Organization Tree
Create a hierarchical structure for a cat shelter with different areas and the cats in each area.

```javascript
class CatShelter {
    // Structure: Shelter -> Wings -> Rooms -> Cats
    // Include methods to:
    // - Add new areas and cats
    // - Move cats between rooms
    // - Find empty spaces
    // - Get shelter statistics
}
```

### Exercise 3: Decision Tree for Cat Behavior
Create a decision tree that helps determine what a cat wants based on various conditions.

```javascript
class CatBehaviorTree {
    // Decision nodes with conditions like:
    // - Time of day
    // - Last meal time
    // - Energy level
    // - Weather
    // Lead to decisions like "wants food", "wants to play", "wants to nap"
}
```

### Exercise 4: Cat Skill Tree
Create a skill tree system where cats can learn new abilities based on prerequisites.

```javascript
class CatSkillTree {
    // Skills like "Basic Hunting" -> "Advanced Pouncing" -> "Master Hunter"  
    // Include methods to:
    // - Check if cat can learn a skill
    // - Learn new skills
    // - Display available skills
}
```

### Exercise 5: Cat Territory Binary Search Tree
Implement a binary search tree to manage cat territories sorted by territory size.

```javascript
class TerritoryBST {
    // Store territories sorted by square footage
    // Include methods for:
    // - Adding new territories
    // - Finding territories by size
    // - Finding territories in a size range
    // - Getting all territories in sorted order
}
```

## Cat-ch the Pattern

### Key Takeaways

1. **Trees represent hierarchical relationships**: Perfect for family structures, organizations, and nested data
2. **Every node has one parent (except root)**: Like family trees where each cat has one mother
3. **Traversal methods serve different purposes**: Depth-first for detailed exploration, breadth-first for level-by-level processing
4. **Binary search trees enable efficient searching**: Sorted structure makes finding data much faster
5. **Trees are recursive by nature**: Each subtree is also a tree, making recursive algorithms natural

### Tree Traversal Summary

**Depth-First (DFS)**:
```javascript
// Visit node, then all its children recursively
function depthFirst(node) {
    if (!node) return;
    visit(node);                    // Process current node
    for (let child of node.children) {
        depthFirst(child);          // Recurse on children
    }
}
```

**Breadth-First (BFS)**:
```javascript
// Visit all nodes at current level before going deeper
function breadthFirst(root) {
    let queue = [root];
    while (queue.length > 0) {
        let node = queue.shift();
        visit(node);                // Process current node
        for (let child of node.children) {
            queue.push(child);      // Add children to queue
        }
    }
}
```

### Binary Search Tree Rules

1. **Left child < Parent < Right child**: Enables efficient searching
2. **No duplicate values**: Each value appears only once
3. **In-order traversal gives sorted sequence**: Left -> Node -> Right
4. **Search time is O(log n) for balanced trees**: Much faster than linear search

### When to Use Trees

**Use Trees for:**
- Hierarchical data (family trees, org charts, file systems)
- Decision-making processes
- Searching sorted data efficiently (BST)
- Representing nested structures
- Modeling parent-child relationships
- Menu systems and navigation

**Don't use Trees for:**
- Simple linear data that doesn't have hierarchical relationships
- When you need constant-time access to arbitrary elements
- Data that changes frequently (unless you need the hierarchy)

### Common Tree Operations Complexity

```javascript
// General Tree Operations
// Access specific node: O(n) - might need to search entire tree
// Search for value: O(n) - might need to check every node
// Insert new node: O(1) - if you know where to put it
// Delete node: O(1) - if you know where it is

// Binary Search Tree Operations (balanced tree)
// Search: O(log n) - eliminate half the possibilities each step
// Insert: O(log n) - find correct position, then insert
// Delete: O(log n) - find node, then reorganize
// Min/Max value: O(log n) - go all the way left/right
```

### Common Mistakes to Avoid

```javascript
// Forgetting to set parent references
class BadTreeNode {
    addChild(child) {
        this.children.push(child);
        // Missing: child.parent = this;
    }
}

// Infinite recursion (no base case)
function badTraversal(node) {
    console.log(node.name);
    for (let child of node.children) {
        badTraversal(child);  // What if there's a cycle?
    }
    // Add: if (!node || visited.has(node)) return;
}

// Not handling empty trees
function findNode(root, name) {
    // if (!root) return null;  // Add this check!
    if (root.name === name) return root;
    // ...rest of function
}

// Modifying tree during traversal
function removeYoungCats(node) {
    for (let child of node.children) {
        if (child.age < 2) {
            node.removeChild(child);  // Modifying while iterating!
        }
    }
    // Better: collect nodes to remove first, then remove them
}
```

### Advanced Tree Concepts

```javascript
// Tree balancing (keeping tree roughly even on both sides)
// Unbalanced tree: O(n) search time (essentially a linked list)
//     1
//      \
//       2
//        \
//         3
//          \
//           4

// Balanced tree: O(log n) search time
//       2
//      / \
//     1   3
//          \
//           4

// Self-balancing trees (AVL, Red-Black) automatically maintain balance
// Most programming languages provide these in their standard libraries
```

### Looking Ahead

Trees help us organize hierarchical data, but what happens when we need to find specific information within those structures? In the next chapter, we'll explore searching algorithms - the techniques cats use to hunt down hidden mice, and programmers use to find data quickly and efficiently.

Just as a cat uses different strategies to locate a mouse depending on the environment (systematic room-by-room search vs. following scent trails), programmers use different search algorithms depending on how the data is organized and what they're looking for.

---

*Next Chapter: [Searching for Hidden Mice](chapter-09-searching-for-mice.md)*

*Previous: [Stacks, Queues, and Cat Towers](chapter-07-stacks-and-queues.md)*