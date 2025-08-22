# UCTools Utilities

A collection of Unity C# utility scripts designed to simplify common development patterns and improve code organization. These utilities provide reusable solutions for coroutine management, event handling, and singleton patterns.

## Contents

- **Coroutines** - Static coroutine management for non-MonoBehaviour classes
- **EventRegistry** - Automatic UI Toolkit event registration and cleanup system  
- **Singleton** - Generic singleton pattern implementation for MonoBehaviours

## Quick Start

1. Copy the scripts to your Unity project under a `Scripts/Utilities` folder
2. Add the `UCTools_Utilities` namespace to your using statements
3. Follow the usage examples below for each utility

## Usage Guide

### Coroutines Utility

Allows running coroutines from non-MonoBehaviour classes like ScriptableObjects or regular C# classes.

```csharp
// Initialize once with any MonoBehaviour (typically in a manager)
Coroutines.Initialize(this);

// Start coroutines from anywhere
var routine = Coroutines.StartCoroutine(MyCoroutine());

// Stop coroutines
Coroutines.StopCoroutine(routine);
```
**Pros:**
- Enables coroutines in non-MonoBehaviour classes
- Clean static API
- Lightweight implementation

**Cons:**
- Requires initialization with a MonoBehaviour
- Single point of failure if runner is destroyed
- No built-in error handling

### EventRegistry Utility

Manages UI Toolkit event registration with automatic cleanup to prevent memory leaks.

```csharp
// Create registry
EventRegistry eventRegistry = new EventRegistry();

// Register events with full event data
eventRegistry.RegisterCallback<ClickEvent>(myButton, (evt) => 
    Debug.Log($"Clicked at: {evt.mousePosition}"));

// Register simple events without event data
eventRegistry.RegisterCallback<ClickEvent>(myButton, () => 
    Debug.Log("Button clicked!"));

// Register value change events
eventRegistry.RegisterValueChangedCallback<float>(mySlider, (newValue) => 
    Debug.Log($"Slider value: {newValue}"));

// Clean up all events (call in OnDisable)
eventRegistry.Dispose();
```
**Pros:**
- Automatic memory leak prevention
- Clean disposal pattern
- Supports multiple event types
- Simplified callback registration

**Cons:**
- Additional abstraction layer
- Requires manual disposal
- UI Toolkit specific


### Singleton Utility

Generic singleton pattern for MonoBehaviours with automatic GameObject creation.

```csharp
public class GameManager : Singleton<GameManager>
{
    protected override void OnAwake()
    {
        // Your initialization code here instead of Awake()
    }
    
    public void DoSomething()
    {
        Debug.Log("GameManager doing something!");
    }
}

// Access from anywhere
GameManager.Instance.DoSomething();
```

**Pros:**
- Thread-safe lazy initialization
- Automatic GameObject creation
- DontDestroyOnLoad handling
- Clean inheritance pattern

**Cons:**
- Global state (can make testing difficult)
- Tight coupling
- Potential race conditions during initialization

  
## When to Use These Systems

### Good Use Cases:
- **Coroutines**: ScriptableObject-based systems, data managers, non-MonoBehaviour singletons
- **EventRegistry**: UI-heavy applications, editor tools, complex UI with many interactive elements
- **Singleton**: Game managers, audio managers, save/load systems, settings managers

### Avoid When:
- **Coroutines**: Simple MonoBehaviour scripts (use built-in coroutines instead)
- **EventRegistry**: Simple UIs with few events, temporary/short-lived UI elements
- **Singleton**: You need multiple instances, testing scenarios, or when dependency injection is preferred

## 📝 Attribution

Some scripts in this collection are based on or modified from publicly available Unity community resources and projects. The core patterns have been adapted and enhanced for specific use cases:

- Singleton pattern based on common Unity community implementations
- EventRegistry inspired by UI Toolkit best practices and community solutions
- Coroutines utility adapted from various Unity forums and documentation examples

I think most stuff came from the UI Toolkit 'Quiz U' project, but if I have made a mistake then please let me know.

All modifications have been made to improve usability, add features, or fix potential issues in the original implementations.

## 📄 License

This project is provided as-is for educational and development purposes. Feel free to modify and use in your projects.

## 🤝 Contributing

Feel free to submit issues, feature requests, or pull requests to improve these utilities.
