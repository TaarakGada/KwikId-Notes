# Fluid

## What is it?
- **Fluid** (also known as Fluid Framework) is a collection of technologies developed by Microsoft for **real-time collaboration** in web apps
- Enables multiple users to see and edit the same data simultaneously — like Google Docs for any app
- Uses **Distributed Data Structures (DDS)** that sync automatically between clients

## Core Idea
- Instead of sending events ("user typed X"), you work with shared data structures
- The framework handles merging changes from multiple users automatically
- Uses **CRDT** (Conflict-free Replicated Data Types) and **Operational Transforms** internally

## Key Concepts

### Fluid Container
- The shared document/state that all clients connect to

### Distributed Data Structures (DDS)
- Shared objects that automatically sync across clients
- Types: `SharedMap`, `SharedString`, `SharedArray`, `SharedCounter`

### Fluid Service (Relay Service)
- A server that synchronizes state between all clients
- Can use: Azure Fluid Relay (managed) or Routerlicious (self-hosted)

## Basic Example
```javascript
import { SharedMap } from '@fluidframework/map';
import { AzureClient } from '@fluidframework/azure-client';

const client = new AzureClient({ connection });

// Create or load a Fluid container
const { container } = await client.createContainer(containerSchema);

// Get the shared map
const sharedData = container.initialObjects.myData;

// Set a value — automatically synced to ALL clients
sharedData.set('status', 'active');

// Listen for changes from other clients
sharedData.on('valueChanged', (changed) => {
  console.log(`Key "${changed.key}" changed to:`, sharedData.get(changed.key));
});
```

## Use Cases
- Collaborative whiteboards
- Real-time form editing (multiple agents editing a form)
- Live dashboard data shared among team members
- Co-authoring documents
- Multiplayer features in web apps

## Good to Know
- Originally built for Microsoft Office (Teams, Loop)
- Works with Azure Fluid Relay service for production
- Can self-host the relay service (open source)
- Lower level than something like Liveblocks — more flexible but more setup
