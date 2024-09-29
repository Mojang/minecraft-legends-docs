# B# Reference Sheet

## Core `B#`

### Listeners

Listeners are how we listen to specific game events, and direct those events to snippets when fired.

```jsx
LISTENFOR_EntitySpawned({
    snippet: "es_snippet_to_execute",
    ownerVillageId: OWNER_VILLAGE_OPT_OUT,
    includeTags: ["mob"],
    payloadString: "hello"
})
```

- `snippet` : required for all listeners, this defines the snippet to execute.
- `ownerVillageId` : required for all listeners, this defines the village that owns this listener. See section Villages below.
- `includeTags` : a listener rule for this type of listener. Each listener accepts a different set of rules (sometimes required) that determine if the event should trigger the snippet.
- `payloadString` : a payload to forward to the snippet when it executes. See section Payloads below.

### Snippets

Snippets are your “functions” or logic to execute when a listener triggers.

```jsx
SNIPPET_EntitySpawned("es_snippet_to_execute", (entity, payload) => {
    // do stuff here!
})
```

- `"es_snippet_to_execute"` : First parameter is the snippet name. Listeners with the same `snippet` value will trigger this snippet.
  - It is possible to have multiple listeners point to a single snippet, but not the other way around (when a snippet is defined its name must be unique).
- `(entity, ...)` : Each snippet will be provided a set number of arguments to operate on. See the documentation for more details on each snippet type.
- `(..., payload)` : The last argument for any snippet is always the payload.

### Bootstraps

Are a special classification of snippets that execute when the game starts in some way. They are special as they do not require a listener to be executed. Generally the snippet name is used as a listener rule instead.

There are 4 bootstrap snippets, taken from the documentation itself:

```jsx
/**
 * Logic to perform when the server begins.
 * No listener required. All bootstrap snippets will fire at the beginning of a new game.
 */
declare function SNIPPET_Bootstrap(snippetName: string, callback: BootstrapCallback): void
declare type BootstrapCallback = () => void

/**
 * Logic to perform when the server begins.
 * No listener required. All bootstrap snippets will fire at the beginning of a new game.
 * This snippet runs at the start of EVERY game, even after save-load.
 */
declare function SNIPPET_GameLoadBootstrap(snippetName: string, callback: GameLoadBootstrapCallback): void
declare type GameLoadBootstrapCallback = (isSaveLoaded: boolean) => void

/**
 * Similar to Bootstrap. The name of the snippet should match the rule that is being reacted to.
 * Passes along the value of the rule as a parameter.
 * No listener required. All rule initialized snippets will fire at the beginning of a new game.
 */
declare function SNIPPET_RuleInitialized(snippetName: string, callback: RuleInitializedCallback): void
declare type RuleInitializedCallback = (ruleValue: VariantData) => void

/**
 * Similar to Bootstrap. The name of the snippet should match the game mode this is being reacted to.
 * No listener required. All rule initialized snippets will fire at the beginning of a new game.
 */
declare function SNIPPET_InheritsFromGameMode(snippetName: string, callback: RuleInitializedCallback): void
declare type RuleInitializedCallback = () => void
```

In general, this is when you would use this specific bootstrap:

- `SNIPPET_Bootstrap` when you need shared “core” functionality between all game modes. **In general this shouldn’t be used**.
- `SNIPPET_InheritsFromGameMode` when you need behavior specific to a game mode. **This should be your default go to.**
- `SNIPPET_RuleInitialized` when you want to enable modular behavior defined by game-rules, the idea being it is functional across multiple game modes.
- `SNIPPET_GameLoadBootstrap` used in special circumstances when you need to do something on game reload (from a save file). The intended use for this is for DLC islands, when we need to add content after a world has already been generated.

### Callbacks

Another class of special snippets that don’t use a listener. Instead some`B#`functions (OUTPUTS) when executed can specify a callback as the last parameter, which will execute the associated snippet when relevant.

There are 2 callbacks, taken from the documentation itself:

