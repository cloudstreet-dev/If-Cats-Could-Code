# Chapter 18: APIs and Cat Communication Protocols

## The Opening Tail

The Neighborhood Cat Network had evolved into a sophisticated communication system that would make any telecommunications engineer proud. At the center of it all was Ambassador Whiskers, a wise tabby who had established himself as the primary coordinator for inter-household cat communications.

Ambassador Whiskers understood that different situations required different communication protocols. For routine territory updates, a simple scent mark on a fence post was sufficient - a basic "GET request" that other cats could read to understand the current status of his domain. For more urgent matters, like alerting about a new dog in the neighborhood, he would use the standardized "Urgent Yowl Protocol" - a specific sequence of meows that carried both the alert level and basic location information.

The most impressive aspect of the network was how it handled complex, multi-step interactions. When Princess Fluffy wanted to arrange a play session with cats from three different households, she didn't just randomly meow at everyone. Instead, she followed a structured protocol: first, she would send an availability request to each potential participant using the "Inquiry Chirp." Each cat would respond with their current status using standardized response signals. Once she had confirmed availability, she would send detailed meeting information using the "Coordination Sequence" - a series of specific vocalizations that conveyed time, location, and activity type.

What made the system truly robust was its error handling and fallback mechanisms. If a cat didn't respond to the initial inquiry, the protocol included retry attempts at different volumes and from different locations. If the primary communication channel (vocalizations) was blocked by human activity or competing noise, cats would seamlessly switch to visual signals, scent markers, or even physical messenger systems where one cat would travel to deliver important information directly.

The network even had versioning and backward compatibility. Older cats who hadn't learned the newer, more efficient protocols could still participate using the legacy "Basic Meow Standard," while younger cats could take advantage of advanced features like "Contextual Purr Modulation" for more nuanced information sharing.

## The Technical Purr-spective

An API (Application Programming Interface) is like the cat communication network - a standardized way for different systems to interact, share information, and coordinate activities. APIs define the protocols, data formats, and rules that enable different applications to communicate effectively, just as cats have evolved standardized communication patterns to share information across their social networks.

### What Makes a Good API

**Consistency**: Predictable patterns like standardized cat vocalizations
**Clarity**: Clear documentation like well-understood territorial markers  
**Reliability**: Consistent behavior like dependable scent trails
**Flexibility**: Adaptable to different use cases like multi-purpose meow patterns
**Security**: Controlled access like territorial boundary enforcement

### Types of APIs

**REST APIs**: Use HTTP methods (GET, POST, PUT, DELETE) for different operations
**GraphQL**: Flexible query language for requesting specific data
**WebSockets**: Real-time, bidirectional communication
**RPC**: Remote Procedure Calls for function-style interactions

### API Communication Patterns

**Request-Response**: Client asks, server responds (like cat inquiry and reply)
**Publish-Subscribe**: Broadcast messages to interested parties (like territorial alerts)
**Webhooks**: Server notifies client when events occur (like automatic updates)

## Code in the Wild

### RESTful Cat Management API

