# Chapter 15: Testing the Boundaries

## The Opening Tail

Luna was a methodical cat. Every morning, she performed what her human called "the inspection routine," but Luna knew it was much more than that - it was systematic boundary testing that ensured her world remained safe, predictable, and under control.

First, she'd test the food bowl situation: a gentle paw tap to confirm it was still in the right place, a sniff to verify the food quality, and a small taste to ensure it met her standards. If the food passed all tests, breakfast could proceed. If not, she'd escalate to more vigorous testing - knocking the bowl around, meowing complaints, or performing the dramatic "starving cat" routine until the humans addressed the issue.

Next came the furniture stability tests. Luna would leap onto each chair, bookshelf, and table, applying varying amounts of weight and observing any wobbling or creaking. She'd test the scratch-ability of different surfaces, the bounce factor of cushions, and the slidability of objects on smooth surfaces. Any furniture that failed her tests would be marked for human attention through strategic claw marks or knocked-over items.

The perimeter security tests were equally thorough. Luna would walk the entire boundary of her territory, testing each window latch, door frame, and potential entry point. She'd simulate various scenarios: what if a window was left slightly open? What if an unfamiliar sound came from the basement? How quickly could she reach her emergency hiding spots from different locations?

Most importantly, Luna tested the humans themselves. She'd experiment with different meowing frequencies to see which ones produced treats, test the optimal timing for lap requests, and validate the effectiveness of various cute poses. She maintained detailed mental records of what worked, what didn't, and under which conditions each strategy was most effective.

Luna understood something fundamental about maintaining a reliable system: regular testing prevents major failures. By continuously validating that everything worked as expected, she could catch problems early and ensure her comfortable cat life remained uninterrupted.

## The Technical Purr-spective

Software testing, like Luna's boundary testing, is the systematic process of validating that a system behaves correctly under various conditions. It's not just about finding bugs - it's about building confidence that your code works as intended, handles edge cases gracefully, and continues to function as it evolves.

### Types of Testing

**Unit Testing**: Testing individual components (like Luna testing each food bowl)
**Integration Testing**: Testing how components work together (food bowl + feeding schedule + human response)
**System Testing**: Testing the entire system end-to-end (complete daily routine)
**Acceptance Testing**: Validating against user requirements (does it keep the cat happy?)

### Testing Pyramid

**Unit Tests**: Fast, numerous, test small pieces
**Integration Tests**: Medium speed, moderate quantity, test connections
**E2E Tests**: Slow, few, test complete workflows

### Testing Principles

**FIRST**: Fast, Independent, Repeatable, Self-validating, Timely
**Test Early**: Catch bugs when they're cheapest to fix
**Test Often**: Automated tests run frequently
**Test Everything**: Edge cases, happy paths, error conditions

## Code in the Wild

### Unit Testing: Testing Individual Cat Behaviors

