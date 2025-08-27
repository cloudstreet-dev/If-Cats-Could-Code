# Chapter 14: Architecture as Territory Management

## The Opening Tail

The old Victorian mansion at the end of Maple Street had been home to cats for over a century. What fascinated Dr. Chen, the animal behaviorist studying the property, was how the cats had naturally evolved a sophisticated territorial architecture that made perfect sense for their needs.

The basement belonged to Hunter, the senior mouser, who had established it as the "Security Layer" - monitoring all entry points and dealing with any pest infiltrations. The main floor was Princess Fluffy's domain, the "Presentation Layer," where she greeted visitors, managed human interactions, and maintained the house's public image.

The second floor served as the "Business Logic Layer," overseen by Professor Whiskers. Here, important decisions were made about food distribution, territory disputes, and daily scheduling. The third floor was Sage's "Data Storage Layer," where all important information was kept: the location of hidden treats, maps of the best sunny spots, and detailed records of each human's petting preferences.

What impressed Dr. Chen most was how well-defined the boundaries were, yet how seamlessly the cats could communicate between layers. Hunter would report security concerns to Professor Whiskers through a established protocol (specific meow patterns). Professor Whiskers would consult with Sage about historical data before making decisions. Princess Fluffy would relay human requests upward and communicate decisions back down.

The system was scalable too. When new kittens arrived, they were easily integrated into the appropriate layer based on their skills and interests. The architecture supported growth, handled failures gracefully (if one cat was napping, others could temporarily cover their responsibilities), and maintained clear separation of concerns while enabling efficient communication.

Most remarkably, each layer could evolve independently. Hunter could change his patrol routes without affecting how Princess Fluffy greeted visitors. Sage could reorganize her information storage without disrupting Professor Whiskers' decision-making process. The architecture was both structured and flexible - a perfect balance for a thriving cat community.

## The Technical Purr-spective

Software architecture, like the cats' territorial organization, is about creating structure and boundaries that enable systems to grow, adapt, and function efficiently. It's the high-level design that determines how different parts of an application interact, where responsibilities lie, and how the system can scale and evolve over time.

### Key Architectural Principles

**Separation of Concerns**: Each layer has distinct responsibilities (like Hunter focusing on security)
**Loose Coupling**: Layers interact through well-defined interfaces, not tight dependencies
**High Cohesion**: Related functionality is grouped together
**Scalability**: Architecture supports growth without fundamental changes
**Maintainability**: Changes can be made without affecting the entire system
**Fault Tolerance**: System continues working even when parts fail

### Common Architectural Patterns

**Layered Architecture**: Organized in horizontal layers (presentation, business, data)
**Model-View-Controller (MVC)**: Separates data, presentation, and control logic
**Microservices**: Independent services that communicate over networks
**Event-Driven**: Components communicate through events and messages
**Client-Server**: Separation between requesters and providers of services

## Code in the Wild

### Layered Architecture: The Cat Mansion System