```javascript
// RESTful API for cat colony management
class CatColonyAPI {
    constructor() {
        this.cats = new Map();
        this.territories = new Map();
        this.activities = [];
        this.nextCatId = 1;
        this.nextTerritoryId = 1;
    }
    
    // GET /api/cats - List all cats
    getCats(queryParams = {}) {
        console.log("🔍 GET /api/cats");
        
        let cats = Array.from(this.cats.values());
        
        // Apply filtering based on query parameters
        if (queryParams.breed) {
            cats = cats.filter(cat => cat.breed.toLowerCase().includes(queryParams.breed.toLowerCase()));
        }
        
        if (queryParams.age) {
            let ageFilter = parseInt(queryParams.age);
            cats = cats.filter(cat => cat.age === ageFilter);
        }
        
        if (queryParams.territory) {
            cats = cats.filter(cat => cat.territoryId === queryParams.territory);
        }
        
        // Apply sorting
        if (queryParams.sortBy) {
            cats.sort((a, b) => {
                if (queryParams.sortBy === 'age') return a.age - b.age;
                if (queryParams.sortBy === 'name') return a.name.localeCompare(b.name);
                return 0;
            });
        }
        
        // Apply pagination
        let page = parseInt(queryParams.page) || 1;
        let limit = parseInt(queryParams.limit) || 10;
        let startIndex = (page - 1) * limit;
        let endIndex = startIndex + limit;
        
        let paginatedCats = cats.slice(startIndex, endIndex);
        
        return {
            status: 200,
            data: {
                cats: paginatedCats,
                pagination: {
                    currentPage: page,
                    totalPages: Math.ceil(cats.length / limit),
                    totalCount: cats.length,
                    hasNextPage: endIndex < cats.length,
                    hasPreviousPage: page > 1
                }
            },
            meta: {
                timestamp: new Date().toISOString(),
                version: "1.0.0"
            }
        };
    }
    
    // GET /api/cats/:id - Get specific cat
    getCat(catId) {
        console.log(`🔍 GET /api/cats/${catId}`);
        
        let cat = this.cats.get(catId);
        
        if (!cat) {
            return {
                status: 404,
                error: {
                    code: "CAT_NOT_FOUND",
                    message: `Cat with ID ${catId} not found`,
                    details: "The requested cat does not exist in our records"
                }
            };
        }
        
        // Include related data
        let territory = this.territories.get(cat.territoryId);
        let recentActivities = this.activities
            .filter(activity => activity.catId === catId)
            .slice(-5); // Last 5 activities
        
        return {
            status: 200,
            data: {
                cat: cat,
                territory: territory,
                recentActivities: recentActivities
            },
            links: {
                self: `/api/cats/${catId}`,
                territory: territory ? `/api/territories/${territory.id}` : null,
                activities: `/api/cats/${catId}/activities`
            }
        };
    }
    
    // POST /api/cats - Create new cat
    createCat(catData) {
        console.log("📝 POST /api/cats");
        
        // Validate required fields
        let requiredFields = ['name', 'breed', 'age'];
        for (let field of requiredFields) {
            if (!catData[field]) {
                return {
                    status: 400,
                    error: {
                        code: "VALIDATION_ERROR",
                        message: `Missing required field: ${field}`,
                        details: `The field '${field}' is required but was not provided`
                    }
                };
            }
        }
        
        // Validate data types and ranges
        if (typeof catData.age !== 'number' || catData.age < 0 || catData.age > 25) {
            return {
                status: 400,
                error: {
                    code: "VALIDATION_ERROR",
                    message: "Invalid age value",
                    details: "Age must be a number between 0 and 25"
                }
            };
        }
        
        // Check for duplicate names
        let existingCat = Array.from(this.cats.values()).find(cat => cat.name === catData.name);
        if (existingCat) {
            return {
                status: 409,
                error: {
                    code: "DUPLICATE_NAME",
                    message: "A cat with this name already exists",
                    details: `Cat '${catData.name}' already exists with ID ${existingCat.id}`
                }
            };
        }
        
        // Create new cat
        let newCat = {
            id: this.nextCatId++,
            name: catData.name,
            breed: catData.breed,
            age: catData.age,
            color: catData.color || "unknown",
            personality: catData.personality || "friendly",
            territoryId: catData.territoryId || null,
            healthStatus: "healthy",
            registrationDate: new Date().toISOString(),
            lastUpdated: new Date().toISOString()
        };
        
        this.cats.set(newCat.id, newCat);
        
        // Log activity
        this.activities.push({
            id: this.activities.length + 1,
            catId: newCat.id,
            type: "registration",
            description: `${newCat.name} registered in the colony`,
            timestamp: new Date().toISOString()
        });
        
        return {
            status: 201,
            data: {
                cat: newCat
            },
            links: {
                self: `/api/cats/${newCat.id}`,
                activities: `/api/cats/${newCat.id}/activities`
            },
            meta: {
                message: "Cat successfully registered"
            }
        };
    }
    
    // PUT /api/cats/:id - Update existing cat
    updateCat(catId, updateData) {
        console.log(`📝 PUT /api/cats/${catId}`);
        
        let cat = this.cats.get(catId);
        if (!cat) {
            return {
                status: 404,
                error: {
                    code: "CAT_NOT_FOUND",
                    message: `Cat with ID ${catId} not found`
                }
            };
        }
        
        // Store original values for change tracking
        let originalCat = { ...cat };
        
        // Update allowed fields
        let allowedUpdates = ['name', 'age', 'personality', 'territoryId', 'healthStatus'];
        let changes = [];
        
        for (let field of allowedUpdates) {
            if (updateData[field] !== undefined) {
                if (cat[field] !== updateData[field]) {
                    changes.push({
                        field: field,
                        oldValue: cat[field],
                        newValue: updateData[field]
                    });
                    cat[field] = updateData[field];
                }
            }
        }
        
        cat.lastUpdated = new Date().toISOString();
        
        // Log changes
        if (changes.length > 0) {
            this.activities.push({
                id: this.activities.length + 1,
                catId: catId,
                type: "update",
                description: `${cat.name} information updated`,
                changes: changes,
                timestamp: new Date().toISOString()
            });
        }
        
        return {
            status: 200,
            data: {
                cat: cat,
                changes: changes
            },
            meta: {
                message: changes.length > 0 ? "Cat updated successfully" : "No changes detected"
            }
        };
    }
    
    // DELETE /api/cats/:id - Remove cat (adoption, etc.)
    deleteCat(catId) {
        console.log(`🗑️ DELETE /api/cats/${catId}`);
        
        let cat = this.cats.get(catId);
        if (!cat) {
            return {
                status: 404,
                error: {
                    code: "CAT_NOT_FOUND",
                    message: `Cat with ID ${catId} not found`
                }
            };
        }
        
        // Store cat info before deletion
        let deletedCat = { ...cat };
        
        // Remove cat
        this.cats.delete(catId);
        
        // Log activity
        this.activities.push({
            id: this.activities.length + 1,
            catId: catId,
            type: "removal",
            description: `${deletedCat.name} removed from colony (adopted)`,
            timestamp: new Date().toISOString()
        });
        
        return {
            status: 200,
            data: {
                message: `${deletedCat.name} successfully removed from colony`,
                cat: deletedCat
            }
        };
    }
    
    // GET /api/cats/:id/activities - Get cat activities
    getCatActivities(catId, queryParams = {}) {
        console.log(`🔍 GET /api/cats/${catId}/activities`);
        
        if (!this.cats.has(catId)) {
            return {
                status: 404,
                error: {
                    code: "CAT_NOT_FOUND",
                    message: `Cat with ID ${catId} not found`
                }
            };
        }
        
        let activities = this.activities.filter(activity => activity.catId === catId);
        
        // Apply filtering
        if (queryParams.type) {
            activities = activities.filter(activity => activity.type === queryParams.type);
        }
        
        if (queryParams.since) {
            let sinceDate = new Date(queryParams.since);
            activities = activities.filter(activity => new Date(activity.timestamp) >= sinceDate);
        }
        
        // Sort by timestamp (newest first)
        activities.sort((a, b) => new Date(b.timestamp) - new Date(a.timestamp));
        
        return {
            status: 200,
            data: {
                catId: catId,
                activities: activities,
                count: activities.length
            }
        };
    }
    
    // POST /api/cats/:id/activities - Add activity
    addCatActivity(catId, activityData) {
        console.log(`📝 POST /api/cats/${catId}/activities`);
        
        if (!this.cats.has(catId)) {
            return {
                status: 404,
                error: {
                    code: "CAT_NOT_FOUND",
                    message: `Cat with ID ${catId} not found`
                }
            };
        }
        
        let activity = {
            id: this.activities.length + 1,
            catId: catId,
            type: activityData.type || "general",
            description: activityData.description,
            metadata: activityData.metadata || {},
            timestamp: new Date().toISOString()
        };
        
        this.activities.push(activity);
        
        return {
            status: 201,
            data: {
                activity: activity
            }
        };
    }
}

// Demonstrate REST API usage
console.log("=== REST API DEMONSTRATION ===");

let catAPI = new CatColonyAPI();

// Create some cats
console.log("\\n--- Creating Cats ---");
let whiskers = catAPI.createCat({
    name: "Whiskers",
    breed: "Tabby",
    age: 3,
    color: "brown",
    personality: "curious"
});
console.log("Create result:", whiskers.status, whiskers.data?.cat?.name || whiskers.error?.message);

let mittens = catAPI.createCat({
    name: "Mittens",
    breed: "Persian",
    age: 2,
    color: "white"
});
console.log("Create result:", mittens.status, mittens.data?.cat?.name || mittens.error?.message);

// Try to create duplicate
let duplicate = catAPI.createCat({
    name: "Whiskers", // Duplicate name
    breed: "Siamese",
    age: 4
});
console.log("Duplicate result:", duplicate.status, duplicate.error?.message);

// Get all cats
console.log("\\n--- Retrieving Cats ---");
let allCats = catAPI.getCats();
console.log(`Found ${allCats.data.cats.length} cats`);

// Get specific cat
let specificCat = catAPI.getCat(1);
console.log("Specific cat:", specificCat.status, specificCat.data?.cat?.name);

// Update cat
console.log("\\n--- Updating Cat ---");
let updateResult = catAPI.updateCat(1, { age: 4, personality: "very curious" });
console.log("Update result:", updateResult.status, `${updateResult.data?.changes?.length || 0} changes`);

// Add activity
console.log("\\n--- Adding Activities ---");
let activityResult = catAPI.addCatActivity(1, {
    type: "play",
    description: "Played with feather toy for 15 minutes",
    metadata: { duration: 15, toy: "feather wand" }
});
console.log("Activity result:", activityResult.status);

// Get activities
let activities = catAPI.getCatActivities(1);
console.log(`Cat has ${activities.data?.activities?.length || 0} activities`);

// Filtered search
console.log("\\n--- Filtered Search ---");
let filteredCats = catAPI.getCats({ breed: "tabby", sortBy: "age" });
console.log(`Filtered search found ${filteredCats.data.cats.length} cats`);
```

