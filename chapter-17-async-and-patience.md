# Chapter 17: Async Programming and Patient Waiting

## The Opening Tail

Zen Master Whiskers had perfected the art of simultaneous awareness. While most cats could only focus on one thing at a time, Zen had learned to maintain multiple streams of attention simultaneously. He could sit by the window, apparently motionless, yet be acutely aware of the bird feeder outside, the sounds from the kitchen, footsteps on the stairs, and the subtle changes in sunlight across the room.

His human, watching this display of feline multitasking, marveled at how Zen could instantly pivot his attention based on the most important or time-sensitive event. If a bird landed at the feeder, Zen's focus would shift there immediately, but he never completely stopped monitoring the other streams of information. The sound of a can opener in the kitchen might take priority over the bird, while an unexpected noise on the stairs could supersede both.

Zen had also mastered the art of patient waiting without blocking other activities. When waiting for his dinner, he didn't just sit staring at the food bowl, completely oblivious to everything else. Instead, he maintained his dinner vigil while continuing to track the movements of dust motes, monitor the status of his favorite sunny spot, and stay alert to any interesting developments elsewhere in the house.

Most impressively, Zen could coordinate multiple time-sensitive operations. He could start grooming his paw, pause mid-lick when a bird appeared, stalk the bird until it flew away, return to complete his grooming exactly where he left off, and still be ready to respond if his human called his name. Each activity had its own timeline and priority, but none completely blocked the others.

This wasn't mere multitasking - it was orchestrated parallel processing with intelligent priority management, something that would take human programmers decades to master, but came naturally to a cat who understood that survival and comfort both depended on never missing important opportunities.

## The Technical Purr-spective

Asynchronous programming is the art of managing multiple operations that don't complete immediately, without freezing your program while waiting. Like Zen Master Whiskers monitoring multiple streams of activity simultaneously, async code allows programs to remain responsive and efficient by continuing other work while waiting for slow operations to complete.

### Why Asynchronous Programming Matters

**Responsiveness**: User interfaces stay interactive while loading data
**Efficiency**: CPU can work on other tasks while waiting for I/O operations
**Scalability**: Servers can handle many requests simultaneously
**Resource Utilization**: Better use of available system resources

### Key Concepts

**Synchronous**: Operations complete one after another, blocking until finished
**Asynchronous**: Operations can start and complete independently
**Callbacks**: Functions called when async operations complete
**Promises**: Objects representing eventual completion or failure
**Async/Await**: Syntax that makes async code look like sync code

### Common Async Operations

- Network requests (API calls, downloading data)
- File system operations (reading, writing files)
- Database queries
- Timer operations (delays, timeouts)
- User input events

## Code in the Wild

### The Callback Pattern: Basic Cat Coordination