```javascript
// Cat behavior functions to test
class CatBehavior {
    static calculateHunger(lastMealTime, currentTime, activityLevel = 'normal') {
        if (!lastMealTime || !currentTime) {
            throw new Error('Both meal time and current time are required');
        }
        
        let hoursSinceLastMeal = (currentTime - lastMealTime) / (1000 * 60 * 60);
        
        if (hoursSinceLastMeal < 0) {
            throw new Error('Last meal time cannot be in the future');
        }
        
        let baseHunger = Math.min(hoursSinceLastMeal * 10, 100);
        
        // Activity level affects hunger
        let activityMultiplier = {
            'low': 0.8,
            'normal': 1.0,
            'high': 1.3
        }[activityLevel] || 1.0;
        
        return Math.min(baseHunger * activityMultiplier, 100);
    }
    
    static determineMood(hunger, energy, socialInteraction, weather = 'neutral') {
        if (hunger > 80) return 'hangry';
        if (energy < 20) return 'sleepy';
        if (socialInteraction < 30 && weather === 'rainy') return 'lonely';
        if (hunger < 30 && energy > 70 && socialInteraction > 50) return 'playful';
        if (weather === 'sunny' && energy > 40) return 'content';
        return 'neutral';
    }
    
    static shouldAcceptPetting(mood, previousPettingCount, pettingDuration) {
        if (mood === 'hangry') return false;
        if (mood === 'sleepy' && pettingDuration > 30) return false;
        if (previousPettingCount > 5) return false; // Overstimulated
        if (mood === 'playful' || mood === 'content') return true;
        return Math.random() > 0.5; // Cats are unpredictable
    }
}

// Jest-style unit tests
function createTest(description, testFn) {
    try {
        console.log(`Testing: ${description}`);
        testFn();
        console.log(`✅ PASS: ${description}`);
    } catch (error) {
        console.log(`❌ FAIL: ${description}`);
        console.log(`   Error: ${error.message}`);
    }
}

function expect(actual) {
    return {
        toBe: (expected) => {
            if (actual !== expected) {
                throw new Error(`Expected ${expected}, got ${actual}`);
            }
        },
        toBeCloseTo: (expected, precision = 2) => {
            let factor = Math.pow(10, precision);
            if (Math.round(actual * factor) !== Math.round(expected * factor)) {
                throw new Error(`Expected ${expected}, got ${actual} (precision: ${precision})`);
            }
        },
        toBeTruthy: () => {
            if (!actual) {
                throw new Error(`Expected truthy value, got ${actual}`);
            }
        },
        toBeFalsy: () => {
            if (actual) {
                throw new Error(`Expected falsy value, got ${actual}`);
            }
        },
        toThrow: () => {
            if (typeof actual !== 'function') {
                throw new Error('Expected a function that throws');
            }
            try {
                actual();
                throw new Error('Expected function to throw, but it didn\'t');
            } catch (e) {
                // This is expected
            }
        }
    };
}

console.log("=== UNIT TESTING DEMONSTRATION ===");

// Test calculateHunger function
createTest('should calculate basic hunger correctly', () => {
    let lastMeal = new Date('2023-01-01T08:00:00');
    let currentTime = new Date('2023-01-01T12:00:00'); // 4 hours later
    let hunger = CatBehavior.calculateHunger(lastMeal, currentTime);
    expect(hunger).toBe(40); // 4 hours * 10 = 40
});

createTest('should handle high activity level', () => {
    let lastMeal = new Date('2023-01-01T08:00:00');
    let currentTime = new Date('2023-01-01T12:00:00'); // 4 hours later
    let hunger = CatBehavior.calculateHunger(lastMeal, currentTime, 'high');
    expect(hunger).toBeCloseTo(52, 0); // 40 * 1.3 = 52
});

createTest('should cap hunger at 100', () => {
    let lastMeal = new Date('2023-01-01T08:00:00');
    let currentTime = new Date('2023-01-01T20:00:00'); // 12 hours later
    let hunger = CatBehavior.calculateHunger(lastMeal, currentTime);
    expect(hunger).toBe(100);
});

createTest('should throw error for missing parameters', () => {
    expect(() => CatBehavior.calculateHunger(null, new Date())).toThrow();
});

createTest('should throw error for future meal time', () => {
    let futureTime = new Date(Date.now() + 3600000); // 1 hour in future
    let currentTime = new Date();
    expect(() => CatBehavior.calculateHunger(futureTime, currentTime)).toThrow();
});

// Test determineMood function
createTest('should return hangry when very hungry', () => {
    let mood = CatBehavior.determineMood(90, 50, 30);
    expect(mood).toBe('hangry');
});

createTest('should return sleepy when low energy', () => {
    let mood = CatBehavior.determineMood(50, 10, 30);
    expect(mood).toBe('sleepy');
});

createTest('should return playful when well-fed and energetic', () => {
    let mood = CatBehavior.determineMood(20, 80, 60);
    expect(mood).toBe('playful');
});

createTest('should consider weather in mood calculation', () => {
    let mood = CatBehavior.determineMood(50, 30, 20, 'rainy');
    expect(mood).toBe('lonely');
});

// Test shouldAcceptPetting function
createTest('should reject petting when hangry', () => {
    let accepts = CatBehavior.shouldAcceptPetting('hangry', 2, 15);
    expect(accepts).toBeFalsy();
});

createTest('should reject long petting when sleepy', () => {
    let accepts = CatBehavior.shouldAcceptPetting('sleepy', 1, 45);
    expect(accepts).toBeFalsy();
});

createTest('should reject petting when overstimulated', () => {
    let accepts = CatBehavior.shouldAcceptPetting('content', 6, 10);
    expect(accepts).toBeFalsy();
});

createTest('should accept petting when content', () => {
    let accepts = CatBehavior.shouldAcceptPetting('content', 2, 15);
    expect(accepts).toBeTruthy();
});
```

### Integration Testing: Testing Cat System Components Together

