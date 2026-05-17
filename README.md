# Ghoot (Game Jam Project)

A 3D stealth/puzzle-action game built with Unity where you play as a ghost whose goal is to haunt and scare NPCs by possessing everyday furniture.
Made for PESU Altverse 2025 (GameJam hosted by G Cube PES University RR Campus)

## Gameplay Mechanics

### Possess & Haunt
- **Ghost Form**: Roam the world as a ghost. When you get close to interactable furniture, it highlights.
- **Possession**: Press `Space` to possess the highlighted furniture. You hide inside it.
- **Scare Tactics**: While possessing an object, press `P` to release a terrifying scare. Any NPCs within your scare radius will lose **Sanity**.

### NPC AI & Sanity System
- NPCs wander around their environment dynamically using Unity's NavMesh.
- Every NPC has a **Sanity meter**.
- When you scare them, their sanity drops, and they will run away in terror.
- If you deplete their sanity entirely, they are "scared to death" (removed from the game).

## Controls

- **Move**: Standard movement controls (WASD).
- **Space**: Possess / Un-possess furniture.
- **P**: Scare nearby NPCs (only works when possessing furniture).

## Built With
- **Engine**: Unity 3D
- **Language**: C#

## Technical Implementation Details

### NPC Artificial Intelligence (AI)
- **NavMesh Pathfinding**: The game uses Unity's built-in NavMesh and NavMeshAgent. NPCs dynamically find paths around obstacles (like furniture) to reach their random destinations.
- **State Machine**: NPC logic is built on a state machine pattern driven by time and distances.
  - *Wandering*: NPCs pick a random position within a set walk radius and move towards it.
  - *Fleeing (Scared)*: When triggered by a scare, they halt their current path, rotate away from the possessed furniture, and pick a new destination within a larger "Run" radius to flee the area.
- **Animations**: The AI seamlessly crossfades between Idle, Walk, Run, and Scared states via the Animator component.

### Player Movement (Ghost)
- **Character Controller**: The ghost's movement is handled by a standard Unity CharacterController.
- **Locked Axis & Floating**: The player's vertical movement is locked to maintain a consistent "floating" height, while WASD provides planar movement along the horizontal axes.
- **Possession Mechanics**: When possessing an object, the ghost's visual renderer is disabled and the CharacterController is deactivated. The camera focus shifts, and the furniture script takes over, running a local trigonometric "wiggle" algorithm (Yaw/Pitch/Roll clamped rotation) to simulate a haunted, rattling object.

### Interaction & Highlighting
- **Distance Checking**: Furniture items check their distance relative to the ghost's position. When the ghost is within the interactionRadius, the furniture becomes active.
- **Material Swapping (Highlight)**: Rather than an expensive post-processing outline shader, highlighting is handled efficiently by swapping the MeshRenderer's default materials with a Highlight.mat (a custom URP material). When the ghost moves away, the original materials are instantly restored.

### Map Design
- **Indoor Environment**: Set in a residential layout containing modular rooms.
- **Dynamic Obstacles**: Furniture objects aren't just static props, they act as both dynamic NavMesh obstacles (which NPCs must walk around) and possessable vessels for the player.
- **Dynamic Initialization**: Instead of manually assigning scripts to every prop, initializeFurniture.cs runs on Awake(), dynamically injecting the scripthaunt logic and necessary materials into every valid piece of furniture in a provided list.