```javascript
// Cat coordination using callbacks - the original async pattern
class CatCoordinator {
    constructor() {
        this.activities = [];
        this.completedTasks = [];
    }
    
    // Traditional callback-based approach
    checkWeather(callback) {
        console.log("🌤️ Checking weather for outdoor patrol...");
        
        // Simulate API call delay
        setTimeout(() => {
            let weather = Math.random() > 0.5 ? 'sunny' : 'rainy';
            let temperature = Math.floor(Math.random() * 30) + 5; // 5-35°C
            
            console.log(`   Weather: ${weather}, ${temperature}°C`);
            callback(null, { weather, temperature });
        }, 1000);
    }
    
    scheduleFeeding(catName, callback) {
        console.log(`🍽️ Scheduling feeding for ${catName}...`);
        
        setTimeout(() => {
            let nextMealTime = new Date(Date.now() + (4 * 60 * 60 * 1000)); // 4 hours from now
            console.log(`   ${catName}'s next meal: ${nextMealTime.toLocaleTimeString()}`);
            callback(null, { catName, nextMealTime });
        }, 800);
    }
    
    findOptimalNapSpot(preferences, callback) {
        console.log("😴 Finding optimal nap spot...");
        
        setTimeout(() => {
            let spots = ['sunny windowsill', 'warm radiator', 'human bed', 'cardboard box'];
            let optimalSpot = spots[Math.floor(Math.random() * spots.length)];
            console.log(`   Best nap spot: ${optimalSpot}`);
            callback(null, { location: optimalSpot, comfort: 8 });
        }, 1200);
    }
    
    // Callback hell example - deeply nested callbacks (the pyramid of doom)
    // This is what happens when cats plan their day the old-fashioned way
    coordinateDailyRoutine(catName, callback) {
        console.log(`\\n🏠 Starting daily coordination for ${catName}`);
        console.log("(Prepare for callback hell - abandon hope, all ye who enter here)\\n");
        
        // Check weather first (because outdoor plans depend on it)
        this.checkWeather((err, weather) => {
            if (err) return callback(err); // Weather service down, panic
            
            console.log("Weather check complete, scheduling feeding...");
            
            // Then schedule feeding (nested because cat logic)
            this.scheduleFeeding(catName, (err, feeding) => {
                if (err) return callback(err);
                
                console.log("Feeding scheduled, finding nap spot...");
                
                // Then find nap spot
                this.findOptimalNapSpot({ temperature: weather.temperature }, (err, napSpot) => {
                    if (err) return callback(err);
                    
                    console.log("Nap spot found, coordination complete!");
                    
                    // All done - return combined results
                    callback(null, {
                        weather: weather,
                        feeding: feeding,
                        napSpot: napSpot,
                        coordinationTime: new Date()
                    });
                });
            });
        });
    }
    
    // Parallel callbacks - multiple operations at once
    coordinateMultipleCats(catNames, callback) {
        console.log(`\\n👥 Coordinating ${catNames.length} cats in parallel`);
        
        let completedCats = [];
        let errors = [];
        let completed = 0;
        
        catNames.forEach(catName => {
            this.coordinateDailyRoutine(catName, (err, result) => {
                completed++;
                
                if (err) {
                    errors.push({ cat: catName, error: err });
                } else {
                    completedCats.push({ cat: catName, result: result });
                }
                
                // Check if all cats are done
                if (completed === catNames.length) {
                    if (errors.length > 0) {
                        callback(new Error(`Failed for ${errors.length} cats`), { completedCats, errors });
                    } else {
                        callback(null, completedCats);
                    }
                }
            });
        });
    }
}

// Demonstrate callback-based coordination
console.log("=== CALLBACK-BASED ASYNC COORDINATION ===");

let coordinator = new CatCoordinator();

// Single cat coordination
coordinator.coordinateDailyRoutine("Whiskers", (err, result) => {
    if (err) {
        console.error("❌ Coordination failed:", err.message);
    } else {
        console.log("✅ Coordination complete for Whiskers:");
        console.log("   Weather:", result.weather.weather, result.weather.temperature + "°C");
        console.log("   Next meal:", result.feeding.nextMealTime.toLocaleTimeString());
        console.log("   Nap spot:", result.napSpot.location);
    }
});

// Multiple cats coordination
setTimeout(() => {
    coordinator.coordinateMultipleCats(["Mittens", "Shadow", "Patches"], (err, results) => {
        if (err) {
            console.error("❌ Multi-cat coordination had issues:", err.message);
        } else {
            console.log(`✅ Successfully coordinated ${results.length} cats`);
        }
    });
}, 5000);
```

### Promises: Better Async Cat Management

```javascript
// Promise-based cat management - cleaner than callbacks
class PromiseCatManager {
    constructor() {
        this.operationId = 0;
    }
    
    createDelay(ms, label) {
        return new Promise((resolve) => {
            console.log(`⏳ ${label} starting (${ms}ms delay)...`);
            setTimeout(() => {
                console.log(`✅ ${label} completed`);
                resolve();
            }, ms);
        });
    }
    
    checkCatHealth(catName) {
        return new Promise((resolve, reject) => {
            console.log(`🏥 Checking health for ${catName}...`);
            
            setTimeout(() => {
                // Simulate occasional health issues
                if (Math.random() < 0.1) { // 10% chance of health issue
                    reject(new Error(`${catName} needs veterinary attention`));
                } else {
                    let healthScore = Math.floor(Math.random() * 20) + 80; // 80-100
                    resolve({ 
                        catName, 
                        healthScore, 
                        status: healthScore > 90 ? 'excellent' : 'good',
                        lastCheckup: new Date()
                    });
                }
            }, 1500);
        });
    }
    