```javascript
// Cat system with multiple interacting components
class CatFeedingSystem {
    constructor(foodStorage, scheduleManager, healthChecker) {
        this.foodStorage = foodStorage;
        this.scheduleManager = scheduleManager;
        this.healthChecker = healthChecker;
        this.feedingHistory = [];
    }
    
    async attemptFeeding(catId, requestTime = new Date()) {
        console.log(`🍽️ Attempting to feed cat ${catId} at ${requestTime.toLocaleTimeString()}`);
        
        // Check schedule
        let scheduleCheck = await this.scheduleManager.isValidFeedingTime(catId, requestTime);
        if (!scheduleCheck.valid) {
            console.log(`❌ Schedule check failed: ${scheduleCheck.reason}`);
            return { success: false, reason: scheduleCheck.reason };
        }
        
        // Check health restrictions
        let healthCheck = await this.healthChecker.checkFeedingAllowed(catId);
        if (!healthCheck.allowed) {
            console.log(`❌ Health check failed: ${healthCheck.reason}`);
            return { success: false, reason: healthCheck.reason };
        }
        
        // Check food availability
        let foodType = healthCheck.recommendedFood || 'regular';
        let foodCheck = await this.foodStorage.checkAvailability(foodType, scheduleCheck.recommendedPortion);
        if (!foodCheck.available) {
            console.log(`❌ Food availability check failed: ${foodCheck.reason}`);
            return { success: false, reason: foodCheck.reason };
        }
        
        // All checks passed, proceed with feeding
        let feedingResult = await this.performFeeding(catId, foodType, scheduleCheck.recommendedPortion);
        
        if (feedingResult.success) {
            // Record successful feeding
            this.feedingHistory.push({
                catId: catId,
                time: requestTime,
                foodType: foodType,
                portion: scheduleCheck.recommendedPortion,
                status: 'completed'
            });
            
            // Update components
            await this.scheduleManager.recordFeeding(catId, requestTime);
            await this.foodStorage.consumeFood(foodType, scheduleCheck.recommendedPortion);
            
            console.log(`✅ Successfully fed cat ${catId}`);
        }
        
        return feedingResult;
    }
    
    async performFeeding(catId, foodType, portion) {
        // Simulate feeding process
        console.log(`🥘 Dispensing ${portion}g of ${foodType} food for cat ${catId}`);
        
        // Simulate potential feeding issues
        if (Math.random() < 0.1) { // 10% chance of dispenser jam
            return { success: false, reason: 'Food dispenser malfunction' };
        }
        
        return { 
            success: true, 
            foodType: foodType, 
            portion: portion,
            timestamp: new Date()
        };
    }
}

// Mock components for integration testing
class MockFoodStorage {
    constructor() {
        this.inventory = {
            'regular': 1000,
            'prescription': 200,
            'senior': 500,
            'kitten': 300
        };
    }
    
    async checkAvailability(foodType, amount) {
        console.log(`🏪 Checking availability of ${amount}g ${foodType} food`);
        
        if (!this.inventory[foodType]) {
            return { available: false, reason: `Unknown food type: ${foodType}` };
        }
        
        if (this.inventory[foodType] < amount) {
            return { 
                available: false, 
                reason: `Insufficient ${foodType} food (need: ${amount}g, have: ${this.inventory[foodType]}g)` 
            };
        }
        
        return { available: true };
    }
    
    async consumeFood(foodType, amount) {
        this.inventory[foodType] -= amount;
        console.log(`📦 Consumed ${amount}g of ${foodType}, ${this.inventory[foodType]}g remaining`);
    }
}

class MockScheduleManager {
    constructor() {
        this.lastFeedings = new Map();
        this.feedingRules = {
            minHoursBetweenMeals: 4,
            maxMealsPerDay: 3,
            portions: {
                kitten: 100,
                adult: 80,
                senior: 60
            }
        };
    }
    
    async isValidFeedingTime(catId, requestTime) {
        console.log(`📅 Checking feeding schedule for cat ${catId}`);
        
        let lastFeeding = this.lastFeedings.get(catId);
        if (lastFeeding) {
            let hoursSinceLastMeal = (requestTime - lastFeeding) / (1000 * 60 * 60);
            if (hoursSinceLastMeal < this.feedingRules.minHoursBetweenMeals) {
                return {
                    valid: false,
                    reason: `Too soon since last meal (${hoursSinceLastMeal.toFixed(1)} hours ago)`
                };
            }
        }
        
        // Simulate age lookup
        let catAge = Math.random() > 0.7 ? 'senior' : 'adult';
        let recommendedPortion = this.feedingRules.portions[catAge];
        
        return {
            valid: true,
            recommendedPortion: recommendedPortion,
            nextAllowedFeeding: new Date(requestTime.getTime() + (this.feedingRules.minHoursBetweenMeals * 60 * 60 * 1000))
        };
    }
    
    async recordFeeding(catId, feedingTime) {
        this.lastFeedings.set(catId, feedingTime);
        console.log(`📝 Recorded feeding for cat ${catId} at ${feedingTime.toLocaleTimeString()}`);
    }
}

class MockHealthChecker {
    constructor() {
        this.healthProfiles = new Map([
            ['whiskers-001', { restrictions: [], recommendedFood: 'regular' }],
            ['fluffy-002', { restrictions: ['weight-management'], recommendedFood: 'prescription' }],
            ['senior-003', { restrictions: ['kidney-disease'], recommendedFood: 'senior' }]
        ]);
    }
    
    async checkFeedingAllowed(catId) {
        console.log(`🏥 Performing health check for cat ${catId}`);
        
        let profile = this.healthProfiles.get(catId);
        if (!profile) {
            // Unknown cat, assume healthy
            return { allowed: true, recommendedFood: 'regular' };
        }
        
        // Simulate health-based feeding restrictions
        if (profile.restrictions.includes('fasting-required')) {
            return { 
                allowed: false, 
                reason: 'Cat is fasting for medical procedure' 
            };
        }
        
        return { 
            allowed: true, 
            recommendedFood: profile.recommendedFood,
            restrictions: profile.restrictions
        };
    }
}

// Integration tests
async function runIntegrationTests() {
    console.log("\n=== INTEGRATION TESTING DEMONSTRATION ===");
    
    let foodStorage = new MockFoodStorage();
    let scheduleManager = new MockScheduleManager();
    let healthChecker = new MockHealthChecker();
    let feedingSystem = new CatFeedingSystem(foodStorage, scheduleManager, healthChecker);
    
    // Test successful feeding
    createTest('should successfully feed healthy cat', async () => {
        let result = await feedingSystem.attemptFeeding('whiskers-001');
        expect(result.success).toBeTruthy();
    });
    
    // Test feeding too soon after last meal
    createTest('should reject feeding too soon after last meal', async () => {
        // First feeding
        await feedingSystem.attemptFeeding('whiskers-001');
        // Immediate second attempt
        let result = await feedingSystem.attemptFeeding('whiskers-001');
        expect(result.success).toBeFalsy();
    });
    
    // Test special dietary requirements
    createTest('should use prescription food for cat with restrictions', async () => {
        let result = await feedingSystem.attemptFeeding('fluffy-002');
        expect(result.success).toBeTruthy();
        // Check that prescription food was used (would need to verify in real system)
    });
    
    // Test food shortage scenario
    createTest('should handle food shortage gracefully', async () => {
        // Deplete regular food
        foodStorage.inventory['regular'] = 5; // Very low
        let result = await feedingSystem.attemptFeeding('whiskers-001');
        // Depending on portion size, this might fail
        console.log(`Food shortage test result: ${result.success ? 'passed' : 'failed as expected'}`);
    });
    
    // Test system integration over time
    createTest('should handle multiple feedings over time', async () => {
        let results = [];
        let testCats = ['cat-1', 'cat-2', 'cat-3'];
        
        for (let cat of testCats) {
            let result = await feedingSystem.attemptFeeding(cat);
            results.push(result);
        }
        
        let successfulFeedings = results.filter(r => r.success).length;
        console.log(`Successfully fed ${successfulFeedings} out of ${testCats.length} cats`);
        expect(successfulFeedings).toBe(testCats.length);
    });
    
    console.log(`\nFeeding history: ${feedingSystem.feedingHistory.length} recorded feedings`);
}

// Run integration tests (simulate async)
setTimeout(runIntegrationTests, 100);
```