```javascript
// Data Access Layer - Sage's information storage
class CatDataLayer {
    constructor() {
        this.cats = new Map();
        this.territories = new Map();
        this.activities = [];
        this.preferences = new Map();
        
        console.log("📚 Data Layer initialized - Sage is ready to store information");
    }
    
    // Cat management
    saveCat(cat) {
        this.cats.set(cat.id, { ...cat, lastUpdated: new Date() });
        console.log(`📝 Sage stored information about ${cat.name}`);
        return true;
    }
    
    getCat(id) {
        console.log(`📖 Sage retrieving information about cat ${id}`);
        return this.cats.get(id);
    }
    
    getAllCats() {
        console.log("📋 Sage providing complete cat directory");
        return Array.from(this.cats.values());
    }
    
    // Territory management
    saveTerritory(territory) {
        this.territories.set(territory.id, territory);
        console.log(`🗺️ Sage updated territory map for ${territory.name}`);
        return true;
    }
    
    getTerritory(id) {
        return this.territories.get(id);
    }
    
    // Activity logging
    logActivity(activity) {
        this.activities.push({
            ...activity,
            timestamp: new Date(),
            id: this.activities.length + 1
        });
        console.log(`📅 Sage logged activity: ${activity.type}`);
    }
    
    getActivitiesByType(type) {
        return this.activities.filter(activity => activity.type === type);
    }
    
    // Preferences
    savePreference(catId, preference, value) {
        if (!this.preferences.has(catId)) {
            this.preferences.set(catId, {});
        }
        this.preferences.get(catId)[preference] = value;
        console.log(`💾 Sage stored ${preference} preference for cat ${catId}`);
    }
    
    getPreferences(catId) {
        return this.preferences.get(catId) || {};
    }
}

// Business Logic Layer - Professor Whiskers' decision making
class CatBusinessLayer {
    constructor(dataLayer) {
        this.dataLayer = dataLayer;
        this.rules = new Map();
        this.setupBusinessRules();
        
        console.log("🎓 Business Layer initialized - Professor Whiskers is ready to make decisions");
    }
    
    setupBusinessRules() {
        // Feeding rules
        this.rules.set('feeding', {
            maxMealsPerDay: 3,
            minTimeBetweenMeals: 4, // hours
            specialDietCats: ['Princess Fluffy'], // cats with special needs
        });
        
        // Territory rules
        this.rules.set('territory', {
            maxCatsPerRoom: 2,
            quietHours: { start: 22, end: 6 }, // 10 PM to 6 AM
            restrictedAreas: ['kitchen counter', 'human bedroom'],
        });
        
        // Activity rules
        this.rules.set('activity', {
            maxPlayTimePerCat: 120, // minutes per day
            requiredNapTime: 480,   // minutes per day
            socialInteractionRequired: true,
        });
    }
    
    // Feeding decisions
    approveFeedingRequest(catId, requestTime = new Date()) {
        console.log(`🤔 Professor Whiskers evaluating feeding request for cat ${catId}`);
        
        let cat = this.dataLayer.getCat(catId);
        if (!cat) {
            console.log(`❌ Unknown cat ${catId} - request denied`);
            return { approved: false, reason: 'Unknown cat' };
        }
        
        // Check recent meals
        let recentMeals = this.dataLayer.getActivitiesByType('meal')
            .filter(activity => activity.catId === catId)
            .filter(activity => {
                let timeDiff = (requestTime - activity.timestamp) / (1000 * 60 * 60);
                return timeDiff < 24; // Last 24 hours
            });
        
        let feedingRules = this.rules.get('feeding');
        
        if (recentMeals.length >= feedingRules.maxMealsPerDay) {
            console.log(`❌ ${cat.name} has reached daily meal limit`);
            return { approved: false, reason: 'Daily meal limit reached' };
        }
        
        // Check time since last meal
        if (recentMeals.length > 0) {
            let lastMeal = recentMeals[recentMeals.length - 1];
            let hoursSinceLastMeal = (requestTime - lastMeal.timestamp) / (1000 * 60 * 60);
            
            if (hoursSinceLastMeal < feedingRules.minTimeBetweenMeals) {
                console.log(`❌ Too soon since ${cat.name}'s last meal`);
                return { approved: false, reason: 'Too soon since last meal' };
            }
        }
        
        // Special diet check
        let foodType = feedingRules.specialDietCats.includes(cat.name) ? 'special' : 'regular';
        
        console.log(`✅ Feeding approved for ${cat.name}`);
        return { 
            approved: true, 
            foodType: foodType,
            portion: this.calculatePortion(cat),
            instructions: this.getSpecialInstructions(cat)
        };
    }
    
    calculatePortion(cat) {
        // Business logic for portion calculation
        let baseAmount = 100; // grams
        if (cat.age < 1) baseAmount *= 1.5; // Kittens need more
        if (cat.age > 7) baseAmount *= 0.8; // Seniors need less
        if (cat.activityLevel === 'high') baseAmount *= 1.2;
        
        return Math.round(baseAmount);
    }
    
    getSpecialInstructions(cat) {
        let instructions = [];
        
        if (cat.age < 1) instructions.push("Use kitten bowl");
        if (cat.healthIssues && cat.healthIssues.includes('dental')) {
            instructions.push("Soften dry food with water");
        }
        if (cat.personality === 'shy') {
            instructions.push("Feed in quiet location");
        }
        
        return instructions;
    }
    
    // Territory management decisions
    requestTerritoryAccess(catId, territoryId, activityType) {
        console.log(`🏠 Professor Whiskers evaluating territory access for cat ${catId}`);
        
        let cat = this.dataLayer.getCat(catId);
        let territory = this.dataLayer.getTerritory(territoryId);
        let territoryRules = this.rules.get('territory');
        
        if (!cat || !territory) {
            return { approved: false, reason: 'Invalid cat or territory' };
        }
        
        // Check if territory is restricted
        if (territoryRules.restrictedAreas.includes(territory.name)) {
            console.log(`❌ ${territory.name} is a restricted area`);
            return { approved: false, reason: 'Restricted area' };
        }
        
        // Check quiet hours for noisy activities
        let currentHour = new Date().getHours();
        let isQuietHours = currentHour >= territoryRules.quietHours.start || 
                          currentHour <= territoryRules.quietHours.end;
        
        if (isQuietHours && ['play', 'vocalization'].includes(activityType)) {
            console.log(`❌ Quiet hours in effect - noisy activities not allowed`);
            return { approved: false, reason: 'Quiet hours - no noisy activities' };
        }
        
        // Check occupancy
        let currentOccupants = this.getCurrentTerritoryOccupants(territoryId);
        if (currentOccupants.length >= territoryRules.maxCatsPerRoom) {
            console.log(`❌ ${territory.name} is at capacity`);
            return { approved: false, reason: 'Territory at capacity' };
        }
        
        console.log(`✅ Territory access approved for ${cat.name} in ${territory.name}`);
        return { approved: true, maxDuration: this.calculateMaxDuration(activityType) };
    }
    
    getCurrentTerritoryOccupants(territoryId) {
        // In real system, would track current occupancy
        return []; // Simplified for demo
    }
    
    calculateMaxDuration(activityType) {
        const durations = {
            'nap': 240,      // 4 hours
            'play': 60,      // 1 hour
            'grooming': 30,  // 30 minutes
            'patrol': 120    // 2 hours
        };
        return durations[activityType] || 60;
    }
}