```jsx
/**
 * Logic to execute when an entity has arrived or failed to reach its destination.
 * ex. **OUTPUT_MoveEntity(..., "mc_retreat_after_reaching_destination")**
 * 
 * @param snippetName The snippet name.
 * @param callback The callback to call.
 */
declare function SNIPPET_MoveCallback(snippetName: string, callback: MoveCallbackCallback): void
declare type MoveCallbackCallback = (moverEntityGroup: number, successfullyArrived: boolean, payload: SnippetPayload) => void

/**
 * Use `SpawnedBuildableCallback(snippetName)` instead!
 * ex. **OUTPUT_SpawnBuildableAt(..., "sbc_spawn_guarding_mobs")**
 *
 * @param snippetName The snippet name.
 * @param callback The callback to call.
 */
declare function SNIPPET_SpawnedBuildableCallback(snippetName: string, callback: SpawnedBuildableCallbackCallback): void
declare type SpawnedBuildableCallbackCallback = (completedStructure: number, payload: SnippetPayload) => void
```

### Payloads

Payloads are supported for all listeners and snippets. It allows you to past contextual information at the time of registering the listener, to when the snippet is executed.

- Listeners can pass **one of each** payload type via `payloadString`, `payloadInt`, `payloadFloat`, `payloadEntities`
- Snippets can receive the payload **as the last argument of the snippet** via `payload.string`, `payload.int`, `payload.float`, `payload.entities`
  - You can also use `payload.ownerVillageId` which will be defined if the listener defined `ownerVillageId`. (This is the preferred way of getting the village in a snippet when possible)

### Villages & Suspension