    updateFoodInventory(catName) {
        return new Promise((resolve, reject) => {
            console.log(`📦 Updating food inventory for ${catName}...`);
            
            setTimeout(() => {
                let currentStock = Math.floor(Math.random() * 1000) + 100; // 100-1100g
                
                if (currentStock < 200) {
                    reject(new Error(`Low food stock for ${catName}: ${currentStock}g remaining`));
                } else {
                    resolve({
                        catName,
                        currentStock,
                        daysRemaining: Math.floor(currentStock / 150), // Assuming 150g per day
                        needsReorder: currentStock < 500
                    });
                }
            }, 800);
        });
    }
    
    scheduleCatActivity(catName, activity) {
        return new Promise((resolve) => {
            console.log(`📅 Scheduling ${activity} for ${catName}...`);
            
            setTimeout(() => {
                let scheduledTime = new Date(Date.now() + Math.random() * 8 * 60 * 60 * 1000); // Random time in next 8 hours
                resolve({
                    catName,
                    activity,
                    scheduledTime,
                    priority: Math.floor(Math.random() * 5) + 1 // 1-5 priority
                });
            }, 600);
        });
    }
    
    // Promise chaining - operations that depend on each other
    setupNewCat(catName) {
        console.log(`\\n🐱 Setting up new cat: ${catName}`);
        
        return this.checkCatHealth(catName)
            .then(healthResult => {
                console.log(`Health check passed for ${catName}`);
                return this.updateFoodInventory(catName);
            })
            .then(inventoryResult => {
                console.log(`Inventory updated for ${catName}`);
                return this.scheduleCatActivity(catName, 'initial_orientation');
            })
            .then(scheduleResult => {
                console.log(`Activities scheduled for ${catName}`);
                return {
                    setup: 'complete',
                    catName: catName,
                    setupTime: new Date()
                };
            })
            .catch(error => {
                console.error(`❌ Setup failed for ${catName}:`, error.message);
                throw error;
            });
    }
    
    // Promise.all - parallel operations
    setupMultipleCats(catNames) {
        console.log(`\\n👥 Setting up multiple cats: ${catNames.join(', ')}`);
        
        // Start all setups simultaneously
        let setupPromises = catNames.map(catName => this.setupNewCat(catName));
        
        return Promise.all(setupPromises)
            .then(results => {
                console.log(`✅ All ${catNames.length} cats set up successfully`);
                return results;
            })
            .catch(error => {
                console.error(`❌ Failed to set up all cats:`, error.message);
                throw error;
            });
    }
    
    // Promise.allSettled - handle partial failures gracefully
    bulkHealthCheck(catNames) {
        console.log(`\\n🏥 Bulk health check for: ${catNames.join(', ')}`);
        
        let healthPromises = catNames.map(catName => 
            this.checkCatHealth(catName)
                .catch(error => ({ error: error.message, catName }))
        );
        
        return Promise.allSettled(healthPromises)
            .then(results => {
                let successful = results.filter(r => r.status === 'fulfilled').map(r => r.value);
                let failed = results.filter(r => r.status === 'rejected').map(r => r.reason);
                
                console.log(`Health check results: ${successful.length} passed, ${failed.length} failed`);
                
                return {
                    successful,
                    failed,
                    totalChecked: catNames.length
                };
            });
    }
    
    // Promise racing - first response wins
    findQuickestVet(vetNames) {
        console.log(`\\n🏥 Finding quickest available vet from: ${vetNames.join(', ')}`);
        
        let vetPromises = vetNames.map(vetName => 
            new Promise((resolve) => {
                let responseTime = Math.random() * 3000 + 500; // 0.5-3.5 seconds
                setTimeout(() => {
                    resolve({ 
                        vetName, 
                        responseTime: Math.round(responseTime),
                        availability: 'available'
                    });
                }, responseTime);
            })
        );
        
        return Promise.race(vetPromises)
            .then(quickestVet => {
                console.log(`✅ Quickest vet: ${quickestVet.vetName} (${quickestVet.responseTime}ms)`);
                return quickestVet;
            });
    }
}