// Presentation Layer - Princess Fluffy's human interaction
class CatPresentationLayer {
    constructor(businessLayer) {
        this.businessLayer = businessLayer;
        this.activeRequests = new Map();
        
        console.log("👑 Presentation Layer initialized - Princess Fluffy is ready to manage interactions");
    }
    
    // Handle feeding requests from humans
    handleFeedingRequest(humanRequest) {
        console.log(`👑 Princess Fluffy received feeding request: "${humanRequest}"`);
        
        // Parse human request
        let parsedRequest = this.parseHumanRequest(humanRequest);
        if (!parsedRequest.catId) {
            return this.formatResponse({
                success: false,
                message: "Please specify which cat needs feeding"
            });
        }
        
        // Forward to business layer
        let businessDecision = this.businessLayer.approveFeedingRequest(parsedRequest.catId);
        
        // Format response for human
        return this.formatFeedingResponse(businessDecision, parsedRequest);
    }
    
    parseHumanRequest(request) {
        // Simple parsing - in real system would be more sophisticated
        let catId = null;
        
        if (request.toLowerCase().includes('whiskers')) catId = 'whiskers-001';
        if (request.toLowerCase().includes('mittens')) catId = 'mittens-002';
        if (request.toLowerCase().includes('fluffy')) catId = 'fluffy-003';
        
        return { catId, originalRequest: request };
    }
    
    formatFeedingResponse(businessDecision, request) {
        if (businessDecision.approved) {
            let response = `✅ Feeding approved! Please serve ${businessDecision.portion}g of ${businessDecision.foodType} food.`;
            
            if (businessDecision.instructions.length > 0) {
                response += `\\nSpecial instructions: ${businessDecision.instructions.join(', ')}`;
            }
            
            return {
                success: true,
                message: response,
                action: 'feed',
                details: businessDecision
            };
        } else {
            return {
                success: false,
                message: `❌ Feeding denied: ${businessDecision.reason}`,
                suggestion: this.getSuggestion(businessDecision.reason)
            };
        }
    }
    
    getSuggestion(reason) {
        const suggestions = {
            'Daily meal limit reached': 'Try again tomorrow or offer a small healthy treat instead',
            'Too soon since last meal': 'Wait at least 4 hours between meals',
            'Unknown cat': 'Please register this cat in our system first'
        };
        
        return suggestions[reason] || 'Please try again later';
    }
    