### End-to-End Testing: Complete Cat Day Simulation

```javascript
// Complete cat day simulation for E2E testing
class CatDaySimulator {
    constructor() {
        this.cats = new Map();
        this.schedule = new Map();
        this.activities = [];
        this.environment = {
            time: new Date(),
            weather: 'sunny',
            temperature: 22,
            humanPresence: true
        };
    }
    
    addCat(cat) {
        this.cats.set(cat.id, {
            ...cat,
            energy: 70,
            hunger: 30,
            happiness: 60,
            location: 'living_room'
        });
        
        // Set up daily schedule
        this.schedule.set(cat.id, {
            wakeTime: 7,
            breakfastTime: 7.5,
            morningPlayTime: 9,
            noonNapTime: 12,
            afternoonPlayTime: 15,
            dinnerTime: 18,
            eveningNapTime: 20,
            bedTime: 22
        });
        
        console.log(`🐱 Added ${cat.name} to simulation`);
    }
    
    async simulateFullDay(catId) {
        console.log(`\n🌅 Starting full day simulation for cat ${catId}`);
        
        let cat = this.cats.get(catId);
        let schedule = this.schedule.get(catId);
        
        if (!cat || !schedule) {
            throw new Error(`Cat ${catId} not found in simulation`);
        }
        
        let activities = [];
        let startTime = new Date();
        startTime.setHours(7, 0, 0, 0); // Start at 7 AM
        
        // Wake up
        activities.push(await this.simulateActivity(catId, 'wake_up', startTime));
        
        // Breakfast
        let breakfastTime = new Date(startTime);
        breakfastTime.setMinutes(30);
        activities.push(await this.simulateActivity(catId, 'eat', breakfastTime));
        
        // Morning play
        let playTime = new Date(startTime);
        playTime.setHours(9);
        activities.push(await this.simulateActivity(catId, 'play', playTime));
        
        // Noon nap
        let napTime = new Date(startTime);
        napTime.setHours(12);
        activities.push(await this.simulateActivity(catId, 'nap', napTime));
        
        // Afternoon activities
        let afternoonTime = new Date(startTime);
        afternoonTime.setHours(15);
        activities.push(await this.simulateActivity(catId, 'explore', afternoonTime));
        
        // Dinner
        let dinnerTime = new Date(startTime);
        dinnerTime.setHours(18);
        activities.push(await this.simulateActivity(catId, 'eat', dinnerTime));
        
        // Evening relaxation
        let eveningTime = new Date(startTime);
        eveningTime.setHours(20);
        activities.push(await this.simulateActivity(catId, 'groom', eveningTime));
        
        // Bedtime
        let bedTime = new Date(startTime);
        bedTime.setHours(22);
        activities.push(await this.simulateActivity(catId, 'sleep', bedTime));
        
        return this.analyzeDayResults(catId, activities);
    }
    
    async simulateActivity(catId, activityType, scheduledTime) {
        let cat = this.cats.get(catId);
        let startTime = new Date();
        
        console.log(`⏰ ${scheduledTime.toLocaleTimeString()}: ${cat.name} ${activityType}`);
        
        let activity = {
            catId: catId,
            type: activityType,
            scheduledTime: scheduledTime,
            actualStartTime: startTime,
            status: 'in_progress'
        };
        
        try {
            // Simulate activity execution
            let result = await this.executeActivity(cat, activityType);
            
            activity.status = 'completed';
            activity.result = result;
            activity.endTime = new Date();
            activity.duration = activity.endTime - activity.actualStartTime;
            
            // Update cat state based on activity
            this.updateCatState(cat, activityType, result);
            
            console.log(`  ✅ ${activityType} completed (${activity.duration}ms)`);
            
        } catch (error) {
            activity.status = 'failed';
            activity.error = error.message;
            activity.endTime = new Date();
            
            console.log(`  ❌ ${activityType} failed: ${error.message}`);
        }
        
        this.activities.push(activity);
        return activity;
    }
    
    async executeActivity(cat, activityType) {
        // Simulate different activity types with potential failures
        switch (activityType) {
            case 'wake_up':
                return this.simulateWakeUp(cat);
                
            case 'eat':
                return this.simulateEating(cat);
                
            case 'play':
                return this.simulatePlay(cat);
                
            case 'nap':
                return this.simulateNap(cat);
                
            case 'explore':
                return this.simulateExplore(cat);
                
            case 'groom':
                return this.simulateGroom(cat);
                
            case 'sleep':
                return this.simulateSleep(cat);
                
            default:
                throw new Error(`Unknown activity type: ${activityType}`);
        }
    }
    
    simulateWakeUp(cat) {
        if (cat.energy < 20) {
            throw new Error('Too tired to wake up properly');
        }
        
        cat.energy = Math.min(cat.energy + 10, 100);
        cat.hunger = Math.min(cat.hunger + 5, 100);
        
        return { energyGain: 10, mood: 'alert' };
    }
    
    simulateEating(cat) {
        if (cat.hunger < 20) {
            throw new Error('Not hungry enough to eat');
        }
        
        // Simulate potential feeding problems
        if (Math.random() < 0.05) { // 5% chance
            throw new Error('Food bowl is empty');
        }
        
        cat.hunger = Math.max(cat.hunger - 40, 0);
        cat.happiness = Math.min(cat.happiness + 15, 100);
        
        return { hungerReduction: 40, happinessGain: 15, foodType: 'regular' };
    }
    
    simulatePlay(cat) {
        if (cat.energy < 30) {
            throw new Error('Too tired to play actively');
        }
        
        cat.energy = Math.max(cat.energy - 20, 0);
        cat.happiness = Math.min(cat.happiness + 25, 100);
        cat.hunger = Math.min(cat.hunger + 10, 100);
        
        return { energyUsed: 20, happinessGain: 25, playType: 'interactive' };
    }
    
    simulateNap(cat) {
        cat.energy = Math.min(cat.energy + 30, 100);
        cat.happiness = Math.min(cat.happiness + 10, 100);
        
        return { energyGain: 30, duration: 'short_nap' };
    }
    
    simulateExplore(cat) {
        if (cat.energy < 15) {
            throw new Error('Too tired to explore');
        }
        
        cat.energy = Math.max(cat.energy - 10, 0);
        cat.happiness = Math.min(cat.happiness + 15, 100);
        
        // Random discoveries
        let discoveries = ['sunny_spot', 'interesting_smell', 'new_hiding_place', 'dust_bunny'];
        let discovery = discoveries[Math.floor(Math.random() * discoveries.length)];
        
        return { energyUsed: 10, discovery: discovery };
    }
    
    simulateGroom(cat) {
        cat.happiness = Math.min(cat.happiness + 10, 100);
        
        return { groomingComplete: true, relaxation: 10 };
    }
    
    simulateSleep(cat) {
        if (cat.energy > 80) {
            throw new Error('Too energetic to sleep now');
        }
        
        cat.energy = Math.min(cat.energy + 40, 100);
        
        return { energyGain: 40, sleepQuality: 'good' };
    }
    
    updateCatState(cat, activityType, result) {
        // Update cat location based on activity
        let locationMapping = {
            'eat': 'kitchen',
            'play': 'living_room',
            'nap': 'sunny_spot',
            'sleep': 'bedroom',
            'groom': 'bathroom',
            'explore': 'various'
        };
        
        if (locationMapping[activityType]) {
            cat.location = locationMapping[activityType];
        }
        
        // Update last activity time
        cat.lastActivity = new Date();
        cat.lastActivityType = activityType;
    }
    
    analyzeDayResults(catId, activities) {
        let cat = this.cats.get(catId);
        
        let completedActivities = activities.filter(a => a.status === 'completed');
        let failedActivities = activities.filter(a => a.status === 'failed');
        
        let totalDuration = activities.reduce((sum, a) => sum + (a.duration || 0), 0);
        let averageDuration = totalDuration / activities.length;
        
        let finalState = {
            energy: cat.energy,
            hunger: cat.hunger,
            happiness: cat.happiness,
            location: cat.location
        };
        
        let dayQuality = this.calculateDayQuality(finalState, completedActivities, failedActivities);
        
        return {
            catId: catId,
            catName: cat.name,
            totalActivities: activities.length,
            completedActivities: completedActivities.length,
            failedActivities: failedActivities.length,
            successRate: (completedActivities.length / activities.length) * 100,
            totalDuration: totalDuration,
            averageDuration: averageDuration,
            finalState: finalState,
            dayQuality: dayQuality,
            activities: activities
        };
    }
    
    calculateDayQuality(finalState, completed, failed) {
        let qualityScore = 0;
        
        // Final state contributions
        qualityScore += finalState.energy * 0.3;
        qualityScore += (100 - finalState.hunger) * 0.2; // Lower hunger is better
        qualityScore += finalState.happiness * 0.4;
        
        // Activity completion rate
        let completionRate = completed.length / (completed.length + failed.length);
        qualityScore += completionRate * 10;
        
        if (qualityScore >= 80) return 'excellent';
        if (qualityScore >= 60) return 'good';
        if (qualityScore >= 40) return 'fair';
        return 'poor';
    }
}

// E2E Test Runner
async function runE2ETests() {
    console.log("\n=== END-TO-END TESTING DEMONSTRATION ===");
    
    let simulator = new CatDaySimulator();
    
    // Add test cats
    simulator.addCat({
        id: 'whiskers-e2e',
        name: 'Whiskers',
        breed: 'Tabby',
        age: 4,
        personality: 'active'
    });
    
    simulator.addCat({
        id: 'fluffy-e2e',
        name: 'Princess Fluffy',
        breed: 'Persian',
        age: 6,
        personality: 'lazy'
    });
    
    // Test complete day simulation
    createTest('should complete full day for active cat', async () => {
        let results = await simulator.simulateFullDay('whiskers-e2e');
        
        console.log(`\n📊 Day Results for ${results.catName}:`);
        console.log(`   Success Rate: ${results.successRate.toFixed(1)}%`);
        console.log(`   Day Quality: ${results.dayQuality}`);
        console.log(`   Final Energy: ${results.finalState.energy}`);
        console.log(`   Final Happiness: ${results.finalState.happiness}`);
        console.log(`   Final Hunger: ${results.finalState.hunger}`);
        
        // Assertions for successful day
        expect(results.successRate).toBe(100); // Assuming no random failures
        expect(results.dayQuality).toBe('good'); // At least good quality day
        expect(results.finalState.happiness).toBe(60); // Should be reasonably happy
    });
    
    createTest('should handle different cat personalities', async () => {
        let results = await simulator.simulateFullDay('fluffy-e2e');
        
        console.log(`\n📊 Day Results for ${results.catName}:`);
        console.log(`   Success Rate: ${results.successRate.toFixed(1)}%`);
        console.log(`   Day Quality: ${results.dayQuality}`);
        
        // Different cats may have different success patterns
        expect(results.totalActivities).toBe(8); // Should have attempted all activities
    });
    
    console.log(`\n📈 Total activities logged: ${simulator.activities.length}`);
}

// Run E2E tests
setTimeout(runE2ETests, 500);
```