### WebSocket-based Real-time Cat Monitoring

```javascript
// Real-time cat monitoring system using WebSocket-style communication
class CatMonitoringWebSocket {
    constructor() {
        this.connections = new Map(); // clientId -> connection info
        this.catSensors = new Map();  // catId -> sensor data
        this.subscriptions = new Map(); // clientId -> subscribed events
        this.eventHistory = [];
        this.nextConnectionId = 1;
    }
    
    // Simulate WebSocket connection
    connect(clientInfo) {
        let connectionId = this.nextConnectionId++;
        let connection = {
            id: connectionId,
            clientInfo: clientInfo,
            connectedAt: new Date(),
            lastActivity: new Date(),
            subscriptions: new Set()
        };
        
        this.connections.set(connectionId, connection);
        console.log(`🔌 Client ${connectionId} connected: ${clientInfo.name}`);
        
        // Send welcome message
        this.sendToClient(connectionId, {
            type: 'welcome',
            connectionId: connectionId,
            serverTime: new Date().toISOString(),
            availableEvents: ['cat_location', 'cat_activity', 'cat_health', 'system_alert']
        });
        
        return connectionId;
    }
    
    disconnect(connectionId) {
        let connection = this.connections.get(connectionId);
        if (connection) {
            console.log(`🔌 Client ${connectionId} disconnected`);
            this.connections.delete(connectionId);
            this.subscriptions.delete(connectionId);
        }
    }
    
    // Handle incoming messages from clients
    handleMessage(connectionId, message) {
        let connection = this.connections.get(connectionId);
        if (!connection) {
            console.log(`❌ Message from unknown connection: ${connectionId}`);
            return;
        }
        
        connection.lastActivity = new Date();
        
        console.log(`📨 Message from ${connectionId}: ${message.type}`);
        
        switch (message.type) {
            case 'subscribe':
                this.handleSubscription(connectionId, message.data);
                break;
                
            case 'unsubscribe':
                this.handleUnsubscription(connectionId, message.data);
                break;
                
            case 'get_cat_status':
                this.handleCatStatusRequest(connectionId, message.data);
                break;
                
            case 'send_cat_command':
                this.handleCatCommand(connectionId, message.data);
                break;
                
            case 'ping':
                this.sendToClient(connectionId, { type: 'pong', timestamp: new Date().toISOString() });
                break;
                
            default:
                this.sendToClient(connectionId, {
                    type: 'error',
                    message: `Unknown message type: ${message.type}`
                });
        }
    }
    
    handleSubscription(connectionId, subscriptionData) {
        let connection = this.connections.get(connectionId);
        let { eventTypes, catIds, filters } = subscriptionData;
        
        eventTypes.forEach(eventType => {
            connection.subscriptions.add(eventType);
        });
        
        console.log(`📋 Client ${connectionId} subscribed to: ${eventTypes.join(', ')}`);
        
        this.sendToClient(connectionId, {
            type: 'subscription_confirmed',
            subscribedEvents: Array.from(connection.subscriptions),
            message: `Subscribed to ${eventTypes.length} event types`
        });
    }
    
    handleUnsubscription(connectionId, unsubscriptionData) {
        let connection = this.connections.get(connectionId);
        let { eventTypes } = unsubscriptionData;
        
        eventTypes.forEach(eventType => {
            connection.subscriptions.delete(eventType);
        });
        
        console.log(`📋 Client ${connectionId} unsubscribed from: ${eventTypes.join(', ')}`);
        
        this.sendToClient(connectionId, {
            type: 'unsubscription_confirmed',
            remainingSubscriptions: Array.from(connection.subscriptions)
        });
    }
    
    handleCatStatusRequest(connectionId, requestData) {
        let { catId } = requestData;
        let sensorData = this.catSensors.get(catId);
        
        if (!sensorData) {
            this.sendToClient(connectionId, {
                type: 'error',
                message: `No sensor data available for cat ${catId}`
            });
            return;
        }
        
        this.sendToClient(connectionId, {
            type: 'cat_status_response',
            catId: catId,
            status: sensorData,
            timestamp: new Date().toISOString()
        });
    }
    
    handleCatCommand(connectionId, commandData) {
        let { catId, command, parameters } = commandData;
        
        console.log(`🎮 Command for cat ${catId}: ${command}`);
        
        // Simulate command execution
        let success = Math.random() > 0.1; // 90% success rate
        
        if (success) {
            // Broadcast command execution to subscribers
            this.broadcast('cat_command_executed', {
                catId: catId,
                command: command,
                parameters: parameters,
                executedBy: connectionId,
                timestamp: new Date().toISOString()
            });
        } else {
            this.sendToClient(connectionId, {
                type: 'command_failed',
                catId: catId,
                command: command,
                error: 'Cat is currently busy and cannot execute command'
            });
        }
    }
    
    // Send message to specific client
    sendToClient(connectionId, message) {
        let connection = this.connections.get(connectionId);
        if (connection) {
            console.log(`📤 To ${connectionId}: ${message.type}`);
            // In real WebSocket, this would be: connection.send(JSON.stringify(message))
        }
    }
    
    // Broadcast message to all subscribed clients
    broadcast(eventType, eventData) {
        let messagesSent = 0;
        
        this.connections.forEach((connection, connectionId) => {
            if (connection.subscriptions.has(eventType)) {
                this.sendToClient(connectionId, {
                    type: 'event',
                    eventType: eventType,
                    data: eventData,
                    timestamp: new Date().toISOString()
                });
                messagesSent++;
            }
        });
        
        console.log(`📡 Broadcast ${eventType} to ${messagesSent} clients`);
        
        // Store in event history
        this.eventHistory.push({
            type: eventType,
            data: eventData,
            timestamp: new Date().toISOString(),
            subscriberCount: messagesSent
        });
    }
    
    // Simulate cat sensor updates
    updateCatSensor(catId, sensorData) {
        let existingData = this.catSensors.get(catId) || {};
        let updatedData = { ...existingData, ...sensorData, lastUpdate: new Date().toISOString() };
        
        this.catSensors.set(catId, updatedData);
        
        // Broadcast location updates
        if (sensorData.location) {
            this.broadcast('cat_location', {
                catId: catId,
                location: sensorData.location,
                previousLocation: existingData.location
            });
        }
        
        // Broadcast activity updates
        if (sensorData.activity) {
            this.broadcast('cat_activity', {
                catId: catId,
                activity: sensorData.activity,
                duration: sensorData.activityDuration || 0
            });
        }
        
        // Broadcast health alerts
        if (sensorData.healthAlert) {
            this.broadcast('cat_health', {
                catId: catId,
                alert: sensorData.healthAlert,
                severity: sensorData.alertSeverity || 'low'
            });
        }
    }
    
    // Get connection statistics
    getConnectionStats() {
        let connectionCount = this.connections.size;
        let totalSubscriptions = Array.from(this.connections.values())
            .reduce((sum, conn) => sum + conn.subscriptions.size, 0);
        
        let eventTypeCounts = {};
        this.connections.forEach(conn => {
            conn.subscriptions.forEach(eventType => {
                eventTypeCounts[eventType] = (eventTypeCounts[eventType] || 0) + 1;
            });
        });
        
        return {
            activeConnections: connectionCount,
            totalSubscriptions: totalSubscriptions,
            eventTypeSubscriptions: eventTypeCounts,
            totalEvents: this.eventHistory.length,
            recentEvents: this.eventHistory.slice(-10)
        };
    }
}

// Demonstrate WebSocket-style real-time communication
console.log("\\n=== WEBSOCKET-STYLE REAL-TIME COMMUNICATION ===");

let catMonitor = new CatMonitoringWebSocket();

// Simulate multiple client connections
let mobileApp = catMonitor.connect({ name: "Mobile App", type: "mobile" });
let webDashboard = catMonitor.connect({ name: "Web Dashboard", type: "web" });
let alertSystem = catMonitor.connect({ name: "Alert System", type: "service" });

// Clients subscribe to different events
catMonitor.handleMessage(mobileApp, {
    type: 'subscribe',
    data: {
        eventTypes: ['cat_location', 'cat_activity'],
        catIds: ['whiskers-001', 'mittens-002']
    }
});

catMonitor.handleMessage(webDashboard, {
    type: 'subscribe',
    data: {
        eventTypes: ['cat_location', 'cat_activity', 'cat_health', 'system_alert'],
        catIds: null // All cats
    }
});

catMonitor.handleMessage(alertSystem, {
    type: 'subscribe',
    data: {
        eventTypes: ['cat_health', 'system_alert'],
        catIds: null
    }
});

// Simulate sensor data updates
setTimeout(() => {
    console.log("\\n--- Simulating Sensor Updates ---");
    
    catMonitor.updateCatSensor('whiskers-001', {
        location: { room: 'kitchen', coordinates: { x: 10, y: 15 } },
        activity: 'eating',
        activityDuration: 300
    });
    
    catMonitor.updateCatSensor('mittens-002', {
        location: { room: 'living_room', coordinates: { x: 25, y: 30 } },
        activity: 'napping'
    });
    
    catMonitor.updateCatSensor('shadow-003', {
        location: { room: 'bedroom', coordinates: { x: 5, y: 8 } },
        activity: 'playing',
        healthAlert: 'elevated_heart_rate',
        alertSeverity: 'medium'
    });
}, 1000);

// Client sends commands
setTimeout(() => {
    console.log("\\n--- Client Sending Commands ---");
    
    catMonitor.handleMessage(mobileApp, {
        type: 'send_cat_command',
        data: {
            catId: 'whiskers-001',
            command: 'call_to_food_bowl',
            parameters: { urgency: 'low' }
        }
    });
    
    catMonitor.handleMessage(webDashboard, {
        type: 'get_cat_status',
        data: {
            catId: 'mittens-002'
        }
    });
}, 2000);

// Show connection statistics
setTimeout(() => {
    console.log("\\n--- Connection Statistics ---");
    let stats = catMonitor.getConnectionStats();
    console.log("Active connections:", stats.activeConnections);
    console.log("Event subscriptions:", stats.eventTypeSubscriptions);
    console.log("Recent events:", stats.recentEvents.length);
}, 3000);
```

