# Minecraft Legends Event Triggers

Event Triggers give entities life by allowing them to react to changes in gameplay state. Things like animations, audio, particles are driven through event triggers.

## Animation

Set the animation state on the entity which has the `badger:presentation_event` component.

Example:

```json
"range_ability_a": {  
    "animation": "range_ability_a"  
}
```

## Audio

Play a sound effect at the position of the entity which has the `badger:presentation_event` component.

We can specify music to play by setting `is_music` to `true`. Music can also have a type (`"combat","cinematic","exploration","proximity"`). The reason for the type is that we can set audio parameters on music as well. Each type can have it's own set of parameters, but be careful and make sure to reset your music parameters per type when you want to use it.

Example:  

```json
"on_construction_start": {  
    "audio": "BAE_STRUC_Generic_BuildSequence"  
}
```

For music:  

```json
"on_piglin_combat_start": {  
    "audio": {  
        "event": "BAE_MUS_Piglin_Combat",  
        "is_music": true,  
        "music_type": "combat"  
    }  
},
```

## External Audio

Lists of Audio Presentation Events can be defined in separate files and then used in any entity by adding the `external_audio_events` type.

External Presentation Event lists are defined in files by using the `badger:external_audio_presentation_events` component.

Here is an example of the `on_hero_lured` presentation event being defined in a new JSON file inside the `badger:mob_skeleton` list (NOTE: The list name does not need to match the entity it will be used on):

```json
{
    "badger:external_audio_presentation_events" : {
        "badger:mob_skeleton": {
            "on_hero_lured":{
                "audio": "BAE_test_oneshot_3d"
            }
        }
    },
    "format_version" : "1.0.0"
}
```

External lists can then be referenced in an entity's `badger:presentation_events` component by using the `external_audio_events` type and specifying an `external_event_list` to be imported.

Here is an example of the `badger:mob_skeleton` list being imported into the `badger:mob_skeleton` entity's presentation events:

```json
   "badger:presentation_event": {
       "external_audio_events": {
           "external_event_list": "badger:mob_skeleton"
       },
       "intense_hit": {
           "audio": "BAE_mob_skeleton_dx_rps_notifies_intense_hit"
       }
    }
```

## Entity

Spawn a new entity on the client or server. The new entity's position and team is copied from the entity which has the `badger:presentation_event` component.

You can also spawn multiple entities with an array: `"entity": ["badger:mob_zombie", "badger:piglin_runt", "badger:mob_wolf"]`

Example:

```json
"on_world_hit": {  
    "entity": "badger:pres_entity_test"  
}
```

## Particles

Play a particle effect on the entity which has the `badger:presentation_event` component.

Example:

```json
"on_initialized": {  
    "particles": {  
        "effect": "fx_trail",  
        "locator": "loc_fx_tip",  
        "enabled": true,  
        "bind_to_actor": true  
    }

"on_world_hit": {  
    "particles": "fx_impact"
```

If you want to call more then one effect on a presentation event use this syntax:

```json
"on_construction_end": {  
    "particles": [  
        {  
            "effect": "badger:fx_creeper_death",  
            "enabled": true  
        },  
        {  
            "effect": "badger:fx_block_butterflies",  
            "enabled": true  
        }  
    ]  
}
```

## Script

Calls a script on the entity which has the  `badger:presentation_event` component.

Example:

```json
"event_scripts": {  
    "melee_attack_a": "variable.attack_used = 1;",  
    "melee_attack_b": "variable.attack_used = 2;"  
}
```

```json
"badger:presentation_event": {  
    "on_melee_attack_a": {  
    "script": "melee_attack_a"  
}
```

## Outline

Set the outline color on the entity which has the `badger:presentation_event` component.

Example:

```json
"on_lured": {  
    "outline": {  
        "color": 4, // an index into the outline colors array defined in PostFX.dragonfx (0 = None, 1 = Teammate, 2 = Enemy, 3 = Directed, 4 = Lured)
        "duration": 2.0 // the seconds to display the outline for before it disappears (don't define this field, or set it to -1.0 to make it last forever)
    }
}
```

## List of Triggers

This is a list of all of the current triggers in the game.