// Demonstrate promise-based operations
setTimeout(() => {
    console.log("\\n=== PROMISE-BASED ASYNC MANAGEMENT ===");
    
    let promiseManager = new PromiseCatManager();
    
    // Single cat setup with promise chaining
    promiseManager.setupNewCat("Whiskers")
        .then(result => {
            console.log("Single cat setup result:", result);
            
            // Multiple cats setup
            return promiseManager.setupMultipleCats(["Mittens", "Shadow"]);
        })
        .then(results => {
            console.log("Multiple cat setup results:", results.length, "cats");
            
            // Bulk health check with partial failures handled
            return promiseManager.bulkHealthCheck(["Whiskers", "Mittens", "Shadow", "Patches"]);
        })
        .then(healthResults => {
            console.log("Bulk health check:", healthResults);
            
            // Find quickest vet
            return promiseManager.findQuickestVet(["Dr. Smith", "Dr. Johnson", "Dr. Brown"]);
        })
        .then(vetResult => {
            console.log("Quickest vet found:", vetResult.vetName);
        })
        .catch(error => {
            console.error("Promise chain error:", error.message);
        });
    
}, 6000);
```

### Async/Await: The Modern Cat Whisperer

```javascript
// Modern async/await syntax - most readable async code
class ModernCatWhisperer {
    constructor() {
        this.cats = new Map();
    }
    
    // Utility method for creating realistic delays
    delay(ms, label = '') {
        return new Promise(resolve => {
            if (label) console.log(`⏳ ${label} (${ms}ms)...`);
            setTimeout(resolve, ms);
        });
    }
    
    // Simulate API calls and external services
    async fetchWeatherData() {
        await this.delay(1200, 'Fetching weather data');
        return {
            temperature: Math.floor(Math.random() * 25) + 10,
            humidity: Math.floor(Math.random() * 40) + 40,
            windSpeed: Math.floor(Math.random() * 20) + 5,
            conditions: ['sunny', 'cloudy', 'rainy'][Math.floor(Math.random() * 3)]
        };
    }
    
    async queryVetDatabase(catId) {
        await this.delay(800, `Querying vet records for cat ${catId}`);
        
        // Simulate occasional network issues
        if (Math.random() < 0.1) {
            throw new Error('Veterinary database connection timeout');
        }
        
        return {
            catId,
            lastVisit: new Date(Date.now() - Math.random() * 30 * 24 * 60 * 60 * 1000), // Last 30 days
            vaccinations: ['rabies', 'distemper', 'hepatitis'],
            nextVisit: new Date(Date.now() + 180 * 24 * 60 * 60 * 1000), // 6 months from now
            healthNotes: 'Healthy and active'
        };
    }
    
    async calculateOptimalSchedule(catId, weather, vetInfo) {
        await this.delay(600, `Calculating schedule for cat ${catId}`);
        
        let schedule = {
            morning: [],
            afternoon: [],
            evening: []
        };
        
        // Weather-dependent activities
        if (weather.conditions === 'sunny') {
            schedule.morning.push('outdoor patrol', 'sunbathing');
            schedule.afternoon.push('garden exploration');
        } else {
            schedule.morning.push('indoor play', 'window watching');
            schedule.afternoon.push('puzzle feeders', 'hide and seek');
        }
        
        // Health-based activities
        let daysSinceVet = (Date.now() - vetInfo.lastVisit) / (24 * 60 * 60 * 1000);
        if (daysSinceVet > 150) { // More than 5 months
            schedule.morning.push('health monitoring');
        }
        
        schedule.evening = ['dinner', 'grooming', 'cuddle time'];
        
        return schedule;
    }
    
    // Sequential async operations using async/await
    async setupCatDay(catId) {
        try {
            console.log(`\\n🌅 Setting up day for cat ${catId}`);
            
            // Step 1: Get weather (this could be slow)
            let weather = await this.fetchWeatherData();
            console.log(`Weather: ${weather.conditions}, ${weather.temperature}°C`);
            
            // Step 2: Get vet information (this could also be slow)
            let vetInfo = await this.queryVetDatabase(catId);
            console.log(`Vet info: Last visit ${Math.floor((Date.now() - vetInfo.lastVisit) / (24 * 60 * 60 * 1000))} days ago`);
            
            // Step 3: Calculate schedule based on previous results
            let schedule = await this.calculateOptimalSchedule(catId, weather, vetInfo);
            console.log(`Schedule created with ${Object.values(schedule).flat().length} activities`);
            
            return {
                catId,
                weather,
                vetInfo,
                schedule,
                setupComplete: new Date()
            };
            
        } catch (error) {
            console.error(`❌ Failed to set up day for cat ${catId}:`, error.message);
            
            // Provide fallback schedule
            return {
                catId,
                schedule: {
                    morning: ['basic care'],
                    afternoon: ['rest'],
                    evening: ['dinner']
                },
                setupComplete: new Date(),
                fallback: true,
                error: error.message
            };
        }
    }
    
