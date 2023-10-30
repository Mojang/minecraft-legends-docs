# Minecraft Legends - Custom Game Settings

![](images/custom_game_settings/image01.png)

**Figure 1:** Screenshot of custom settings screen

## What are custom game settings?

Campaign and PVP have separate 'custom' game modes that feature a wide variety of settings to adjust various gameplay systems. These can range from player unit and enemy behaviors, starting and max resources, available structures, etc.

The data for custom settings lives in `resource_packs/badger_base/gamelayer/launcher/custom_game_settings.json`

## How to add a custom game setting?

Adding a custom game setting is done through a combination of adding Custom Settings Data as well as data that reads the value of the setting to apply its effects in game. Both of these aspects are explained in greater detail below.

The vast majority of custom game settings that already exist are configured this way and can serve as examples for additional options. Note however that options such as 'world_world_seed' and 'world_world_size' have specific functionality in the game's engine which are not accessible.

### Custom Settings Data

The settings data is laid out with a category name for the main object, which will contain several sub categories and each sub category will have a list of settings.

The main categories are the names of the Tabs in the custom settings screen and the sub categories are the names of the headers above each section of settings per tab.

```
{
    "mainCategory": {
        "subCategory": [
			{ //  ========  Example ========
				"id": "world_world_seed",
				"value_type": "string",
				"value": "",
				"backend_type": "customgame",
				"ui_type": "textfield",
				"game_mode_type": "allmodes",
				"is_editable": false,
				"save_last_used_value": false,
				"enable_requirements": [],
				"options": [
				]
			},
		]
    }
}
```

#### Category ("mainCategory" from example) & Sub Category ("subCategory" from example)

Add the setting to an existing category/subcategory. May choose to create a new category or subcategory, this will create a new tab or sub header respectively.

// TODO: Add image of tabs

#### "id"

This will be the identifier for the setting, and how it will be referenced when connecting the value to the gameplay system.

#### "backend_type"

"customgame" (always)

#### "value_type"

"int", "float" or "string"

#### "ui_type"

| Type        | Visual                                          | "uiType"                | "valueType"    | "options"                         |
| ----------- | ----------------------------------------------- | ----------------------- | -------------- | --------------------------------- |
| Switch      | ![](images/custom_game_settings/switch.png)     | "switch"                | "int"          |                                   |
| Slider      | ![](images/custom_game_settings/slider.png)     | "slider"                | "int", "float" | [{"min": #}, {"max": #}]          |
| Button      | ![](images/custom_game_settings/button.png)     | "button" or "rowbutton" | n/a            |                                   |
| Dropdown    | ![](images/custom_game_settings/dropdown.png)   | "dropdown"              | "int"          | {"label": string}                 |
| Textfield   | ![](images/custom_game_settings/textfield.png)  | "textfield"             | "string"       |                                   |
| Radio Group | ![](images/custom_game_settings/radiogroup.png) | "radiogroup"            | "string"       | [{"label": option name}, {} ....] |
| Toggles     | ![](images/custom_game_settings/toggles.png)    | "toggles"               | "int"          |                                   |

#### "is_editable"

Dictates if the setting can be edited when loading a saved custom game.

#### "save_last_used_value"

Default should be _true_

#### "enable_requirements"

What conditions are required for the setting to be enabled (meaning visible)

| Requirement             | Description                |
| ----------------------- | -------------------------- |
| "platform_ps"           | If Playstation platform    |
| "platform_not_gamecore" | If not PC or Xbox platform |
| "platform_pc"           | If PC platform             |
| "platform_nx"           | If Switch platform         |
| "platform_not_ps"       | If not PS platform         |
| "platform_not_nx"       | If not Switch platform     |
| "platform_not_steam"    | If not Steam platform      |

#### "game_mode_type"

Dictates which game mode the setting will appear in

"allmodes", "campaign" or "pvp"

#### slider_convert_to_percent

For slider settings only. Setting this to _true_ will convert the value to a percent visually in the menu. It will not affect the actual value used by the gameplay system.

#### "slider_step_incremenet"

For slider settings only. Numeric value to be used as the slider value increment. (Sliders default to 1)
![](images/custom_game_settings/sliderSteps.png)

### Using Custom Settings

After a custom game setting has been added [to the service? See what Allie has written above], it should appear in the front end when starting a new custom game, or loading a previously saved one in the case of custom campaigns. When the game is started (from the lobby) each setting and their values will be stored on the server and saved along with other game data.

The values for each custom game setting can be used with entity archetype data and/or in B# scripts.

#### Entity Archetypes

#### B# Scripts

_This section only covers specifically how custom game settings are used within B# scripts. For a better idea how B# is used generally, check out the [B# Reference Sheet](https://github.com/Mojang/minecraft-legends-docs/blob/main/BSharpReferenceSheet.md#b-reference-sheet)_

When the game is started from the lobby and the values of each setting have been receieved, each value is provided to scripts through a special bootstrap snippet **SNIPPET_GameSettingInitialized** wherever the custom game setting's name matches the first argument of the snippet.

```
SNIPPET_GameSettingInitialized("custom_game_setting_name", (value) => {
    const settingvalue = value.value

    // Do stuff with the setting's value
})
```

These snippets trigger before any other bootstrap snippets so that their effects can be applied before they are used anywhere else. This can be helpful in cases where things like data structures need to be altered with the provided value(s).

These snippets also trigger every time a game mode is launched, either from a new game or one that was previously saved. Since B# script files are always parsed at this time, previous changes in scripts that are not saved need to be re-applied each time a saved game is loaded to keep the behaviour consistent.

Note (for Devon): These settings have special handling in the game's engine which is not accessible through data:

- world_world_seed
- world_world_size
- world_world_mirror