### GraphQL-style Flexible Cat Data Queries

```javascript
// GraphQL-inspired flexible query system for cat data
class CatGraphQLResolver {
    constructor() {
        this.cats = new Map([
            ['1', { id: '1', name: 'Whiskers', breed: 'Tabby', age: 3, territoryId: 'T1' }],
            ['2', { id: '2', name: 'Mittens', breed: 'Persian', age: 2, territoryId: 'T1' }],
            ['3', { id: '3', name: 'Shadow', breed: 'Black Cat', age: 5, territoryId: 'T2' }]
        ]);
        
        this.territories = new Map([
            ['T1', { id: 'T1', name: 'Garden Area', size: 500, climate: 'temperate' }],
            ['T2', { id: 'T2', name: 'Indoor Sanctuary', size: 200, climate: 'controlled' }]
        ]);
        
        this.activities = [
            { id: '1', catId: '1', type: 'hunting', duration: 45, timestamp: '2023-10-15T09:00:00Z' },
            { id: '2', catId: '1', type: 'napping', duration: 120, timestamp: '2023-10-15T11:00:00Z' },
            { id: '3', catId: '2', type: 'grooming', duration: 30, timestamp: '2023-10-15T10:30:00Z' },
            { id: '4', catId: '3', type: 'patrolling', duration: 60, timestamp: '2023-10-15T08:45:00Z' }
        ];
    }
    
    // Process GraphQL-style queries
    query(queryObject) {
        console.log("🔍 Processing GraphQL-style query");
        console.log("Query:", JSON.stringify(queryObject, null, 2));
        
        try {
            let result = this.resolveQuery(queryObject);
            return {
                data: result,
                errors: null
            };
        } catch (error) {
            return {
                data: null,
                errors: [{ message: error.message, type: error.constructor.name }]
            };
        }
    }
    
    resolveQuery(queryObject) {
        let result = {};
        
        for (let [fieldName, fieldQuery] of Object.entries(queryObject)) {
            switch (fieldName) {
                case 'cat':
                    result[fieldName] = this.resolveCat(fieldQuery);
                    break;
                case 'cats':
                    result[fieldName] = this.resolveCats(fieldQuery);
                    break;
                case 'territory':
                    result[fieldName] = this.resolveTerritory(fieldQuery);
                    break;
                case 'activities':
                    result[fieldName] = this.resolveActivities(fieldQuery);
                    break;
                default:
                    throw new Error(`Unknown query field: ${fieldName}`);
            }
        }
        
        return result;
    }
    
    resolveCat(catQuery) {
        let { id, fields } = catQuery;
        let cat = this.cats.get(id);
        
        if (!cat) {
            throw new Error(`Cat with ID ${id} not found`);
        }
        
        return this.selectFields(cat, fields, {
            territory: () => this.territories.get(cat.territoryId),
            activities: () => this.activities.filter(a => a.catId === cat.id),
            totalActivities: () => this.activities.filter(a => a.catId === cat.id).length,
            averageActivityDuration: () => {
                let catActivities = this.activities.filter(a => a.catId === cat.id);
                return catActivities.length > 0 
                    ? catActivities.reduce((sum, a) => sum + a.duration, 0) / catActivities.length
                    : 0;
            }
        });
    }
    
    resolveCats(catsQuery) {
        let { filters, fields, limit, orderBy } = catsQuery;
        let cats = Array.from(this.cats.values());
        
        // Apply filters
        if (filters) {
            if (filters.breed) {
                cats = cats.filter(cat => cat.breed.toLowerCase().includes(filters.breed.toLowerCase()));
            }
            if (filters.minAge) {
                cats = cats.filter(cat => cat.age >= filters.minAge);
            }
            if (filters.maxAge) {
                cats = cats.filter(cat => cat.age <= filters.maxAge);
            }
            if (filters.territoryId) {
                cats = cats.filter(cat => cat.territoryId === filters.territoryId);
            }
        }
        
        // Apply ordering
        if (orderBy) {
            cats.sort((a, b) => {
                let aValue = a[orderBy.field];
                let bValue = b[orderBy.field];
                let comparison = aValue < bValue ? -1 : aValue > bValue ? 1 : 0;
                return orderBy.direction === 'DESC' ? -comparison : comparison;
            });
        }
        
        // Apply limit
        if (limit) {
            cats = cats.slice(0, limit);
        }
        
        return cats.map(cat => this.selectFields(cat, fields, {
            territory: () => this.territories.get(cat.territoryId),
            activities: () => this.activities.filter(a => a.catId === cat.id),
            recentActivities: () => this.activities
                .filter(a => a.catId === cat.id)
                .sort((a, b) => new Date(b.timestamp) - new Date(a.timestamp))
                .slice(0, 3)
        }));
    }
    
    resolveTerritory(territoryQuery) {
        let { id, fields } = territoryQuery;
        let territory = this.territories.get(id);
        
        if (!territory) {
            throw new Error(`Territory with ID ${id} not found`);
        }
        
        return this.selectFields(territory, fields, {
            cats: () => Array.from(this.cats.values()).filter(cat => cat.territoryId === territory.id),
            catCount: () => Array.from(this.cats.values()).filter(cat => cat.territoryId === territory.id).length,
            averageCatAge: () => {
                let territoryCats = Array.from(this.cats.values()).filter(cat => cat.territoryId === territory.id);
                return territoryCats.length > 0 
                    ? territoryCats.reduce((sum, cat) => sum + cat.age, 0) / territoryCats.length
                    : 0;
            }
        });
    }
    
    resolveActivities(activitiesQuery) {
        let { filters, fields, limit, orderBy } = activitiesQuery;
        let activities = [...this.activities];
        
        // Apply filters
        if (filters) {
            if (filters.catId) {
                activities = activities.filter(activity => activity.catId === filters.catId);
            }
            if (filters.type) {
                activities = activities.filter(activity => activity.type === filters.type);
            }
            if (filters.minDuration) {
                activities = activities.filter(activity => activity.duration >= filters.minDuration);
            }
            if (filters.since) {
                activities = activities.filter(activity => 
                    new Date(activity.timestamp) >= new Date(filters.since)
                );
            }
        }
        
        // Apply ordering
        if (orderBy) {
            activities.sort((a, b) => {
                let aValue = a[orderBy.field];
                let bValue = b[orderBy.field];
                
                if (orderBy.field === 'timestamp') {
                    aValue = new Date(aValue);
                    bValue = new Date(bValue);
                }
                
                let comparison = aValue < bValue ? -1 : aValue > bValue ? 1 : 0;
                return orderBy.direction === 'DESC' ? -comparison : comparison;
            });
        }
        
        // Apply limit
        if (limit) {
            activities = activities.slice(0, limit);
        }
        
        return activities.map(activity => this.selectFields(activity, fields, {
            cat: () => this.cats.get(activity.catId)
        }));
    }
    
    selectFields(object, requestedFields, computed = {}) {
        if (!requestedFields || requestedFields.length === 0) {
            return object;
        }
        
        let result = {};
        
        for (let field of requestedFields) {
            if (typeof field === 'string') {
                // Simple field
                if (object.hasOwnProperty(field)) {
                    result[field] = object[field];
                } else if (computed[field]) {
                    result[field] = computed[field]();
                }
            } else if (typeof field === 'object') {
                // Nested field query
                let [fieldName, subQuery] = Object.entries(field)[0];
                if (computed[fieldName]) {
                    let subObject = computed[fieldName]();
                    if (Array.isArray(subObject)) {
                        result[fieldName] = subObject.map(item => this.selectFields(item, subQuery.fields, {}));
                    } else if (subObject) {
                        result[fieldName] = this.selectFields(subObject, subQuery.fields, {});
                    }
                }
            }
        }
        
        return result;
    }
}

// Demonstrate GraphQL-style flexible queries
console.log("\\n=== GRAPHQL-STYLE FLEXIBLE QUERIES ===");

let graphQLResolver = new CatGraphQLResolver();

// Query 1: Get specific cat with basic fields
console.log("\\n--- Query 1: Specific Cat Basic Info ---");
let query1 = graphQLResolver.query({
    cat: {
        id: '1',
        fields: ['id', 'name', 'breed', 'age']
    }
});
console.log("Result:", JSON.stringify(query1, null, 2));

// Query 2: Get cat with territory information
console.log("\\n--- Query 2: Cat with Territory Info ---");
let query2 = graphQLResolver.query({
    cat: {
        id: '1',
        fields: [
            'name', 
            'age',
            { territory: { fields: ['name', 'size', 'climate'] } },
            'totalActivities'
        ]
    }
});
console.log("Result:", JSON.stringify(query2, null, 2));

// Query 3: Get multiple cats with filtering and computed fields
console.log("\\n--- Query 3: Filtered Cats with Computed Fields ---");
let query3 = graphQLResolver.query({
    cats: {
        filters: { minAge: 2 },
        orderBy: { field: 'age', direction: 'DESC' },
        limit: 2,
        fields: [
            'name', 
            'age', 
            'breed',
            'averageActivityDuration',
            { recentActivities: { fields: ['type', 'duration', 'timestamp'] } }
        ]
    }
});
console.log("Result:", JSON.stringify(query3, null, 2));

// Query 4: Complex query with multiple root fields
console.log("\\n--- Query 4: Complex Multi-Root Query ---");
let query4 = graphQLResolver.query({
    cats: {
        filters: { breed: 'Tabby' },
        fields: ['name', 'age']
    },
    territory: {
        id: 'T1',
        fields: ['name', 'catCount', 'averageCatAge']
    },
    activities: {
        filters: { type: 'hunting', minDuration: 30 },
        orderBy: { field: 'timestamp', direction: 'DESC' },
        limit: 5,
        fields: ['type', 'duration', { cat: { fields: ['name'] } }]
    }
});
console.log("Result:", JSON.stringify(query4, null, 2));
```