    // Handle territory requests
    handleTerritoryRequest(humanRequest) {
        console.log(`👑 Princess Fluffy processing territory request: "${humanRequest}"`);
        
        // Parse territory request
        let parsed = this.parseTerritoryRequest(humanRequest);
        
        if (!parsed.catId || !parsed.territoryId) {
            return this.formatResponse({
                success: false,
                message: "Please specify cat and location clearly"
            });
        }
        
        // Forward to business layer
        let decision = this.businessLayer.requestTerritoryAccess(
            parsed.catId, 
            parsed.territoryId, 
            parsed.activityType
        );
        
        return this.formatTerritoryResponse(decision, parsed);
    }
    
    parseTerritoryRequest(request) {
        // Simplified parsing logic
        let catId = null;
        let territoryId = null;
        let activityType = 'general';
        
        // Extract cat
        if (request.includes('Whiskers')) catId = 'whiskers-001';
        if (request.includes('Mittens')) catId = 'mittens-002';
        
        // Extract territory
        if (request.includes('living room')) territoryId = 'living-room';
        if (request.includes('bedroom')) territoryId = 'bedroom';
        if (request.includes('kitchen')) territoryId = 'kitchen';
        
        // Extract activity
        if (request.includes('play')) activityType = 'play';
        if (request.includes('nap')) activityType = 'nap';
        if (request.includes('groom')) activityType = 'grooming';
        
        return { catId, territoryId, activityType, originalRequest: request };
    }
    
    formatTerritoryResponse(decision, request) {
        if (decision.approved) {
            return {
                success: true,
                message: `✅ Territory access granted for ${decision.maxDuration} minutes`,
                action: 'allow_access',
                details: decision
            };
        } else {
            return {
                success: false,
                message: `❌ Access denied: ${decision.reason}`,
                suggestion: this.getTerritoryAlternative(decision.reason)
            };
        }
    }
    
    getTerritoryAlternative(reason) {
        const alternatives = {
            'Restricted area': 'Try a different room that\'s cat-friendly',
            'Territory at capacity': 'Wait for other cats to finish, or try another location',
            'Quiet hours - no noisy activities': 'Please wait until morning for active play'
        };
        
        return alternatives[reason] || 'Please try a different request';
    }
    
    formatResponse(response) {
        return {
            timestamp: new Date(),
            ...response
        };
    }
    
    // Generate status reports for humans
    generateStatusReport() {
        console.log("👑 Princess Fluffy preparing status report for humans");
        
        let allCats = this.businessLayer.dataLayer.getAllCats();
        let recentActivities = this.businessLayer.dataLayer.activities.slice(-10);
        
        return {
            summary: `Currently managing ${allCats.length} cats`,
            catStatus: allCats.map(cat => ({
                name: cat.name,
                status: this.determineCatStatus(cat),
                lastSeen: cat.lastUpdated
            })),
            recentActivities: recentActivities.map(activity => ({
                time: activity.timestamp,
                description: this.humanizeActivity(activity)
            })),
            recommendations: this.generateRecommendations()
        };
    }
    
    determineCatStatus(cat) {
        // Simple status determination logic
        let hoursSinceUpdate = (new Date() - cat.lastUpdated) / (1000 * 60 * 60);
        
        if (hoursSinceUpdate > 12) return "Not seen recently - check on them";
        if (cat.mood === 'hungry') return "May need feeding soon";
        if (cat.mood === 'playful') return "Ready for interaction";
        return "Content and well";
    }
    
    humanizeActivity(activity) {
        const descriptions = {
            'meal': `${activity.catName} had a meal`,
            'play': `${activity.catName} played`,
            'nap': `${activity.catName} took a nap`,
            'territory_change': `${activity.catName} moved to ${activity.details}`
        };
        
        return descriptions[activity.type] || `${activity.catName} did something interesting`;
    }
    
    generateRecommendations() {
        return [
            "Check that all cats have fresh water",
            "Consider interactive play if cats seem restless",
            "Monitor senior cats more closely"
        ];
    }
}

// Security Layer - Hunter's protective services
class CatSecurityLayer {
    constructor(dataLayer) {
        this.dataLayer = dataLayer;
        this.threats = [];
        this.securityLevel = 'normal';
        this.patrols = new Map();
        
        console.log("🛡️ Security Layer initialized - Hunter is on patrol");
    }
    
    detectThreat(type, location, severity = 'medium') {
        let threat = {
            id: this.threats.length + 1,
            type: type,
            location: location,
            severity: severity,
            detected: new Date(),
            status: 'active'
        };
        
        this.threats.push(threat);
        console.log(`🚨 Hunter detected ${severity} ${type} threat at ${location}`);
        
        this.dataLayer.logActivity({
            type: 'security_event',
            catName: 'Hunter',
            details: `Detected ${type} at ${location}`,
            severity: severity
        });
        
        this.adjustSecurityLevel();
        return this.respondToThreat(threat);
    }
    
