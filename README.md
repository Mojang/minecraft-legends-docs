# Minecraft Legends Documentation
This repository houses the documentation for Minecraft Legends. It will help you understand how the data is laid out and how to make your own mods for the game.

Watch this space as we're planning on releasing more documentation in the future. For now, you can check out the entry below which describes all of the behaviours available to entities in the game.

## Entities
From Piglins invading the overworld to allays gathering resources, all of the Minecraft Legends gameplay comes from entities. You can think of them as things that have behavior and may or may not have a visual representation. They are defined in JSON files and split across Behavior packs and Resource packs.

More documentation can be found on the [entities page](Entities.md).

### Event Triggers
[Event Triggers](EventTriggers.md) give entities life by allowing them to react to changes in gameplay state. Things like animations, audio, particles are driven through event triggers.

## Blockbench
[Blockbench](https://www.blockbench.net/) is a free tool used to create models and animations for Minecraft. We have created a new Minecraft Legends plugin for this tool that will be able to export these models and animations as JSON files for use in Minecraft Legends Resource Packs! Referencing these models and animations in Entity JSON definitions (Behavior Pack and Resource Pack) as well as animation and render controllers (Resource Pack only) allows users to easily create new visuals for their own custom enemies and allies. For information on how to use the new Minecraft Legends plugin, see our documentation [here](BlockBench.md)

## World Generation
All of the [biomes](Biomes.md) and interesting things in the world use locations determined by the [World Placement](WorldPlacement.md) system. It is the core of the procedural world generation system in Minecraft Legends and is the starting place for creating the whole world. These rules drive the placement of [blocks](Blocks.md) in the voxel world.

### Village Generation
Villages and Bases are key components to the campaign mode in Minecraft Legends. These locations are procedurally generated using the deck system which draws actions cards to create the final set of structures. This system is described on the [village generation page](VillageGeneration.md).

### Geology Service
[The Geology Service](GeologyService.md) is a system that lets us place special “geology” textures in the world. It allows us to generate terrain features that we could not easily achieve with our standard procedural terrain generation.

## Barrier Blocks
Structures in Minecraft Legends can use special variations of [barrier blocks](BarrierBlocks.md) to control how entities collide with them.

## BSharp
The bulk of the heavy lifting in the campaign mode is accomplished through our scripting language called BSharp. It is built on JavaScript and can do quite a lot of powerful things in the game. Check out the [BSharp Reference Sheet](BSharpReferenceSheet.md) for more information.
