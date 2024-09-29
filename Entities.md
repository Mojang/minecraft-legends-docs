# Client Entity Documentation

Client entity definitions are contained within a Resource Pack and contain all of the information necessary to describe how an entity should look (model, texture, animation) and sound. To create your own, start by navigating to the "entity" folder inside the root of the Resource Pack. In the entity folder create a JSON file and give it a name. The JSON file needs a format version and minecraft:client_entity information.

The minecraft:client_entity section contains the description for the entity. Under description there are a number of things that you can set about an entity.

**Example client entity definition JSON for the prismarine item drop**:

```json
{
  "format_version": "1.10.0",
  "minecraft:client_entity": {
    "components": {
      "badger:rendering": {
        "definition": "badger:item_prismarine",
        "static": true
      },
      "badger:presentation_event": {
        "on_item_collected": {
          "audio": "BAE_hero_pickup_prismarine"
        },
        "on_item_collected_arrived": {
          "audio": "BAE_hero_pickup_prismarine_stop"
        }
      },
      "badger:template": ["badger:template_item_orb"]
    },
    "description": {
      "enable_attachables": false,
      "geometry": {
        "default": "geometry.item_prismarine"
      },
      "identifier": "badger:item_prismarine",
      "materials": {
        "default": "entity_model",
        "mat_item_prismarine": "mat_item_prismarine"
      },
      "render_controllers": ["controller.render.item_prismarine"]
    }
  }
}

```

## Entity Description Properties

### badger:animal_interaction

Handles animal interaction information.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| duration| | How long the animal interaction should last in seconds |
| String| molang_variable| | The molang variable to toggle the pat pat animation on both the animal and the player |
| String| trigger_event| | The presentation event to trigger when the interaction occurs |

### badger:animation_variation

Settings to determine which animation is played based on which interval an action occurs within

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Array| intervals| | The set of intervals used to determine the molang index based on the last time an action was performed. |
| String| molang_index_name| | The name of the molang used by animation for a given set of intervals. |

### badger:area_overlays

Defines any visual overlay effects applied to this entity, such as auras and attack range circles.

**Area Overlays**:

| **Type** | **Name** | **Description** |
|----------|----------|-----------------|
| **List** | **auras** | Defines a list of aura-type overlays. |
| **List** | **circles** | Defines a list of circle-type overlays. |

| **Attributes**      | **Description**                   | **Default Value** | **Valid Values**                     |
|---------------------|-------------------------------|-------------------|--------------------------------------|
| filter_team          | When true, the overlay is only visible to allied players.          | _Unknown_             | true, false                          |
| views                | Defines when the overlay should be visible.            | gameplay          | gameplay, placement                  |
| layer                | Defines what layer the aura is rendered to.                | aura              | zone, aoi, aura, overlay             |
| size                 | Size of the overlay, in blocks.                    |                   | Any positive integer                 |
| type                 | Defines the type of overlay, which affects when and how it will be rendered.                 | default           | default, aura, buff, culture, tower, highlight |
| uses_zone_size       | When true, the overlay expands to match the entity's zone of effect.       | _unknown_             | true, false                          |

---

### badger:attacker_damage_prediction

Defines presentation behaviour for when the entity anticipates performing an attack that deals damage of a certain hit reaction threshold.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| String| apply_buff| | The buff applied to the entity about to perform an attack that deals damage of a certain hit reaction threshold. |
| String| hit_reaction_type| | The hit reaction threshold which applies the buff. |

### badger:audio_beat_sync

This component allows an entity to trigger presentation events based on beats or marker triggers from the music.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Boolean| receive_only_markers| | If this is true, this entity will not be sent any Beat callbacks, only Marker callbacks. |

### badger:audio_block_scanning

This component provides information for the block scanning system for which biomes to look for and which audio events to play when it finds it.

| **Type**    | **Name**          | **Description** |
|-------------|-------------------|-----------------|
| **Array**   | **biomes**        | Array of all the biomes this block scan will look for. |
|             | biome_name        | The name of the biome the block scan is looking for. Type: _string_ |
|             | fmod_event        | The name of the FMOD Event that will start when the biome is found. It's recommended to use a looping sound. Type: _string_ |
|             | liquid_only       | When this boolean is true, only liquid-type blocks will be valid when detecting a biome type. Type: _boolean_ |
| **Integer** | **max_step_dist** | The maximum distance in blocks that this block scanner will go. |
| **Integer** | **min_step_dist** | The minimum distance in blocks that this block scanner will go. |

---

### badger:audio_interactable

This flags an entity to apply attributes about the audio event that should be played when the player comes into interaction range with this interactable entity and is prompted to interact with it.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| String| enter_range_audio_event| | The name of the audio event that should be played when the player comes into interaction range. |

### badger:audio_molang_to_fmod

This flag component indicates that the entity will update any specified Molang to FMOD audio parameters, these molang parameters are not data driven due to expensive molang calls.

### badger:audio_time_of_day

This flags an entity to apply an FMOD event parameter for 'time_of_day'.

### badger:audio_update_FMOD_projectile_velocity

This flags an entity so its velocity will be sent to FMOD for all of the audio events it triggers.

### badger:audio_vo_sequence_speaker_tag

This flags an entity to apply attributes about a tag that will allow the VO Sequence System to play 3D audio at it's position.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| String| speaker_tag| | A tag for this entity so that VO sequence audio may be positioned on it. |

### badger:beacon_render_info

Provides a beacon on a village entity

| **Type**  | **Name**          | **Description** |
|-----------|-------------------|-----------------|
| **List**  | **states**        | A list of beacon states this entity can have. By default, the first state is active. |
|           | beacon_color      | Base color to be multiplied onto the beacon texture. (String) |
|           | beacon_width      | Width of the beacon. (Integer) |
|           | emissive_scale    | Flat value to scale the emissiveness of the beacon - Default is tuned for near-camera beacons. (Float) |
|           | max_distance      | Maximum distance after which this beacon is no longer rendered. (Integer) |
|           | meta_material     | Meta material name used to render the beacon. (String) |
|           | min_distance      | Minimum distance before this beacon is no longer rendered. (Integer) |

---

### badger:block_source_cull_distance_override

Adds a distance override for the block source culling distance for this entity

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| distance| | Override to the maximum distance from the player at which the block source culling occurs |

### badger:block_source_info

Allows the entity to track the block source information for the current block it's on. This is to detect things like, biome data, water, on the ground, etc...

### badger:block_source_removal

Remove the BlockSourceComponent on the client because this entity doesn't need one.

### badger:boom_camera

Badger's boom camera.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| boom_length| | Length of the camera boom. |
| Decimal| control_react_spring| | How quickly the camera reacts when the target starts moving. |
| Decimal| control_stop_spring| | How quickly the camera reacts to the target stopping. |
| Decimal| follow_spring| | The amount of spring the camera has when following the target. |
| Decimal| max_follow_distance| | Maximum distance the camera can be from the follow target. |
| Decimal| max_pitch| | Maximum pitch of the boom camera. |
| Decimal| min_pitch| | Minimum pitch of the boom camera. |
| Decimal| starting_pitch| | The initial pitch of the boom camera. |
| Decimal| starting_relative_yaw| | The initial yaw of the boom camera. |
| Decimal| target_vertical_offset| | Vertical offset from the camera's follow target. |

### badger:cam_overrides_look_orientation

Attach to a camera to indicate whether a camera overrides the direction the player looks when they move

### badger:cam_overrides_move_orientation

Attach to a camera to indicate whether a camera inhibits the player from moving

### badger:camera_avoidance

Badger's main camera avoidance logic.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| camera_disk_radius| | Camera disk radius to use to move camera for avoidance. |
| Decimal| camera_in_geometry_radius| | Radius of sphere around camera to determine if it is in world geometry. |
| Decimal| distance_from_obstacle| | Distance from object impeding the camera when avoidance is triggered |
| Decimal| ease| | Easing lerp to get camera back to normal non-avoidance position |
| Decimal| min_distance_from_target| | Minimum distance camera  is allowed to be within of target when avoiding. |
| Decimal| snap| | Snapping lerp to get camera to avoidance position |
| Decimal| target_disk_radius| | Target disk radius to use to move camera for avoidance. |
| Decimal| target_vertical_offset| | Vertical offset from target that should use as anchor avoidance/follow point. |

### badger:camera_bounds

Describes how and when a camera makes rotational and/or positional changes

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| hard_bounds_cone_angle| | Angle for the spherical cone that will determine the width of non-movement of the camera. |
| Decimal| look_priority| | Priority percentage given to the look target in comparison to the follow target when there is a follow target. If there is no follow target, the look target takes full priority. Value between [0.0f, 1.0f], inclusive. If 0.5f priority given camera will focus on a spot halfway between the look and follow target. If 1.0f priority given camera will focus on the look target. |
| Decimal| look_smoothing_spring| | Smoothing Spring constant determining how long the spring takes to get the camera to the appropriate orientation. |
| Decimal| position_smoothing_spring| | Smoothing Spring constant determining how long the spring takes to get the camera to the appropriate position. |
| Decimal| soft_bounds_scale| | Scale applied to the spherical cone used to recenter the camera IF the hard bounds spherical cone angle has been broken. |

### badger:camera_distance_activation

Describes the range this camera can activate in.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| distance| | The distance from the camera to it's look target. This determines the range this camera will try to activate in. |

### badger:camera_input_layout_override

Defines how cameras can override an existing input layout.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| String| layout_name| | The name of the input layout. |

### badger:camera_locomotion

Alters the camera position based on follow target's locomotion

| **Type** | **Name**                         | **Description**                                                                                         |
|----------|----------------------------------|---------------------------------------------------------------------------------------------------------|
| **Decimal** | **default_seconds_ahead**      | Default amount of seconds to multiply the follow target's average velocity with to position the camera ahead of the follow target. |
| **List**    | **pitch_modifiers**            | List of pitch angle modifiers [0, 360] written in ascending order. These modifiers identify which yaw modifiers should be applied for camera positioning. Note: This angle is the difference in pitch between the player's velocity vector and the camera look pitch. |
|            | angle                           | Angle boundary in degrees [0, 360]. Interpolation occurs between these boundaries. (Float)             |
|            | yaw_modifiers                   | List of yaw angle modifiers [0, 360] written in ascending order. These modifiers identify which additive changes should be applied to our seconds ahead and camera positioning calculation. This angle is the difference in yaw between the player's velocity vector and the camera look yaw. (List) |
|            | seconds_ahead_delta             | Additive change in seconds ahead amount if the modifier is met. (Decimal)                             |
| **List**    | **speed_modifiers**            | List of possible speed modifiers.                                                                        |
|            | seconds_ahead_multiplier         | Multiplying change in seconds ahead amount if the modifier is met. (Decimal)                           |
|            | speed                           | Speed boundary. Interpolation occurs between these boundaries. (Decimal)                                |
| **Integer** | **ticks_to_average_velocity_over** | Amount of ticks used to create the average velocity. A larger value results in smoother camera positioning changes relative to velocity. A value of 10 is a good baseline. |

---

### badger:camera_show_indicators

Describes whether or not the camera is able to see circular indicators under mobs.

### badger:camera_show_zone

Describes whether or not the camera be able to see zones.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Boolean| show_all_zones| | When set to true, this camera will display all zones in preview |
| Decimal| zone_distance| | The distance that this camera should show zones at |

### badger:cinematic_camera

Attach to a camera to indicate that it's used in the cinematic system as a camera.

### badger:clamp_camera

Badger's boom camera.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| horizontal_clamp| | Horizontal clamp for the camera. |
| Decimal| vertical_clamp| | Vertical clamp for the camera. |

### badger:decal_visualization

Optional parameters to control how decals are rendered.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| alpha| | Sets the opacity of the decal. |
| Integer| animation_frame_count| | Number of frames in the decal's animation |
| Integer| animation_framerate| | Framerate of the decal's animation |
| Boolean| apply_to_owned| | Bitwise flag for checking if rendered object entity is in a valid team. |
| Boolean| apply_to_unowned| | Bitwise flag for checking if rendered object entity is not in a team. |
| String| bin| | Name of the group to organize the decal. |
| Vector [a, b, c]| decal_size| | Size of the decal in blocks. May be overridden by code. |
| String| decal_textures| | When set, will use the stated textures for this decal, defaulting to the first. Textures after the first can be switched to via code |
| Decimal| fade_out_start| | Duration in seconds to begin fading out the decal. |
| Vector [a, b, c, d]| fill_color| | A vector 4 of the color of the fill [R,G,B,A]. |
| Boolean| height_fade| | Bitwise flag for enabling height fade. |
| Decimal| height_fade_center| | Vertical position of the decal to center the opacity fade. |
| Decimal| height_fade_height| | Vertical offset from HeightFadeCenter to lower opacity. |
| Boolean| is_symmetric_texture| | Bitwise flag for mirroring the texture vertically and horizontally. |
| Decimal| lifetime| | When set and > 0, the decal disappears after this many seconds. |
| Boolean| linear_pulse_mode| | Bitwise flag for selecting whether to use the shape of the distance function or object z position for the pulse size. |
| Boolean| mask_entities| | Bitwise flag for checking if rendered object is an entity. |
| Boolean| normal_mapped| | Boolean for whether a normal map for the decal texture exists. |
| Vector [a, b, c, d]| outline_color| | A vector 4 of the color of the outline [R,G,B,A]. |
| Decimal| outline_softness| | Size of the edge gradient of the outline. |
| Decimal| outline_thickness| | Size of the outline of the decal. |
| Vector [a, b, c, d]| pulse_color| | A vector 4 of the color of the pulse [R,G,B,A]. |
| Decimal| pulse_count| | Number of times to repeat the pulse. |
| Decimal| pulse_softness| | Size of the edge gradient of the pulse. |
| Decimal| pulse_speed| | Speed over global time the pulse increases in size. |
| Decimal| pulse_thickness| | Size of the pulse decal. |
| Vector [a, b, c]| rotation_center| | Local vector 3 position for the pivot point of rotation. |
| String| shape| | Shape type of the decal. |
| Boolean| sliced_texture| | Bitwise flag for slicing the texture atlas correctly if symmetric. |
| Integer| sort_key| | Sorting index. |
| Decimal| sparkle_blink_speed| | Frequency of the sparkles blinking. |
| Decimal| sparkle_density| | Density of the sparkles. |
| Decimal| sparkle_movement_speed| | Speed of the sparkles moving about the decal. |
| Decimal| sparkle_size| | Size of the sparkles. |
| Decimal| sparkle_strength| | Opacity of additive sparkles. |
| Boolean| start_enabled| | When true, this decal will be enabled and visible when spawned. When false, it will not render unless code is in place to cause it to do so. |
| Decimal| texture_slice_corner_size| | Amount from [0.0-1.0] to trim the corner of the square texture. |
| Decimal| yaw| | Sets the yaw rotation of the decal. |

### badger:decoration_collider_request

Define the decoration collider to be attached to an entity.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| String| name| | Archetype name of the decoration collider. |

### badger:entity_age

Enables the age timer.

### badger:face_animation

Defines facial animation properties for the entity.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Positive Integer| anim_columns| | The number of columns in the animation texture page. |
| Positive Integer| anim_rows| | The number of rows in the animation texture page. |
| Positive Integer| blink_frame| | Frame index for blinks. |
| Array| blink_time_closed| | Range (in seconds) to hold blink frame. |
| Array| blink_time_open| | Range (in seconds) to hold non-blink frame. |
| Positive Integer| default_frame| | Default frame index. |

### badger:fast_travel_presentation

Defines the entity's presentation event & molang variable names for fast travel.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| String| cooldown| | The name of the cooldown presentation event & molang variable. |
| Decimal| cooldown_event_time| | The time, in seconds, when the fast travel end trigger event is triggered after initiating fast travel. |
| JSON Object| fade_to_black| | The data containing a fade to black screen transition |
| String| windup| | The name of the windup presentation event & molang variable. |
| Decimal| windup_event_time| | The time, in seconds, when the fast travel start trigger event is triggered after initiating fast travel. |

### badger:hero_lure_presentation

Defines the entity's presentation event names for hero lure.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| String| start_trigger| | The name of the start presentation event. |
| String| stop_trigger| | The name of the stop presentation event. |

### badger:hit_reaction_threshold

Defines this entity as a hit reaction threshold prototype.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| activation_percent| | The percentage of target's max health that attacking damage will do, on the range [0,1]. |

### badger:hud_message_interaction_failed

This describes HUD messages to show if we have a failed interaction that requires a resource/unlock cost.

| **Type**       | **Name**                           | **Description**                                                                                                                                                                                |
|----------------|------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **JSON Object**| **resource_cost_failed_message**   | Defines the HUD message displayed when the player lacks the resources for the interaction cost.                                                                                               |
|                | hud_message_args                   | Arguments to pass into the HUD message; the message must support these arguments.                                                                                                            |
|                | hud_message_id                     | HUD message ID sourced from `hud_text_messages.json`.                                                                                                                                         |
| **JSON Object**| **unlock_cost_failed_message**     | Defines the HUD message displayed when the player lacks the resources to unlock an item or feature.                                                                                           |
|                | hud_message_args                   | Arguments to pass into the HUD message; the message must support these arguments.                                                                                                            |
|                | hud_message_id                     | HUD message ID sourced from `hud_text_messages.json`.                                                                                                                                         |

---

### badger:input_actions

Defines the entity's input mapping of possible actions.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| String| action_name| | The name of the action. |
| String| action_targeting_type| | The targeting type of the action. Can be set to "none", "directional" or "target" |
| String| input_action_mode| | Controls the action's effect, whether it is instant or sustained over time. Can be set to "trigger" or "sustain" |
| String| input_id| | The button id from the input overlay. e.g. button.charge. |
| String| target_facing| | The relative direction to shoot in e.g. player_facing (only valid when action type is set to target) |
| Decimal| target_range| | The range of the target (only valid when action type is set to target) |

### badger:interaction_presentation