### Test Automation and Reporting

```javascript
// Test suite runner with reporting
class CatTestSuite {
    constructor() {
        this.tests = [];
        this.results = [];
        this.startTime = null;
        this.endTime = null;
    }
    
    describe(suiteName, testFunction) {
        console.log(`\n📋 Test Suite: ${suiteName}`);
        console.log("=" + "=".repeat(suiteName.length + 12));
        
        this.startTime = new Date();
        let suite = {
            name: suiteName,
            tests: [],
            startTime: this.startTime
        };
        
        // Provide 'it' function for individual tests
        let it = (testName, testFn) => {
            suite.tests.push({ name: testName, fn: testFn });
        };
        
        // Run the test suite
        testFunction(it);
        
        // Execute all tests in the suite
        this.runSuite(suite);
    }
    
    async runSuite(suite) {
        let passed = 0;
        let failed = 0;
        
        for (let test of suite.tests) {
            try {
                console.log(`  🧪 ${test.name}`);
                
                if (test.fn.constructor.name === 'AsyncFunction') {
                    await test.fn();
                } else {
                    test.fn();
                }
                
                console.log(`    ✅ PASS`);
                passed++;
                
            } catch (error) {
                console.log(`    ❌ FAIL: ${error.message}`);
                failed++;
            }
        }
        
        suite.endTime = new Date();
        suite.duration = suite.endTime - suite.startTime;
        suite.passed = passed;
        suite.failed = failed;
        suite.total = passed + failed;
        
        this.results.push(suite);
        
        console.log(`\n📊 Suite Results: ${passed} passed, ${failed} failed (${suite.duration}ms)`);
        
        if (failed > 0) {
            console.log(`⚠️  ${suite.name} has failing tests!`);
        } else {
            console.log(`✅ All tests in ${suite.name} passed!`);
        }
    }
    
    generateReport() {
        let totalTests = this.results.reduce((sum, suite) => sum + suite.total, 0);
        let totalPassed = this.results.reduce((sum, suite) => sum + suite.passed, 0);
        let totalFailed = this.results.reduce((sum, suite) => sum + suite.failed, 0);
        let totalDuration = this.results.reduce((sum, suite) => sum + suite.duration, 0);
        
        console.log(`\n${"=".repeat(50)}`);
        console.log(`📊 COMPREHENSIVE TEST REPORT`);
        console.log(`${"=".repeat(50)}`);
        console.log(`Total Test Suites: ${this.results.length}`);
        console.log(`Total Tests: ${totalTests}`);
        console.log(`Passed: ${totalPassed} (${((totalPassed/totalTests)*100).toFixed(1)}%)`);
        console.log(`Failed: ${totalFailed} (${((totalFailed/totalTests)*100).toFixed(1)}%)`);
        console.log(`Total Duration: ${totalDuration}ms`);
        console.log(`Average Suite Duration: ${(totalDuration/this.results.length).toFixed(1)}ms`);
        
        console.log(`\n📋 Suite Breakdown:`);
        this.results.forEach(suite => {
            let status = suite.failed === 0 ? '✅' : '❌';
            console.log(`  ${status} ${suite.name}: ${suite.passed}/${suite.total} passed (${suite.duration}ms)`);
        });
        
        if (totalFailed === 0) {
            console.log(`\n🎉 ALL TESTS PASSED! The cat system is working purrfectly!`);
        } else {
            console.log(`\n⚠️  ${totalFailed} tests failed. The cats are not completely satisfied.`);
        }
        
        return {
            totalSuites: this.results.length,
            totalTests: totalTests,
            passed: totalPassed,
            failed: totalFailed,
            successRate: (totalPassed / totalTests) * 100,
            duration: totalDuration,
            suites: this.results
        };
    }
}

// Comprehensive test execution
setTimeout(() => {
    console.log("\n=== COMPREHENSIVE TEST SUITE EXECUTION ===");
    
    let testSuite = new CatTestSuite();
    
    // Cat Behavior Tests
    testSuite.describe('Cat Behavior Functions', (it) => {
        it('should calculate hunger correctly', () => {
            let hunger = CatBehavior.calculateHunger(
                new Date('2023-01-01T08:00:00'),
                new Date('2023-01-01T12:00:00')
            );
            expect(hunger).toBe(40);
        });
        
        it('should determine mood based on multiple factors', () => {
            let mood = CatBehavior.determineMood(90, 50, 30);
            expect(mood).toBe('hangry');
        });
        
        it('should handle petting decisions', () => {
            let accepts = CatBehavior.shouldAcceptPetting('content', 2, 15);
            expect(accepts).toBeTruthy();
        });
    });
    
    // System Integration Tests
    testSuite.describe('Cat System Integration', (it) => {
        it('should integrate feeding components', async () => {
            let foodStorage = new MockFoodStorage();
            let scheduleManager = new MockScheduleManager();
            let healthChecker = new MockHealthChecker();
            let feedingSystem = new CatFeedingSystem(foodStorage, scheduleManager, healthChecker);
            
            let result = await feedingSystem.attemptFeeding('test-cat-001');
            expect(result.success).toBeTruthy();
        });
    });
    
    // Edge Case Tests
    testSuite.describe('Edge Cases and Error Handling', (it) => {
        it('should handle invalid input gracefully', () => {
            expect(() => CatBehavior.calculateHunger(null, new Date())).toThrow();
        });
        
        it('should handle future meal times', () => {
            let futureTime = new Date(Date.now() + 3600000);
            let currentTime = new Date();
            expect(() => CatBehavior.calculateHunger(futureTime, currentTime)).toThrow();
        });
    });
    
    // Generate final report
    setTimeout(() => {
        let report = testSuite.generateReport();
        
        // Save report (in real system, would write to file)
        console.log(`\n💾 Test report generated with ${report.successRate.toFixed(1)}% success rate`);
    }, 100);
    
}, 1000);
```