## Paw-sible Problems

### Exercise 1: Design a Cat Adoption API
Create a comprehensive REST API for a cat adoption service.

```javascript
class CatAdoptionAPI {
    // Implement endpoints for:
    // - Browsing available cats (GET /api/cats)
    // - Cat profiles with photos (GET /api/cats/:id)
    // - Adoption applications (POST /api/applications)
    // - Application status tracking (GET /api/applications/:id)
    // - Adoption center locations (GET /api/centers)
    // Include proper validation, error handling, and documentation
}
```

### Exercise 2: Build a Real-time Cat Health Monitoring System
Create a WebSocket-based system for monitoring cat health in real-time.

```javascript
class CatHealthMonitoringSystem {
    // Implement real-time monitoring for:
    // - Vital signs (heart rate, temperature, activity)
    // - Location tracking within home/clinic
    // - Medication reminders and compliance
    // - Emergency alerts to veterinarians
    // - Historical data analysis and trends
}
```

### Exercise 3: Create a Cat Social Network API
Design an API for cats to "communicate" and interact digitally.

```javascript
class CatSocialNetworkAPI {
    // Features to implement:
    // - Cat profiles and friendships
    // - Territory sharing and boundaries
    // - Activity feeds and "meows" (posts)
    // - Play date scheduling
    // - Photo sharing with other cats
    // - Reputation and compatibility scoring
}
```