A core concept in Minecraft Legends (not just B#) are villages. You can think of villages as a “Location” - an area in the game world which can house entities such as buildings, mobs, or even invisible things (like music emitters, trigger volumes, etc.).

Suspension is the act of saving out an entity and having it enter a state of stasis due to our massive open world which makes keeping everything loaded infeasible. Special care has been made sure to ensure`B#`only ever works with fully loaded active entities (so all the information on the entity will be on as you expect).

Villages are crucial to`B#`as they can guarantee what entities are queryable and useable at the time of your snippet execution. **When a listener that belongs to a village executes it is guaranteed that all entities belonging to the same village will also be loaded**.

- For example: querying the number of structures in a village when a village owned listener executes is safe.

**Note when a village is suspended, your listener will also be suspended.**

- For example: listening for a global variable is generally not safe in a village owned listener as the village may be unloaded.

The mechanism that drives this group un/suspension is called Atomic Village Suspension which you may hear BSharpies refer to.

### Entity Groups, Entities, and Entity

An Entity Group (EG) is how we represent an entity or entities in B#. It is a special data type, don't ask about it.

Note some functions require an entity group of one and only one (referred to as "**entity**" in the documentation).

Conversely, some functions require a group of entities (referred to as “**entities**” in the documentation) which includes a group of 0 entities or more.

### Queries

Queries are how we read game state - without mutating it - such as getting the count in an entity group.

All queries are prefixed with `QUERY_`

### Outputs

Outputs are how we “output” changes to game state such as spawning a new entity.

All outputs are prefixed with `OUTPUT_`

### Filters

Filters are how we filter down an entity group into something of the same size or smaller. For example returning all the entities within range of another entity.

All filters are prefixed with `FILTER_`

### Operations

Operations are how combine entity groups into another group. For example doing the union of two groups (combine with no duplicates). If you consider an EG as a set of entities, there is an operation function for each mathematical set operation.

All operations are prefixed with `OPER_`

### Global Variables

B# is largely stateless, we listen to events and respond to them "right away". However, if you do need to store some information you can use our global variable storage via commands such as `OUTPUT_SetGlobalVariable(key, integerValue)` allowing you to store integer values to retrieve at a later time.

Please **DO NOT** store variables like this `var thing = 123` at the global scope. Values like this still be lost to the void when the player exits and reloads.

## **Advanced topics**

These topics dive a little deeper into the nuance of our game, how we author scripts, as well as functions specific to game-modes like campaign.

### Configuration objects / files

Located at the top of scripts generally, are configuration objects which only contain “data” (and no logic). This presents an easy way to tune the functionality of a script as all the tunable values are grouped in one location.

**Notes**:
You may notice special config objects existing in separate files entirely by themselves, with *another* script utilizing the config elsewhere.
For example imagine `const gameConfig = {}` moved to a `a_new_game_mode_config.js` file. This is generally done for two reasons

- This allows developers to completely override the config (including removing it!) for separate game-modes, such as user-created `Lost Legends` and `Myths`!
- Separates massive config files to shorten scripts, this is especially useful if multiple configs exists (e.g. `a_config_horde_attack.js`, `a_config_horde_defend.js`) which makes tracking changes much easier.
- Modularity is always nice to have!

### Naming Conventions

When a constant or function at the global scope is prefixed with `_` it means this constant/function is **only** used within this file.

Take, for example:
`_SpawnAttackingUnits = () => { // return stuff }`

This provides an easy way to *know* if changing a function’s behavior could affect other scripts.

Mojang note on semantics:
To be clear this is purely used for semantics - all scripts are loaded at the global level meaning you *can* reference other file’s constants (but you shouldn’t!). If you get a ‘x declared already’ error this is most likely why!
When `_` is used in a function though, it means the **function argument is not used**. For example `SNIPPET_EntitySpawned(..., (_spawnedEntity))` - if you don’t actually need to operate on the spawned entity in anyway.

### File Naming Conventions & Load Order

The file *path* (that includes the folder name!) determines the load order, sorted alphabetically. This is why we have some files named prefixed with `aaa_` to guarantee it loads first - this is used exclusively for files with helper functions, constants, and config files.

### Helper functions

Helper functions (local to a file or global to all) are ways to execute common functionality repeatedly. If you notice yourself duplicating scripts or doing something very similar make a helper function or reach out to fellow developers for assistance!

In the wonderful and mythical world of `B# land` we define functions like so:

```jsx
// or MyLocalFunction if you wanted it to be global!
const _myLocalFunction = (argument1, argument2) => {
   // do stuff!
}

```

If you have used Javascript before then you may be more familiar with this syntax:

```function _myLocalFunction(argument1, argument2) => { }```

We don’t do this because you can redefine and *stomp* an existing function - which leads to really hard to catch bugs!

### Special “Libraries”

`B#` has some special libraries fully*(some things do rely on the C++ backend) defined in script, utilizing all of `B#` to do neat things.

For example

- `DECK_` : Used to drawn and set`B#`decks (this is a special case and is not *fully* scripted)
- [DEPRECATED] `RALLYMAN_` : Used to group and dispatch units.
- `aaaa_ai_helpers.js` : Supersedes above, and unfortunately doesn’t have a prefix. Used to interface with base AI.

### Entity Destruction Listeners

Due to the massive open world nature of Minecraft Legends, we’ve had to introduce several destruction listeners each with their own nuance. Here’s an overview of all 3.

|  | Guaranteed to fire | Provides destroyed entity | Valid entities | Intended usecase |
| --- | --- | --- | --- | --- |
| NonPopCappedEntityDestroyed | Yes | Yes | Non pop cap entities (entities that can only be destroyed while unsuspended). Eg. Structures and bosses. | When you want to do critical work based off an entity that was destroyed. (eg. progression beats) |
| PopCappedEntityDestroyed | No | Yes | Any entity. | When you want to do non-critical work based off an entity that was destroyed. (eg. play effects) |
| EntitiesAmountDestroyed | Yes | No | Any entity. | When you care about amount of entities destroyed OR need to guarantee your snippet triggers. |

### Random Number Generation

Avoid the standard library's `Math.random()` as it is not deterministic between instances of the game. Stick to `QUERY_RandomNumber` and `QUERY_RandomNumberGroup`.

The `Number` returned by `QUERY_RandomNumber` and `QUERY_RandomNumberGroup` is an integer and can immediately be used as an array index.

Consider using `QUERY_RandomNumberGroup` for consistent randomness that you do not want to be affected by other areas of the game (eg: the player affecting the state of the game world).