## Paw-sible Problems

### Exercise 1: Test-Driven Cat Development
Practice test-driven development by writing tests first, then implementing cat behavior functions.

```javascript
class CatBehaviorTDD {
    // Write tests first for these functions, then implement:
    // - calculatePlayfulness(age, energy, weather, toys)
    // - determineNapLocation(temperature, humanPresence, noiseLevel)
    // - shouldChaseLaser(energy, mood, previousChaseCount)
    // - calculateTreatDeservingness(behavior, lastTreat, cuteness)
}
```

### Exercise 2: Mock-heavy Cat Service Testing
Create comprehensive mocks for testing a complex cat service system.

```javascript
class CatServiceTestSuite {
    // Create mocks for:
    // - WeatherService
    // - VeterinaryService  
    // - HumanScheduleService
    // - ToyInventoryService
    // Test various interaction scenarios and failure modes
}
```

### Exercise 3: Property-Based Testing for Cat Logic
Implement property-based tests that generate random inputs to verify cat behavior properties.

```javascript
class CatPropertyTests {
    // Test properties like:
    // - Hunger always increases over time
    // - Energy is conserved (input equals output + waste)
    // - Mood transitions follow logical patterns
    // - Cat locations are always valid
}
```

### Exercise 4: Performance Testing Cat Operations
Create performance tests for cat-related operations under various loads.