    // Parallel async operations using Promise.all with async/await
    async setupMultipleCatDays(catIds) {
        console.log(`\\n👥 Setting up days for ${catIds.length} cats in parallel`);
        
        try {
            // Start all setups simultaneously
            let setupPromises = catIds.map(catId => this.setupCatDay(catId));
            
            // Wait for all to complete
            let results = await Promise.all(setupPromises);
            
            let successful = results.filter(r => !r.fallback);
            let fallbacks = results.filter(r => r.fallback);
            
            console.log(`✅ Setup complete: ${successful.length} successful, ${fallbacks.length} fallbacks`);
            
            return { successful, fallbacks, total: results.length };
            
        } catch (error) {
            console.error(`❌ Parallel setup failed:`, error.message);
            throw error;
        }
    }
    
    // Error handling with async/await
    async robustCatOperation(catId, operation) {
        const maxRetries = 3;
        
        for (let attempt = 1; attempt <= maxRetries; attempt++) {
            try {
                console.log(`🔄 Attempt ${attempt}/${maxRetries} for ${operation} on cat ${catId}`);
                
                switch (operation) {
                    case 'health_check':
                        return await this.queryVetDatabase(catId);
                    case 'schedule_setup':
                        return await this.setupCatDay(catId);
                    default:
                        throw new Error(`Unknown operation: ${operation}`);
                }
                
            } catch (error) {
                console.log(`❌ Attempt ${attempt} failed: ${error.message}`);
                
                if (attempt === maxRetries) {
                    throw new Error(`Operation ${operation} failed after ${maxRetries} attempts: ${error.message}`);
                }
                
                // Exponential backoff delay
                let delayMs = Math.pow(2, attempt) * 1000;
                console.log(`⏳ Waiting ${delayMs}ms before retry...`);
                await this.delay(delayMs);
            }
        }
    }
    
    // Concurrent operations with timeout handling
    async timedCatOperation(catId, timeoutMs = 5000) {
        console.log(`⏰ Starting timed operation for cat ${catId} (timeout: ${timeoutMs}ms)`);
        
        // Create timeout promise
        let timeoutPromise = new Promise((_, reject) => {
            setTimeout(() => reject(new Error(`Operation timed out after ${timeoutMs}ms`)), timeoutMs);
        });
        
        // Race between actual operation and timeout
        try {
            let result = await Promise.race([
                this.setupCatDay(catId),
                timeoutPromise
            ]);
            
            console.log(`✅ Timed operation completed for cat ${catId}`);
            return result;
            
        } catch (error) {
            if (error.message.includes('timed out')) {
                console.log(`⏰ Operation timed out for cat ${catId}`);
                
                // Provide minimal fallback result
                return {
                    catId,
                    schedule: { morning: ['basic care'], afternoon: ['rest'], evening: ['dinner'] },
                    timedOut: true,
                    setupComplete: new Date()
                };
            } else {
                throw error;
            }
        }
    }
    
    // Advanced pattern: async generators for streaming results
    async* streamCatUpdates(catIds, intervalMs = 2000) {
        console.log(`\\n📡 Streaming updates for ${catIds.length} cats every ${intervalMs}ms`);
        
        for (let catId of catIds) {
            try {
                console.log(`Processing cat ${catId}...`);
                let result = await this.setupCatDay(catId);
                yield { success: true, catId, result };
                
                // Wait before processing next cat
                await this.delay(intervalMs, `Waiting before next cat`);
                
            } catch (error) {
                yield { success: false, catId, error: error.message };
            }
        }
        
        console.log('📡 Stream complete');
    }
}