### Exercise 4: Implement a Cat Service Mesh
Build a microservices architecture for a comprehensive cat management system.

```javascript
class CatServiceMesh {
    // Design separate services:
    // - UserService (cat owners)
    // - CatService (cat profiles)
    // - HealthService (medical records)
    // - ActivityService (behavior tracking)
    // - NotificationService (alerts and reminders)
    // Implement service discovery, load balancing, and fault tolerance
}
```

### Exercise 5: Build a Cat IoT Device API
Create APIs for various cat-related IoT devices.

```javascript
class CatIoTDeviceAPI {
    // Support devices like:
    // - Smart feeders (scheduling, portion control)
    // - Litter box sensors (usage tracking, cleanliness)
    // - GPS collars (location tracking, geofencing)
    // - Health monitoring wearables
    // - Smart doors and access control
    // Implement device registration, data collection, and remote control
}
```

## Cat-ch the Pattern

### Key Takeaways

1. **APIs are contracts**: Like standardized cat communication protocols, they define how systems interact
2. **Consistency matters**: Predictable patterns make APIs easier to use and maintain  
3. **Error handling is crucial**: Graceful failure modes and clear error messages improve user experience
4. **Documentation is essential**: Clear documentation is like well-understood territorial markers
5. **Security and validation**: Always validate inputs and control access appropriately