```javascript
class CatPerformanceTests {
    // Test performance of:
    // - Feeding 1000 cats simultaneously
    // - Processing cat behavior updates
    // - Territory conflict resolution
    // - Data querying under load
}
```

### Exercise 5: End-to-End Cat Colony Testing
Build comprehensive E2E tests for an entire cat colony management system.

```javascript
class CatColonyE2ETests {
    // Test complete workflows:
    // - New cat adoption process
    // - Daily colony routine execution
    // - Emergency response procedures
    // - Multi-day behavioral tracking
}
```

## Cat-ch the Pattern

### Key Takeaways

1. **Test early and often**: Catch bugs when they're cheapest to fix, like Luna's daily inspections
2. **Test at multiple levels**: Unit tests for components, integration tests for interactions, E2E tests for workflows
3. **Test edge cases**: Consider what happens when things go wrong
4. **Automate testing**: Manual testing doesn't scale, automated tests run consistently
5. **Make tests readable**: Tests are documentation of expected behavior

### Testing Pyramid Strategy

**Unit Tests** (70% of tests):
- Fast execution (milliseconds)
- Test single functions/methods
- High code coverage
- Easy to debug failures

**Integration Tests** (20% of tests):
- Medium execution time (seconds)
- Test component interactions
- Verify interfaces work together
- Catch integration bugs