    respondToThreat(threat) {
        console.log(`🏃 Hunter responding to ${threat.type} threat`);
        
        let response = {
            threatId: threat.id,
            response: 'investigating',
            estimatedDuration: 30, // minutes
            backupRequired: false
        };
        
        switch (threat.type) {
            case 'intruder_cat':
                response.action = 'territorial_display';
                response.escalation = 'vocal_warning_then_chase';
                if (threat.severity === 'high') {
                    response.backupRequired = true;
                    response.backupType = 'all_cats_alert';
                }
                break;
                
            case 'mouse':
                response.action = 'hunt_and_eliminate';
                response.estimatedDuration = 60;
                break;
                
            case 'suspicious_human':
                response.action = 'monitor_and_report';
                response.reportTo = 'presentation_layer';
                break;
                
            case 'loud_noise':
                response.action = 'investigate_source';
                response.caution = 'high';
                break;
        }
        
        console.log(`📋 Response plan: ${response.action}`);
        return response;
    }
    
    adjustSecurityLevel() {
        let activeThreats = this.threats.filter(t => t.status === 'active');
        let highSeverityThreats = activeThreats.filter(t => t.severity === 'high');
        
        if (highSeverityThreats.length > 0) {
            this.securityLevel = 'high';
            console.log("🔴 Security level elevated to HIGH");
        } else if (activeThreats.length > 2) {
            this.securityLevel = 'elevated';
            console.log("🟡 Security level raised to ELEVATED");
        } else {
            this.securityLevel = 'normal';
            console.log("🟢 Security level normal");
        }
    }
    
    schedulePatrol(area, frequency = 'hourly') {
        this.patrols.set(area, {
            frequency: frequency,
            lastPatrol: new Date(),
            findings: []
        });
        
        console.log(`📅 Hunter scheduled ${frequency} patrols of ${area}`);
    }
    
    conductPatrol(area) {
        console.log(`👮 Hunter conducting patrol of ${area}`);
        
        let patrol = this.patrols.get(area);
        if (patrol) {
            patrol.lastPatrol = new Date();
            
            // Simulate patrol findings
            let findings = this.simulatePatrolFindings(area);
            patrol.findings.push(...findings);
            
            this.dataLayer.logActivity({
                type: 'patrol',
                catName: 'Hunter',
                details: `Patrolled ${area}`,
                findings: findings
            });
        }
        
        return patrol ? patrol.findings : [];
    }
    
    simulatePatrolFindings(area) {
        // Simulate random patrol findings
        let possibleFindings = [
            'area secure',
            'unusual scent detected',
            'entry point found',
            'food crumbs located',
            'toy retrieved'
        ];
        
        let numFindings = Math.floor(Math.random() * 3);
        let findings = [];
        
        for (let i = 0; i < numFindings; i++) {
            let randomFinding = possibleFindings[Math.floor(Math.random() * possibleFindings.length)];
            findings.push(randomFinding);
        }
        
        return findings;
    }
    
    getSecurityReport() {
        return {
            currentLevel: this.securityLevel,
            activeThreats: this.threats.filter(t => t.status === 'active').length,
            totalThreats: this.threats.length,
            patrolsScheduled: this.patrols.size,
            lastActivity: new Date(),
            recommendations: this.getSecurityRecommendations()
        };
    }
    
    getSecurityRecommendations() {
        let recommendations = [];
        
        if (this.securityLevel === 'high') {
            recommendations.push("Increase patrol frequency");
            recommendations.push("Alert all cats to potential danger");
        }
        
        let activeThreats = this.threats.filter(t => t.status === 'active');
        if (activeThreats.length > 0) {
            recommendations.push("Address active security threats");
        }
        
        if (this.patrols.size === 0) {
            recommendations.push("Establish regular patrol schedule");
        }
        
        return recommendations;
    }
}
```

### Complete System Integration

```javascript
// The complete Cat Mansion Management System
class CatMansionArchitecture {
    constructor() {
        console.log("🏰 Initializing Cat Mansion Management System...");
        
        // Initialize layers in dependency order
        this.dataLayer = new CatDataLayer();
        this.securityLayer = new CatSecurityLayer(this.dataLayer);
        this.businessLayer = new CatBusinessLayer(this.dataLayer);
        this.presentationLayer = new CatPresentationLayer(this.businessLayer);
        
        this.setupInitialData();
        console.log("✅ Cat Mansion Management System fully operational");
    }
    