// Demonstrate modern async/await patterns
setTimeout(async () => {
    console.log("\\n=== MODERN ASYNC/AWAIT PATTERNS ===");
    
    let modernManager = new ModernCatWhisperer();
    
    try {
        // Sequential async operations
        console.log("--- Sequential Operations ---");
        let singleResult = await modernManager.setupCatDay("whiskers-001");
        console.log(`Single setup result: ${singleResult.catId} (fallback: ${singleResult.fallback || false})`);
        
        // Parallel async operations
        console.log("\\n--- Parallel Operations ---");
        let parallelResults = await modernManager.setupMultipleCatDays(["mittens-002", "shadow-003", "patches-004"]);
        console.log(`Parallel results: ${parallelResults.successful.length} successful`);
        
        // Robust operation with retries
        console.log("\\n--- Robust Operations ---");
        let robustResult = await modernManager.robustCatOperation("fluffy-005", "health_check");
        console.log(`Robust operation completed for ${robustResult.catId}`);
        
        // Timed operation with timeout
        console.log("\\n--- Timed Operations ---");
        let timedResult = await modernManager.timedCatOperation("speedy-006", 3000);
        console.log(`Timed operation result: ${timedResult.catId} (timeout: ${timedResult.timedOut || false})`);
        
        // Streaming async generator
        console.log("\\n--- Streaming Operations ---");
        let streamingCats = ["stream-001", "stream-002", "stream-003"];
        
        for await (let update of modernManager.streamCatUpdates(streamingCats, 1000)) {
            if (update.success) {
                console.log(`📨 Stream update: ${update.catId} setup complete`);
            } else {
                console.log(`📨 Stream error: ${update.catId} failed - ${update.error}`);
            }
        }
        
    } catch (error) {
        console.error("Modern async demonstration error:", error.message);
    }
    
}, 12000);
```

## Paw-sible Problems

### Exercise 1: Build an Async Cat Activity Coordinator
Create a system that coordinates multiple cat activities with different timing requirements.

```javascript
class CatActivityCoordinator {
    // Implement coordination for:
    // - Feeding schedules (every 6 hours)
    // - Play sessions (daily, weather dependent)
    // - Grooming appointments (weekly)
    // - Vet checkups (quarterly)
    // Handle conflicts, delays, and priority changes
}
```

### Exercise 2: Implement a Cat Monitoring Dashboard
Build a real-time dashboard that monitors multiple cats simultaneously.

```javascript
class CatMonitoringDashboard {
    // Use async patterns to:
    // - Stream real-time cat location data
    // - Monitor health sensor readings
    // - Track feeding and activity patterns
    // - Alert on unusual behaviors
    // Handle sensor failures gracefully
}
```

### Exercise 3: Create a Cat Care API Client
Build a robust API client for a cat care service with proper error handling.

```javascript
class CatCareAPIClient {
    // Implement with async/await:
    // - Authentication and token refresh
    // - CRUD operations for cat profiles
    // - Batch operations for multiple cats
    // - Retry logic for transient failures
    // - Rate limiting and queue management
}
```

### Exercise 4: Design a Cat Behavior Prediction System
Create a system that processes streaming cat behavior data to predict needs.

```javascript
class CatBehaviorPredictor {
    // Use async generators and streaming:
    // - Process continuous behavior data streams
    // - Apply machine learning models asynchronously
    // - Cache predictions with TTL
    // - Handle model updates without service interruption
}
```

### Exercise 5: Build a Distributed Cat Colony Manager
Create a system for managing cats across multiple locations with network challenges.

```javascript
class DistributedColonyManager {
    // Handle distributed async operations:
    // - Sync data between multiple colony sites
    // - Coordinate activities across locations
    // - Handle network partitions gracefully
    // - Implement eventual consistency for cat records
}
```

## Cat-ch the Pattern

### Key Takeaways

1. **Async is essential for responsiveness**: Like cats monitoring multiple streams of activity
2. **Promises are better than callbacks**: Avoid callback hell with cleaner promise chains
3. **Async/await makes async code readable**: Write async code that looks synchronous
4. **Handle errors at every level**: Network calls, timeouts, and unexpected failures
5. **Use appropriate concurrency patterns**: Parallel, sequential, or racing operations as needed

### Async Patterns Comparison

**Callbacks** (Legacy):
```javascript
operation1((err, result1) => {
    if (err) return handleError(err);
    operation2(result1, (err, result2) => {
        if (err) return handleError(err);
        operation3(result2, (err, result3) => {
            // Callback hell continues...
        });
    });
});
```

**Promises** (Better):
```javascript
operation1()
    .then(result1 => operation2(result1))
    .then(result2 => operation3(result2))
    .then(result3 => handleSuccess(result3))
    .catch(err => handleError(err));