**E2E Tests** (10% of tests):
- Slow execution (minutes)
- Test complete user journeys
- Verify system works end-to-end
- Catch workflow issues

### Testing Best Practices

```javascript
// Good test structure: Arrange, Act, Assert
function testCatFeeding() {
    // Arrange - set up test data
    let cat = { name: 'Whiskers', hunger: 80, energy: 60 };
    let food = { type: 'premium', amount: 100 };
    
    // Act - perform the operation
    let result = feedCat(cat, food);
    
    // Assert - verify the outcome
    expect(result.success).toBe(true);
    expect(cat.hunger).toBeLessThan(50);
}

// Test naming conventions
// ✅ Good: descriptive test names
test('should reduce hunger when cat eats appropriate food')
test('should reject feeding when cat is not hungry')
test('should handle empty food bowl gracefully')

// ❌ Bad: unclear test names
test('test1')
test('feeding test')
test('works correctly')

// Test independence - each test should be isolated
beforeEach(() => {
    // Reset state before each test
    resetCatState();
    clearFoodBowls();
});

// Use descriptive assertions
// ✅ Good
expect(cat.mood).toBe('content');
expect(feedingResult.portionSize).toBeGreaterThan(0);

// ❌ Less clear
expect(cat.mood).toBeTruthy();
expect(feedingResult.portionSize).not.toBeFalsy();
```

### Common Testing Patterns

```javascript
// Test doubles (mocks, stubs, spies)
class MockCatFeeder {
    constructor() {
        this.feedingCalls = [];
    }
    
    feed(cat, food) {
        this.feedingCalls.push({ cat, food, timestamp: new Date() });
        return { success: true };
    }
    
    getCallCount() {
        return this.feedingCalls.length;
    }
}

// Parameterized tests
const testCases = [
    { hunger: 90, expected: 'very_hungry' },
    { hunger: 50, expected: 'moderately_hungry' },
    { hunger: 10, expected: 'satisfied' }
];

testCases.forEach(({ hunger, expected }) => {
    test(`should return ${expected} when hunger is ${hunger}`, () => {
        expect(determineHungerLevel(hunger)).toBe(expected);
    });
});

// Async testing
test('should complete feeding within timeout', async () => {
    let promise = feedCatAsync('whiskers-001');
    await expect(promise).resolves.toEqual({ success: true });
});

// Error testing
test('should throw error for invalid cat ID', () => {
    expect(() => feedCat(null, food)).toThrow('Invalid cat ID');
});
```

### Test Coverage and Quality

```javascript
// Code coverage doesn't guarantee quality
function calculateCatHappiness(treats, play, naps) {
    if (treats > 0) return 100;  // Line is covered by test
    if (play > 0) return 80;     // Line is never reached in tests!
    if (naps > 0) return 60;     // Line is never reached in tests!
    return 0;
}

// Better: test all branches
test('happiness calculation covers all conditions', () => {
    expect(calculateCatHappiness(1, 0, 0)).toBe(100); // treats path
    expect(calculateCatHappiness(0, 1, 0)).toBe(80);  // play path  
    expect(calculateCatHappiness(0, 0, 1)).toBe(60);  // naps path
    expect(calculateCatHappiness(0, 0, 0)).toBe(0);   // default path
});

// Mutation testing - verify tests catch bugs
// If you change `>` to `>=` in production code,
// do your tests fail? If not, tests might be weak.
```

### Continuous Integration and Testing

```javascript
// CI/CD pipeline testing stages
const testPipeline = {
    stages: [
        {
            name: 'Unit Tests',
            command: 'npm test:unit',
            fastFail: true,
            parallel: true
        },
        {
            name: 'Integration Tests', 
            command: 'npm test:integration',
            dependencies: ['Unit Tests'],
            environment: 'test'
        },
        {
            name: 'E2E Tests',
            command: 'npm test:e2e',
            dependencies: ['Integration Tests'],
            environment: 'staging',
            artifacts: ['test-reports/', 'screenshots/']
        },
        {
            name: 'Performance Tests',
            command: 'npm test:performance',
            optional: true,
            schedule: 'nightly'
        }
    ]
};
```

### Looking Ahead

Testing ensures our code works correctly, but what happens when things go wrong despite our best efforts? In our next chapter, we'll explore debugging - the detective work of finding and fixing problems in code, much like how cats methodically investigate and resolve issues in their environment.

We'll learn systematic approaches to identifying bugs, understanding their causes, and implementing effective solutions, just as cats use their natural problem-solving abilities to overcome obstacles and challenges.

---

*Next Chapter: [Debugging the Hairball Problem](chapter-16-debugging-hairballs.md)*

*Previous: [Architecture as Territory Management](chapter-14-architecture-as-territory.md)*