    setupInitialData() {
        // Add some cats to the system
        let cats = [
            { id: 'whiskers-001', name: 'Professor Whiskers', age: 6, activityLevel: 'medium', personality: 'intellectual' },
            { id: 'fluffy-003', name: 'Princess Fluffy', age: 4, activityLevel: 'low', personality: 'dignified' },
            { id: 'hunter-004', name: 'Hunter', age: 8, activityLevel: 'high', personality: 'protective' },
            { id: 'sage-005', name: 'Sage', age: 10, activityLevel: 'low', personality: 'wise' }
        ];
        
        cats.forEach(cat => this.dataLayer.saveCat(cat));
        
        // Add territories
        let territories = [
            { id: 'living-room', name: 'living room', capacity: 3, type: 'social' },
            { id: 'kitchen', name: 'kitchen', capacity: 1, type: 'restricted' },
            { id: 'bedroom', name: 'master bedroom', capacity: 2, type: 'quiet' },
            { id: 'study', name: 'study', capacity: 2, type: 'work' }
        ];
        
        territories.forEach(territory => this.dataLayer.saveTerritory(territory));
        
        // Set up security patrols
        this.securityLayer.schedulePatrol('perimeter', 'hourly');
        this.securityLayer.schedulePatrol('basement', 'daily');
        this.securityLayer.schedulePatrol('first_floor', 'every_2_hours');
    }
    
    // Public interface for the entire system
    processRequest(type, request) {
        console.log(`\n🎯 Processing ${type} request: "${request}"`);
        
        switch (type) {
            case 'feeding':
                return this.presentationLayer.handleFeedingRequest(request);
            case 'territory':
                return this.presentationLayer.handleTerritoryRequest(request);
            case 'security':
                return this.handleSecurityEvent(request);
            case 'status':
                return this.presentationLayer.generateStatusReport();
            default:
                return { success: false, message: 'Unknown request type' };
        }
    }
    
    handleSecurityEvent(eventDescription) {
        // Parse security event
        let eventType = 'suspicious_activity';
        let location = 'unknown';
        let severity = 'medium';
        
        if (eventDescription.includes('intruder')) eventType = 'intruder_cat';
        if (eventDescription.includes('mouse')) eventType = 'mouse';
        if (eventDescription.includes('loud')) eventType = 'loud_noise';
        
        if (eventDescription.includes('kitchen')) location = 'kitchen';
        if (eventDescription.includes('basement')) location = 'basement';
        if (eventDescription.includes('yard')) location = 'yard';
        
        if (eventDescription.includes('urgent') || eventDescription.includes('emergency')) {
            severity = 'high';
        }
        
        let response = this.securityLayer.detectThreat(eventType, location, severity);
        
        return {
            success: true,
            message: `Security response initiated: ${response.action}`,
            details: response
        };
    }
    
    // System health and monitoring
    getSystemStatus() {
        return {
            timestamp: new Date(),
            layers: {
                presentation: { status: 'operational', owner: 'Princess Fluffy' },
                business: { status: 'operational', owner: 'Professor Whiskers' },
                security: { status: 'operational', owner: 'Hunter' },
                data: { status: 'operational', owner: 'Sage' }
            },
            metrics: {
                totalCats: this.dataLayer.getAllCats().length,
                totalActivities: this.dataLayer.activities.length,
                securityLevel: this.securityLayer.securityLevel,
                activeThreats: this.securityLayer.threats.filter(t => t.status === 'active').length
            },
            recommendations: this.getSystemRecommendations()
        };
    }
    
    getSystemRecommendations() {
        let recommendations = [];
        
        let securityReport = this.securityLayer.getSecurityReport();
        recommendations.push(...securityReport.recommendations);
        
        if (this.dataLayer.activities.length > 1000) {
            recommendations.push("Consider archiving old activity data");
        }
        
        if (this.dataLayer.getAllCats().length > 10) {
            recommendations.push("System is at high capacity - monitor performance");
        }
        
        return recommendations;
    }
}