### REST API Design Principles

**Use HTTP methods appropriately**:
- GET: Retrieve data (safe, idempotent)
- POST: Create new resources
- PUT: Update/replace entire resource
- PATCH: Partial updates
- DELETE: Remove resources

**Design intuitive URLs**:
```javascript
GET    /api/cats           // List all cats
GET    /api/cats/123       // Get specific cat
POST   /api/cats           // Create new cat
PUT    /api/cats/123       // Update cat
DELETE /api/cats/123       // Delete cat
GET    /api/cats/123/activities  // Cat's activities
```

**Use appropriate HTTP status codes**:
```javascript
200 OK              // Successful GET, PUT, PATCH
201 Created         // Successful POST
204 No Content      // Successful DELETE
400 Bad Request     // Invalid input
401 Unauthorized    // Authentication required
403 Forbidden       // Insufficient permissions
404 Not Found       // Resource doesn't exist
409 Conflict        // Resource already exists
422 Unprocessable   // Validation errors
500 Server Error    // Internal server problems
```

### API Response Patterns

**Consistent Response Structure**:
```javascript
// Success response
{
    "status": 200,
    "data": { /* actual data */ },
    "meta": {
        "timestamp": "2023-10-15T12:00:00Z",
        "version": "1.0.0"
    },
    "links": {
        "self": "/api/cats/123",
        "related": "/api/cats/123/activities"
    }
}

// Error response
{
    "status": 400,
    "error": {
        "code": "VALIDATION_ERROR",
        "message": "Invalid input data",
        "details": "Age must be a positive number"
    },
    "meta": {
        "timestamp": "2023-10-15T12:00:00Z",
        "requestId": "req-123-456"
    }
}
```