| Name | Description |
| --- | --- |
| `disabled_by_health` | The health of this entity has dropped enough to be disabled. |
| `on_advanced_direct_targeted` | Continuously applied when the entity is being targeted for attack using the Advanced Direct. |
| `on_aim_rotate_start` | Aiming has started rotating (redstone launcher). |
| `on_aim_rotate_stop` | Aiming has stopped rotating (redstone launcher). |
| `on_aim_selected` | Aiming has found a target (redstone launcher). |
| `on_aim_start` | Aiming has started (redstone launcher). |
| `on_aim_stop` | Aiming has stopped (redstone launcher). |
| `on_allay_deployed` | When an allay spawns from the player and begins flying to its target. |
| `on_allay_despawned` | When the allay reaches the player and disappears. |
| `on_allay_returning` | When an allay finishes its task and begins moving back to the player. |
| `on_allay_working` | When an allay reaches its target and begins working (gathering, deconstructing, or building). |
| `on_battle_view_exit` | Exiting banner view. |
| `on_battle_view_start` | Entering banner view. |
| `on_beat_1_received` | Music beat 1. Used for syncing events to the beat of the music. |
| `on_beat_2_received` | Music beat 2. Used for syncing events to the beat of the music. |
| `on_beat_3_received` | Music beat 3. Used for syncing events to the beat of the music. |
| `on_beat_4_received` | Music beat 4. Used for syncing events to the beat of the music. |
| `on_block_build` | Used when a block is placed in the world by an allay. |
| `on_block_gather` | Used when a block is gathered by an allay. |
| `on_block_layer_converted` | When a block has been converted. |
| `on_block_swap_start` | When a building's blocks start being converted by a block swap modifier (ie. the mason). |
| `on_block_swap_stop` | When a building's blocks finish being converted by a block swap modifier (ie. the mason). |
| `on_buff_utilize_start` | When an entity starts getting buffed (like the carpenter hut). |
| `on_buff_utilize_stop` | When an entity stops getting buffed.|
| `on_build_preview_exit` | When building placement preview stops. |
| `on_build_preview_start` | When building placement preview starts. |
| `on_building_deconstructed` | When deconstructing an existing structure. |
| `on_building_destroyed` | When a structure is destroyed (not despawned). |
| `on_building_failed` | When attempting to place a structure without meeting its requirements. |
| `on_building_placed` | When a structure is placed. |
| `on_building_start` | When a structure begins to construct. |
| `on_building_stop` | When a structure finishes constructing. |
| `on_damage_applied_to_structure_0` | When building enters first damage state. |
| `on_damage_applied_to_structure_1` | When building enters second damaged state. |
| `on_damage_applied_to_structure_2` | When building enters third damaged state. |
| `on_death` | When an entity dies, not necessarily destroyed at this time. |
| `on_destroyed` | When an entity is destroyed. |
| `on_disband` | When an entity hits the popcap limit and is marked for death. |
| `on_entity_hit` | When a projectile collides with another entity. |
| `on_gate_close` | When a gate closes. |
| `on_gate_open` | When a gate opens. |
| `on_healing_applied_to_structure_0` | When building leaves first damage state. |
| `on_healing_applied_to_structure_1` | When building leaves second damaged state. |
| `on_healing_applied_to_structure_2` | When building leaves third damaged state. |
| `on_initialized` | When any entity is spawned, this trigger is fired. |
| `on_interact_interrupt` | When damage is taken during a long interact, such as opening zombie cages, and the action is interrupted. |
| `on_item_collected_arrived` | When a collectable item on the ground reaches the player. |
| `on_item_collected` | When a collectable item starts being collected. |
| `on_liquid_deep_enter` | Entering deep (ocean) liquid. |
| `on_liquid_deep_exit` | Exiting deep (ocean) liquid. |
| `on_liquid_shallow_enter` | Entering shallow (river) liquid. |
| `on_liquid_shallow_exit` | Exiting shallow (river) liquid. |
| `on_mount_swap` | When the player swaps a mount. |
| `on_mounted` | When the player rides a mount. |
| `on_recall_complete` | Recalling mobs has completed. |
| `on_recall_interrupt` | Recalling mobs was interrupted. |
| `on_recall_start` | Recalling mobs starts. |
| `on_recall_stop` | Recalling mobs stops. |
| `on_spawn_batch` | Called when spawning a batch of mobs from a spawner. |
| `on_spawn_cap_reached` | Called when your spawn cap has been reached. |
| `on_stamina_full` | Triggers once when a mount depletes some of its stamina, and then refills it. |
| `on_stretch_foundation_placed` | When a stretch preview is initially placed. |
| `on_throwable_landed` | When a throwable has hit the ground. Entities being knocked back are in a throwable state. |
| `on_throwable_launched` | When a throwable starts its trajectory. Entities being knocked back are in a throwable state. |
| `on_unmounted` | When the player leaves a mount. |
| `on_world_hit` | When a projectile collides with the world |
| `reticle_alternate_effect_start` | The state where the reticle turns red when selecting a target with advanced direct or redstone launcher. |
| `reticle_alternate_effect_stop` | The state where the reticle turns red when selecting a target with advanced direct or redstone launcher. |