// Demonstration of the complete system
console.log("=== CAT MANSION ARCHITECTURE DEMO ===");

let catMansion = new CatMansionArchitecture();

// Test feeding requests
console.log("\n--- FEEDING REQUESTS ---");
console.log(catMansion.processRequest('feeding', 'Whiskers is hungry'));
console.log(catMansion.processRequest('feeding', 'Feed Fluffy please'));

// Test territory requests  
console.log("\n--- TERRITORY REQUESTS ---");
console.log(catMansion.processRequest('territory', 'Whiskers wants to play in living room'));
console.log(catMansion.processRequest('territory', 'Mittens needs to nap in bedroom'));

// Test security events
console.log("\n--- SECURITY EVENTS ---");
console.log(catMansion.processRequest('security', 'Intruder cat in yard - urgent'));
console.log(catMansion.processRequest('security', 'Mouse spotted in kitchen'));

// Generate status reports
console.log("\n--- STATUS REPORTS ---");
console.log(JSON.stringify(catMansion.processRequest('status', ''), null, 2));

console.log("\n--- SYSTEM STATUS ---");
console.log(JSON.stringify(catMansion.getSystemStatus(), null, 2));
```

## Paw-sible Problems

### Exercise 1: Microservices Cat Colony
Design a microservices architecture for managing multiple cat colonies.

```javascript
class CatColonyMicroservices {
    // Design separate services:
    // - CatRegistrationService
    // - FeedingScheduleService  
    // - TerritoryManagementService
    // - HealthMonitoringService
    // - CommunicationService
    // Each service should be independent with API interfaces
}
```

### Exercise 2: Event-Driven Cat Behavior System
Create an event-driven architecture for tracking and responding to cat behaviors.

```javascript
class CatBehaviorEventSystem {
    // Implement event sourcing:
    // - BehaviorEventStore
    // - EventPublisher/Subscriber
    // - BehaviorAnalysisHandler
    // - AlertingService
    // Events: movement, eating, playing, sleeping, vocalizing
}
```

### Exercise 3: Plugin Architecture for Cat Accessories
Design a plugin system for adding new cat accessory functionalities.

```javascript
class CatAccessoryFramework {
    // Core framework with plugin interface
    // Sample plugins:
    // - GPSTrackingPlugin
    // - HealthMonitorPlugin  
    // - AutoFeederPlugin
    // - ToyInteractionPlugin
    // Plugins should be loadable/unloadable at runtime
}
```

### Exercise 4: Hexagonal Architecture Cat Management
Implement hexagonal (ports and adapters) architecture for a cat management system.

```javascript
class HexagonalCatSystem {
    // Core domain in center
    // Ports: interfaces for external communication
    // Adapters: implementations for specific technologies
    // Example ports: CatRepository, NotificationService, ExternalAPI
    // Example adapters: DatabaseAdapter, EmailAdapter, RESTAdapter
}
```

### Exercise 5: CQRS for Cat Activity Tracking
Create a Command Query Responsibility Segregation system for cat activities.

```javascript
class CatActivityCQRS {
    // Command side: Handle cat behavior commands
    // Query side: Optimized for reading cat data and analytics
    // Event store: Store all cat activity events
    // Read models: Specialized views for different queries
    // Demonstrate separation of write and read concerns
}
```

## Cat-ch the Pattern

### Key Takeaways

1. **Architecture defines structure**: Like territorial boundaries, architecture creates clear organization
2. **Separation of concerns**: Each layer/component has specific responsibilities
3. **Loose coupling**: Components interact through well-defined interfaces
4. **Scalability planning**: Good architecture supports growth and change
5. **Fault tolerance**: System continues working even when parts fail

### Common Architectural Patterns

**Layered Architecture**:
- Presentation Layer (user interface)
- Business Logic Layer (rules and decisions)
- Data Access Layer (storage and retrieval)
- Clear separation, easy to understand

**Model-View-Controller (MVC)**:
- Model: Data and business logic
- View: User interface and presentation
- Controller: Handles user input and coordinates

**Microservices**:
- Small, independent services
- Each service has single responsibility
- Services communicate over network
- Enables independent scaling and deployment

**Event-Driven Architecture**:
- Components communicate through events
- Loose coupling between producers and consumers
- Good for real-time and reactive systems

### Architectural Principles

```javascript
// Single Responsibility Principle
class CatFeeder {
    // Only responsible for feeding logic
    feed(cat, amount) { /* feeding logic */ }
}