```

**Async/Await** (Best):
```javascript
try {
    let result1 = await operation1();
    let result2 = await operation2(result1);
    let result3 = await operation3(result2);
    handleSuccess(result3);
} catch (err) {
    handleError(err);
}
```

### Concurrency Patterns

**Sequential** (one after another):
```javascript
async function sequential() {
    let result1 = await operation1(); // Wait for completion
    let result2 = await operation2(); // Then start this one
    return [result1, result2];
}
```

**Parallel** (start all, wait for all):
```javascript
async function parallel() {
    let [result1, result2] = await Promise.all([
        operation1(), // Start immediately
        operation2()  // Start immediately
    ]);
    return [result1, result2];
}
```

**Racing** (first one wins):
```javascript
async function racing() {
    let result = await Promise.race([
        operation1(),
        operation2()
    ]);
    return result; // Whichever completes first
}
```

### Error Handling Strategies

```javascript
// Try-catch for async/await
async function robustOperation() {
    try {
        let result = await riskyOperation();
        return { success: true, data: result };
    } catch (error) {
        return { success: false, error: error.message };
    }
}

// Promise.allSettled for partial failures
async function partialFailureHandling(operations) {
    let results = await Promise.allSettled(operations);
    
    let successful = results.filter(r => r.status === 'fulfilled').map(r => r.value);
    let failed = results.filter(r => r.status === 'rejected').map(r => r.reason);
    
    return { successful, failed };
}

// Retry with exponential backoff
async function retryOperation(operation, maxRetries = 3) {
    for (let attempt = 1; attempt <= maxRetries; attempt++) {
        try {
            return await operation();
        } catch (error) {
            if (attempt === maxRetries) throw error;
            
            let delay = Math.pow(2, attempt) * 1000;
            await new Promise(resolve => setTimeout(resolve, delay));
        }
    }
}

// Timeout handling
async function withTimeout(operation, timeoutMs) {
    return Promise.race([
        operation(),
        new Promise((_, reject) => 
            setTimeout(() => reject(new Error('Timeout')), timeoutMs)
        )
    ]);
}
```

### Performance Considerations

```javascript
// Good: Process items in batches to avoid overwhelming system
async function processCatsBatched(cats, batchSize = 5) {
    let results = [];
    
    for (let i = 0; i < cats.length; i += batchSize) {
        let batch = cats.slice(i, i + batchSize);
        let batchResults = await Promise.all(batch.map(processCat));
        results.push(...batchResults);
        
        // Small delay between batches
        await new Promise(resolve => setTimeout(resolve, 100));
    }
    
    return results;
}

// Bad: Process all items simultaneously (could overwhelm server)
async function processCatsAll(cats) {
    return Promise.all(cats.map(processCat)); // Could create thousands of concurrent requests!
}

// Good: Use concurrency limit
async function processCatsLimited(cats, limit = 10) {
    let semaphore = new Semaphore(limit);
    
    return Promise.all(cats.map(async (cat) => {
        await semaphore.acquire();
        try {
            return await processCat(cat);
        } finally {
            semaphore.release();
        }
    }));
}
```

### Memory Management with Async

```javascript
// Avoid memory leaks with proper cleanup
class AsyncCatManager {
    constructor() {
        this.activeOperations = new Set();
        this.timers = new Set();
    }
    
    async manageCat(catId) {
        let operation = this.createCatOperation(catId);
        this.activeOperations.add(operation);
        
        try {
            return await operation;
        } finally {
            this.activeOperations.delete(operation);
        }
    }
    
    schedulePeriodicTask(task, intervalMs) {
        let timer = setInterval(task, intervalMs);
        this.timers.add(timer);
        return timer;
    }
    
    cleanup() {
        // Cancel all active operations
        this.activeOperations.forEach(op => op.cancel && op.cancel());
        this.activeOperations.clear();
        
        // Clear all timers
        this.timers.forEach(timer => clearInterval(timer));
        this.timers.clear();
    }
}
```

### Looking Ahead

Asynchronous programming enables our applications to handle multiple concurrent operations efficiently, but modern applications often need to communicate with other systems and services. In our next chapter, we'll explore APIs and communication protocols - the ways different systems talk to each other, much like the complex communication networks cats use to coordinate across territories and colonies.

We'll learn how to design and use APIs that allow different applications to share data and functionality, just as cats have evolved sophisticated methods for sharing information across their social networks.

---

*Next Chapter: [APIs and Cat Communication Protocols](chapter-18-apis-and-communication.md)*

*Previous: [Debugging the Hairball Problem](chapter-16-debugging-hairballs.md)*