### Real-time Communication Patterns

**WebSocket Message Types**:
```javascript
// Connection management
{ "type": "welcome", "connectionId": "conn-123" }
{ "type": "ping" } / { "type": "pong" }

// Subscription management  
{ "type": "subscribe", "data": { "events": ["cat_location"] } }
{ "type": "unsubscribe", "data": { "events": ["cat_location"] } }

// Data events
{ "type": "event", "eventType": "cat_location", "data": { "catId": "123", "location": "kitchen" } }

// Commands and responses
{ "type": "command", "data": { "catId": "123", "action": "call" } }
{ "type": "command_result", "success": true, "data": { "executed": true } }

// Error handling
{ "type": "error", "message": "Unknown command", "code": "INVALID_COMMAND" }
```

### API Security Best Practices

```javascript
// Authentication and authorization
class APISecurityMiddleware {
    authenticateRequest(request) {
        let token = request.headers['authorization'];
        if (!token) throw new Error('Authentication required');
        
        let user = this.validateToken(token);
        if (!user) throw new Error('Invalid token');
        
        return user;
    }
    
    authorizeResource(user, resource, action) {
        let permissions = this.getUserPermissions(user);
        let required = `${resource}:${action}`;
        
        if (!permissions.includes(required)) {
            throw new Error('Insufficient permissions');
        }
    }
}

// Input validation
class APIValidator {
    validateCatData(data) {
        let errors = [];
        
        if (!data.name || data.name.trim().length === 0) {
            errors.push('Name is required');
        }
        
        if (!data.age || data.age < 0 || data.age > 25) {
            errors.push('Age must be between 0 and 25');
        }
        
        if (!['cat', 'kitten'].includes(data.type)) {
            errors.push('Type must be cat or kitten');
        }
        
        if (errors.length > 0) {
            throw new ValidationError(errors);
        }
    }
}

// Rate limiting
class RateLimiter {
    constructor(maxRequests = 100, windowMs = 60000) {
        this.maxRequests = maxRequests;
        this.windowMs = windowMs;
        this.clients = new Map();
    }
    
    checkRateLimit(clientId) {
        let now = Date.now();
        let client = this.clients.get(clientId) || { requests: [], blocked: false };
        
        // Remove old requests outside window
        client.requests = client.requests.filter(time => now - time < this.windowMs);
        
        if (client.requests.length >= this.maxRequests) {
            throw new Error('Rate limit exceeded');
        }
        
        client.requests.push(now);
        this.clients.set(clientId, client);
    }
}
```

### API Testing Strategies

```javascript
// API testing patterns
class APITester {
    async testCatAPI() {
        // Test successful creation
        let createResponse = await this.postJSON('/api/cats', {
            name: 'Test Cat',
            breed: 'Tabby',
            age: 3
        });
        assert.equal(createResponse.status, 201);
        assert.equal(createResponse.data.cat.name, 'Test Cat');
        
        // Test validation errors
        let invalidResponse = await this.postJSON('/api/cats', {
            name: '',  // Invalid empty name
            age: -1    // Invalid negative age
        });
        assert.equal(invalidResponse.status, 400);
        assert.isArray(invalidResponse.error.validationErrors);
        
        // Test resource not found
        let notFoundResponse = await this.getJSON('/api/cats/99999');
        assert.equal(notFoundResponse.status, 404);
        
        // Test rate limiting
        for (let i = 0; i < 101; i++) {
            let response = await this.getJSON('/api/cats');
            if (i === 100) {
                assert.equal(response.status, 429); // Too Many Requests
            }
        }
    }
}
```

### Looking Ahead

APIs enable different systems to communicate and share functionality, but how do we manage changes to these systems over time? In our next chapter, we'll explore version control - the practice of tracking and managing changes to code, much like how cats maintain memories of their territory and adapt to changes while preserving important knowledge about their environment.

We'll learn how to track changes, collaborate with others, and maintain the history of our projects, just as cats build and maintain their territorial knowledge over time.

---

*Next Chapter: [Version Control and Territory Memories](chapter-19-version-control-memories.md)*

*Previous: [Async Programming and Patient Waiting](chapter-17-async-and-patience.md)*