class CatHealthMonitor {
    // Only responsible for health monitoring
    checkHealth(cat) { /* health logic */ }
}

// Open/Closed Principle
class CatBehaviorProcessor {
    constructor() {
        this.handlers = [];
    }
    
    addHandler(handler) {
        this.handlers.push(handler); // Open for extension
    }
    
    processBehavior(behavior) {
        // Closed for modification
        this.handlers.forEach(handler => handler.handle(behavior));
    }
}

// Dependency Inversion
class CatService {
    constructor(repository, notifier) { // Depend on abstractions
        this.repository = repository;   // Not concrete implementations
        this.notifier = notifier;
    }
}
```

### Architecture Decision Factors

**Consider when choosing architecture:**
- System complexity and size
- Team size and expertise
- Performance requirements
- Scalability needs
- Maintenance constraints
- Technology constraints

**Layered Architecture**: Good for traditional business applications
**Microservices**: Good for large, complex systems with multiple teams
**Event-Driven**: Good for real-time, reactive systems
**Client-Server**: Good for distributed applications

### Common Architecture Anti-Patterns

```javascript
// Big Ball of Mud (no clear structure)
class BadCatManager {
    doEverything(request) {
        // Feeding logic mixed with territory management
        // Security checks mixed with data access
        // Presentation logic mixed with business rules
        // Impossible to maintain or test
    }
}

// God Object (does too much)
class GodCat {
    // Handles feeding, security, territory, health, 
    // communication, scheduling, reporting, etc.
    // Violates single responsibility principle
}

// Spaghetti Code (tangled dependencies)
// Component A depends on B, which depends on C,
// which depends on A - circular dependencies
// Makes testing and modification very difficult

// Golden Hammer (one solution for everything)
// Using the same pattern/technology for every problem
// "When you have a hammer, everything looks like a nail"
```

### Testing Architectural Components

```javascript
// Test layers independently
describe('CatBusinessLayer', () => {
    it('should approve valid feeding requests', () => {
        let mockDataLayer = {
            getCat: () => ({ name: 'Whiskers', age: 3 }),
            getActivitiesByType: () => []
        };
        
        let businessLayer = new CatBusinessLayer(mockDataLayer);
        let result = businessLayer.approveFeedingRequest('whiskers-001');
        
        expect(result.approved).toBe(true);
    });
});

// Integration tests for layer communication
describe('System Integration', () => {
    it('should process feeding request end-to-end', async () => {
        let system = new CatMansionArchitecture();
        let result = system.processRequest('feeding', 'Whiskers is hungry');
        
        expect(result.success).toBe(true);
    });
});
```

### Architecture Documentation

```javascript
/**
 * Cat Mansion Management System Architecture
 * 
 * Layers:
 * 1. Presentation Layer (CatPresentationLayer)
 *    - Handles human interactions
 *    - Formats requests and responses
 *    - Managed by Princess Fluffy
 * 
 * 2. Business Logic Layer (CatBusinessLayer)
 *    - Implements business rules and decisions
 *    - Validates requests against policies
 *    - Managed by Professor Whiskers
 * 
 * 3. Security Layer (CatSecurityLayer)
 *    - Monitors threats and security
 *    - Manages access control
 *    - Managed by Hunter
 * 
 * 4. Data Access Layer (CatDataLayer)
 *    - Stores and retrieves data
 *    - Manages persistence
 *    - Managed by Sage
 * 
 * Communication Flow:
 * Human Request -> Presentation -> Business -> Data
 * Security events can be triggered from any layer
 * 
 * Key Design Decisions:
 * - Layered architecture for clear separation
 * - Each layer has single responsibility
 * - Loose coupling through interfaces
 * - Security as cross-cutting concern
 */
```

### Looking Ahead

Architecture provides the foundation for building robust systems, but even the best architecture needs verification that it works correctly. In our next chapter, we'll explore testing - the systematic process of validating that our systems behave as expected, much like how cats test the boundaries of their environment to ensure everything is safe and working properly.

We'll discover how comprehensive testing strategies help maintain system quality and reliability, just as cats' natural boundary-testing behaviors help maintain the stability of their territorial systems.

---

*Next Chapter: [Testing the Boundaries](chapter-15-testing-the-boundaries.md)*

*Previous: [Design Patterns in Cat Behavior](chapter-13-design-patterns.md)*