Defines toggles to enable or disable certain interaction presentation events from firing when interacting with this entity.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Boolean| trigger_generic_events| true| When true, will trigger generic presentation events when interacted with (archetype doesn't matter). Can be combined with specific events. |
| Boolean| trigger_specific_events| false| When true, will trigger archetype specific presentation events when interacted with. Can be combined with generic events. |

### badger:interpolation

Defines the entity's interpolation parameters.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| orientation_interpolation_steps| | How many fixed update ticks (there are 20 per second) an interpolation of an orientation change takes. Default is 1, the value can be greater then 1 but also be a fraction (for example 0.5f). |
| Decimal| position_interpolation_steps| | How many fixed update ticks (there are 20 per second) an interpolation of an position change takes. Default is 1, the value can be greater then 1 but also be a fraction (for example 0.5f). |
| Decimal| position_interpolation_steps_y_down| | How many fixed update ticks (there are 20 per second) an interpolation of an position change while falling downwards takes. Default is 1, the value can be greater then 1 but also be a fraction (for example 0.5f). |

### badger:invasion_icon

The colour for the backing of the healthbar icon during invasions

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Array| icon_color| | The The RGBA value of the icon colour. Formatted as an array |

### badger:invulnerability_heartbeat_view

This component provides which audio events to play when the invulnerability heartbeat pulses.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| String| audio_bae_event| | This is the name of the Audio BAE Event that will start when the invulnerability render pulse occurs. It's recommended to not use a looping sound. |

### badger:keep_alive_timer

Defines the settings for keeping client-side entities alive after they have died.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| time| | How much time, in seconds, the entity will stay alive for after it has died, before being unloaded. |
| Decimal| time_if_refunded| | How much time, in seconds, the entity will stay alive for after it has died, before being unloaded IF it dies due to refund. |
| Boolean| trigger_event_only_on_death| false| If true, the on_death event will only be triggered if the entity died due to reaching zero health. |

### badger:knockback_reaction_threshold

Defines this entity as a knockback reaction threshold prototype.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| activation_value| | The amount of knockback force that an attacking entity needs to do. |

### badger:locator

This flags an entity that it is used as a locator for vfx

### badger:locator_effect

Defines how effects are spawned at given locators.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| String| effect_file| | The name of the effect file to use. |
| String| locator| | The name of the locator to bind the particle effect to. |
| Array| particle_effects| | The container that holds the data for each tyep of particle effect. |
| Decimal| removal_time| | The time delay between when the effect is disabled and when to remove it. |

### badger:lookat

Defines the ability for an entity to look at other things.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| String| joint| | Defines the joint to adjust for the look at. |
| String| look_at_axis| | The axis of the look joint that should be rotated, defaults to y |
| Decimal| max_angle| | Defines the max angle a head can turn. |
| Decimal| max_range| | Defines the max range that this entity will look at. |
| Decimal| min_range| | Defines the min range that this entity will look at. |
| Decimal| turning_delta| | Defines how many degrees per frame the head will turn to target. |

### badger:map3_child_offset

Used on a Map Entity (for World Map 3), defines a relative offset another Map Entity can use (like a VFX entity).

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| x| | X offset. |
| Decimal| y| | Y offset. |
| Decimal| z| | Z offset. |

### badger:map3_copy_local_camera_orientation

Map entity will inherit the local camera's orientation.

### badger:map3_copy_orientation

Map entity will inherit the icon entity's (and game entity) orientation.

### badger:map_entity_3d_icon

Map Entity (for World Map 3) will have an icon on the map screen

### badger:map_icon_offset

Adds an offset to an map source entity.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| x| | X offset. |
| Decimal| y| | Y offset. |
| Decimal| z| | Z offset. |

### badger:mount_molang

Defines the molang variable name to be used when this entity is mounted.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| String| mount_variable| | The molang variable name that is set when this entity is mounted. |

### badger:music_2d_emitter

Defines the 2D Music emitter component. Attaching this to an entity allows it to interface with the music system and determine what music to play.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Array| audio_parameters| | An array of FMOD parameters to set when this emitter activates. "param_name": <int_value> |
| String| heatmap_influence_global_music_parameter| | Name of the global music parameter in FMOD where the heatmap influence should be sent if this music emitter is spawned by a heatmap. |
| Decimal| max_distance| | How far away the local player is before the emitter activates |
| Array| music_states| | An array of different states this emitter can be in. Active state is determined by the music_emitter_states component |
| String| on_enter_bae| | The name of the audio event to request when this emitter activates |
| Integer| priority| | Determines which emitter to choose when more than one is active. Higher is more priority. |
| String| state_name| | Name of the state for Debugging |

### badger:music_emitter_states

Defines the current state of this music emitter entity.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| List| badger:music_emitter_states| | A list of the current states this emitter can be in. This must match the resource_pack music_states for badger:music_2d_emitter. |

### badger:on_destruction

This flags an entity to apply attributes with the details about the entity to spawn when the applied-to entity is destroyed.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| String| locator| | The locator at the position where the new entity should be spawned |
| String| spawn_entity| | The archetype name for the entity that should be spawned |

### badger:orbital_arm

Settings that define how a camera orbits a follow target while focusing on a look target.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| end_arm_height| | Minimum amount the camera can zoom in along the height arm. |
| Decimal| end_arm_length| | Minimum amount the camera can zoom in along the length arm. |
| Decimal| horizontal_lerp| | How smoothly the orbital arm follows its targets on the horizontal plane, on the range (0,1]. |
| Decimal| start_arm_height| | The height offset of the arm, measured from the follow target. |
| Decimal| start_arm_length| | The length of the arm, measured from the follow target. |
| Vector [a, b, c]| target_offset| | The offset from the follow target to rotate around. |
| Decimal| vertical_lerp| | How smoothly the orbital arm follows its targets on the vertical plane, on the range (0,1]. |
| Decimal| zoom_cursor_multiplier| | A multiplier to drive the amount of zoom relative to cursor distance. Larger numbers means greater cursor distance change is needed for same amount of zoom |
| Decimal| zoom_lerp| | How smoothly the orbital camera zooms in and out, on the range [0,1]. |

### badger:placement_molang

Defines the molang string sent for structure placement

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| String| name| | Defines the tag string |

### badger:point_lights

Defines point lights possibly attached to entity locators.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Vector [a, b, c]| color| | Color of light (can also be a Molang expression per channel). |
| Decimal| enabled| | Is the light enabled (can also be a Molang expression). |
| Decimal| falloff_power| | Power of rate of falloff for light (can also be a Molang expression). |
| Decimal| intensity| | Intensity of light (can also be a Molang expression). |
| Array| lights| | Container for the list of lights. |
| String| locator| | The name of the locator to bind the light to (optional). |
| Vector [a, b, c]| offset| | Offset for light, in local space (can also be a Molang expression per channel). |
| Decimal| radius| | Radius of light (can also be a Molang expression). |

### badger:presentation_event

Defines the presentation triggers this entity can respond to, and the animation events to play.

| **Type**       | **Name**                     | **Description**                                                                                                                                                                                                                                    |
|----------------|------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **JSON Object**| **animation**                | Descriptor for an animation object.                                                                                                                                                                                                                |
| **String**     | **attachment**               | Descriptor for an attachment object.                                                                                                                                                                                                               |
| **JSON Object**| **audio**                    | Descriptor for an audio effect object.                                                                                                                                                                                                              |
| **JSON Object**| **beacon**                   | Descriptor for a beacon effect object.                                                                                                                                                                                                              |
| **JSON Object**| **distance_curve**           | Details on how the duration curve is affected by the distance between the event sender and the client player.                                                                                                                                       |
|                | min_range                    | Minimum distance; any distance smaller than or equal to this will be treated as a 1.0f multiplier.                                                                                                                                                |
|                | multiplier_function           | Multiplier curve used to process the distance curve.                                                                                                                                                                                              |
|                | range                         | Maximum distance; any distance greater than or equal to this will be treated as a 0.0f multiplier.                                                                                                                                                  |
| **JSON Object**| **duration_curve**           | Details for configuring the appearance of the duration curve.                                                                                                                                                                                     |
|                | duration                      | Total duration of the rumble in seconds. Note: If there is a start delay, this duration begins after that delay.                                                                                                                                 |
|                | in_duration                   | Time taken to transition from minimum to maximum rumble strength.                                                                                                                                                                                |
|                | in_easing_function            | Function used to ease into the rumble (i.e., at the start of the rumble).                                                                                                                                                                         |
|                | minimum_strength              | Minimum strength of the curve; this will be the initial amount set at the start of the rumble, towards which the easing-out function will move.                                                                                                     |
|                | out_duration                  | Time taken to transition from maximum rumble strength to minimum.                                                                                                                                                                                |
|                | out_easing_function           | Function used to ease out of the rumble (i.e., at the end of the rumble).                                                                                                                                                                          |
|                | pulse_duration                | Duration of each pulse in seconds. The number of pulses is calculated as total duration divided by pulse duration, requiring the total duration to be divisible by pulse duration for an integer result.                                          |
|                | start_delay                   | Time delay before initiating the initial rumble.                                                                                                                                                                                                  |
|                | strength                      | Maximum strength of the rumble that the in-easing function will approach.                                                                                                                                                                         |
| **JSON Object**| **invulnerability_decal**     | Descriptor for a visual-only invulnerability decal.                                                                                                                                                                                               |
| **String**     | **outline**                  | Descriptor for an outline object.                                                                                                                                                                                                                  |
| **Boolean**    | **override**                 | Determines whether this type of rumble can override other rumble events (true/false).                                                                                                                                                             |
| **JSON Object**| **particles**                | Descriptor for a particle effect object.                                                                                                                                                                                                          |
| **Integer**    | **priority**                 | Priority of this rumble event; lower integer values indicate higher priority.                                                                                                                                                                      |
| **String**     | **script**                   | Descriptor for the script name to execute upon trigger.                                                                                                                                                                                           |
| **String**     | **spawn_entity**             | If the handler is a single string, it indicates that a single entity is spawned with a 100% chance of success.                                                                                                                                   |
| **String**     | **visual_state**             | Descriptor for a visual state object.                                                                                                                                                                                                              |

---

### badger:projectile_trail_request

Determine a projectile trail from projectile trail collection.

### badger:removal_time (_duplicate?_)

destroys an entity after a given period of time

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| String| presentation_event| | Name of the presentation event to fire when the timer triggers |
| Boolean| sync_presentation_event| | Whether or not the event should be synced over the server, or if it should be local only |
| Decimal| time| | Amount of time in seconds before the entity is destroyed |

### badger:rendering

The rendering parameters for the entity.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| String| definition| | The name of the rendering definition to use. |
| Integer| limit| 1024| Setting sets a maximum limit to number of instances for an entity |
| Boolean| render_controllers_are_constant| | Flag to indicate if the render controllers should be reprocessed each frame. |
| Boolean| static| | Flag to indicate the entity type requires a static intitialization render pass. |

### badger:respawn_trigger

The trigger that occurs when an entity is respawned

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| String| trigger| | The trigger event to send when an entity respawns |

### badger:rig_operations

Specifies a list of rig operations to apply.

### badger:rotation_presentation

Defines clientside presentation-related tunings for rotation.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| String| rotation_stop_presentation_event_delay| | Defines the delay after rotation ends before the stop presentation event is sent. This can be used to avoid excessive event triggering when rapidly starting and stopping rotation. |

### badger:skin

Defines the entity with this data as a clientside skin archetype

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| String| archetype| | The name of the base archetype that this clientside skin is for. Gameplay behaviour will be inherited from this base archetype.. |

### badger:smooth_look_at

Settings for smoothing camera look.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| focus_radius| | The radius around the look target that will trigger smoothing if the target moves beyond it. |

### badger:speed_data_to_fmod

This component allows an entity to send data related to its speed to FMOD. It sends its speed and it's Y velocity to the FMOD parameters specified in data.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| String| fmod_speed_param| | This is the name of the FMOD Parameter we're converting the entity's speed parameter to. |
| String| fmod_velocity_y_param| | This is the name of the FMOD Parameter we're converting the entity's Y velocity parameter to. |
| Decimal| max_speed| | The maximum value we want to track for the entity's speed parameter. Setting this will normalize the parameter value between the min and max. |
| Decimal| max_velocity_y| | The maximum value we want to track for the entity's Y velocity parameter. Setting this will normalize the parameter value between the min and max. |
| Decimal| min_speed| | The minimum value we want to track for the entity's speed parameter. Setting this will normalize the parameter value between the min and max. |
| Decimal| min_velocity_y| | The minimum value we want to track for the entity's Y velocity parameter. Setting this will normalize the parameter value between the min and max. |

### badger:terrain_type_molang

Specify that an entity should define and update the 'variable.terrain_type' molang variable

### badger:threat_owner

Defines this entity as a host for music threat level.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| decay_rate| | How fast threat value interpolates when decaying towards base value. |
| Decimal| grow_rate| | How fast threat value interpolates when increasing towards base value. |
| Decimal| influence_distance| | How far away will this host broadcast it's threat level. |
| Decimal| max_threat| | Max possible threat value for this host. |
| Decimal| min_threat| | Min threat value for this host. |

### badger:threat_source

Defines this entity as a contributor to threat level for an owning source village/base.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| base_value| | How much threat this entity contributes after the transition time has passed. |
| Decimal| spawn_value| | How much threat this entity contributes when spawned. |
| Decimal| transition_time| | Time in seconds it takes for this entity to transition in threat value from the spawn value to the base value. |

### badger:toggleable_decorative_child

Defines an entity archetype to add as a child of this entity, that will be destroyed and recreated as necessary to hide and display it.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| String| child_entity_id| | Defines the name of the entity archetype to use. |
| Integer| molang_variant_id| | Sets the molang parameter 'variable.variant_id' on the child when it is created. This is used to create visual variants of the child entity using things like material swaps. |
| Boolean| start_enabled| | Defines whether the child should be enabled by default. |

### badger:variable_propagation

Allows specification of what variables propagate from a parent to a parented entity

### badger:verlet_dynamics

Specifies a list of verlet dynamics to apply.

### badger:visual_only_invulnerability_decal

Triggers invulnerability decal on entity along with offsets for modifying the bounds of the AABB stored by the decal

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Vector [a, b, c]| max_aabb_subtractive_offset| | Offset vector to subtract from max AABB bound of the decal |
| Vector [a, b, c]| min_aabb_additive_offset| | Offset vector to add to min AABB bound of the decal |
| List| states| | A list of invulnerability states this entity can have. By default the first state is active. |

### badger:visualize_local_player_allegiance

This flags an entity to apply a molang variable encoding allegiance to the local player for visual purposes.

### minecraft:camera

Defines Badger functionality to the existing Bedrock camera.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| far_plane| | The far clipping plane distance. |
| Decimal| field_of_view| | The angle in degrees that determines the visible area from the camera's perspective |
| Decimal| max_shadow_range| | Constrains the range of the shadow cascades by overriding the max shadow distance. |
| Decimal| min_shadow_range| | Constrains the range of the shadow cascades by moving the starting point further from the camera. |
| String| name| | Identifies the type of camera. |
| Decimal| near_plane| | The near clipping plane distance. |
| Vector [a, b, c]| orientation| | In degrees: The pitch around the x axis, yaw around the y axis, and roll around the z axis. |
| Vector [a, b, c]| position| | The location of the camera in world coordinates. |
| Integer| priority| | The priority of this camera relative to other cameras. This is used to resolve conflicts when multiple cameras can activate. |
| Boolean| render_player_model| | Determines if the player's model should render while this camera is active. |
| Integer| sensitivity_input_option| | Identifies which user camera sensitivity slider to refer to. |
| Decimal| shadow_depth_bias| | Overrides the depth bias used for shadow cascades. |
| Decimal| shadow_slope_bias| | Overrides the slope bias used for shadow cascades. |

### minecraft:camera_attach

Describes how a camera entity should attach to another entity.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| String| locator| | The name of the locator where the camera will be attached. |
| Vector [a, b, c]| offset| | The offset from the locator/entity where the camera will be attached. |
| Boolean| use_locator_orientation| | True if the camera should use the locator's orientation as it's view orientation. |

### minecraft:camera_avoidance

Describes how cameras that orbit a target should avoid collision and occlusion.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| String| relax_distance_smoothing_spring| | How quickly the camera will return to it's desired distance once a collison or occulsion is cleared. |

### minecraft:camera_blend_in_default

Describes how a camera can pitch around the x axis and yaw around the y axis.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| blend_time| | The duration of the blend in seconds. |
| Boolean| camera_to_accepts_input| | Determines if the camera being blended to will accept input during the blend. |
| String| ease| | The type of easing function used to control the blend. |
| Decimal| input_dampening_coefficient| | Dampens the input on the camera to if the camera to is accepting input during the blend. |

### minecraft:camera_fly

Describes how a camera flies around.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| speed| | The speed of the camera when flying. |
| Decimal| speed_alt| | The alternative speed of the camera when flying. |
| Decimal| sprint_multiplier| | The multiplier set by /sprint of the speed of the camera when flying. |

### minecraft:camera_look_at

Describes how a camera frames it's look target.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Vector [a, b]| hard_bounds_max| | The top right coordinates of the hard bounds. |
| Vector [a, b]| hard_bounds_min| | The bottom left coordinates of the hard bounds. |
| Vector [a, b, c]| look_at_offset| | Offsets the point that the camera looks at in look target relative space. |
| Decimal| look_target_vertical_offset| | Offsets the point that the camera looks at up or down in world space. |
| Vector [a, b]| soft_bounds_max| | The top right coordinates of the soft bounds. |
| Vector [a, b]| soft_bounds_min| | The bottom left coordinates of the soft bounds. |
| Decimal| soft_bounds_smoothing_spring| | How quickly the camera's orientation will correct to keep it's look target within the soft bounds. |

### minecraft:camera_orbit

Describes how a camera orbits around it's follow target.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| azimuth_smoothing_spring| | How quickly the camera's azimuth will correct to match the desired azimuth. |
| Decimal| distance_smoothing_spring| | How quickly the camera's distance will correct to match the desired distance. |
| Decimal| pivot_point_vertical_offset| | Offsets the point that the camera orbits around up or down. |
| Decimal| polar_angle_max| | Maximum polar angle. |
| Decimal| polar_angle_min| | Minimum polar angle. |
| Decimal| polar_smoothing_spring| | How quickly the camera's polar angle will correct to match the desired azimuth. |
| Decimal| radius| | The ideal distance the camera will keep from it's follow target. |

### minecraft:camera_speed_modifier

Used as a multiplier for the fly camera / first person camera.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| look_at_speed_modifier| | Multiplier for the look at speed. |
| Boolean| slow_mode_modified_by_sprint| | Boolean for whether or not slow mode speed is affected by sprint multiplier. |
| Decimal| translation_speed_modifier| | Multiplier for the translation speed. |

### minecraft:first_person_look

Describes how a camera can pitch around the x axis and yaw around the y axis.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| pitch_max| | The maximum pitch angle in degrees that the camera can have. |
| Decimal| pitch_min| | The minimum pitch angle in degrees that the camera can have. |

### minecraft:follow_target

A target to follow.

### minecraft:look_target

A target to look at.

## identifier

The identifier is used to register the entity with the server. In the Client Entity Definitions JSON the identifier sets the appearance of the entity(materials, textures, geometry, etc.) The matching identifier in the Entity Behavior JSON in the Behavior Pack is what gives the entity its behaviors.

## materials, textures, animations

Players can set the materials, texture and geometry used for the entity in this section. Players can set one or more materials, textures, and geometries that can be used by the mob. Players must set user defined names for them. These names are used in the Render Controllers JSON. Players can reference materials, textures, and geometry from the vanilla Minecraft Resource Pack or create their own.  Custom materials, textures, and geometry should be in the corresponding folder at the root of the Resource Pack.

## scripts

Scripts allow players to use MoLang to compute calculations once and store that value. This value than can be used over and over again without the need to constantly recompute the calculations. Scripts currently support pre - animation and scale.More script types will be added later.
-Pre-animation scripts are evaluated immediately before animations are processed.
-Scale sets the scale of the mob's geometry.

_NOTE: This section is to be completely rewritten as it is thoroughly inaccurate. FISH and a BAT in LEGENDS? Imagine_
**Example pre-animation script for cod**

```json
"scripts": {
  "pre_animation": [
    "variable.ZRot = !query.is_in_water ? Math.cos((query.time_stamp + global.frame_alpha) * 14.32) * 90 : 0.0;",
    "variable.AnimationAmountBlend = Math.lerp(variable.AnimationAmountPrev, variable.AnimationAmount, global.frame_alpha);"
  ]
},
```

**Example scale script for the bat**:

```json
"scripts": {
  "scale": "0.35"
},
```

## animations

Allows the player to assign names to reference the long name for animations. These names are used by the animation controller JSON. Players can reference animations from the vanilla Minecraft Resource Pack or create their own. Custom animations should be in the animation folder at the root of the Resource Pack.

## render_controllers

Specifies the names of render controllers. This name needs to match the name of a corresponding JSON located in the Render Controllers folder. Players can reference Render Controllers from the vanilla Minecraft Resource Pack or create their own. Custom Render Controllers should be in the textures folder at the root of the Resource Pack.

## enable_attachables

Legacy, not used.

## Filters

Filters allow data objects to specify test criteria which allows their use.

For example, a model that includes a filter will only be used when the filter criteria is true.

A typical filter consists of four parameters:

 name: the name of the test to apply.

 domain: the domain the test should be performed in. An armor slot, for example. This parameter is only used by a few tests.

 operator: the comparison to apply with the value, such as 'equal' or 'greater'.

 value: the value being compared with the test.

A typical filter looks like the following:

 { "test" : "moon_intensity", "subject" : "self", "operator" : "greater", "value" : "0.5" }

Which results in the calling entity (self) calculating the moon_intensity at its location and returning true if the result is greater than 0.5.

Tests can be combined into groups using the collections 'all_of', 'any_of', or 'none_of'.

 All tests in an 'all_of' group must pass in order for the group to pass.

 One or more tests in an 'any_of' group must pass in order for the group to pass.

 All tests in a 'none_of' group must fail in order for the group to pass.

## has_biome_tag

Tests whether the biome the subject is in has the specified tag.

| **Type**       | **Name**    | **Default** | **Description**                                                                                          |
|----------------|-------------|-------------|----------------------------------------------------------------------------------------------------------|
| **String**     | **operator**| equals      | (Optional) The comparison to apply with 'value'. Possible options include:                              |
|                |             |             | - `!=` : Test for inequality.                                                                             |
|                |             |             | - `<` : Test for less-than the value.                                                                    |
|                |             |             | - `<=` : Test for less-than or equal to the value.                                                       |
|                |             |             | - `<>` : Test for inequality.                                                                             |
|                |             |             | - `=` : Test for equality.                                                                                 |
|                |             |             | - `==` : Test for equality.                                                                                |
|                |             |             | - `>` : Test for greater-than the value.                                                                  |
|                |             |             | - `>=` : Test for greater-than or equal to the value.                                                    |
|                |             |             | - `equals` : Test for equality.                                                                           |
|                |             |             | - `not` : Test for inequality.                                                                            |
| **String**     | **subject** | self        | (Optional) The subject of this filter test. Possible options include:                                    |
|                |             |             | - `block` : The block involved with the interaction.                                                     |
|                |             |             | - `damager` : The damaging actor involved with the interaction.                                          |
|                |             |             | - `other` : The other member of an interaction, not the caller.                                          |
|                |             |             | - `parent` : The caller's current parent.                                                                 |
|                |             |             | - `player` : The player involved with the interaction.                                                   |
|                |             |             | - `self` : The entity or object calling the test.                                                        |
|                |             |             | - `target` : The caller's current target.                                                                 |
| **String**     | **value**   |             | (Required) The tag to look for.                                                                          |

---

### Examples

**Full..**

```json
{ "test": "has_biome_tag", "subject": "self", "operator": "equals", "value": " " }
```

**Short (using Defaults)..**

```json
{ "test": "has_biome_tag", "value": " " }
```

## Server Entity Documentation

Server entity definitions are contained within a Behaviour Pack and contain all of the information necessary to describe how an entity should behave. To create your own, start by navigating to the "entity" folder inside the root of the Behaviour Pack. In the entity folder create a JSON file and give it a name. The JSON file needs a format version and minecraft:entity information.

The minecraft:entity section contains the description for the entity. Under description there are a number of things that you can set about an entity.

**Example server entity definition JSON for the prismarine item drop**:

```json
{
  "format_version": "1.8.0",
  "minecraft:entity": {
    "description": {
      "identifier": "badger:item_prismarine",
      "is_summonable": true,
      "runtime_identifier": "badger:badger_mob"
    },
    "components": {
      "badger:aabb": {
        "height": 0.562500076294,
        "length": 0.0625,
        "width": 0.15625
      },
      "badger:template": "badger:template_item_orb",
      "badger:tags": {
        "tags": [
          "item",
          "xp_common",
          "ghostTriggerFriendly"
        ]
      }
    }
  }
}

```

## Server Entity Description Properties

### badger:aabb

Dimensions of this entities bounding box

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| height| | The height of the entity. |
| Decimal| length| | The length of the entity (optional if not specified the width is used). |
| Decimal| width| | The width of the entity. |

### badger:action_tickets

Defines the action tickets that this entity has

| Type         | Name            | Description                                                                                                                                                      |
|:------------:|:---------------:|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------|
| JSON Object  | action_tickets  | Defines the number of "action tickets" this entity has, which limits how many entities can target it simultaneously. Separated by category of action:          |
|              | - cc            | Close combat tickets.                                                                                                                                         |
|              | - ct            | Creeper-target tickets.                                                                                                                                       |
|              | - ht            | Heal-target tickets.                                                                                                                                          |
|              | - rc            | Ranged combat tickets.                                                                                                                                       |
|              | - sc            | Secondary combat tickets.                                                                                                                                     |

### badger:advanced_direct_reticle

Defines the entity archetype applied to the advanced direct reticle on the clientside.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| String| reticle_entity_id| | Defines the name of the entity archetype to use. |
| Decimal| reticle_radius| | Defines the size of the advanced direct targeting reticle. |

### badger:aimable

Defines the ability to be aimed at a target location.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| aim_speed| | Defines the speed multiplier for the aiming reticle when using the mouse. |
| Boolean| exit_aim_on_shoot| | Default is true, setting to false will not cancel the aim upon shooting |
| Decimal| gamepad_aim_speed| | Defines the speed multiplier for the aiming reticle when using the thumb stick. |
| Decimal| max_range| | Defines the max range that this entity can be aimed at. |
| Decimal| min_range| | Defines the min range that this entity can be aimed at. |
| String| reticle_entity_id| | Defines the name of the reticle entity to use. |
| Boolean| reticle_inherits_movement| | When true, causes the aimable reticle to follow this entity when it moves. |
| String| shoot_action| | Defines the name of the shoot action that this aimable is related to. |
| Decimal| starting_range| | Defines the starting range that this entity will be aimed at when first interacted with. |
| Boolean| uses_move_stick| | Sets whether this aimable should be controlled by the move stick, rather than the aim stick, when using a controller. |

### badger:aimable_turret

Flags an entity as a turret for an aimable entity.

### badger:allay

Handles allay movement information.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| between_blocks_travel_time| | How many seconds it takes to move between blocks when working. |
| Decimal| bounce_scale| | How frequently the allay bobs up and down. |
| Decimal| bounce_speed| | How fast the allay bobs up and down. |
| Vector [a, b, c]| landed_position| | Where the allay goes when it is working (offset from its target). |
| Decimal| max_range| | Maximum distance the allay with travel to/from the player. When exceeded it disappears instead. |
| Decimal| radius_scale| | How wide of a circle the allay makes around the target. Value of 1 should be just touching the corners. |
| Vector [a, b, c]| spawn_offset| | Where the allay spawns/despawns in relation to the player |
| Decimal| spin_speed| | How fast the allay rotates around its target |
| Decimal| travel_time| | How many seconds it takes to move between destinations. |

### badger:allay_panic

sends a panicked allay home after a given time, destroying the gather order

| **Type**       | **Name**             | **Description**                                                                                                 |
|----------------|----------------------|-----------------------------------------------------------------------------------------------------------------|
| **String**     | **panicked_path**    | Icon name representing the panicked version of the allay.                                                      |
| **Decimal**    | **return_timer**     | Duration in seconds after being scared before the allay returns home. Leave undeclared for an infinite return time. |
| **Decimal**    | **scare_delay**      | Duration in seconds before the allay becomes scared.                                                            |
| **JSON Object**| **scare_filter**     | **Configuration for the allay's panic filtering** criteria.                                                        |
|                | alliance_rule_filter  | Name of the alliance rule filter applied to the allay's panic behavior.                                        |
|                | exclude_tags         | List of tags to be excluded when filtering potential threats.                                                  |
|                | include_tags         | List of tags to be included when filtering potential threats.                                                  |

---

### badger:aoe

Defines the settings used to spawn an AoE entity

| **Type**       | **Name**              | **Description**                                                                                                                                           |
|----------------|-----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Array**      | **damage_effects**    | Defines the damage effects applied within the AoE.                                                                                                       |
|                | damage                 | The amount of damage dealt to entities inside this AoE.                                                                                                |
|                | exclude_tags           | The tags required for an entity to **not** be influenced by this AoE.                                                                                  |
|                | include_tags           | The tags required for an entity to be influenced by this AoE.                                                                                          |
|                | requires_los          | Indicates whether the AoE requires line of sight (LoS) from the shape pivot to the target to apply damage.                                             |
| **JSON Object**| **shape**             | Specifies the shape used for the AoE.                                                                                                                  |
| **JSON Object**| **timer**             | Defines the timing for applying the effects of the AoE.                                                                                                |
|                | interval               | The time, in seconds, between applying the effect(s) of the AoE.                                                                                       |
|                | jitter                 | An array of ticks that modifies the intervals, providing pseudo-randomness to ease performance strain on the AoE.                                         |

---

### badger:applies_buffs

Defines this entity's ability to confer buffs to nearby entities.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| String| apply_buff_cancel_trigger| | The trigger event to send when a buff candidate that started having a buff applied to it is instead interrupted and does not receive a buff. |
| String| apply_buff_complete_trigger| | The trigger event to send when a buff candidate finishes having a buff applied to it. |
| String| apply_buff_start_trigger| | The trigger event to send when a buff candidate starts having a buff applied to it |
| Integer| available_charges| | How many times can this entity dispense a buff. After charges are spent the entity destroys itself. -1 means infinite charges. |
| String| charge_used_trigger| | The trigger event to send when a charge is used. |
| String| charges_molang_name| | The name of the molang variable representing the number of charges. |
| Array| preset_buffs| | The preset buffs to be applied by this entity. Use this if the buffs applied by this entity should not be modifiable. |
| Boolean| wipe_buffs| | Whether this buff applicator wipes existing buffs on the receiving entity before applying its own buffs |

### badger:apply_damage

Defines the settings for applying damage.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| JSON Object| damage| | The damage to apply. |

### badger:apply_status

Defines the settings for applying status effects.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| JSON Object| apply_status| | The list of status effects to apply. |

### badger:atomic_village_exempt

Dictates whether this entity can cause its owning village to force suspension.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Boolean| exempt_from_requests| | This entity won't cause its owning village to force suspension. |
| Boolean| exempt_from_requests_and_suspension| | This entity won't cause its owning village to force suspension nor will it forcefully be suspended when it's owning village is. |

### badger:audio_ignore_vo_reminder

This flags an entity to ignore VO reminders if the player is near this entity. Used for entities like villages

### badger:aura_applicator

When applied to a structure, provides a buffing aura to entities within its zone of control.

| **Type**       | **Name**                           | **Description**                                                                                                                                                                                                                                           |
|----------------|------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Array**      | **auras**                          | A list of buff auras used by this entity.                                                                                                                                                                                                               |
|                | **alliance_rule_filter**           | The rule that determines which types of entities will be buffed (friendly, enemy, etc.).                                                                                                                                                                 |
|                | **apply_to_build_previews**        | Indicates whether this buff applies to build previews (e.g., area overlay buffs).                                                                                                                                                                         |
|                | **buff_id**                        | The identifier of the buff to be applied by this entity.                                                                                                                                                                                               |
|                | **can_apply_to_self**              | Specifies whether this buff can be applied to the aura applicator itself.                                                                                                                                                                               |
|                | **exclude_tags**                   | Entities with any of these tags will not receive a buff.                                                                                                                                                                                                |
|                | **include_tags**                   | Entities must have all these tags to receive a buff.                                                                                                                                                                                                    |
|                | **load_affected_entities_on_construction_completion** | If true, the entity will load all suspended entities affected by the aura.                                                                                                                                                                              |
|                | **only_apply_to_construction_completed** | Specifies whether this buff only applies to structures that have completed construction.                                                                                                                                                                 |
|                | **only_apply_to_damaged**          | Specifies whether this buff only applies to damaged entities.                                                                                                                                                                                            |
|                | **persistent**                     | Indicates whether this buff should persist after the aura applicator is gone.                                                                                                                                                                           |
|                | **search_mode**                    | Defines what entities this aura affects. Valid values include `zone`, `village`, and `spawner_controller`.                                                                                                                                               |
|                | **unlock_condition**               | Enables or disables this aura depending on the presence of a given unlock resource.                                                                                                                                                                       |
|                | **has_unlock**                     | If true, this aura will only be active when the specified resource is acquired; if false, it will only be active if the resource is not acquired.                                                                                                        |
|                | **resource**                       | The name of the resource used to enable or disable this aura.                                                                                                                                                                                            |
| **JSON Object**| **buff_queue**                     | Optional data to add if the aura should be applied to entities in a queue.                                                                                                                                                                               |
|                | **limit_concurrent_applications**  | The number of entities to which the aura is applied per unit of time.                                                                                                                                                                                  |
|                | **time_interval**                  | The time interval for applying the aura.                                                                                                                                                                                                                  |

---

### badger:auto_deconstruction

Values used to tune the trigger points for auto deconstruction

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Integer| distance| | The minimum block distance the player has to be from this entity to trigger deconstruction |
| Boolean| team_owned| true| If true, will not deconstruct as long as in range of at least 1 team member. If false, deconstructs based on distance to constructing player. |
| Integer| warning_distance| | The minimum block distance at which the player receives a warning that their structures will be deconstructed |

### badger:auto_despawn

Makes this entity be despawned when its too far from any player.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Integer| distance| | Distance from players at which this entity will despawn. |

### badger:auto_fire

Settings to determine how auto firing behaves.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| confirm_cooldown_time| | amount of time in seconds before another round of auto firing can begin. |
| String| cooldown_molang_name| | The name of the molang variable representing the cooldown state. |
| String| cooldown_start_event| | The name of the presentation event fired when entering the cooldown state. |
| String| cooldown_stop_event| | The name of the presentation event fired when exiting the cooldown state. |
| Decimal| shot_cooldown_time| | amount of time in seconds in between auto fired shots. |
| String| shots_molang_name| | The name of the molang variable representing the number of shots. |
| Integer| total_shots| | number of shots to auto fire before needing to cool down. |

### badger:auto_lure

Controls if this spawner will automatically lure units on spawn/recall and which lures to apply

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| String| lure_action_id| | The action ID triggered on the player if the player does not already have an active lure of the defined archetype. This should be the action that spawns the desired lure on the player. |
| String| lure_archetype| | The lure Archetype to apply when this spawner creates a unit (Between it and the player that triggered it) |

### badger:auto_teleport

Attributes required for auto teleport lured distant entities to their luring entities

| **Type**       | **Name**                               | **Description**                                                                                                                                    |
|----------------|----------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------|
| **Decimal**    | **auto_teleport_distance**             | Minimum distance required between a lured entity and the luring entity for the lured entity to be auto-teleported.                               |
| **Decimal**    | **auto_teleport_offset_behind_camera** | Distance from the camera to the auto-teleport destination, measured in the opposite direction of the camera's orientation.                        |
| **JSON Object**| **auto_teleport_tag_filter**           | Tag filter to determine if a lured entity is eligible for auto-teleportation.                                                                    |
|                | exclude_tags                           | Tags that a lured entity must not possess to be considered for auto-teleportation to its luring entity.                                          |
|                | include_tags                           | Tags that a lured entity must have to qualify for auto-teleportation to its luring entity.                                                       |

---

### badger:background_loading

Dictates how a village entity is loaded in the background.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Integer| priority| | Villages with a lower priority value will be planned and loaded first. The default value is 0. |

### badger:becomes_lost

Signifies that this entity can become lost, and gives the configuration for the lost state

| **Type**       | **Name**                            | **Description**                                                                                                                                       |
|----------------|-------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Array**      | **alone_exclude_filters**           | A list of criteria specifying which units cannot be nearby for the entity to be considered lost.                                                     |
|                | alliance_rule_filter                | The alliance rule filter for this type of unit.                                                                                                      |
|                | exclude_tags                        | Tags that units must not have to be considered in this filter.                                                                                      |
|                | exclusion_radius                    | The minimum distance that this type of unit must maintain from the entity to be considered lost.                                                    |
|                | include_tags                        | Tags that units must have to be included in this filter.                                                                                            |
| **String**     | **lost_unit_pacing_buff_id**       | The buff applied to the unit when it is pacing.                                                                                                      |
| **Boolean**    | **must_be_player_spawned**          | Indicates whether the unit must be spawned by the player to be considered lost.                                                                      |
| **Decimal**    | **pace_destination_radius**         | The radius within which the unit will choose destinations to pace toward from the initial lost location.                                              |
| **Decimal**    | **pace_frequency**                  | The average frequency (in times per second) with which the unit changes direction while pacing.                                                      |
| **String**     | **return_to_player_alliance_rule**  | The alliance rule determining which player the unit will return to.                                                                                  |
| **Decimal**    | **time_alone_to_become_lost**      | The duration for which a unit must be 'alone' (as defined by the filter) before it is considered lost.                                             |
| **Decimal**    | **time_pacing_before_returning**    | The duration a lost unit will pace before returning to the nearest friendly player.                                                                   |
| **Decimal**    | **time_suspended_to_become_lost**   | The duration for which a unit must be suspended before it is considered lost.                                                                        |

---

### badger:behavior_offline_traits

The component allows behaviors to change offline trait phase on behaviour swaps.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Enumerator| traits_phase| | Defines which offline phase will be applied by the behaviour swap. List of valid values: PRECONSTRUCTION, CONSTRUCTING, BUILT, DECONSTRUCTING, DISABLED |

### badger:behavior_swap_interaction

Enables the use of an interaction to trigger behavior swap.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| String| behavior| | The behavior name to swap to upon interacting |

### badger:block_conversion

Converts any listed blocks to a modified version

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| duration_percentage| | What percentage of the build time this conversion process should take |
| Array| exclude| | The block types to exclude from this block conversion |
| Array| include| | The block types to affect with this block conversion |
| JSON Object| modifiers| | Key value pairs that describe which keywords should be replaced with other strings in block names |
| Boolean| top_to_bottom| | Changes the order of block conversion to top to bottom |

### badger:block_damage

states what is required to place a buildable.

| **Type** | **Name**   | **Description**                                                                                                        |
|----------|------------|------------------------------------------------------------------------------------------------------------------------|
| **List** | **phases** | An array of damage phase descriptions, each containing the following properties:                                      |
|          | damage_level | The normalized damage level applied to the phase, ranging from 0.0 (no damage) to the maximum defined damage.      |
|          | max_clamp    | The normalized position that represents the minimum damage applied.                                                  |
|          | max_damage   | The maximum damage level that can be applied during this phase.                                                      |
|          | min_clamp    | The normalized position that represents the maximum damage applied.                                                  |
|          | min_damage   | The minimum damage level that can be applied during this phase.                                                      |

---

### badger:block_reversal

Reverts any changed blocks to the original version

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| duration_percentage| | What percentage of the time this block revert process should take |

### badger:bsharp_interact_cost

states what is required to perform a B# interaction with this entity.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Array| resource_cost| | An array of items and amounts. |
| Array| unlock| | These are the unlocks needed to use this interaction. An array of items with an amount. |

### badger:bsharp_lifetime

Defines which global variable controls the lifetime of an entity.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Integer| value| | Defines the value of the global variable that causes the entity to despawn. |
| String| variable| | Defines name of the bSharp global variable. |

### badger:bsharp_teleport_presentation

Defines the entity's presentation event for teleportation.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| String| arrived_event| | The name of the presentation event fired after the entity teleports |
| String| engaged_event| | The name of the presentation event fired before/during the entity's teleportation |
| Decimal| windup_time| | The time, in seconds, the entity waits after it and the teleport engaged presentation event is triggered before teleporting. |

### badger:buff

Defines a buff

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| application_time_seconds| | The time in seconds an entity is required to wait before the buff is applied. |
| String| buff_category| | A category for the buff. Only for organizational grouping purposes and has no effect on gameplay. |
| Array| ingredients| | The resources which will be spent / consumed to apply this buff. A buff must have costs to be placeable on a slotted buff applicator. |
| Boolean| propagates_to_projectiles| | Whether this buff is propagated to projectiles created by the entity this buff is applied on. |

### badger:build_speed_change_on_damage

Temporarily change the build speed of a buildable structure when being damaged

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| build_speed_multiplier| | the multiplier that will be used to adjust the build speed |
| Integer| duration| | the time in seconds for which the adjustment will be applied |

### badger:buildable_controller_spawner

will spawn Mobs from components position.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| String| biome| | Title of the the Biome Selector |
| Array| biome_set| | The set of valid Biomes |
| Array| claimed_area| | List of Spawners to spawn based on claimed areas |
| String| claimed_by| | Max mobs spawned that are still alive. -1 indicates there is no cap |
| String| generic_BSharp| | Title of the B-Sharp Selector |
| Array| global_active| | List of Spawners to spawn based on global variables |
| String| horde_name| | Name Of Global Horde |
| String| horde_variant| | Set the horde variant to spawn (only for Biome / BSharp), the others will auto pick |
| Boolean| match_any| | If we should force all conditions to check |
| Array| spawn| | Names of Spawners to Place |
| String| static| | Names of spawners to always place |
| Integer| ticks_between_checks| | How many ticks to wait between checking conditions |
| Array| times| | List of Times we want this option to be valid |
| Array| valid_value_set| | The set of valid values of the watch B-Sharp Value |
| String| variable_name| | The Gobal B-Sharp Variable to Watch |
| Integer| weight| | The weight to pick this choice |

### badger:buildable_creation_influence_source

Provides a list of sources of additive or substractive influence upon building creation for this AI.

| **Type**       | **Name**                      | **Description**                                                                                                                                                      |
|----------------|-------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **List**       | **sensors**                   | List of building creation sources.                                                                                                                                 |
|                | **building_start_or_complete** | Designate whether building creation will be tracked at the start or upon completion of placement.                                                                    |
|                | **damage_receiver_filter**     | Specify particular types of structures to track based on damage received.                                                                                          |
|                | **alliance_rule_filter**       | Define team alliance rules for tracking.                                                                                                                           |
|                | **exclude_tags**               | Tags to exclude for building structures.                                                                                                                            |
|                | **include_tags**               | Tags to include for building structures.                                                                                                                            |
|                | **multiplier**                 | The weight of the heat stamp for this building creation source, affecting how significant the heat impact is.                                                        |
|                | **propagation_decay**          | The propagation or spread decay of the heat stamp for this building creation source, determining how quickly the heat impact diminishes over time.                  |

---

### badger:buildable_destruction

states what is required to animate a building's destruction.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| String| particle_effect| | The name of the particle effect that will be played when the structure is removed. |

### badger:buildable_destruction_influence_source

Provides a list of sources of additive or substractive influence upon building destruction for this AI.

| **Type** | **Name**                       | **Description**                                                                                                       |
|----------|--------------------------------|-----------------------------------------------------------------------------------------------------------------------|
| **List** | **sensors**                    | List of building creation sources.                                                                                   |
|          | damage_receiver_filter         | Tracks particular types of structures.                                                                                |
|          | alliance_rule_filter           | Defines team alliance rules for tracking.                                                                             |
|          | exclude_tags                   | Tags to exclude for building structures.                                                                              |
|          | include_tags                   | Tags to include for building structures.                                                                              |
|          | multiplier                     | The weight of the heat stamp for this building destruction source.                                                   |
|          | propagation_decay              | The decay rate of the propagation/spread of the heat stamp for this building destruction source.                     |

---

### badger:buildable_foundation

Dictates what to place as the foundation.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| String| custom_block| | The full name of the block to be used on the foundation |
| String| nbt_file| | Name of the nbt file to place as the foundation. |
| Integer| offset| | Number of blocks downwards to offset the placement of this foundation. |
| String| pillar_height| | The height of the pillar-style foundation |
| Boolean| place_instant_block_by_block| | Whether to override the instant placement behavior of this foundation. |
| String| placement_rule| | The rule that dictates when to place this foundation: always, instant_only |
| String| style| | Style of foundation to generate: inverted_pyramid, pillar, bowl, dome, supports |

### badger:buildable_health_generation

Parameters used to calculate the normalized initial health of structures upon initial constructing.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| initial_health_percentage| | The initial health of the entity normalized. Calculated by a percent of their max health |

### badger:buildable_intangible

When applied to a buildable, it prevents it from blocking the placement of other buildings.

### badger:buildable_presentation

Structure information to display in the UI.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Boolean| allow_stacking| false| Does this entity support stacking |
| String| buildable_category| | Which hotbar category this placeable belongs in. |
| Boolean| force_hide_when_locked| false| will hide even if show_locked_recipes is true. |
| Boolean| has_alternate_icon| false| Does this entity have an aulternate icon |
| Integer| height_offset| | The height above or bellow the ground the cursor will sit when actively placing. |
| String| icon| | What thumbnail image to display in the UI. |
| String| ping_message| | The message that appears to teammates when pinging the structure from build preview. Can accept a parameter for the pinging player's name. |
| Integer| segment_offset| | The height above or bellow the multisegment placement line this segment should be placed. |
| Integer| sort_order| | value for the sorting order within the ui. |
| String| tactical_description| | The structure tactical description to display in the UI. |
| String| tooltip_description| | The structure tooltip description to display in the UI. |
| String| tooltip_title| | The structure tooltip title to display in the UI. |

### badger:buildable_replaceable

Flag Attribute that indicates if the structure can be replaced by other structures

### badger:buildable_requirement

states what is required to place a buildable.

| **Type**       | **Name**                          | **Description**                                                                                                                                     |
|----------------|-----------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------|
| **JSON Object**| **blocker_entities**              | The entities that must not collide with this structure for it to be built.                                                                         |
|                | alliance_rule_filter              | Alliance filter rule that applies to this structure.                                                                                               |
|                | exclude_tags                      | List of tags for entities that are excluded from colliding with this structure.                                                                     |
|                | include_tags                      | List of tags for entities that are included as blockers for this structure.                                                                         |
| **String**     | **exclusive_zone_alliance_rule**  | Alliance rule that defines the restrictions for this exclusive zone.                                                                                |
| **Boolean**    | **invalid_block_snaptoground**    | Determines whether to snap to ground blocks below the building when checking for invalid blocks.                                                    |
| **Array**      | **invalid_block_tags**            | List of block tag types that this buildable cannot be placed on.                                                                                   |
| **Array**      | **invalid_blocks**                | List of block types (e.g., dirt, air, water, lava) that this structure cannot be built upon.                                                       |
| **Array**      | **recipe**                        | An array of items and their corresponding amounts needed for crafting this structure.                                                               |
| **Array**      | **ticket_cost**                   | An array representing the cost in tickets and their respective amounts required for this structure.                                                 |
| **Array**      | **unlock**                        | List of items and amounts required to unlock certain buildings.                                                                                     |
| **String**     | **zone_alliance_rule**            | Alliance rule filter that determines the zones in which this structure can be placed (e.g., any_team, enemy, friendly, etc.).                      |

---

### badger:buildable_snap_point

When applied to a buildable, it flags it as being a position snapping target for previews.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Integer| top_snap_offset| | This adjusts the height which previews snap to, when they snap to the top of this structure. |

### badger:buildable_spawner

will spawn Mobs from components position.

| **Type**       | **Name**                          | **Description**                                                                                                                                     |
|----------------|-----------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------|
| **JSON Object**| **blocker_entities**              | The entities that must not collide with this structure for it to be built.                                                                         |
|                | alliance_rule_filter              | Alliance filter rule that applies to this structure.                                                                                               |
|                | exclude_tags                      | List of tags for entities that are excluded from colliding with this structure.                                                                     |
|                | include_tags                      | List of tags for entities that are included as blockers for this structure.                                                                         |
| **String**     | **exclusive_zone_alliance_rule**  | Alliance rule that defines the restrictions for this exclusive zone.                                                                                |
| **Boolean**    | **invalid_block_snaptoground**    | Determines whether to snap to ground blocks below the building when checking for invalid blocks.                                                    |
| **Array**      | **invalid_block_tags**            | List of block tag types that this buildable cannot be placed on.                                                                                   |
| **Array**      | **invalid_blocks**                | List of block types (e.g., dirt, air, water, lava) that this structure cannot be built upon.                                                       |
| **Array**      | **recipe**                        | An array of items and their corresponding amounts needed for crafting this structure.                                                               |
| **Array**      | **ticket_cost**                   | An array representing the cost in tickets and their respective amounts required for this structure.                                                 |
| **Array**      | **unlock**                        | List of items and amounts required to unlock certain buildings.                                                                                     |
| **String**     | **zone_alliance_rule**            | Alliance rule filter that determines the zones in which this structure can be placed (e.g., any_team, enemy, friendly, etc.).                      |

---

### badger:buildable_structure

Dictates what to place as the structure.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| build_time| | How long it takes for the buildable to be constructed in the world in seconds. |
| String| buildable_placed_player_trigger| | The name of the Presentation Trigger to fire on the PLAYER when placing this structure. |
| String| construction_end_trigger| | The name of the Presentation Trigger to fire when construction ends. |
| String| construction_start_trigger| | The name of the Presentation Trigger to fire when construction starts. |
| JSON Object| faction_variations| | Alternative NBT file to place for a given faction name. |
| String| nbt_file| | Name of the nbt file to place as the structure. |
| Boolean| silence_destroyed_trigger_when_killed| | Whether the on_building_destroyed trigger should be silenced when the building is killed. |

### badger:campaign_team

Some general attributes that describe a faction.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| String| team_tag| | Name of the team to put this entity on. |

### badger:causes_fear

Defines this entity as a status effect that causes its target to fear the status inflictor.

### badger:chunk_reload_indestructible_flag

Used to exclude a buildable from a destructive world reload

### badger:cinematic_death

The entity should play a cinematic on death. This encodes which cinematic plays. Note there is a 60 second failsafe to ensure the entity dies.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| List| cine_id| | The IDs of the cinematic that plays when this entity dies. |

### badger:claimed_area_excluded

For entities that should always be excluded from ClaimedAreaSystem. This is only in regards to bases being assigned to claimed areas.

### badger:clear_from_zone_of_control

Flags this entity to be removed if it overlaps with a Zone of Control of an entity that passes the tag filter

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Array| exclude_tags| | The tags to be excluded in when filtering |
| Array| filter_tag_set| | Tags that filters out entities with ZOC |
| Array| include_tags| | The tags to be included in when filtering |

### badger:collision_weight

Defines the weight of an entity.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| weight| | Defines weight of this entity for the purposes of collision with other entities. |

### badger:conditionally_solid_blocks

Defines which extra blocks will be considered solid during pathfinding & collision checks.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Array| exclude_tags| | Defines the exclude set of block tags to consider conditionally solid. |
| Array| include_tags| | Defines the include set of block tags to consider conditionally solid. |

### badger:conflict_config

Configuration data if this entity has special overrides when managed by a conflict coordinator.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| String| invulnerable_behavior| | Behaviour to use when this entity is invulnerable via a conflict coordinator. Reverted to default when vulnerable. |

### badger:conflict_coordinator

Declares a (spawn controller) entity to be a conflict coordinator, prolonging spawned mob conflict until player interaction.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| String| activated_tag| | Tag added/removed from the coordinator when activated. |
| Decimal| player_engagement_distance| | Distance from the coordinator a player needs to be, to be considered as engaged. |

### badger:core_resource_consumer

Dictates resource consumer behaviour.

| **Type**       | **Name**                          | **Description**                                                                                                                                     |
|----------------|-----------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------|
| **JSON Object**| **blocker_entities**              | The entities that must not collide with this structure for it to be built.                                                                         |
|                | alliance_rule_filter              | Alliance filter rule that applies to this structure.                                                                                               |
|                | exclude_tags                      | List of tags for entities that are excluded from colliding with this structure.                                                                     |
|                | include_tags                      | List of tags for entities that are included as blockers for this structure.                                                                         |
| **String**     | **exclusive_zone_alliance_rule**  | Alliance rule that defines the restrictions for this exclusive zone.                                                                                |
| **Boolean**    | **invalid_block_snaptoground**    | Determines whether to snap to ground blocks below the building when checking for invalid blocks.                                                    |
| **Array**      | **invalid_block_tags**            | List of block tag types that this buildable cannot be placed on.                                                                                   |
| **Array**      | **invalid_blocks**                | List of block types (e.g., dirt, air, water, lava) that this structure cannot be built upon.                                                       |
| **Array**      | **recipe**                        | An array of items and their corresponding amounts needed for crafting this structure.                                                               |
| **Array**      | **ticket_cost**                   | An array representing the cost in tickets and their respective amounts required for this structure.                                                 |
| **Array**      | **unlock**                        | List of items and amounts required to unlock certain buildings.                                                                                     |
| **String**     | **zone_alliance_rule**            | Alliance rule filter that determines the zones in which this structure can be placed (e.g., any_team, enemy, friendly, etc.).                      |

---

### badger:counterattacker

Defines counterattacks that are performed when the entity is attacked by others.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| List| counterattacks| | List of counterattacks. |

### badger:culture

Base culture values for entity

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| base_culture_a| | Base culture Score_A value intended for scoring buildings. Todo: Can later be improved to do dynamic culture modifiers based on upgrades. |
| Decimal| base_culture_b| | Base culture Score_A value intended for scoring buildings. Todo: Can later be improved to do dynamic culture modifiers based on upgrades. |
| Decimal| base_culture_c| | Base culture Score_A value intended for scoring buildings. Todo: Can later be improved to do dynamic culture modifiers based on upgrades. |
| Boolean| is_hq| | Does this culture count as an HQ (used for determining new buildings culture type, for now) |

### badger:culture_status

Data related to the accumulation of culture.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Integer| thresholdA| | Reward threshold for culture value A. |
| Integer| thresholdB| | Reward threshold for culture value B. |
| Integer| thresholdC| | Reward threshold for culture value C. |
| Array| thresholds| | Array of culture reward thresholds. |

### badger:damage_influence_source

Provides a list of sources of additive or substractive damage influence for this AI.

| **Type**       | **Name**                            | **Description**                                                                                                                                         |
|----------------|-------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------|
| **List**       | **sensors**                         | A collection of damage sources with specific filters and settings.                                                                                     |
|                | **damage_receiver_filter**          | Tracks particular types of units or structures that receive damage.                                                                                   |
|                | **alliance_rule_filter**            | Specifies team alliance rules for tracking damage sources.                                                                                            |
|                | **exclude_tags**                    | Tags for units that should be excluded from receiving damage.                                                                                          |
|                | **include_tags**                    | Tags for units that should be included in damage tracking.                                                                                             |
|                | **damage_type_filter**              | Tracks specific types of damage relevant to the sensors.                                                                                              |
|                | **exclude_any**                     | Excludes any tags of damage from being considered.                                                                                                     |
|                | **include_all**                     | Includes all tags of damage for tracking purposes.                                                                                                     |
|                | **inflictor_or_receiver_position**  | Designates whether the tracking occurs at the inflictor (source of damage) or receiver (recipient of damage) position.                                |
|                | **multiplier**                      | The weight or intensity of the heat stamp applied to this damage source.                                                                               |
|                | **propagation_decay**               | The rate at which the heat stamp decays or spreads over time for this damage source.                                                                   |

---

### badger:damage_over_time

Makes a status effect apply damage over time to its host.

| **Type**       | **Name**         | **Description**                                                                                     |
|----------------|------------------|-----------------------------------------------------------------------------------------------------|
| **JSON Object**| **damage**       | Contains the damage data applied by this effect.                                                  |
|                | damage_amount     | Specifies the amount of damage inflicted.                                                           |
|                | damage_types      | A list of damage type names, represented as strings.                                              |
| **Decimal**    | **interval**     | Defines the time interval, in seconds, at which damage is applied.                                |

---

### badger:damage_receiver

Allows entities to be damaged by attacks.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Boolean| attack_slots| | Specifies whether this damage receiver generates specified attack spots for attackers |
| String| auto_generate_shape| | Set this to 'capsule', 'cylinder' or 'aabb' to auto generate the damage receiver shape using the entity's collision aabb. |
| JSON Object| damage_resistances| | Specifies the damage resistance values |
| String| shape| | Defines the shape of the damage receiver (the shape tested as the recipient for damage during an attack) to be used for collision against damage applicators (weapons' collision shapes). |

### badger:damage_receiver_material

Defines the material tag to use when an entity is hit with an action.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| String| material| | Defines the material to be used for presentation events when this entity is hit by an appropriate damage applicator. |

### badger:damage_source_telemetry_tracker

Determines whether or not the latest damage source is tracked for telemetry by this entity

### badger:death_telemetry_tracking

Defines the settings used when tracking telemetry for killed entities.

### badger:deconstruction

Scalar values to tune the amount of resources refunded and the time to deconstruct structures

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Boolean| clear_siblings| | Set to true if you wish all sibling (multistep) entities should be removed on deconstruction |
| Decimal| deconstruct_time_percent| | The scalar to speed up/slow down deconstruction of the entity |
| Boolean| isIndestructable| | A flag used to determine if the entity cannot be deconstructed |
| Array| resource_cost| | The list of resources required to apply this deconstruction action. |
| Decimal| resource_refund_percent| | The scalar to refund more/less of the resources that was required to construct the entity |
| Array| ticket_cost| | The list of tickets required to apply this deconstruction action. |
| Array| unlock| | The list of unlocks required to apply this deconstruction action. |

### badger:delayed_jump

An entity with this component is able to jump after leaving the group for a set amount of time.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| delay_time| | The time that the player will still be able to jump after no longer touching the ground. |

### badger:destroy_on_village_destruction

Determines if this entity has custom destruction behaviour when its owner village is destroyed.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Boolean| group| | The special destruction behaviour to use (eg. staggered destruction). Correlates to what is defined in badger:village_heart_destruction on the village entity. |
| Boolean| ignore| false| If this entity should not be destroyed when the village is destroyed. |

### badger:destruction_vfx

Customized VFX behaviour upon being destroyed.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Boolean| always_play_vfx| | Whether or not to force the destruction VFX. |

### `badger:difficulty_modifier_accuracy_min_range` & `badger:difficulty_modifier_accuracy_max_range`

Modifiers for min and max accuracy range based on difficulty.

**Custom Game Settings**:

| **Type**       | **Name**                     | **Description**                                                                                                  |
|----------------|------------------------------|------------------------------------------------------------------------------------------------------------------|
| **JSON Object**| **custom_game_settings**      | Defines which custom game settings impact this numeric modifier.                                                |
|                | **numeric_modifier_type**     | Type of modifier for the named custom game setting. Valid values include: `pre_multiply`, `pre_add`, `pre_subtract`, `pre_divide`, and their corresponding `post_` counterparts. Multiple settings with the same modifier will stack. |
|                | **scaling_factor**            | Multiplier that adjusts how much the custom game setting affects this numeric modifier. The formula used is: `((CustomGameSettingValue - 1.0) * ScalingFactor) + 1.0`. Useful for scaling modifiers differently. |
|                | **setting_name**              | Name of the custom game setting to read from.                                                                    |

---

**Difficulty Settings**:

| **Type**       | **Difficulty**  | **Modifiers**                                                                                                          |
|----------------|------------------|------------------------------------------------------------------------------------------------------------------------|
| **JSON Object**| **difficulties**  | Settings for each difficulty level.                                                                                    |
|                | **Custom**        | Custom difficulty modifiers.                                                                                           |
|                | **Easy**          | Easy difficulty modifiers.                                                                                             |
|                | **Normal**        | Normal difficulty modifiers.                                                                                           |
|                | **Hard**          | Hard difficulty modifiers.                                                                                             |
|                | **Peaceful**      | Peaceful difficulty modifiers.                                                                                         |

For each difficulty, the following modifiers are applicable:

| **Modifier**              | **Description**                                                                 |
|--------------------------|---------------------------------------------------------------------------------|
| **post_add**             | Amount added after all other modifier values.                                   |
| **post_multiply**        | Percentage multiplied after all pre-modifier values are applied.               |
| **pre_add**              | Amount added after pre-multiply values, but before any other modifiers.        |
| **pre_multiply**         | Percentage multiplied before any other modifier values are applied.            |
| **wave_post_multiply**   | Multiplied by the Wave Level, then added after all other modifiers. Applied only if the `WAVE_SCALING_DIFFICULTY` game rule is true. |

---

### badger:difficulty_modifier_build_speed

Modifiers for build speed based on difficulty and custom game options

**Custom Game Settings**:

| **Type**       | **Name**                     | **Description**                                                                                                  |
|----------------|------------------------------|------------------------------------------------------------------------------------------------------------------|
| **JSON Object**| **custom_game_settings**      | Defines which custom game settings impact this numeric modifier.                                                |
|                | **numeric_modifier_type**     | Type of modifier for the named custom game setting. Valid values include: `pre_multiply`, `pre_add`, `pre_subtract`, `pre_divide`, and their corresponding `post_` counterparts. Multiple settings with the same modifier will stack. |
|                | **scaling_factor**            | Multiplier that adjusts how much the custom game setting affects this numeric modifier. The formula used is: `((CustomGameSettingValue - 1.0) * ScalingFactor) + 1.0`. Useful for scaling modifiers differently. |
|                | **setting_name**              | Name of the custom game setting to read from.                                                                    |

---

**Difficulty Settings**:

| **Type**       | **Difficulty**  | **Modifiers**                                                                                                          |
|----------------|------------------|------------------------------------------------------------------------------------------------------------------------|
| **JSON Object**| **difficulties**  | Settings for each difficulty level.                                                                                    |
|                | **Custom**        | Custom difficulty modifiers.                                                                                           |
|                | **Easy**          | Easy difficulty modifiers.                                                                                             |
|                | **Normal**        | Normal difficulty modifiers.                                                                                           |
|                | **Hard**          | Hard difficulty modifiers.                                                                                             |
|                | **Peaceful**      | Peaceful difficulty modifiers.                                                                                         |

For each difficulty, the following modifiers are applicable:

| **Modifier**              | **Description**                                                                 |
|--------------------------|---------------------------------------------------------------------------------|
| **post_add**             | Amount added after all other modifier values.                                   |
| **post_multiply**        | Percentage multiplied after all pre-modifier values are applied.               |
| **pre_add**              | Amount added after pre-multiply values, but before any other modifiers.        |
| **pre_multiply**         | Percentage multiplied before any other modifier values are applied.            |

---

### badger:difficulty_modifier_building_cost

Modifier for building costs based on difficulty or custom game setting.

**Custom Game Settings**:

| **Type**       | **Name**                     | **Description**                                                                                                  |
|----------------|------------------------------|------------------------------------------------------------------------------------------------------------------|
| **JSON Object**| **custom_game_settings**      | Defines which custom game settings impact this numeric modifier.                                                |
|                | **numeric_modifier_type**     | Type of modifier for the named custom game setting. Valid values include: `pre_multiply`, `pre_add`, `pre_subtract`, `pre_divide`, and their corresponding `post_` counterparts. Multiple settings with the same modifier will stack. |
|                | **scaling_factor**            | Multiplier that adjusts how much the custom game setting affects this numeric modifier. The formula used is: `((CustomGameSettingValue - 1.0) * ScalingFactor) + 1.0`. Useful for scaling modifiers differently. |
|                | **setting_name**              | Name of the custom game setting to read from.                                                                    |

---

**Difficulty Settings**:

| **Type**       | **Difficulty**  | **Modifiers**                                                                                                          |
|----------------|------------------|------------------------------------------------------------------------------------------------------------------------|
| **JSON Object**| **difficulties**  | Settings for each difficulty level.                                                                                    |
|                | **Custom**        | Custom difficulty modifiers.                                                                                           |
|                | **Easy**          | Easy difficulty modifiers.                                                                                             |
|                | **Normal**        | Normal difficulty modifiers.                                                                                           |
|                | **Hard**          | Hard difficulty modifiers.                                                                                             |
|                | **Peaceful**      | Peaceful difficulty modifiers.                                                                                         |

For each difficulty, the following modifiers are applicable:

| **Modifier**              | **Description**                                                                 |
|--------------------------|---------------------------------------------------------------------------------|
| **post_add**             | Amount added after all other modifier values.                                   |
| **post_multiply**        | Percentage multiplied after all pre-modifier values are applied.               |
| **pre_add**              | Amount added after pre-multiply values, but before any other modifiers.        |
| **pre_multiply**         | Percentage multiplied before any other modifier values are applied.            |

---

### badger:difficulty_modifier_damage

Modifiers for damage based on difficulty

**Custom Game Settings**:

| **Type**       | **Name**                     | **Description**                                                                                                  |
|----------------|------------------------------|------------------------------------------------------------------------------------------------------------------|
| **JSON Object**| **custom_game_settings**      | Defines which custom game settings impact this numeric modifier.                                                |
|                | **numeric_modifier_type**     | Type of modifier for the named custom game setting. Valid values include: `pre_multiply`, `pre_add`, `pre_subtract`, `pre_divide`, and their corresponding `post_` counterparts. Multiple settings with the same modifier will stack. |
|                | **scaling_factor**            | Multiplier that adjusts how much the custom game setting affects this numeric modifier. The formula used is: `((CustomGameSettingValue - 1.0) * ScalingFactor) + 1.0`. Useful for scaling modifiers differently. |
|                | **setting_name**              | Name of the custom game setting to read from.                                                                    |

---

**Difficulty Settings**:

| **Type**       | **Difficulty**  | **Modifiers**                                                                                                          |
|----------------|------------------|------------------------------------------------------------------------------------------------------------------------|
| **JSON Object**| **difficulties**  | Settings for each difficulty level.                                                                                    |
|                | **Custom**        | Custom difficulty modifiers.                                                                                           |
|                | **Easy**          | Easy difficulty modifiers.                                                                                             |
|                | **Normal**        | Normal difficulty modifiers.                                                                                           |
|                | **Hard**          | Hard difficulty modifiers.                                                                                             |
|                | **Peaceful**      | Peaceful difficulty modifiers.                                                                                         |

For each difficulty, the following modifiers are applicable:

| **Modifier**              | **Description**                                                                 |
|--------------------------|---------------------------------------------------------------------------------|
| **post_add**             | Amount added after all other modifier values.                                   |
| **post_multiply**        | Percentage multiplied after all pre-modifier values are applied.               |
| **pre_add**              | Amount added after pre-multiply values, but before any other modifiers.        |
| **pre_multiply**         | Percentage multiplied before any other modifier values are applied.            |
| **wave_post_multiply**   | Multiplied by the Wave Level, then added after all other modifiers. Applied only if the `WAVE_SCALING_DIFFICULTY` game rule is true. |

---

### badger:difficulty_modifier_fall_damage

Modifiers for fall damage on difficulty and custom game option

**Custom Game Settings**:

| **Type**       | **Name**                     | **Description**                                                                                                  |
|----------------|------------------------------|------------------------------------------------------------------------------------------------------------------|
| **JSON Object**| **custom_game_settings**      | Defines which custom game settings impact this numeric modifier.                                                |
|                | **numeric_modifier_type**     | Type of modifier for the named custom game setting. Valid values include: `pre_multiply`, `pre_add`, `pre_subtract`, `pre_divide`, and their corresponding `post_` counterparts. Multiple settings with the same modifier will stack. |
|                | **scaling_factor**            | Multiplier that adjusts how much the custom game setting affects this numeric modifier. The formula used is: `((CustomGameSettingValue - 1.0) * ScalingFactor) + 1.0`. Useful for scaling modifiers differently. |
|                | **setting_name**              | Name of the custom game setting to read from.                                                                    |

---

**Difficulty Settings**:

| **Type**       | **Difficulty**  | **Modifiers**                                                                                                          |
|----------------|------------------|------------------------------------------------------------------------------------------------------------------------|
| **JSON Object**| **difficulties**  | Settings for each difficulty level.                                                                                    |
|                | **Custom**        | Custom difficulty modifiers.                                                                                           |
|                | **Easy**          | Easy difficulty modifiers.                                                                                             |
|                | **Normal**        | Normal difficulty modifiers.                                                                                           |
|                | **Hard**          | Hard difficulty modifiers.                                                                                             |
|                | **Peaceful**      | Peaceful difficulty modifiers.                                                                                         |

For each difficulty, the following modifiers are applicable:

| **Modifier**              | **Description**                                                                 |
|--------------------------|---------------------------------------------------------------------------------|
| **post_add**             | Amount added after all other modifier values.                                   |
| **post_multiply**        | Percentage multiplied after all pre-modifier values are applied.               |
| **pre_add**              | Amount added after pre-multiply values, but before any other modifiers.        |
| **pre_multiply**         | Percentage multiplied before any other modifier values are applied.            |

---

### badger:difficulty_modifier_fall_damage_distance

Modifiers for fall damage distance based on difficulty and custom game option

**Custom Game Settings**:

| **Type**       | **Name**                     | **Description**                                                                                                  |
|----------------|------------------------------|------------------------------------------------------------------------------------------------------------------|
| **JSON Object**| **custom_game_settings**      | Defines which custom game settings impact this numeric modifier.                                                |
|                | **numeric_modifier_type**     | Type of modifier for the named custom game setting. Valid values include: `pre_multiply`, `pre_add`, `pre_subtract`, `pre_divide`, and their corresponding `post_` counterparts. Multiple settings with the same modifier will stack. |
|                | **scaling_factor**            | Multiplier that adjusts how much the custom game setting affects this numeric modifier. The formula used is: `((CustomGameSettingValue - 1.0) * ScalingFactor) + 1.0`. Useful for scaling modifiers differently. |
|                | **setting_name**              | Name of the custom game setting to read from.                                                                    |

---

**Difficulty Settings**:

| **Type**       | **Difficulty**  | **Modifiers**                                                                                                          |
|----------------|------------------|------------------------------------------------------------------------------------------------------------------------|
| **JSON Object**| **difficulties**  | Settings for each difficulty level.                                                                                    |
|                | **Custom**        | Custom difficulty modifiers.                                                                                           |
|                | **Easy**          | Easy difficulty modifiers.                                                                                             |
|                | **Normal**        | Normal difficulty modifiers.                                                                                           |
|                | **Hard**          | Hard difficulty modifiers.                                                                                             |
|                | **Peaceful**      | Peaceful difficulty modifiers.                                                                                         |

For each difficulty, the following modifiers are applicable:

| **Modifier**              | **Description**                                                                 |
|--------------------------|---------------------------------------------------------------------------------|
| **post_add**             | Amount added after all other modifier values.                                   |
| **post_multiply**        | Percentage multiplied after all pre-modifier values are applied.               |
| **pre_add**              | Amount added after pre-multiply values, but before any other modifiers.        |
| **pre_multiply**         | Percentage multiplied before any other modifier values are applied.            |

---

### badger:difficulty_modifier_gravity

Modifiers for gravity based on difficulty and custom game options

**Custom Game Settings**:

| **Type**       | **Name**                     | **Description**                                                                                                  |
|----------------|------------------------------|------------------------------------------------------------------------------------------------------------------|
| **JSON Object**| **custom_game_settings**      | Defines which custom game settings impact this numeric modifier.                                                |
|                | **numeric_modifier_type**     | Type of modifier for the named custom game setting. Valid values include: `pre_multiply`, `pre_add`, `pre_subtract`, `pre_divide`, and their corresponding `post_` counterparts. Multiple settings with the same modifier will stack. |
|                | **scaling_factor**            | Multiplier that adjusts how much the custom game setting affects this numeric modifier. The formula used is: `((CustomGameSettingValue - 1.0) * ScalingFactor) + 1.0`. Useful for scaling modifiers differently. |
|                | **setting_name**              | Name of the custom game setting to read from.                                                                    |

---

**Difficulty Settings**:

| **Type**       | **Difficulty**  | **Modifiers**                                                                                                          |
|----------------|------------------|------------------------------------------------------------------------------------------------------------------------|
| **JSON Object**| **difficulties**  | Settings for each difficulty level.                                                                                    |
|                | **Custom**        | Custom difficulty modifiers.                                                                                           |
|                | **Easy**          | Easy difficulty modifiers.                                                                                             |
|                | **Normal**        | Normal difficulty modifiers.                                                                                           |
|                | **Hard**          | Hard difficulty modifiers.                                                                                             |
|                | **Peaceful**      | Peaceful difficulty modifiers.                                                                                         |

For each difficulty, the following modifiers are applicable:

| **Modifier**              | **Description**                                                                 |
|--------------------------|---------------------------------------------------------------------------------|
| **post_add**             | Amount added after all other modifier values.                                   |
| **post_multiply**        | Percentage multiplied after all pre-modifier values are applied.               |
| **pre_add**              | Amount added after pre-multiply values, but before any other modifiers.        |
| **pre_multiply**         | Percentage multiplied before any other modifier values are applied.            |

---

### badger:difficulty_modifier_health_regeneration

Modifier for health regeneration based on difficulty or custom game setting.

**Custom Game Settings**:

| **Type**       | **Name**                     | **Description**                                                                                                  |
|----------------|------------------------------|------------------------------------------------------------------------------------------------------------------|
| **JSON Object**| **custom_game_settings**      | Defines which custom game settings impact this numeric modifier.                                                |
|                | **numeric_modifier_type**     | Type of modifier for the named custom game setting. Valid values include: `pre_multiply`, `pre_add`, `pre_subtract`, `pre_divide`, and their corresponding `post_` counterparts. Multiple settings with the same modifier will stack. |
|                | **scaling_factor**            | Multiplier that adjusts how much the custom game setting affects this numeric modifier. The formula used is: `((CustomGameSettingValue - 1.0) * ScalingFactor) + 1.0`. Useful for scaling modifiers differently. |
|                | **setting_name**              | Name of the custom game setting to read from.                                                                    |

---

**Difficulty Settings**:

| **Type**       | **Difficulty**  | **Modifiers**                                                                                                          |
|----------------|------------------|------------------------------------------------------------------------------------------------------------------------|
| **JSON Object**| **difficulties**  | Settings for each difficulty level.                                                                                    |
|                | **Custom**        | Custom difficulty modifiers.                                                                                           |
|                | **Easy**          | Easy difficulty modifiers.                                                                                             |
|                | **Normal**        | Normal difficulty modifiers.                                                                                           |
|                | **Hard**          | Hard difficulty modifiers.                                                                                             |
|                | **Peaceful**      | Peaceful difficulty modifiers.                                                                                         |

For each difficulty, the following modifiers are applicable:

| **Modifier**              | **Description**                                                                 |
|--------------------------|---------------------------------------------------------------------------------|
| **post_add**             | Amount added after all other modifier values.                                   |
| **post_multiply**        | Percentage multiplied after all pre-modifier values are applied.               |
| **pre_add**              | Amount added after pre-multiply values, but before any other modifiers.        |
| **pre_multiply**         | Percentage multiplied before any other modifier values are applied.            |

---

### badger:difficulty_modifier_jump_gravity

Modifiers for jump gravity based on difficulty and custom game options

**Custom Game Settings**:

| **Type**       | **Name**                     | **Description**                                                                                                  |
|----------------|------------------------------|------------------------------------------------------------------------------------------------------------------|
| **JSON Object**| **custom_game_settings**      | Defines which custom game settings impact this numeric modifier.                                                |
|                | **numeric_modifier_type**     | Type of modifier for the named custom game setting. Valid values include: `pre_multiply`, `pre_add`, `pre_subtract`, `pre_divide`, and their corresponding `post_` counterparts. Multiple settings with the same modifier will stack. |
|                | **scaling_factor**            | Multiplier that adjusts how much the custom game setting affects this numeric modifier. The formula used is: `((CustomGameSettingValue - 1.0) * ScalingFactor) + 1.0`. Useful for scaling modifiers differently. |
|                | **setting_name**              | Name of the custom game setting to read from.                                                                    |

---

**Difficulty Settings**:

| **Type**       | **Difficulty**  | **Modifiers**                                                                                                          |
|----------------|------------------|------------------------------------------------------------------------------------------------------------------------|
| **JSON Object**| **difficulties**  | Settings for each difficulty level.                                                                                    |
|                | **Custom**        | Custom difficulty modifiers.                                                                                           |
|                | **Easy**          | Easy difficulty modifiers.                                                                                             |
|                | **Normal**        | Normal difficulty modifiers.                                                                                           |
|                | **Hard**          | Hard difficulty modifiers.                                                                                             |
|                | **Peaceful**      | Peaceful difficulty modifiers.                                                                                         |

For each difficulty, the following modifiers are applicable:

| **Modifier**              | **Description**                                                                 |
|--------------------------|---------------------------------------------------------------------------------|
| **post_add**             | Amount added after all other modifier values.                                   |
| **post_multiply**        | Percentage multiplied after all pre-modifier values are applied.               |
| **pre_add**              | Amount added after pre-multiply values, but before any other modifiers.        |
| **pre_multiply**         | Percentage multiplied before any other modifier values are applied.            |

---

### badger:difficulty_modifier_jump_height

Modifiers for jump height based on difficulty and custom game options

**Custom Game Settings**:

| **Type**       | **Name**                     | **Description**                                                                                                  |
|----------------|------------------------------|------------------------------------------------------------------------------------------------------------------|
| **JSON Object**| **custom_game_settings**      | Defines which custom game settings impact this numeric modifier.                                                |
|                | **numeric_modifier_type**     | Type of modifier for the named custom game setting. Valid values include: `pre_multiply`, `pre_add`, `pre_subtract`, `pre_divide`, and their corresponding `post_` counterparts. Multiple settings with the same modifier will stack. |
|                | **scaling_factor**            | Multiplier that adjusts how much the custom game setting affects this numeric modifier. The formula used is: `((CustomGameSettingValue - 1.0) * ScalingFactor) + 1.0`. Useful for scaling modifiers differently. |
|                | **setting_name**              | Name of the custom game setting to read from.                                                                    |

---

**Difficulty Settings**:

| **Type**       | **Difficulty**  | **Modifiers**                                                                                                          |
|----------------|------------------|------------------------------------------------------------------------------------------------------------------------|
| **JSON Object**| **difficulties**  | Settings for each difficulty level.                                                                                    |
|                | **Custom**        | Custom difficulty modifiers.                                                                                           |
|                | **Easy**          | Easy difficulty modifiers.                                                                                             |
|                | **Normal**        | Normal difficulty modifiers.                                                                                           |
|                | **Hard**          | Hard difficulty modifiers.                                                                                             |
|                | **Peaceful**      | Peaceful difficulty modifiers.                                                                                         |

For each difficulty, the following modifiers are applicable:

| **Modifier**              | **Description**                                                                 |
|--------------------------|---------------------------------------------------------------------------------|
| **post_add**             | Amount added after all other modifier values.                                   |
| **post_multiply**        | Percentage multiplied after all pre-modifier values are applied.               |
| **pre_add**              | Amount added after pre-multiply values, but before any other modifiers.        |
| **pre_multiply**         | Percentage multiplied before any other modifier values are applied.            |

---

### badger:difficulty_modifier_knockback

Modifiers for knockback based on difficulty and custom game option

**Custom Game Settings**:

| **Type**       | **Name**                     | **Description**                                                                                                  |
|----------------|------------------------------|------------------------------------------------------------------------------------------------------------------|
| **JSON Object**| **custom_game_settings**      | Defines which custom game settings impact this numeric modifier.                                                |
|                | **numeric_modifier_type**     | Type of modifier for the named custom game setting. Valid values include: `pre_multiply`, `pre_add`, `pre_subtract`, `pre_divide`, and their corresponding `post_` counterparts. Multiple settings with the same modifier will stack. |
|                | **scaling_factor**            | Multiplier that adjusts how much the custom game setting affects this numeric modifier. The formula used is: `((CustomGameSettingValue - 1.0) * ScalingFactor) + 1.0`. Useful for scaling modifiers differently. |
|                | **setting_name**              | Name of the custom game setting to read from.                                                                    |

---

**Difficulty Settings**:

| **Type**       | **Difficulty**  | **Modifiers**                                                                                                          |
|----------------|------------------|------------------------------------------------------------------------------------------------------------------------|
| **JSON Object**| **difficulties**  | Settings for each difficulty level.                                                                                    |
|                | **Custom**        | Custom difficulty modifiers.                                                                                           |
|                | **Easy**          | Easy difficulty modifiers.                                                                                             |
|                | **Normal**        | Normal difficulty modifiers.                                                                                           |
|                | **Hard**          | Hard difficulty modifiers.                                                                                             |
|                | **Peaceful**      | Peaceful difficulty modifiers.                                                                                         |

For each difficulty, the following modifiers are applicable:

| **Modifier**              | **Description**                                                                 |
|--------------------------|---------------------------------------------------------------------------------|
| **post_add**             | Amount added after all other modifier values.                                   |
| **post_multiply**        | Percentage multiplied after all pre-modifier values are applied.               |
| **pre_add**              | Amount added after pre-multiply values, but before any other modifiers.        |
| **pre_multiply**         | Percentage multiplied before any other modifier values are applied.            |

---

### badger:difficulty_modifier_knockback_resistance

Modifiers for knockback resistance based on difficulty and custom game options

**Custom Game Settings**:

| **Type**       | **Name**                     | **Description**                                                                                                  |
|----------------|------------------------------|------------------------------------------------------------------------------------------------------------------|
| **JSON Object**| **custom_game_settings**      | Defines which custom game settings impact this numeric modifier.                                                |
|                | **numeric_modifier_type**     | Type of modifier for the named custom game setting. Valid values include: `pre_multiply`, `pre_add`, `pre_subtract`, `pre_divide`, and their corresponding `post_` counterparts. Multiple settings with the same modifier will stack. |
|                | **scaling_factor**            | Multiplier that adjusts how much the custom game setting affects this numeric modifier. The formula used is: `((CustomGameSettingValue - 1.0) * ScalingFactor) + 1.0`. Useful for scaling modifiers differently. |
|                | **setting_name**              | Name of the custom game setting to read from.                                                                    |

---

**Difficulty Settings**:

| **Type**       | **Difficulty**  | **Modifiers**                                                                                                          |
|----------------|------------------|------------------------------------------------------------------------------------------------------------------------|
| **JSON Object**| **difficulties**  | Settings for each difficulty level.                                                                                    |
|                | **Custom**        | Custom difficulty modifiers.                                                                                           |
|                | **Easy**          | Easy difficulty modifiers.                                                                                             |
|                | **Normal**        | Normal difficulty modifiers.                                                                                           |
|                | **Hard**          | Hard difficulty modifiers.                                                                                             |
|                | **Peaceful**      | Peaceful difficulty modifiers.                                                                                         |

For each difficulty, the following modifiers are applicable:

| **Modifier**              | **Description**                                                                 |
|--------------------------|---------------------------------------------------------------------------------|
| **post_add**             | Amount added after all other modifier values.                                   |
| **post_multiply**        | Percentage multiplied after all pre-modifier values are applied.               |
| **pre_add**              | Amount added after pre-multiply values, but before any other modifiers.        |
| **pre_multiply**         | Percentage multiplied before any other modifier values are applied.            |

---

### badger:difficulty_modifier_max_health

Modifiers for max health based on difficulty

**Custom Game Settings**:

| **Type**       | **Name**                     | **Description**                                                                                                  |
|----------------|------------------------------|------------------------------------------------------------------------------------------------------------------|
| **JSON Object**| **custom_game_settings**      | Defines which custom game settings impact this numeric modifier.                                                |
|                | **numeric_modifier_type**     | Type of modifier for the named custom game setting. Valid values include: `pre_multiply`, `pre_add`, `pre_subtract`, `pre_divide`, and their corresponding `post_` counterparts. Multiple settings with the same modifier will stack. |
|                | **scaling_factor**            | Multiplier that adjusts how much the custom game setting affects this numeric modifier. The formula used is: `((CustomGameSettingValue - 1.0) * ScalingFactor) + 1.0`. Useful for scaling modifiers differently. |
|                | **setting_name**              | Name of the custom game setting to read from.                                                                    |

---

**Difficulty Settings**:

| **Type**       | **Difficulty**  | **Modifiers**                                                                                                          |
|----------------|------------------|------------------------------------------------------------------------------------------------------------------------|
| **JSON Object**| **difficulties**  | Settings for each difficulty level.                                                                                    |
|                | **Custom**        | Custom difficulty modifiers.                                                                                           |
|                | **Easy**          | Easy difficulty modifiers.                                                                                             |
|                | **Normal**        | Normal difficulty modifiers.                                                                                           |
|                | **Hard**          | Hard difficulty modifiers.                                                                                             |
|                | **Peaceful**      | Peaceful difficulty modifiers.                                                                                         |

For each difficulty, the following modifiers are applicable:

| **Modifier**              | **Description**                                                                 |
|--------------------------|---------------------------------------------------------------------------------|
| **post_add**             | Amount added after all other modifier values.                                   |
| **post_multiply**        | Percentage multiplied after all pre-modifier values are applied.               |
| **pre_add**              | Amount added after pre-multiply values, but before any other modifiers.        |
| **pre_multiply**         | Percentage multiplied before any other modifier values are applied.            |
| **wave_post_multiply**   | Multiplied by the Wave Level, then added after all other modifiers. Applied only if the `WAVE_SCALING_DIFFICULTY` game rule is true. |

---

### badger:difficulty_modifier_resistance

Modifiers for damage resistances based on difficulty

#### Custom Game Settings

| **Type**       | **Name**                     | **Description**                                                                                                                                                   |
|----------------|------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **JSON Object**| **custom_game_settings**      | Defines which custom game settings affect the numeric modifier.                                                                                                |
|                | numeric_modifier_type         | What type of modifier the named custom game setting will apply to this entity. Valid values are: `pre_multiply`, `pre_add`, `pre_subtract`, `pre_divide`, and their corresponding 'post_' counterparts. Effects of multiple game settings set to the same modifier WILL stack. |
|                | scaling_factor                | A multiplier that changes how much the custom game setting will affect the numeric modifier. This is useful for scaling when multiple numeric modifiers need to use the same custom game setting differently. Formula: '((CustomGameSettingValue - 1.0) * ScalingFactor) + 1.0' |
|                | setting_name                  | Name of the custom game setting to read from.                                                                                                                 |

#### Damage Types

| **Type**       | **Name**            | **Description**                                                                                                                                                   |
|----------------|---------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Array**      | **damage_types**     | Modifiers for specified damage types as JSON key names. Modifiers not specified in this array will affect damage resistance across all types.                    |
|                | append_resistance_to_entities | Boolean to append the resistance to entities if absent from the mob data.                                                                                     |
|                | custom_game_settings  | Defines which custom game settings affect this numeric modifier.                                                                                                |
|                | numeric_modifier_type  | What type of modifier the named custom game setting will apply to this entity. Valid values are: `pre_multiply`, `pre_add`, `pre_subtract`, `pre_divide`, and their corresponding 'post_' counterparts. Effects of multiple game settings set to the same modifier WILL stack. |
|                | scaling_factor         | A multiplier that changes how much the custom game setting will affect the numeric modifier. This is useful for scaling when multiple numeric modifiers need to use the same custom game setting differently. Formula: '((CustomGameSettingValue - 1.0) * ScalingFactor) + 1.0' |
|                | setting_name           | Name of the custom game setting to read from.                                                                                                                 |
|                | difficulties           | The settings for each difficulty level.                                                                                                                       |

**Difficulty Settings**:

| **Type**       | **Difficulty**  | **Modifiers**                                                                                                          |
|----------------|------------------|------------------------------------------------------------------------------------------------------------------------|
| **JSON Object**| **difficulties**  | Settings for each difficulty level.                                                                                    |
|                | **Custom**        | Custom difficulty modifiers.                                                                                           |
|                | **Easy**          | Easy difficulty modifiers.                                                                                             |
|                | **Normal**        | Normal difficulty modifiers.                                                                                           |
|                | **Hard**          | Hard difficulty modifiers.                                                                                             |
|                | **Peaceful**      | Peaceful difficulty modifiers.                                                                                         |

For each difficulty, the following modifiers are applicable:

| **Modifier**              | **Description**                                                                 |
|--------------------------|---------------------------------------------------------------------------------|
| **post_add**             | Amount added after all other modifier values.                                   |
| **post_multiply**        | Percentage multiplied after all pre-modifier values are applied.               |
| **pre_add**              | Amount added after pre-multiply values, but before any other modifiers.        |
| **pre_multiply**         | Percentage multiplied before any other modifier values are applied.            |

---

### badger:difficulty_modifier_spawning_cost

Modifier for mob spawning costs based on difficulty or custom game setting.

**Custom Game Settings**:

| **Type**       | **Name**                     | **Description**                                                                                                  |
|----------------|------------------------------|------------------------------------------------------------------------------------------------------------------|
| **JSON Object**| **custom_game_settings**      | Defines which custom game settings impact this numeric modifier.                                                |
|                | **numeric_modifier_type**     | Type of modifier for the named custom game setting. Valid values include: `pre_multiply`, `pre_add`, `pre_subtract`, `pre_divide`, and their corresponding `post_` counterparts. Multiple settings with the same modifier will stack. |
|                | **scaling_factor**            | Multiplier that adjusts how much the custom game setting affects this numeric modifier. The formula used is: `((CustomGameSettingValue - 1.0) * ScalingFactor) + 1.0`. Useful for scaling modifiers differently. |
|                | **setting_name**              | Name of the custom game setting to read from.                                                                    |

---

**Difficulty Settings**:

| **Type**       | **Difficulty**  | **Modifiers**                                                                                                          |
|----------------|------------------|------------------------------------------------------------------------------------------------------------------------|
| **JSON Object**| **difficulties**  | Settings for each difficulty level.                                                                                    |
|                | **Custom**        | Custom difficulty modifiers.                                                                                           |
|                | **Easy**          | Easy difficulty modifiers.                                                                                             |
|                | **Normal**        | Normal difficulty modifiers.                                                                                           |
|                | **Hard**          | Hard difficulty modifiers.                                                                                             |
|                | **Peaceful**      | Peaceful difficulty modifiers.                                                                                         |

For each difficulty, the following modifiers are applicable:

| **Modifier**              | **Description**                                                                 |
|--------------------------|---------------------------------------------------------------------------------|
| **post_add**             | Amount added after all other modifier values.                                   |
| **post_multiply**        | Percentage multiplied after all pre-modifier values are applied.               |
| **pre_add**              | Amount added after pre-multiply values, but before any other modifiers.        |
| **pre_multiply**         | Percentage multiplied before any other modifier values are applied.            |

---

### badger:disable_by_health

Converts any listed blocks to a modified version

| **Type**       | **Name**                          | **Description**                                                                                          |
|----------------|-----------------------------------|----------------------------------------------------------------------------------------------------------|
| **Decimal**    | **block_conversion_cinematic_delay** | The delay in seconds before block conversion starts if a cinematic is played (default: 0).                            |
| **JSON Object**| **block_conversion_states**       | Block conversion data for when the structure is disabled or enabled by health.                          |
|                | **disabled_state**                | Data used when the entity is disabled.                                                                    |
|                | **enabled_state**                 | Data used when the entity is enabled.                                                                     |
| **String**     | **cine_id**                      | The cinematic ID to play when the entity is disabled. This field is optional.                           |
| **Decimal**    | **health_disabled_percent**       | The percentage of health that determines when the entity is disabled.                                   |
| **Decimal**    | **health_enabled_percent**        | The percentage of health that determines when the entity is enabled.                                     |

### badger:disables_actions

Defines this entity as a status effect that disables actions/attacks.

### badger:disables_influence

Defines this entity as a status effect that disables influence on its receiver entity.

| **Type**       | **Name**       | **Description**                                                                                                                                                                 |
|----------------|----------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **JSON Object**| **tag_filter** | The tag filter determines whether influence should be disabled on the receiver entity.                                                                                         |
|                | **exclude_tags** | Tags that the receiver entity must not possess for influence to be disabled.                                                                                                  |
|                | **include_tags** | Tags that the receiver entity must possess for influence to be disabled.                                                                                                     |

---

### badger:disables_movement

Defines this entity as a status effect that disables movement.

### badger:disables_sprint

Defines this entity as a status effect that disables sprint.

### badger:distant_stimulus_movement

Attribute that defines how entity moves to pursue the attacker after being attacked by a distant damage source

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| max_movement_duration| | The maximum amount of time in seconds which the movement is allowed to take |
| Decimal| max_movement_range| | The maximum distance allowed for the movement |
| Integer| min_distant_stimulus_distance| | The minimum distance between the damage source and damage taker for the damage to be considered a distant stimulus |
| Decimal| nearby_ally_propagation_range| | The radius around the attacked entity where nearby allies will also be alerted. |
| String| propagation_exclude_tags| | Tags that nearby mobs must NOT have in order to be alerted when this entity is attacked. |
| String| propagation_include_tags| | Tags that nearby mobs must have in order to be alerted when this entity is attacked. |

### badger:do_not_replace_buildables

Flag Attribute that indicates if the structure can replace existing structures.

### badger:dynamic_scale_interpolate

Component that enables interpolation and specifies an interpolation speed when this entity is dynamically scaled

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| interpolate_time| | The interpolation time in seconds |

### badger:enable_respawn_when_discovered

Adds a flag component that we want to enable features when an entity is discovered

### badger:engineer

Defines this entity as an engineer.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| String| building_behaviour| | The behaviour name to use when building. |
| Decimal| max_travel_timeout| | Maximum duration in seconds for engineer to travel to build zone (will be teleported afterwards). |
| Decimal| travel_timeout| | Duration in seconds for engineer to travel to build zone (a buff will be applied afterwards). |
| String| travel_timeout_buff| | The buff to apply when the engineer times out. |

### badger:engineer_station

Makes this village use engineers.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Boolean| destroy_buildable_on_engineer_destroyed| | If buildables should be destroyed if assigned engineer dies before completion. |
| Decimal| reassignment_delay| | The cooldown time before a build request can be reassigned to another engineer if the current one died. |

### badger:entity_collision

Defines the entity's entity collision properties.

### badger:entity_leap

Enables the leaping functionality for the entity and defines the leap properties.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| downward| | The maximum vertical distance the entity will leap from. |
| Decimal| height_selection_bias| | 0 to 1 value to control how much to bias height proximity over horizontal distance. Defaults to 0.5. |
| Decimal| max_height| | The maximum vertical distance the entity will leap from. |
| Decimal| max_range| | The maximum horizontal distance the entity will leap from. |
| Decimal| max_target_dist_to_destination| | The maximum horizontal distance allowed from a target location to a leap destination. |
| Decimal| max_target_height_to_destination| | The maximum vertical distance allowed from a target location to a leap destination. |
| Decimal| min_height| | The minimum vertical distance the entity will leap from. |
| Decimal| min_range| | The minimum horizontal distance the entity will leap from. |
| Decimal| prep| | The maximum vertical distance the entity will leap from. |
| Decimal| upward| | The maximum vertical distance the entity will leap from. |

### badger:exclude_from_spatial_partition

Controls if this entity should be excluded from spatial queries.

### badger:faction

Some general attributes that describe a faction.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| String| appearance_index| | The appearance index used for faction variation. |
| String| appearance_name| | The appearance name use for faction variation. If not set this faction will not be registered to have an appearance variation. |
| String| description| | Brief description of the faction. |
| String| id| | The unique internal identifier of this faction. |
| Boolean| is_default_appearance| | The default appearance all factions should fallback to. Only one faction is allowed to be the default. |
| String| name| | Name of this faction. |

### badger:fall_damage

Defines the settings used to calculate falling damage.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| damage_per_block| | Defines the damage per block distance fallen past the min damage distance. |
| Decimal| min_damage| | Defines the minimum damage received by an entity that falls the minimum distance. |
| Integer| min_damage_distance| | Defines the minimum distance the entity needs fall to get damaged. Measured in meters. |
| String| trigger_event| | Optional trigger event to be used when fall damage occurs for this entity. |

### badger:fast_travel

Defines parameters for when this entity fast travels

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| cooldown_time| | The amount of time the entity waits after fast travelling. |
| Decimal| windup_time| | The amount of time the entity waits before fast travelling. |

### badger:fast_travel_point

Indicates that this entity supports fast travel functionality

| **Type**       | **Name**                           | **Description**                                                                                                                                                                   |
|----------------|------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **String**     | **incoming_alliance_rule_filter**  | The rule that determines which team's players can arrive at this fast travel point.                                                                                            |
| **String**     | **outgoing_alliance_rule_filter**  | The rule that determines which types of entities will be considered candidates for fast travel (e.g., friendly, enemy, etc.).                                                   |
| **JSON Object**| **outgoing_tag_filter**            | Determines the tag filters and alliance rule for outgoing entities.                                                                                                            |
|                | exclude_tags                       | Entities with any of these tags are excluded from being candidates for fast travel when lured.                                                                                 |
|                | include_tags                       | Entities with these tags are considered candidates for fast travel when lured.                                                                                                 |
| **String**     | **spawn_point_archetype**          | The name of the archetype for the destination spawn point entity.                                                                                                              |

---

### badger:fast_traveller

Indicates that this entity can fast travel.

| **Type**         | **Name**                          | **Description**                                                                                                                           |
|------------------|-----------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------|
| **String**       | **outgoing_alliance_rule_filter** | The rule that determines which types of entities will be considered candidates for fast travel (e.g., friendly, enemy).                   |
| **JSON Object**  | **outgoing_tag_filter**           | Determines the tag filters and alliance rules for outgoing entities.                                                                       |
|                  | **exclude_tags**                  | A list of tags; entities with any of these tags are not candidates for fast travel when lured.                                          |
|                  | **include_tags**                  | A list of tags; entities with these tags are considered candidates for fast travel when lured.                                           |

---

### badger:gatherable_resources

Contains the most commonly available resources to gather at this entity

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Array| gatherable_resource_names| | A list of resources names the entity drops |

### badger:generate_player_village

An entity that creates a village when placed in world

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| String| factionId| | Faction ID name |
| String| ownership_alliance_rule| | Alliance of structures that can become owned by this structure's 'bagder:owned_territory' when it is placed. |
| String| villageId| | Village ID name |

### badger:generate_ruins

With this component entities that contained ownedBlocks will leave behind a portion of their blocks when destroyed.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Integer| max| | maximum block height of columns left behind |
| Integer| min| | minimum block height of columns left behind |

### badger:glide

An entity with this component is capable of gliding.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| drag| | The amount of drag to apply to vertical velocity while gliding, in the range (0, 1] |

### badger:glide_telemetry_tracking

Defines the settings used when tracking telemetry for mounts that are gliding.

### badger:granted_actions

Defines the actions that are given to another entity that this buff entity is applying to.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| List| actions| | List of actions. |

### badger:has_saddle

Granted as a buff to indicated when an entity is seeking out mounts or riders

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Boolean| despawn_if_dismounted| false| Whether or not this entity despawns if it has no rider |
| Decimal| mount_range| 1.0| The minimum distance that an entity with a Saddle buff must be before activating a mount action |
| Decimal| search_range| 10.0| The maximum distance that an entity with a Saddle buff will search for a mount or rider |

### badger:health

Amount of Max health for entity

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| max_health| | How much health this entity has. Entity dies or is destroyed when health reaches zero. |

### badger:health_regeneration

The amount of health that regenerates per second

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| attack_cooldown_seconds| | The cooldown before the entity can regenerate health again after attacks |
| Decimal| health_regen_amount| | The amount of HP (per second) the entity can regenerate |
| Decimal| regen_cooldown_seconds| | The cooldown between each health regeneration |
| Boolean| regen_while_suspended| | Whether or not the entity regens health while suspended |

### badger:heartbeat

An entity with this component will despawn after the timer runs out. The timer is reset by specific (hard coded) interactions related to combat and control.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| min_expire_time| | How much time in seconds before this entity expires if not interacting. |

### badger:hit_effect

Defines a hit effect

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| String| on_hit_target_trigger| | The name of the trigger event to send on the target being hit when this hit effect is applied. |
| String| trigger| | The name of the trigger event to send on the entity applying the hit effect when this hit effect is applied. |

### badger:hud_message_proximity

A list of Hud messages. Each are triggered when player is in proximity

| **Type**   | **Name**                   | **Description**                                                                                                                                     |
|------------|----------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------|
| **List**   | **messages**               | A collection of different HUD messages.                                                                                                            |
|            | cooldown_seconds           | The cooldown period (in seconds) between sending the HUD message.                                                                                  |
|            | filter                     | Tags used to define which type of entity can trigger the HUD message.                                                                               |
|            | alliance_rule_filter       | The alliance rule used to determine which type of entity should trigger the HUD message.                                                            |
|            | exclude                    | Tags that are excluded from filtering when determining which entities can trigger the HUD message.                                                   |
|            | include                    | Tags that are included when filtering for entities that can trigger the HUD message.                                                                |
|            | hud_message_args           | A list of arguments that will be used in the HUD message.                                                                                           |
|            | hud_message_id             | The string ID associated with the HUD message.                                                                                                      |
|            | range                      | The effective range within which an entity must be to trigger the HUD message.                                                                      |

---

### badger:hurtbox

Adding this description to an entity will indicate it is a hurtbox child entity.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Boolean| default_hurtbox| | Designates this as the primary hurtbox. The parent may have either zero or one of these. Used as the default hurtbox to redirect to, when things are targeting redirector hurtboxes. |
| Boolean| no_tickets| | Decides whether or not this entity opts out of inheriting parent's tickets. If set to true this hurtbox will provide no action/attack tickets. |
| Boolean| target_assigner_redirect_to_default| | When true, this will redirect target assigner targeting to the default hurtbox of the parent. |

### badger:hurtbox_tags

Designer prescribed tags that will be transferred to any hurtboxes this entity may have.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Array| tags| | The tags to add to this entity's hurtboxes. |

### badger:influence_map_ai

Denotes this entity as an influence map AI. It will create and manag influence maps.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| map_decay| | Rate of decay of heat wiith regards to time persists in world |
| Decimal| map_resolution| | Resolution of this map |
| Decimal| perception_radius| | Perception range of the map for any influence sources |
| String| query_name| | Query Name for any units assigned to this AI |
| Boolean| sensing_when_idle| | Boolean on whether this AI should be tracking sensor information even when idle (no units/entities currently following this AI). Default set to NOT sense when idle. |

### badger:influencer_coordinator

Defines this entity as a coordinator of one or more (child) influencer entities. A coordinator can be an influencer itself.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Boolean| create_follow_path| | Whether or not to create a follow path. |
| Boolean| despawn_if_empty| | Whether or not the coordinator despawns as soon as there is no entity being influenced by it. |
| Decimal| formation_aspect_ratio| | The aspect ratio of columns to rows for the box formation. |
| Decimal| formation_jitter| | The jitter applied to each position in the box formation. |
| Decimal| formation_separation| | The separation distance between the lure entity and the entities in the box formation. |
| Integer| max_follower_count| | Directly sets the max number of units influenced at once by this entity. If max_followers_resource is also specified it will use the smaller of the two. |
| String| max_followers_resource| | The name of the resource corresponding to the maximum number of entities that can be influenced at once. Unlimited if unspecified. If max_follower_count is also specified it will use the smaller of the two. |
| Decimal| trigger_formation_update_radius| | When the player stops the formation will not update until the player leaves the radius positioned at the players last stationary position |

### badger:interactable

Defines the entity's ability to be interacted with by other entities, and the interactions that can be performed on it.

| **Type**       | **Name**                              | **Description**                                                                                                                                                              |
|----------------|---------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Array**      | **interactions**                      | Entries defining what interactions can be performed on the entity.                                                                                                         |
|                | **alliance_rule_filter**              | Alliance rule that filters which teams can interact with this entity; typically set to "friendly."                                                                         |
|                | **camera**                            | Camera mode to switch to when interacting with this entity; intended for use with "badger:aimable."                                                                        |
|                | **default_disabled**                  | When true, this entity cannot be interacted with by default until another condition enables it.                                                                            |
|                | **delay**                             | Delay, in seconds, before the interact UI appears after highlighting the interactable.                                                                                     |
|                | **exclude_tags**                      | Tags that the _interactor_ must not have in order to interact with this entity.                                                                                           |
|                | **exclusive_interactor**              | When true, only one interactor can interact with this entity at a time.                                                                                                   |
|                | **include_tags**                      | Tags that the _interactor_ must have in order to interact with this entity.                                                                                               |
|                | **input_layout_override**             | Input layout to use when highlighting this interactable, setting the buttons and UI needed for the interaction.                                                             |
|                | **input_layout_under_construction**   | Same purpose as `input_layout_override`, but applies during the construction phase of a buildable entity.                                                                   |
|                | **interaction_priority**              | Defines which interactions take precedence over others when several are in range; zero is the default, with higher values taking priority.                                  |
|                | **name**                              | Name of the interaction.                                                                                                                                                    |
|                | **only_restrict_angle_while_luring**  | When true, the angle restriction defined by `restricted_interact_angle` will only apply if the interactor is luring mobs; this helps reduce input conflicts.                |
|                | **range**                             | The interactor must be within this distance for the interaction to appear.                                                                                                |
|                | **restricted_interact_angle**         | When set, players must aim their cameras at the target within this many degrees to interact with it; when not set, players can interact without aiming at the target.     |

---

### badger:interactor

Defines the entity's ability to interact with other entities, and filters what entities it may interact with.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| String| alliance_rule_filter| | Determines what team an entity must be on for this interactor to interact with it. Set to any_team by default. |
| Array| exclude_tags| | Determines what tags an entity must NOT have for this interactor to interact with it. |
| Array| include_tags| | Determines what tags an entity must have for this interactor to interact with it. |

### badger:interacts_with_blocks

Defines that this entity can interact with world blocks

### badger:invader

For entities that will contribute to the invasion.

### badger:invasion_defense_value

Defines the defensive value of this entity. Used for protecting culture villages.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| defense_multiplier| | Defines this structure's ability to boost all the active defenses of it's village. |
| Integer| defense_value| | Defines this structure's ability to actively defend it's village. |

### badger:inventory

Defines the inventory for this entity.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| drop_force| | The force with which the items are dropped. |
| Decimal| drop_percentage| | The percentage of the inventory that is dropped upon death. |

### badger:item_collection

Defines the rules which regulate which items are able to be collected by players.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| String| alliance_rule_filter| | The alliance rule filter defining whether or not items can be collected by players on a given team. |
| Boolean| allow_failed_collections| | If true, the item will be collected and dissapear even if it adds no resources to the collector's inventory. |
| Decimal| collection_delay| | The time in seconds after dropping before this item can be picked up. |

### badger:jump

An entity with this component is capable of jumping

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| hold_duration| | How long the button will be held down before a full jump is performed. |
| JSON Object| hold_velocity| | The interpolated velocity applied to the entity from the beginning of the jump being held down (start) to the end (end).  This is optional, and by default just ignores gravity |
| Decimal| initial_height| | Approx. how high the entity will jump (on a possessed entity, when tapping the jump button) |
| Boolean| jump_from_water| | Whether or not the entity can jump while in deep water. |
| Integer| number_of_midair_jumps| | Number of midair jumps (double jumps) the entity may make without touching the ground. |
| Array| trigger_event_status_filter| | If one or more status effect specified in this array is active when the entity jumps, trigger the event defined by trigger_event_with_status |
| String| trigger_event_with_status| | If one or more status effect specified in trigger_event_status_filter is active when the entity jumps, this event will be triggered |

### badger:knockback

Defines the settings applying knockback.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| JSON Object| knockback| | Defines an offset, which defines the center point of falloff calculations. (Optional) |

### badger:knockback_resistance

Defines the settings for knock back resistance.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| affected_scale| | Defines how much a mob is affected by knock back. |

### badger:labelled_offsets

A set of labelled positional offsets relative to the entity with this description. Used to define and identify specific points on the entity.

### badger:leap_destination

Marks the entity as a potential destination for entities to leap to.

### badger:leash

Prevents the entity from getting too far from its spawn point.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| leash_range| | The maximum range that the entity can be from its tether point (spawn point). |
| Decimal| return_range| | The range at which the entity will move to once it leaves its leash range. |

### badger:leash_on_stop

Prevents the entity from getting too far from where it last stopped.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| leash_range| | The maximum range that the entity can be from its tether point (where it last stopped). |
| Decimal| return_range| | The range at which the entity will move to once it leaves its leash range. |
| Boolean| return_when_not_targeting| | If true, leashed entity will return when it is not targeting. |

### badger:loot

Dictates the loot drops to spawn when the entity is destroyed.

| **Type** | **Name**    | **Description**                                                                                   |
|----------|-------------|---------------------------------------------------------------------------------------------------|
| **Array**| **overrides**| The overridable loot tables supported by this entity.                                            |
|          | name        | The name of the override used to select the loot table.                                          |
|          | table       | The specific loot table to use if selected.                                                      |
| **String**| **table**   | The loot table that determines the default potential rewards for this entity.                   |

---

### badger:loot_collector

Enables this entity to collect loot.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| String| alliance_rule_filter| | The alliance rule filter determining if the collector can see inventory drops on other teams |
| Decimal| collect_time| | The loot collection time in seconds. |
| Vector [a, b, c]| offset| | The loot collection destination offset. |
| Decimal| radius| | The loot collection radius. |

### badger:lost_entity_recall_point

Attribute that specify the minimum distance between an entity and a recall structure for the entity to be considered lost

| **Type**       | **Name**                           | **Description**                                                                                                       |
|----------------|------------------------------------|-----------------------------------------------------------------------------------------------------------------------|
| **String**     | **alliance_rule_filter**           | The alliance rule that an entity must match for the recall point to potentially recall this entity.                   |
| **Integer**    | **arrival_max_height_difference**  | Maximum height difference between a recalled entity's arrival location and the recall point's location.               |
| **Decimal**    | **arrival_max_radius**             | Maximum radius within which a recalled entity will arrive.                                                            |
| **Decimal**    | **arrival_min_radius**             | Minimum radius within which a recalled entity will arrive.                                                            |
| **Boolean**    | **disable_influencing_during_channel** | If true, entities being recalled by this recall point will not be influenced during the channel.                     |
| **Boolean**    | **include_in_combat**              | If true, the recall point can recall entities that are engaged in combat.                                            |
| **Decimal**    | **min_entity_distance_from_hero**  | The minimum distance between an entity and a hero (the hero should match the alliance rule) for the entity to be considered lost. |
| **Decimal**    | **min_recall_radius**              | The minimum distance between an entity and this recall point for the entity to be considered lost.                   |
| **String**     | **recall_zone_requirement**         | Specifies a zone requirement for recall; recalling from structures outside that zone will no longer work.            |
| **JSON Object**| **tag_filter**                     | The tag filter used to determine if an entity could potentially be recalled to this recall point.                     |
|                | **exclude_tags**                   | Tags that an entity must not have for the recall point to potentially recall this entity.                             |
|                | **include_tags**                   | Tags that an entity must have for the recall point to potentially recall this entity.                                 |

---

### badger:lure

Defines how the entity can lure other entities.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| active_timer| | The number of seconds this lure will actively influence new entities, after which it will become dormant (unless reactivated otherwise). Set to 0 for an instantaneous lure. |
| String| alliance_rule_filter| | The rule that determines which types of entities that will be lured (friendly, enemy, etc.). |
| Boolean| applies_strict_ownership| | When true, this lure will retain "strict ownership" of any influenced mobs, preventing other players from controlling them as long as they are lured. |
| Boolean| can_attack_along_line| | If true, mobs can attack any targets they can reach within the bounds of the linear leash. Requires 'leash_to_line' to be true. |
| Boolean| center_effect_on_owner| | If true, the lure will affect an area around the lure's owner, rather than the lure itself. Lured entities will still move toward the lure itself. |
| String| chained_preceding_lure_archetype| | Spawns the defined lure archetype to act as a precursor to this lure. Mobs that complete the "preceding" lure will then be redirected to move to this lure. |
| String| config_id| | The config name used by lured entities when determining whether to use a non-default lured config. |
| Array| exclude_tags| | The tags required to NOT be on an entity for an effect to trigger. |
| Array| include_tags| | The tags required to be on an entity for an effect to trigger. |
| Integer| influenced_priority| | The priority threshold assigned to entities influenced by this lure. Determines whether the entities it influences can be overridden by other influencers. |
| Decimal| influenced_reprioritization_timer| | The number of seconds this lure need to settle for before reprioritization occurs. |
| Integer| influencer_priority| | The priority of this lure's influence. Determines whether it can influence already-influenced entities. |
| Boolean| leash_to_line| | If true, affected mobs will be leashed to a _line_ from the owner to the lure, rather than just the single point of the lure. Allows for attack-move behaviour. |
| Decimal| lure_range| | The range at which entities can start to be lured. |
| Decimal| max_speed_distance| | The distance from the entity's destination at which the maximum speed factor is applied. |
| Decimal| max_speed_factor| | The multiplier applied to the speed of the lured units based on the distance from the entity's destination. |
| String| on_apply_target_trigger| | The trigger event to send on an entity when it's influenced by this lure. |
| Boolean| release_on_arrival| | If true, entities that reach the lure position will become unlured. Arrival range is defined per entity in the lured attributes |
| Decimal| release_range| | The range at which entities will no longer be influenced by the lure. |
| Integer| reprioritized_influenced_priority| | The influenced priority that entities influenced by this lure are switched to when the reprioritization timer elapses. |
| Boolean| show_influence_direction_line| | Set to true to visualize a dashed line pointing in the direction of movement. |
| Boolean| show_influence_range| | Set to true to visualize influence range. |
| Boolean| sticky_assignment| | If true, entities influenced by this lure will remain assigned even when out of range. |
| String| visual_alliance_rule_filter| | The alliance rule filter defining whether or not to the visualized influence range can seen by players on a given team. |

### badger:lured

Defines settings for when this entity is lured.

| **Type**       | **Name**                     | **Description**                                                                                                                         |
|----------------|------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------|
| **JSON Object**| **configurations**           | Array of configuration overrides used based on the lure's configuration ID.                                                            |
|                | arrive_range                 | The range within which entities will be unlured if the lure is set to release entities upon arrival.                                    |
|                | attack_move_range            | The maximum distance from the lure within which the lured entity can attack move to targets.                                            |
|                | config_id                    | The configuration name that matches with a lure for this specific configuration to be applied.                                          |
|                | leash_range                  | The distance from the lure that the lured entity is allowed to be before it must return to the lure.                                   |
|                | return_range                 | The distance from the lure that the lured entity must move to when initially lured or whenever it exits the leash range before attacking again. |
| **JSON Object**| **default_configuration**     | Default settings applied when this entity is lured.                                                                                     |
|                | arrive_range                 | The range within which entities will be unlured if the lure is set to release entities upon arrival.                                    |
|                | attack_move_range            | The maximum distance from the lure within which the lured entity can attack move to targets.                                            |
|                | leash_range                  | The distance from the lure that the lured entity is allowed to be before it must return to the lure.                                   |
|                | return_range                 | The distance from the lure that the lured entity must move to when initially lured or whenever it exits the leash range before attacking again. |
| **String**     | **resource_requirement**      | The optional unlock required by the influencer to lure this entity.                                                                     |

---

### badger:map3_selectable

Map Entity (for World Map 3) icon on the map screen will be selectable by the cursor

### badger:map3_solo_play_hidden

Map Entity (for World Map 3) will be hidden when in a solo play game

### badger:map3_tooltip

Entity will have an icon on the map screen

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| String| banner| | banner for map tooltip |
| String| description| | description for map tooltip |
| Boolean| is_persistent| | persistent flag for map tooltip |
| String| title| | title for map tooltip |

### badger:map3_tooltip_action

Entity will have an icon on the map screen

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| String| action| | id for the action performed on the tooltip |

### badger:map3_tooltip_audio

audio for detailed map icon tooltip

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| cooldown_time| | [Optional] How long before this audio event can trigger again (resets on map close) |
| String| event_id| | Audio for map tooltip |
| String| event_type| | Audio type for the event. Can be 'Narrative' or 'Music' |
| Decimal| selection_time| | [Optional] How long the icon must be selected for the audio event to trigger |

### badger:map3_tooltip_extended

text for further detailed map icon tooltip

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| String| description| | extended description for map tooltip |
| String| icon| | additional info icon for map tooltip |
| String| priority| | dictate order that extended tooltips appear |

### badger:map_controller

Entity will have a map entity on the world map

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| List| initial_key_values| | The initial key/values this map controller will have. |
| List| initial_team_key_values| | The initial team-specific key/values this map controller will have. |

### badger:map_icon_revealed

Entity will have an icon that is visible on the map screen

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Boolean| teammates_only| | restrict visibility to team |

### badger:meta_aabb

Adding this description to an archetype will indicate to our structure editor it has an editable AABB.

### badger:mob_alliance

Defines whether or not the village entity is a mob alliance village.

### badger:modifier_accuracy_max_range

Buffs an entity's stats.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| post_add| | Amount to add after all other buff values. |
| Decimal| post_multiply| | Percentage to multiply the value by after all pre buff values are applied. |
| Decimal| pre_add| | Amount to add after pre multiply values, but before any other buff values. |
| Decimal| pre_multiply| | Percentage to multiply the value by before any buff values are applied. |
| Boolean| scalable| | More performance costly, but allows this modifier's effect to be scaled up or down. An example would be ramping-down a status effect as it ends. |

### badger:modifier_accuracy_min_range

Buffs an entity's stats.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| post_add| | Amount to add after all other buff values. |
| Decimal| post_multiply| | Percentage to multiply the value by after all pre buff values are applied. |
| Decimal| pre_add| | Amount to add after pre multiply values, but before any other buff values. |
| Decimal| pre_multiply| | Percentage to multiply the value by before any buff values are applied. |
| Boolean| scalable| | More performance costly, but allows this modifier's effect to be scaled up or down. An example would be ramping-down a status effect as it ends. |

### badger:modifier_aimable_range

Buffs an entity's stats.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| post_add| | Amount to add after all other buff values. |
| Decimal| post_multiply| | Percentage to multiply the value by after all pre buff values are applied. |
| Decimal| pre_add| | Amount to add after pre multiply values, but before any other buff values. |
| Decimal| pre_multiply| | Percentage to multiply the value by before any buff values are applied. |
| Boolean| scalable| | More performance costly, but allows this modifier's effect to be scaled up or down. An example would be ramping-down a status effect as it ends. |

### badger:modifier_area_overlays_range

Buffs an area overlay's range.

| **Type** | **Name**     | **Description**                                                                                         |
|----------|--------------|---------------------------------------------------------------------------------------------------------|
| **Array**| **modifiers**| List of modifications to apply to area overlays.                                                       |
|          | post_add     | Amount to add after all other modifier values.                                                         |
|          | post_multiply | Percentage to multiply the value by after all pre-modifier values are applied.                         |
|          | pre_add      | Amount to add after pre-multiply values but before any other modifier values.                          |
|          | pre_multiply  | Percentage to multiply the value by before any other modifier values are applied.                      |
|          | shape        | The shape of the area overlay to buff.                                                                  |
|          | type         | The type of the area overlay to buff.                                                                   |

---

### badger:modifier_autostep

Buffs an entity's stats.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| post_add| | Amount to add after all other buff values. |
| Decimal| post_multiply| | Percentage to multiply the value by after all pre buff values are applied. |
| Decimal| pre_add| | Amount to add after pre multiply values, but before any other buff values. |
| Decimal| pre_multiply| | Percentage to multiply the value by before any buff values are applied. |
| Boolean| scalable| | More performance costly, but allows this modifier's effect to be scaled up or down. An example would be ramping-down a status effect as it ends. |

### badger:modifier_block_revert

Reverts any changed blocks to the original version

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| duration_percentage| | What percentage of the time this block revert process should take |

### badger:modifier_block_swap

Converts any listed blocks to a modified version

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| duration_percentage| | What percentage of the build time this conversion process should take |
| Array| exclude| | The block types to exclude from this block conversion |
| Array| include| | The block types to affect with this block conversion |
| JSON Object| modifiers| | Key value pairs that describe which keywords should be replaced with other strings in block names |
| Boolean| top_to_bottom| | Changes the order of block conversion to top to bottom |

### badger:modifier_build_speed

Buffs an entity's stats.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| post_add| | Amount to add after all other buff values. |
| Decimal| post_multiply| | Percentage to multiply the value by after all pre buff values are applied. |
| Decimal| pre_add| | Amount to add after pre multiply values, but before any other buff values. |
| Decimal| pre_multiply| | Percentage to multiply the value by before any buff values are applied. |
| Boolean| scalable| | More performance costly, but allows this modifier's effect to be scaled up or down. An example would be ramping-down a status effect as it ends. |

### badger:modifier_cooldown

Buffs an entity's stats.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| post_add| | Amount to add after all other buff values. |
| Decimal| post_multiply| | Percentage to multiply the value by after all pre buff values are applied. |
| Decimal| pre_add| | Amount to add after pre multiply values, but before any other buff values. |
| Decimal| pre_multiply| | Percentage to multiply the value by before any buff values are applied. |
| Boolean| scalable| | More performance costly, but allows this modifier's effect to be scaled up or down. An example would be ramping-down a status effect as it ends. |

### badger:modifier_damage

Buffs an entity's stats.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| post_add| | Amount to add after all other buff values. |
| Decimal| post_multiply| | Percentage to multiply the value by after all pre buff values are applied. |
| Decimal| pre_add| | Amount to add after pre multiply values, but before any other buff values. |
| Decimal| pre_multiply| | Percentage to multiply the value by before any buff values are applied. |
| Boolean| scalable| | More performance costly, but allows this modifier's effect to be scaled up or down. An example would be ramping-down a status effect as it ends. |

### badger:modifier_damage_type

Buffs an entity's damage types.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Array| types| | List of the damage types to add. |

### badger:modifier_deconstruction_speed

Buffs an entity's stats.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| post_add| | Amount to add after all other buff values. |
| Decimal| post_multiply| | Percentage to multiply the value by after all pre buff values are applied. |
| Decimal| pre_add| | Amount to add after pre multiply values, but before any other buff values. |
| Decimal| pre_multiply| | Percentage to multiply the value by before any buff values are applied. |
| Boolean| scalable| | More performance costly, but allows this modifier's effect to be scaled up or down. An example would be ramping-down a status effect as it ends. |

### badger:modifier_fall_distance

Buffs an entity's stats.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| post_add| | Amount to add after all other buff values. |
| Decimal| post_multiply| | Percentage to multiply the value by after all pre buff values are applied. |
| Decimal| pre_add| | Amount to add after pre multiply values, but before any other buff values. |
| Decimal| pre_multiply| | Percentage to multiply the value by before any buff values are applied. |
| Boolean| scalable| | More performance costly, but allows this modifier's effect to be scaled up or down. An example would be ramping-down a status effect as it ends. |

### badger:modifier_gravity

Buffs an entity's stats.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| post_add| | Amount to add after all other buff values. |
| Decimal| post_multiply| | Percentage to multiply the value by after all pre buff values are applied. |
| Decimal| pre_add| | Amount to add after pre multiply values, but before any other buff values. |
| Decimal| pre_multiply| | Percentage to multiply the value by before any buff values are applied. |
| Boolean| scalable| | More performance costly, but allows this modifier's effect to be scaled up or down. An example would be ramping-down a status effect as it ends. |

### badger:modifier_health_regen

The amount of health that regenerates per second

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| attack_cooldown_seconds| | The cooldown before the entity can regenerate health again after attacks |
| Decimal| health_regen_amount| | The amount of HP (per second) the entity can regenerate |
| Decimal| regen_cooldown_seconds| | The cooldown between each health regeneration |
| Boolean| regen_while_suspended| | Whether or not the entity regens health while suspended |

### badger:modifier_jump_height

Buffs an entity's stats.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| post_add| | Amount to add after all other buff values. |
| Decimal| post_multiply| | Percentage to multiply the value by after all pre buff values are applied. |
| Decimal| pre_add| | Amount to add after pre multiply values, but before any other buff values. |
| Decimal| pre_multiply| | Percentage to multiply the value by before any buff values are applied. |
| Boolean| scalable| | More performance costly, but allows this modifier's effect to be scaled up or down. An example would be ramping-down a status effect as it ends. |

### badger:modifier_knockback_force

Buffs an entity's stats.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| post_add| | Amount to add after all other buff values. |
| Decimal| post_multiply| | Percentage to multiply the value by after all pre buff values are applied. |
| Decimal| pre_add| | Amount to add after pre multiply values, but before any other buff values. |
| Decimal| pre_multiply| | Percentage to multiply the value by before any buff values are applied. |
| Boolean| scalable| | More performance costly, but allows this modifier's effect to be scaled up or down. An example would be ramping-down a status effect as it ends. |

### badger:modifier_knockback_resistance

Buffs an entity's stats.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| post_add| | Amount to add after all other buff values. |
| Decimal| post_multiply| | Percentage to multiply the value by after all pre buff values are applied. |
| Decimal| pre_add| | Amount to add after pre multiply values, but before any other buff values. |
| Decimal| pre_multiply| | Percentage to multiply the value by before any buff values are applied. |
| Boolean| scalable| | More performance costly, but allows this modifier's effect to be scaled up or down. An example would be ramping-down a status effect as it ends. |

### badger:modifier_max_health

Buffs an entity's stats.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| post_add| | Amount to add after all other buff values. |
| Decimal| post_multiply| | Percentage to multiply the value by after all pre buff values are applied. |
| Decimal| pre_add| | Amount to add after pre multiply values, but before any other buff values. |
| Decimal| pre_multiply| | Percentage to multiply the value by before any buff values are applied. |
| Boolean| scalable| | More performance costly, but allows this modifier's effect to be scaled up or down. An example would be ramping-down a status effect as it ends. |

### badger:modifier_movement_speed

Buffs an entity's stats.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| post_add| | Amount to add after all other buff values. |
| Decimal| post_multiply| | Percentage to multiply the value by after all pre buff values are applied. |
| Decimal| pre_add| | Amount to add after pre multiply values, but before any other buff values. |
| Decimal| pre_multiply| | Percentage to multiply the value by before any buff values are applied. |
| Boolean| scalable| | More performance costly, but allows this modifier's effect to be scaled up or down. An example would be ramping-down a status effect as it ends. |

### badger:modifier_projectile_payload

Defines a list of entities to spawn for a projectile.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| List| entity_ids| | The list of entities to spawn. |

### badger:modifier_resistance

Buffs an entity's damage resistances.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Array| resistances| | List of the damage resistances to add. |

### badger:modifier_saddle

Granted as a buff to indicated when an entity is seeking out mounts or riders

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Boolean| despawn_if_dismounted| false| Whether or not this entity despawns if it has no rider |
| Decimal| mount_range| 1.0| The minimum distance that an entity with a Saddle buff must be before activating a mount action |
| Decimal| search_range| 10.0| The maximum distance that an entity with a Saddle buff will search for a mount or rider |

### badger:modifier_size

Controls the size of this entity

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| scale| | The scaling factor for the size of this entity |

### badger:modifier_stamina_depletion

Buffs an entity's stats.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| post_add| | Amount to add after all other buff values. |
| Decimal| post_multiply| | Percentage to multiply the value by after all pre buff values are applied. |
| Decimal| pre_add| | Amount to add after pre multiply values, but before any other buff values. |
| Decimal| pre_multiply| | Percentage to multiply the value by before any buff values are applied. |
| Boolean| scalable| | More performance costly, but allows this modifier's effect to be scaled up or down. An example would be ramping-down a status effect as it ends. |

### badger:modifier_stamina_regen

Buffs an entity's stats.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| post_add| | Amount to add after all other buff values. |
| Decimal| post_multiply| | Percentage to multiply the value by after all pre buff values are applied. |
| Decimal| pre_add| | Amount to add after pre multiply values, but before any other buff values. |
| Decimal| pre_multiply| | Percentage to multiply the value by before any buff values are applied. |
| Boolean| scalable| | More performance costly, but allows this modifier's effect to be scaled up or down. An example would be ramping-down a status effect as it ends. |

### badger:modifier_stamina_regen_cooldown

Buffs an entity's stats.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| post_add| | Amount to add after all other buff values. |
| Decimal| post_multiply| | Percentage to multiply the value by after all pre buff values are applied. |
| Decimal| pre_add| | Amount to add after pre multiply values, but before any other buff values. |
| Decimal| pre_multiply| | Percentage to multiply the value by before any buff values are applied. |
| Boolean| scalable| | More performance costly, but allows this modifier's effect to be scaled up or down. An example would be ramping-down a status effect as it ends. |

### badger:modifier_targeting_range

Buffs an entity's stats.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| post_add| | Amount to add after all other buff values. |
| Decimal| post_multiply| | Percentage to multiply the value by after all pre buff values are applied. |
| Decimal| pre_add| | Amount to add after pre multiply values, but before any other buff values. |
| Decimal| pre_multiply| | Percentage to multiply the value by before any buff values are applied. |
| Boolean| scalable| | More performance costly, but allows this modifier's effect to be scaled up or down. An example would be ramping-down a status effect as it ends. |

### badger:modifier_wind_up

Buffs an entity's stats.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| post_add| | Amount to add after all other buff values. |
| Decimal| post_multiply| | Percentage to multiply the value by after all pre buff values are applied. |
| Decimal| pre_add| | Amount to add after pre multiply values, but before any other buff values. |
| Decimal| pre_multiply| | Percentage to multiply the value by before any buff values are applied. |
| Boolean| scalable| | More performance costly, but allows this modifier's effect to be scaled up or down. An example would be ramping-down a status effect as it ends. |

### badger:modifier_zone_size

Controls the zone size of this entity

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| scale| | The scaling factor for the zone size of this entity |

### badger:mount

Indicates that an entity can be mounted

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| String| base_skin| | Archetype name of the default skin for this mount type. |
| Array| exclude_tags| | The tags required to NOT be on an entity for an effect to trigger. |
| Array| include_tags| | The tags required to be on an entity for an effect to trigger. |
| String| unlock_requirement| | Name of the unlock resource needed to unlock this as a player mount. If not defined, the mount will not appear as a player mount. |

### badger:move_on_spawn

Makes the entity move after spawn

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Boolean| destroy_on_arrival| | When set to true, the entity will destroy itself after it has travelled to the maximum move distance |
| Decimal| movement_distance| | How far forward the entity will travel after spawn |

### badger:movement

Behavior modifications for movement

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| air_control_factor| | The amount of control authority the entity will have while in air (between 0-1). |
| Boolean| enable_air_control_on_knockback| | Bool to enable air control while being knocked back. |
| Decimal| move_speed| | How fast this entity moves in meters (cubes) per second. |
| Decimal| swim_speed| | How fast this entity moves in meters (cubes) per second while touching water. If left undefined, will default to matching move_speed. |
| Decimal| turn_rate| | Speed at which the entity will rotate to face a new direction, in degrees per second. |

### badger:movement_speed_camera_effects

Defines FOV and camera distance effects that scale off of player move speed.

| **Type**       | **Name**                      | **Description**                                                                                                                                                          |
|----------------|-------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **JSON Object**| **camera_distance**           | Defines effects on the camera follow distance caused by movement speed bonuses.                                                                                         |
|                | **effect_strength**           | Strength of effect. With a value of 1, adds 1% effect for every 1% of move speed above default.                                                                         |
|                | **enter_easing_function**     | Easing function for transitions away from the default. Options include: linear, spring, quad, cubic, quart, quint, sine, expo, circ, bounce, or back. Prefix with in_, out_, or in_out_ (e.g., 'in_out_sine'). |
|                | **enter_time**                | Duration of the transition when moving away from the default (e.g., 1.0x).                                                                                            |
|                | **exit_easing_function**      | Easing function for transitions returning to default, with the same options as the enter easing function.                                                                |
|                | **exit_time**                 | Duration of the transition when moving towards the default (e.g., 1.0x).                                                                                             |
|                | **max_multiplier**            | Maximum multiplier value due to lower than standard move speed. Recommended value [>= 1].                                                                             |
|                | **min_multiplier**            | Minimum multiplier value due to lower than standard move speed. Recommended value [0-1].                                                                               |

---

#### **Field of View Effects**

| **Type**       | **Name**                      | **Description**                                                                                                                                                          |
|----------------|-------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **JSON Object**| **field_of_view**             | Defines effects on the field of view caused by movement speed bonuses. Effects are a multiplier on the base value.                                                     |
|                | **effect_strength**           | Strength of effect. With a value of 1, adds 1% effect for every 1% of move speed above default.                                                                         |
|                | **enter_easing_function**     | Easing function for transitions away from the default. Options include: linear, spring, quad, cubic, quart, quint, sine, expo, circ, bounce, or back. Prefix with in_, out_, or in_out_ (e.g., 'in_out_sine'). |
|                | **enter_time**                | Duration of the transition when moving away from the default (e.g., 1.0x).                                                                                            |
|                | **exit_easing_function**      | Easing function for transitions returning to default, with the same options as the enter easing function.                                                                |
|                | **exit_time**                 | Duration of the transition when moving towards the default (e.g., 1.0x).                                                                                             |
|                | **max_multiplier**            | Maximum multiplier value due to lower than standard move speed. Recommended value [>= 1].                                                                             |
|                | **min_multiplier**            | Minimum multiplier value due to lower than standard move speed. Recommended value [0-1].                                                                               |

---

#### **FOV Safeguards**

| **Type**       | **Name**                      | **Description**                                                                                                                                                          |
|----------------|-------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Decimal**    | **max_fov_degrees**          | Extra safeguard parameter to account for variation in FOV due to user settings. The FOV will be clamped within this range regardless of multiplier.                     |
| **Decimal**    | **min_fov_degrees**          | Extra safeguard parameter to account for variation in FOV due to user settings. The FOV will be clamped within this range regardless of multiplier.                     |

---

### badger:multistep_buildable

Defines the entity as a multi segmented buildable.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Boolean| allow_segments_above_endpoints| | Controls whether segments of this multistep are able to be built above both the end and starting segments. |
| String| apex_name| | (Optional) Name of apex mid section archetype, in case of a sharp peak. |
| Boolean| auto_place_start| | Should the start segment be automatically placed in front of the player? |
| Boolean| cap_fill_around_structures| | (Optional) Set true for cap model to fill around previously place buildings. |
| Boolean| cap_fill_around_terrain| | (Optional) Set true for cap model to only fill around terrain and not replace it. |
| Boolean| cap_piece_at_end| | (Optional) Set true if end of stretch selection gets custom model |
| Boolean| cap_piece_at_start| | (Optional) Set true if start of stretch selection gets custom model |
| String| cap_piece_fill_name| | (Optional) Name of archetype connecting normal section to the end cap. Should be made croppable. |
| String| cap_piece_flat_name| | (Optional) Name of flat end cap archetype |
| Integer| cap_piece_horizontal_offset| | (Optional) horizontal offset of cap piece. |
| String| cap_piece_name| | (Optional) Name of step-up end cap archetype |
| Decimal| end_segment_offset| | How far in front of the start segment does the end segment start at (after placing the start segment)? |
| Boolean| fill_around_terrain| | (Optional) Set true for section model to only fill around terrain and not replace it. |
| Decimal| max_height| | What is the max height variance of placement |
| Integer| max_segments| | The maximum length limit, counted in segments. |
| Decimal| max_slope| | What is the max slope angle of placement |
| Integer| mid_segment_count| | How many mid segments to place between end points. |
| Integer| placement_cursor_range| | When placing this buildable how far can the players cursor extend behond the player |
| String| placement_end_cursor_mode| | The cursor bias for the starting segment. Can be max_height, min_height, or player_height. |
| String| placement_start_cursor_mode| | The cursor bias for the starting segment. Can be max_height, min_height, or player_height. |
| JSON Object| segments| | List of midsection parts to use, along with their selection probability. |
| Integer| shared_health_group_size| | Controls how many segments share one health value. Divides the structure into groups of this many segments. |
| Decimal| start_segment_offset| | How far in front of the player does the start segment go (if automatically placed)? |
| String| stepup_name| | (Optional) Name of step-up mid section archetype |
| String| style| | How is this multipart laid out 'floating' or 'grounded'? 'floating_follow_terrain' can be used for a connected multistep buildable that attempts to avoid intersecting terrain. |

### badger:music_emitter_states (BP)

Defines the current state of this music emitter entity.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| List| badger:music_emitter_states| | A list of the current states this emitter can be in. This must match the resource_pack music_states for badger:music_2d_emitter. |

### badger:navigation

Description of how this entity can navigate.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Boolean| avoid_block_damage| | Whether or not the entity will avoid block damage while navigating |
| Decimal| avoidance_lookahead| | The distance to look ahead for obstacles while navigating. |
| Boolean| disable_space_occupancy| | Enables or disables crowd avoidance |
| Decimal| fall_distance| | The maximum distance this entity will be willing to step down hills |
| String| ignore_solids_rule| | The alliance rule that specifies if this entity should ignore tagged solid blocks while pathfinding |
| Decimal| max_distance_to_ignore_space_occupancy| | The max distance to ignore crowd avoidance |
| Decimal| max_height_to_ignore_space_occupancy| | The max height to ignore crowd avoidance |

### badger:nbt_decorator

Defines an NBT to place into the world.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| String| nbt_file| | Name of the nbt file to place. |

### badger:nbt_parts

Defines NBTs to place in a certain location on a structure.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| String| block| | Name of the block to place (Note: you can't use both a block and an nbt file). |
| String| locator| | Name of the locator at which the nbt is to be placed. |
| String| nbt_file| | Name of the nbt file to place. |

### badger:near_death_telemetry_tracking

Defines the settings used when tracking telemetry for near death experiences.

### badger:negate_status

Defines a set of status that this status will negate.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Array| negated_status| | List of negated status names (strings). |

### badger:netherrack_spreading

Describes how far netherrack and denether spreaders will travel from the source entity.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| denether_spreader_distance| | Manhattan distance that denether spreaders will travel from the outer edges of the source entity. |
| Decimal| nether_spreader_distance| | Manhattan distance that netherrack spreaders will travel from the outer edges of the source entity. |

### badger:objective_health_bar

Causes the entity's health to appear on a dedicated health bar on the player's sceen.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| max_visible_range| | Max range from the player to show healthbar. |
| Integer| priority| | Higher priority will occupy the healthbar over lower priority entities. |
| Boolean| show_at_full_health| | Whether the health bar should always be show neven if it's at 100%. |
| String| type| | The type of the health bar. This affects the icon and the text of the healthbar. |

### badger:offline_traits

This allows an entity to be turned offline, and determines what components get disabled when it is offline/constructing.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Array| construction_disabled_components| | The components to disable when this entity is constructing. |
| Array| deconstruction_disabled_components| | The components to disable when this entity is deconstructing. |
| Array| disabled_by_health_components| | The components to disable when this entity is disablied. |
| Array| offline_disabled_components| | The components to disable when this entity goes offline. |
| Array| pre_construction_disabled_components| | The components to disable until this entity begins constructing. |
| Boolean| visualize_constructing| true| When true, constructing structures will be visualized with a yellow box around them. |
| Array| visualize_offline| false| When true, offline structures will be visualized with a yellow box around them. |

### badger:omit_blocks_outline

This flags a buildable entity to not render with an outline.

### badger:omit_team_id

This flags a buildable entity to not render with a team ID, which makes it appear like normal terrain.

### badger:owned_territory

the zone of owned territory created by building this structure

| **Category**              | **Type**       | **Name**                                          | **Description**                                                                                                                 |
|---------------------------|----------------|---------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------|
| 🏷️ **Localization**       | **Array**      | **override_localization_for_prevent_overlap_tags**| Overrides for localization strings per tag to show building error messages for overlapped zone types.                         |
|                           |                | **localizationId**                                | The localized error message ID.                                                                                               |
|                           |                | **tag**                                          | The tag to override the error message.                                                                                         |
| 🚫 **Overlap Prevention**  | **Array**      | **prevent_overlap_tag_set**                       | Tags that prevent an entity's zone of control (ZoC) from overlapping.                                                         |
| 📏 **Zone Control Score**  | **Decimal**    | **village_gen_zone_control_score_threshold**      | This threshold is subtracted from the zone control score (0.0 to 1.0) when scoring buildable options during village generation. |
| 🏡 **Ownership**          | **Boolean**    | **village_zone**                                  | Determines if structures placed in this zone are owned by the same village that owns this structure.                          |
| 🔲 **Zone Dimensions**     | **Integer**    | **zone**                                         | Side length in blocks of the owned zone area created by building this structure.                                             |

### badger:persistent_entity_influence_source

Provides a list of sources of additive or subtractive PERSISTENT influence for an entity.

| **Type**   | **Name**                           | **Description**                                                                                                                                                      |
|------------|------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **List**   | **sensors**                        | List of persistent influences for entity sources.                                                                                                                   |
|            | **damage_receiver_filter**         | Track particular types of entities.                                                                                                                                 |
|            | **alliance_rule_filter**           | Team alliance rules for this tracking.                                                                                                                               |
|            | **exclude_tags**                   | Exclude tags for entities.                                                                                                                                         |
|            | **include_tags**                   | Include tags for entities.                                                                                                                                         |
|            | **multiplier**                     | The weight of the heat stamp for this entity will persist at.                                                                                                       |
|            | **propagation_decay**              | The propagation/spread decay of the heat stamp for this entity source.                                                                                              |

---

### badger:physics

Defines whether the entity has physics properties.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| buoyancy| 2.0| An inverse multiplier of the entity's gravity. Zero means no buoyancy and the object falls deeper and deeper into water, 1.0 means the object is stationary in water, values higher than 1.0 make the object float toward the surface. |
| Decimal| friction| | The amount of friction experienced when doing physics-based movement. |
| Decimal| gravity| | The intensity of gravity on an entity. Use a negative value (for instance -9.8) to have an entity fall toward the ground. |
| Decimal| water_drag| 0.1| Slows down an object velocity. |
| Decimal| water_submerged_ratio| 0.75| How deep a character is submerged when floating in deep water (0.0 means not submerged at all, 0.5 is roughly waist height, 1.0 is completely sumberged). |

### badger:pop_cap_anchor

This allows an entity to be used as a frame of reference for heuristic calculations when determining which pop capped entities to cull. Can also define a number of units nearby that will not be culled by the pop cap system.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| String| alliance_rule_filter| | Alliance filter for the exempted entities. |
| Boolean| greedy| false| If true, the units that this anchor exempts will not overlap with units exempted by any other greedy anchor. |
| Positive Integer| min_pop_cap| | The number of units near the anchor that will be exempt from pop cap culling. |
| Positive Integer| min_pop_cap_range| | The maximum range for cull exemption. |

### badger:pop_capped

Whether or not this entity counts for team popcap.

### badger:presentation_event (_unsure if good_)

Defines the triggers this entity can respond to, and the entities to spawn (with optional probabilities for success).

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| String| spawn_entity| | If the handler is a single string, then it's spawning only a single entity with 100% chance of success. |

### badger:preview_snapping

Causes a buildable preview to snap to nearby snap-point structures.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| gamepad_snap_range| | How far away the structure will snap from using a gamepad. |
| Decimal| mouse_snap_range| | How far away the structure will snap from using a mouse. |
| Decimal| snap_to_top| | When true, snaps the structure to the tops of other structures rather than the bottom. |

### badger:projectile

Defines the entity's projectile properties.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| horizontal_speed| | The projectiles speed in meters per second across the terrain. |
| Decimal| min_flight_time| | The minimum time in seconds that a projectile should take to reach it's target. This will lower the ground speed in order to make the timing match. |

### badger:projectile_physics

Defines the entity's projectile physics properties.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| gravity| | The projectile gravity in meters per second squared. |

### badger:pvp_win_condition

Flag used to track victory-critical entities. Currently used in PvP to mark the player's main base.

### badger:queueing

Defines this entity's ability to queue up entities.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| String| alliance_rule_filter| | The rule that determines which types of entities that will be queued (friendly, enemy, etc.). |
| Array| exclude_tags| | Entities with any of these tags will not enter the queue. |
| Array| include_tags| | Entities must have all these tags to enter the queue. |
| Decimal| influence_range| | Range within which entities will enter the queue. |
| Array| queue_heads| | The labelled positions (relative to this entity) where influenced entities will go and wait. The number of positions determines the number of entities that can simultaneously be at the head of the queue. |
| Integer| queue_size| | The number of entities that can queue up. -1 means infinite queue size. |

### badger:quick_direct_aim

Defines any parameters used to spawn a quick direct order.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| distance| | Defines the distance ahead of the player to place the destination. |

### badger:ranged_target

Defines where on the entity ranged attacks should aim for

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Array| target_positions| | The list of local positions where ranged attacks should land on this entity |

### badger:recall_prevention

Tunable timers for entities to be prevent being recalled by other players during actions.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| on_lured_disabled_duration| | Duration in seconds for the mob to be unrecallable after being directed. |

### badger:recallable_entity

Flag component that marks entity as recallable

### badger:refundable

Marks this unit as refundable, and gives their priority

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Integer| priority| | Refund priority. Lower values will be refunded first |
| Decimal| refund_multiplier| | How much of the units cost is refunded |

### badger:refunding_spawner

A spawner that can be used to refund mobs

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| String| alliance_rule| | The alliance rule to filter which players can use this spawner to refund mobs. |
| Boolean| can_refund_in_combat| | Whether or not this spawner can refund mobs that are currently in combat. |
| Array| exclude_tags| | Forbidden tags on entities to refund. |
| Array| include_tags| | Required tags on entities to refund. |

### badger:removal_time (possible _Duplicate_)

destroys an entity after a given period of time

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| String| presentation_event| | Name of the presentation event to fire when the timer triggers |
| Boolean| sync_presentation_event| | Whether or not the event should be synced over the server, or if it should be local only |
| Decimal| time| | Amount of time in seconds before the entity is destroyed |

### badger:reports_buildable_state

Defines the settings for what state is reported when placing buildables.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| reports_state| | Defines the state to report. |

### badger:reports_hit_reaction

Whether or not this entity reports hit reactions for visual feedback.

### badger:reset_heartbeat_in_range

An entity with this component will prevent other entities in range of it from despawning due to their heartbeat expiring.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| range| | The range that prevents entities from despawning due to heartbeat. |

### badger:resist_status

Defines entity's ability to resist being affected by status effects.

| **Type**       | **Name**              | **Description**                                                                                                        |
|----------------|-----------------------|------------------------------------------------------------------------------------------------------------------------|
| **JSON Object**| **status_effect_name** | Defines the name of the status effect and its associated properties.                                                   |
|                | resistance_persist_time| Specifies the duration (in seconds) that the status effect is blocked from reactivating after it ends. A value of -1 allows the status effect to be refreshed before the current effect ends. |
|                | resisted               | Indicates whether this status effect is completely resisted.                                                           |
|                | time                   | Sets the duration (in seconds) for how long this status effect will last.                                              |

### badger:resource_cost_multiplier

This value scales resource costs for the player it is attached to, including building placement and mob spawning costs.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| String| building_cost_multiplier| | The multiplier that is applied to all building costs, increasing it or lowering it. Default value 1.0. |
| String| spawning_cost_multiplier| | The multiplier that is applied to all mob spawn costs, increasing it or lowering it. Default value 1.0. |

### badger:resource_modifier

Modify the Pop Cap for a list of resources

| **Type** | **Name**    | **Description**                                                                 |
|----------|-------------|---------------------------------------------------------------------------------|
| **Array** | **modifiers** | Key-value pairs that describe which resources are being modified.               |
|          | amount      | The amount by which to modify the resource cap.                                   |
|          | resource    | The name of the resource to be modified.                                          |

### badger:resource_trader

Marks this entity as a place to exchange resources for other resources.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| String| successful_resource_trade_trigger| | The trigger event to send when a successful resource trade is made using the resource trader |

### badger:respawn_point

Makes this entity act as a respawn point for players.

| **Type**       | **Name**                   | **Description**                                                                                             |
|----------------|----------------------------|-------------------------------------------------------------------------------------------------------------|
| **String**     | **alliance_rule**           | The tag name that defines the alliance rule.                                                                |
| **String**     | **player_respawned_trigger**| The event trigger to send when a player respawns at a respawn point.                                         |
| **JSON Object**| **spawn_blocking**          | Settings that control what blocks a respawn point.                                                          |
|                | blocking_alliance_rule      | The alliance rule tag name used to check for respawn blocking.                                               |
|                | blocking_radius             | The radius around the respawn point used to check for entities that may block respawning.                    |
|                | tag_filter                  | The tag filter used to determine whether an entity should block a respawn point.                             |
|                | exclude_tags                | An array of tags to exclude from entities that block respawn points.                                         |
|                | include_tags                | An array of tags that must be present for entities to block respawn points.                                  |

### badger:rewards

states what rewards will be granted when this entity is created.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Array| reward| | The items that will be awarded to player when entity is created. |

### badger:rider

Marks an entity as one that can ride on mounts.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Array| exclude_tags| | The tags required to NOT be on an entity for an effect to trigger. |
| Array| include_tags| | The tags required to be on an entity for an effect to trigger. |

### badger:scriptable_buildable_spawner

Denotes whether or not this entity can have its spawner settings overwritten by scripts.

### badger:server_block_source_removal

Remove the BlockSourceComponent because this entity doesn't need one.

### badger:show_debug_health

Show the Debug Health of the entity

### badger:spawn_costs

In cases where it could cost to use this entity this will be cost.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Array| costs| | The items that will be deducted from the player when this entity is spawned. |
| Array| tickets| | The ticket type that will be deducted from the player or team when this entity is spawned. |

### badger:spawn_tracking

Dictates whether this entity has its spawning tracked.

### badger:spawner_add_loot_override

Units spawned from this spawner will utilize an override loot table.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| List| name| | The override type name (NOT a loot table ID). This should resolve to a loot table on the entity. |

### badger:spawner_add_tags

Tags added on top of entities existing tags spawned by this spawner.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Array| tags| | Additional tags to be added to the spawned entity. |

### badger:spawner_rules

Defines the rules for when or where to spawn entities.

| **Type**       | **Name**                         | **Description**                                                                                                                                                                         |
|----------------|----------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **JSON Object** | **block_type_rule**              | Defines an optional block type spawner rule.                                                                                                                                             |
|                | **valid_types** _(Array)_        | List of valid block type names that the spawner can use for spawning entities.                                                                                                           |
| **JSON Object** | **bsharp_rule**                  | Defines an optional `bSharp` global variable spawner rule.                                                                                                                               |
|                | **value** _(Number)_             | Value of the global variable that causes the spawner to stop spawning.                                                                                                                   |
|                | **variable** _(String)_          | Name of the `bSharp` global variable used in this rule.                                                                                                                                  |
| **JSON Object** | **distance_rule**                | Defines an optional distance spawner rule.                                                                                                                                               |
|                | **distance_data** _(Object)_     | Specifies the optional entity distance rule.                                                                                                                                            |
|                | **alliance_rule** _(String)_     | Defines an entity's alliance rule, determining whether the spawner should prevent spawning based on the entity’s alliance.                                                               |
|                | **max_distance** _(Number)_      | Maximum distance that entities have to be within for the spawner to start spawning.                                                                                                       |
|                | **min_distance** _(Number)_      | Minimum distance that entities cannot be within for the spawner to start spawning.                                                                                                        |
|                | **tag_filter** _(Object)_        | Filter object defining conditions based on tags.                                                                                                                                          |
|                | **exclude_tags** _(Array)_       | List of tags that must be excluded from the spawning process.                                                                                                                            |
|                | **include_tags** _(Array)_       | List of tags that must be included for spawning to occur.                                                                                                                                |
|                | **village_bounds_distance_data** _(Object)_ | Specifies village bounds distance-based rules for spawning.                                                                                                                              |
|                | **destruction_spawn_delay** _(Number)_ | Duration of time (in seconds) waited after the destruction of a village before applying the "isDestroyed" rule.                                                                         |
|                | **horde_names** _(Array)_        | Defines the hordes to check. If not defined, all villages of any horde are checked.                                                                                                      |
|                | **is_destroyed** _(Boolean)_     | Determines whether this rule applies only to destroyed villages. If not defined, the rule checks both alive and destroyed villages.                                                       |
| **Boolean**    | **spawn_if_village_occupied**    | Specifies whether this spawner should spawn when a village is occupied. Omitting this option allows spawning regardless of the occupation state.                                          |
| **JSON Object** | **time_of_day_rule**             | Defines an optional time-of-day-based spawner rule.                                                                                                                                      |
|                | **times** _(Array)_              | Defines the specific times of day during which the spawner is allowed to spawn entities.                                                                                                 |

### badger:sprint

Defines the settings required for an entity to sprint.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| String| exhausted_negate_status| | The name of the status effect to apply to negate the exhausted effect. |
| String| exhausted_status| | The name of the status effect to apply when exhausted. |
| Decimal| idle_time_to_deactivate| | Amount of time the player must be stopped/walking before sprint ends automatically. A value of -1 disables this feature. |
| String| sprint_negate_status| | The name of the status effect to apply to negate the sprinting effect. |
| String| sprint_status| | The name of the status effect to apply when sprinting. |

### badger:stamina

Defines the settings required for stamina depletion.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| can_regen_during_exhausted| | Allows regeneration while exhausted. |
| Decimal| depletion_rate| | Defines the rate at which stamina is depleted (per tick). |
| Decimal| exhaust_restore_threshold| | Exhausted state will end once stamina is above this % threshold. [0-1]. A value of -1 disables this feature. |
| Decimal| exhausted_cooldown| | Defines how long an entity will be exhausted for once stamina is depleted. |
| Decimal| max_stamina| | Defines the maximum amount of stamina for an entity. |
| Decimal| regen_cooldown| | Defines how long it will take to start regenerating stamina, NOT from exhaustion. |
| Decimal| regen_rate| | Defines the rate at which stamina will regenerate (per tick). |
| Decimal| stamina_replenish_audio_threshold| | Stamina must drop below this value in order for the stamina replenished audio event to trigger when it refills. |

### badger:state_reporting

Defines the settings for game state reporting.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| report_duration| | Defines how long a state will be reported for. |
| Decimal| states| | Defines the list of states that can be reported. |
| Decimal| tag| | Defines the tag for this state. |

### badger:static_entity

Controls if this entity should only be partitioned on demand.

### badger:status

Defines this entity as a status effect prototype.

| **Type**       | **Name**            | **Description**                                                                                                                                             |
|----------------|---------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **String**     | **applied_name**     | The name assigned to this status effect.                                                                                                                    |
| **JSON Object**| **default_resistance** | Sets the default resistance parameters, used if the entity does not have a specific resistance for this status effect (refer to `badger:resist_status`).      |
|                | resistance_persist_time | Duration (in seconds) for which the status effect is blocked from reactivating after it ends. A value of `-1` allows the effect to refresh before ending.    |
|                | resisted             | Determines if this status effect is completely resisted by default.                                                                                          |
|                | time                 | Specifies the default duration (in seconds) for which this status effect lasts.                                                                              |

### badger:status_effect_ramp_off

Causes a gradual reduction of numerical effects, leading up to the expiry of the status effect.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| transition_duration| | Time, in seconds, over which the status effect will ramp down (before the standard end time). |

### badger:status_effect_telemetry_tracking

Defines the settings used when tracking telemetry for certain status effects.

| **Type** | **Name**                          | **Description**                                                                                       |
|----------|-----------------------------------|-------------------------------------------------------------------------------------------------------|
| Array    | **track**                         | List of status effect names to track.                                                                 |
|          | **status_effect_applied_name**    | The name of the status effect that has been applied and is being tracked.                             |
|          | **telemetry_id**                  | The name of the telemetry event associated with the tracking of the status effect.                     |

### badger:sub_objective_health_bar

Causes the entity to display sub-objective information on the dedicated objective health bar on the player's screen.

### badger:suppress_target_action

suppresses target actions for a period of time.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| max_time| | max amount of time in seconds to suppress target actions for. |
| Decimal| min_time| | (optional) min amount of time in seconds to suppress target actions for. If specified, a random value between min and max will be chosen. |

### badger:swaps_mounts

Indicates that this entity allows an interactor to swap their mount.

### badger:swaps_team

Defines this entity as a status effect that causes its host to temporary switch teams.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Boolean| swap_to_inflictor| | If true, swaps the host to the inflicting entity's team. |
| String| team| | The name of the team to specifically swap to (when not swapping to the inflicting entity's team). |

### badger:tag_modifier_source

Modifies the tags of entities within a certain radius, for as long as they are in that radius

| **Type**       | **Name**                     | **Description**                                                            |
|----------------|------------------------------|----------------------------------------------------------------------------|
| **String**     | **alliance_rule_filter**     | Alliance rule filter to determine who to apply tags to.                   |
| **Decimal**    | **radius**                   | Range to apply tag modifications to.                                      |
| **JSON Object**| **tag_filter**               | Tag filter to determine who to apply tags to.                             |
|                | exclude_tags                 | Tags to exclude from the filter.                                          |
|                | include_tags                 | Tags to include in the filter.                                           |
| **Array**      | **tags_to_add**              | Tags to add.                                                              |
| **Array**      | **tags_to_remove**           | Tags to remove.                                                           |

### badger:tags

Designer prescribed tags that can be used to filter or discern this entity from others in a general sense. Main use is targeting rules.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Array| tags| | Add as many different tags to allow to filter this entity by having or not having certain tasks. |

### badger:take_block_space

This description means this entity takes the space of a block and should be removed if a new block want the space

### badger:target_actions

Defines the actions that get carried out when a target is selected by a targeting priority enabling these actions.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| List| actions| | List of actions. |

### badger:target_assigner

Defines this entity as a target assigner which pushes priority targets to influenced entities.

| **Type**       | **Name**                         | **Description**                                                                                                                       |
|----------------|----------------------------------|---------------------------------------------------------------------------------------------------------------------------------------|
| **String**     | **alliance_rule_filter**         | Alliance rule used to filter the target by.                                                                                          |
| **Boolean**    | **despawn_on_target_lost**       | When true, the entire order ends when the target is destroyed, cancelling movement.                                                   |
| **Decimal**    | **drop_target_range**            | The distance a selected target must be from the assigner before it is dropped.                                                       |
| **Decimal**    | **find_target_range**            | The distance the assigner will search for a target to select or assign.                                                               |
| **Boolean**    | **force_move**                   | If true, this command is a force move command, adjusting enemy highlighting to match the action.                                     |
| **Decimal**    | **force_target_range**           | Within this range, mobs will directly attack the target, ignoring any secondary targets that are closer.                               |
| **Boolean**    | **parent_self_to_target**        | When true, the target assigner entity will parent itself to the targeted entity.                                                      |
| **JSON Object**| **pre_redirect_target_filter**    | Early phase alliance and tag filter before redirection. Redirector hurtboxes found in this phase will redirect to target the default hurtbox, which will be further filtered by the main tag filter. Changes require coder approval. |
|                | exclude_tags                     | Array of tags to exclude (any of) when filtering targets.                                                                             |
|                | include_tags                     | Array of tags to require (all of) when filtering targets.                                                                             |
| **Boolean**    | **single_target**                | When true, the target assigner will stop searching for targets after its first selected target is destroyed.                          |
| **JSON Object**| **target_filter**                | Alliance and tag filter used by this assigner to filter targets.                                                                       |
|                | exclude_tags                     | Array of tags to exclude (any of) when filtering targets.                                                                             |
|                | include_tags                     | Array of tags to require (all of) when filtering targets.                                                                             |

### badger:targeting

Defines the entity's targeting behaviour. Used to drive what the entity can target and which actions to use against qualifying targets.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Array| targeting_priorities| | Entries defining what the entity can target at a specified range. Entries are in order of high-to-low priority. |
| Boolean| update_targeting_every_tick| | When true, the entity checks for targets constantly, instead of a couple of times per second. Doing so is more costly for performance and should be avoided outside of special cases. |

### badger:telemetry_category

Settings used when reporting telemetry info.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| String| category_name| | Defines the category name used when reporting telemetry. |

### badger:telemetry_game_zone

Determines if a village will trigger game zone telemetry and additional information regarding it.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Boolean| add_details_in_pvp| | Should a GameZoneDetail be added in PVP |
| Integer| alt_id| | An alternate game zone ID to use if the main id isn't sufficient. |
| Integer| id| | The game zone ID/index to use when entering the area. |

### badger:telemetry_lure_direct

Determines if a village will trigger game zone telemetry and additional information regarding it.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Boolean| is_advanced| | for directors specifically, is it a simple or targeted director? |
| String| order_type| | Is this a lure or director? |

### badger:telemetry_track_banner_order

Determines if a mob should be tracked when given a banner order for telemetry purposes.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Boolean| enabled| | Is this entity being tracked for banner order |

### badger:text_markup_icon_name

Defines the name of the icon to be used in HUD error messages.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| String| name| | Full name of icon. |

### badger:ticket_cap_modifier

Modify the cap for a list of tickets

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| amount| | The amount to modify the ticket cap by. |
| String| ticket| | The name of the ticket cap to modifying |
| Array| ticket_modifiers| | Key value pairs that describe which ticket cap we are modifying |

### badger:track_damage

Dictates whether this entity will track damage it receives or delivers.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Boolean| dealt| | Whether the village should track the damage it deals. |
| Boolean| taken| | Whether the village should track the damage it takes. |

### badger:track_influenced_group_position

Flags a lure or director to track the centroid of its group of influenced entities.

### badger:tracks_health_change

Enables tracking of client-side changes in health. Useful for interrupting certain player actions.

### badger:trigger_criteria

Flag Attribute that indicates this entity is used as a trigger section.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| List| filters| | List of filter include and exclude tags |

### badger:unit_direct_tags

Tags that are used to filter entities for advanced direct groups.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Array| tags| | Tags to add to this entity for advanced direct groupings. |

### badger:unlockable_buffs

When applied to an entity, buffs itself under certain unlock conditions.

| **Type** | **Name** | **Description** |
|----------|----------|------------------|
| Array    | **buffs** | The list of unlockable buffs used by this entity. |
|          | buff_id  | The specific buff to be applied to this entity. |
|          | has_unlock | If true, this buff is active only when the specified resource is acquired. If false, the buff is active only when the resource is not acquired. |
|          | resource | The name of the resource used to enable or disable this buff. |

### badger:village

Some general attributes that describe a village.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Integer| generation_priority| | Villages with a higher generation priority will be planned and built first. If no priority is provided, 0 is the default. |
| String| name| | Identifies the type of village. |

### badger:village_bounds_trigger_volume_size

Defines the size of the village bounds trigger volume. This is also the size which village trigger volumes are relative to.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| padding| | The distance in blocks to expand the trigger volume beyond the village bounds. |

### badger:village_bridge

Determines how villages build bridges.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| bridge_cost_per_meter| | Used to compare with the alternative path without a bridge. |
| Decimal| bridge_horizontal_distance_max| | The largest XZ difference allowed between the start and end of the bridge. |
| Decimal| bridge_horizontal_distance_min| | The smallest XZ difference allowed between the start and end of the bridge. |
| String| bridge_id| | The entity identifier for the bridge that should be placed. |
| Decimal| bridge_vertical_distance_max| | The largest Y difference allowed between the start and end of the bridge. |
| Decimal| diagonal_bridge_degree_tolerance| | The number of degrees a bridge can deviate from right angles before being considered diagonal. |

### badger:village_building_path

Determines how villages build paths from buildings.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Boolean| can_place_bridge_for_height_changes| | Toggles whether bridges can be used as stairs between zones with prohibitively steep height changes. |
| Boolean| can_place_under_buildables| | Setting this to false will force paths to generate around buildables instead of underneath them. |
| Decimal| diagonal_path_degree_tolerance| | The number of degrees a building path can deviate from right angles before being considered diagonal. |
| Array| edge_blocks| | The names of the blocks to be used for the outer edges of paths. |
| Array| edge_decoration_blocks| | The names of the blocks to be used on top of the outer edges of paths. |
| Decimal| edge_noise_amplitude| | How far noise can extend past the path's edge in meters. |
| Decimal| edge_width| | The width of the path's edge on either side in meters. |
| Decimal| height_change_needs_bridge| | If the height change between zones is larger than this, a bridge must be placed to cross them. |
| Array| inner_blocks| | Optionally specify the names of the blocks to be used on the inner part of the path, edge, or edge_deco. |
| Array| outer_blocks| | Optionally specify the names of the blocks to be used on the outer part of the path, edge, or edge_deco. |
| Decimal| outer_edge_threshold| | The width of the path's edge on either side in meters. |
| Decimal| outer_path_threshold| | The width of the path in meters. |
| Decimal| path_block_noise_scale_factor| | A smaller scale factor will cause varied path blocks to alternate less frequently. |
| Array| path_blocks| | The names of the blocks to be used for paths. |
| Decimal| path_deco_noise_scale_factor| | A smaller scale factor will cause varied path edge decorations to alternate less frequently. |
| Decimal| path_edge_block_noise_scale_factor| | A smaller scale factor will cause varied path edge blocks to alternate less frequently. |
| Decimal| path_noise_amplitude| | The noise amplitude affects the severity of a path's weathering. |
| Decimal| path_noise_scale_factor| | A smaller scale factor will produce more gradual changes in a path's weathering. |
| Decimal| path_width| | The width of the path in meters. |
| Decimal| prevent_buildable_placement| | Setting this to false will allow buildable to be placed on top of paths. |

### badger:village_building_placement

Settings for placing buildings inside of village zones.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Boolean| is_placement_jitter_bell_curve| | The generation of jitter values between the min and max will have a probability distribution that approximates a normal distribution. |
| Integer| max_placement_attempts_with_jitter| | The maximum number of times that a placement will try placing with jitter, per village zone. |
| Decimal| minimum_distance_between_buildings| | Buildings cannot place closer than this distance. |
| Decimal| placement_jitter_max| | The maximum distance that the placement will can be offset from a village zone face site. This value will be clamped if it's large enough to push the placement outside of a zone. |
| Decimal| placement_jitter_min| | The minimum distance that the placement will can be offset from a village zone face site. This value will be clamped if it's large enough to push the placement outside of a zone. |

### badger:village_construction

Determines which buildings will be placed during village generation.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| String| start_logic| | The deck logic to run on startup |

### badger:village_district_path

Determines how villages build paths from districts.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Boolean| can_place_bridge_for_height_changes| | Toggles whether bridges can be used as stairs between zones with prohibitively steep height changes. |
| Boolean| can_place_under_buildables| | Setting this to false will force paths to generate around buildables instead of underneath them. |
| Decimal| diagonal_path_degree_tolerance| | The number of degrees a building path can deviate from right angles before being considered diagonal. |
| Array| edge_blocks| | The names of the blocks to be used for the outer edges of paths. |
| Array| edge_decoration_blocks| | The names of the blocks to be used on top of the outer edges of paths. |
| Decimal| edge_noise_amplitude| | How far noise can extend past the path's edge in meters. |
| Decimal| edge_width| | The width of the path's edge on either side in meters. |
| Decimal| height_change_needs_bridge| | If the height change between zones is larger than this, a bridge must be placed to cross them. |
| Array| inner_blocks| | Optionally specify the names of the blocks to be used on the inner part of the path, edge, or edge_deco. |
| Array| outer_blocks| | Optionally specify the names of the blocks to be used on the outer part of the path, edge, or edge_deco. |
| Decimal| outer_edge_threshold| | The width of the path's edge on either side in meters. |
| Decimal| outer_path_threshold| | The width of the path in meters. |
| Decimal| path_block_noise_scale_factor| | A smaller scale factor will cause varied path blocks to alternate less frequently. |
| Array| path_blocks| | The names of the blocks to be used for paths. |
| Decimal| path_deco_noise_scale_factor| | A smaller scale factor will cause varied path edge decorations to alternate less frequently. |
| Decimal| path_edge_block_noise_scale_factor| | A smaller scale factor will cause varied path edge blocks to alternate less frequently. |
| Decimal| path_noise_amplitude| | The noise amplitude affects the severity of a path's weathering. |
| Decimal| path_noise_scale_factor| | A smaller scale factor will produce more gradual changes in a path's weathering. |
| Decimal| path_width| | The width of the path in meters. |
| Decimal| prevent_buildable_placement| | Setting this to false will allow buildable to be placed on top of paths. |

### badger:village_do_not_clear_on_slot_overlap

If this attribute is present, then if any village will generate on the same slot, then this village will not be despawned.

### badger:village_hanging_decoration_placement

Settings for placing hanging decorations between village zones.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| minimum_edge_width| | The minimum shared edge width between two zone considered for placement. |
| Decimal| minimum_height_delta| | The minimum height difference between two zone considered for placement. |

### badger:village_heart

A structure that must remain for the village to continue existing.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| | badger:village_heart| |  |

### badger:village_heart_destruction

Customized destruction behaviour when all Village Hearts are destroyed in a village.

| **Type**       | **Name**           | **Description**                                                                                         |
|----------------|--------------------|---------------------------------------------------------------------------------------------------------|
| **List**       | **groups**         | A list containing various group configurations.                                                        |
|                | **delay** (Decimal)  | The delay before the group effect starts.                                                              |
|                | **interval** (Decimal)| The interval between successive group effects.                                                          |
|                | **disable_vfx** (Boolean)| A flag that determines whether visual effects should be disabled for this group.                       |

### badger:village_heart_disabled_actions

Actions that will occur to village structures when the village heart is disabled by health

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| String| village_structures_action| | The action that will occur to the structures of the village |

### badger:village_height_change

Influences the village height changes.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| clamp_size| | The number of blocks above a height change that are not changed to air. |

### badger:village_influence

This gives an entity an influence with which it can apply to a portion of the map. This influence can be used for things such as placing buildings in areas of high or low influence.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| amount| | The amount of influence exerted by this entity. |

### badger:village_moat

Determines how villages build paths from districts.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| edge_noise_amplitude| | The noise amplitude determines the maximum distance that moat zone edges can shifted inward. |
| Decimal| edge_noise_scale_factor| | A smaller scale factor will produce a more gradual change along the moat zone edges. |

### badger:village_wall

Determines which buildings will be placed during village generation.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| wall_offset| | How far to move the walls away from their zone edges. |

### badger:village_weathering

Determines how villages apply weathering effects to terraformed zone edges.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| gradient_2d_noise_scale| | The magnitude of vertical noise applied to the particular gradient pass. |
| Decimal| gradient_3d_noise_scale| | The magnitude of horizontal noise applied to the particular gradient pass. |
| Boolean| gradient_affected_by_wave| | Sets whether or not the particular gradient pass is affected by the sawtooth wave. |
| Array| gradient_depths| | How many blocks deep the gradient stops being applied for each gradient pass. |
| Array| gradient_passes| | Array of gradient passes. |
| Array| gradient_weight_heights| | The average height of the weather effect for the particular gradient pass.  Higher is steeper. |
| Boolean| is_overhanging_edge| | Determines if the weathering effect will taper up or down. |
| Decimal| position_noise_scale| | Lower values will apply smoother xz jitter to weathering effects. |
| Decimal| wave_depth_max| | The maximum distance in blocks, before noise, that the weathering wave can remove blocks from an edge. |
| Decimal| wave_depth_min| | The minimum distance in blocks, before noise, that the weathering wave can remove blocks from an edge. |
| Decimal| wave_noise_scale| | The magnitude of noise used to soften the wave function. |
| Decimal| wave_period_scale| | A smaller scale factor will reduce the frequency of the weathering wave for a smoother effect. |

### badger:village_zone

Influences the villages topology.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| expansion_distance| | The maximum distance that villages can expand from their center position. |
| Boolean| is_hexagonal_grid_enabled| | False will use standard square grid.  True will change the square grid to hexagonal. |
| Boolean| is_zone_jitter_bell_curve_enabled| | The generation of jitter values between the min and max will have a probability distribution that approximates a normal distribution. |
| Decimal| minimum_loz_connection_width| | The minimum edge width between zones in a layer request before padding is added. |
| Decimal| water_search_resolution| | The distance between each check for water while scanning the expansion area for bodies of water. |
| Decimal| zone_jitter_max| | The maximum amount that a village zone will be offset in world units. |
| Decimal| zone_jitter_min| | The maximum amount that a village zone will be offset in world units. |
| Decimal| zone_spacing| | The default distance between village zones before jitter is applied. |

### badger:village_zone_scoring

Describes how village zones are prioritized during building placement.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| String| close_to_easing| | The type of easing function used to control the attenuation of score provided to nearby zones. |
| String| close_to_wall_easing| | The type of easing function used to control the attenuation of score provided to zones nearby walls. |
| Decimal| close_to_wall_weight| | The weight applied to the score provided to zones nearby walls. |
| Decimal| close_to_weight| | The weight applied to the score provided to zones nearby the close to position. |
| String| direction_easing| | The type of easing function used to control the attenuation of score provided to zones in this direction. |
| Decimal| direction_weight| | The weight applied to the score provided to zones nearby in the desired direction. |
| String| distance_from_easing| | The type of easing function used to control the attenuation of score provided to zones that are some distance away from a district starting position. |
| Decimal| distance_from_weight| | The weight applied to the score provided to zones that are some distance away from a district starting position. |
| String| elevation_easing| | The type of easing function used to control the attenuation of score provided to zones near the specified elevation. |
| Decimal| elevation_weight| | The weight applied to the score provided to zones near the specified elevation. |
| String| far_from_easing| | The type of easing function used to control the attenuation of score provided to nearby zones. |
| Decimal| far_from_weight| | The weight applied to the score provided to zones nearby the far from position. |
| Decimal| random_weight| | The weight applied to the random score (between 0 and 1) provided to zones. |

### badger:wander

Dictates entity's idle behaviour.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| wander_frequency| | How many times per second on average this entity will idle wander, instead of remaining still. |
| Decimal| wander_radius| | How far will this entity wander around a fixed point. |

### badger:waypoint_marker

Entity will have a waypoint marker on the HUD

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| String| alliance_path| | Icon name for an alliance-based icon override. Optional |
| String| alliance_rule_filter| | Type of team alliance for which ALLIANCE_PATH should become active. Optional |
| String| label| | Waypoint label |
| String| local_player_facing_path| | Icon name for a local player owner icon override. Local player must 'own' the icon. Optional |
| String| path| | Icon name |
| Integer| priority| | An integer to specify compass bar sorting importance (higher numbers take precedence); any positive value also 'pins' the marker in strategic view. |
| Vector [a, b]| screen_position_offset| | Waypoint marker screen position offset. Value is in % of screen size. Range:(-100,100) |
| Boolean| use_faction_name| | If true, the waypoint's text will be the name of its faction. |
| Vector [a, b, c]| world_position_offset| | Waypoint marker world position offset. |

### badger:waypoint_marker_visibility

Entity will have a waypoint marker on the HUD

| **Type**       | **Name**                          | **Description**                                                                                                   |
|----------------|-----------------------------------|-------------------------------------------------------------------------------------------------------------------|
| **Integer**    | **compass_level**                 | Determines the compass level where a non-team icon is displayed. Values: -1 (onscreen and bottom), 0 (bottom), 1 (top). |
| **String**     | **compass_max_range**             | Maximum range value to be displayed on the compass (options: short, medium, long).                               |
| **Array**      | **filter_tagsets**                | Array of paired include and exclude tag arrays for determining when this icon can appear.                        |
|                | **exclude_tags**                  | Array of tags to exclude (any of) when filtering compass icons.                                                  |
|                | **include_tags**                  | Array of tags to require (any of) when filtering compass icons.                                                  |
| **Boolean**    | **hide_from_non_alliance**        | Determines if the waypoint should be hidden from opponents.                                                      |
| **Decimal**    | **waypoint_max_range**            | Maximum range value for the waypoint.                                                                             |
| **Decimal**    | **waypoint_max_range_alliance**   | Maximum range value for opponent waypoints.                                                                       |
| **Decimal**    | **waypoint_min_range**            | Minimum range value for the waypoint.                                                                             |
| **Decimal**    | **waypoint_min_range_alliance**   | Minimum range value for opponent waypoints.                                                                       |

### badger:world_collision

Defines the entity's world collision properties.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Decimal| auto_step| | The maximum height of an object that an entity can automatically step on while walking. |
| Decimal| interpolation_speed| | The speed at which the auto speed will interpolate. |

### badger:world_request

World request information associated with a particular entity archetype.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| String| world_request_type| | Determines the priority of the world requests dispatched. |

### badger:world_single_spawn

Used to place a single unique entity during world gen, that won't get double placed when saveloading/reloading chunks

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| String| entity| | The name of the entity to spawn |

### badger:zone_type

Specifies the type of zone that this buildable provides.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| Boolean| non_blocking| | When true, this zone will be used for 'must be in zone' checks but not 'must NOT be in zone' checks. Useful for not blocking placement of enemy structures. |
| String| zone_type| | This is the type of zone that this buildable provides. Ex. Utopia, Village, etc. |

### badger:zone_type_requirement

Defines the different types of zones of control which allows this building to be constructed/online.

| **Type**       | **Name**                           | **Description**                                                                                                                                                  |
|----------------|------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Array**      | **exclude_tags**                   | The zone types that cannot overlap for this placement.                                                                                                         |
| **Array**      | **include_tags**                   | The different types of zones of control that allow this building to be constructed or come online.                                                             |
| **JSON Object**| **invalid_zone_block_conversion**  | Block conversion data for when the structure is no longer in the required zone.                                                                                |
|                | duration_percentage                 | The percentage of build time required to perform block conversion.                                                                                             |
|                | modifiers                           | A list of block conversion modifiers.                                                                                                                           |
|                | reversal_duration_percentage        | The percentage of build time required to reverse block conversion.                                                                                              |
|                | top_to_bottom                       | If true, block conversion occurs from top to bottom, rather than bottom to top.                                                                                |
| **Boolean**    | **set_offline_when_no_zone**       | If set to true, this structure will be set offline instead of being destroyed if it is no longer in the required zone at any point.                           |

### badger:zone_visualization

Specifies which types of zones are displayed when previewing the building.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| String| group| | The zone group name (one of the zone groups from the zone group service) |
| String| zone_alliance_rule| | The alliance rule that specifies which zone(s) should be displayed |

### capped_spawner

If this entity spawns entities, it will be bound by the cap of the type specified here.

| Type| Name| Default Value| Description |
|:-----------:|:-----------:|:-----------:|:-----------:|
| String| cap_type| | The cap type that entities shot or spawned by this spawner contribute towards. |
| Integer| individual_cap| | If the total cap type is not reached, how many entities can any one instance of this spawner spawn individually. |
| Array| toggled_actions| | List of action names on this entity that are disabled when either the cap type limit or the individual limit are reached. |
