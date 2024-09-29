# Minecraft Legends Village Generation

## Village Planning

### Order of Operations

Village planning is done in the following sequence of steps:

**Generate Map** ⟶ **Sample Terrain** ⟶ **Districts & Zones** ⟶ **Plan Walls** ⟶ **Plan Paths** ⟶ **Finalize Wall Plan** ⟶ **Plan Buildings**

These steps often correspond to different kinds of cards. When a village deck is processed, the village generator will handle particular kinds of cards during different steps. For instance, all the District and Zone cards are handled during the Districts & Zones phase. This is true regardless of where those cards exist in the deck.

Cards of the same type will be handled in the order that they’re pulled off the deck. For example, the first buildable card in the deck will be the first buildable card that’s handled. This is important to keep in mind while assembling village generation decks. Buildables will often be competing for space in a village so it’s a good practice to request larger buildables first to ensure they have a good chance of placing.

To increase readability of B# scripts, it is best practice to organize B# scripts so that village generation decks are assembled in this order even though the village generator will automatically enforce an order regardless.

## Village Features

### Districts

All villages are composed of one or more districts. Each district is a collection of zones, but when a new district is placed, it will just exist as a position with a district name until zones are added to it. When a village is first created, a main district will be automatically created at the village’s starting position. Every zone that gets added to the village, will belong to a particular district.

---

#### District Cards

* **Card Library Name**: `district_cards`

To create a new district, add a card from the district_cards library to the deck. Multiply that card with some placement preference cards to influence where the new district should go relative to the main district.
After creating a new district, to add zones, buildables, walls, moats, etc to the district, multiply the card for the new request with the district card. If no district card is multiplied, the main district will be used as the default.

**Settings**:

* **`district`**: The unique name that identifies the district. When zones are added to a district, the district name will be added to their zone tag set. Every zone that belongs to the district will have the district name as part of their tag set.

**Example**:

```json
// JSON
    {
        "district_cards": [
            {
                "cost": 0,
                "tags": ["example_district_card"],
                "district": "example_district"
            }
        ]
    }
```

```jsx
// B#
    // Request a new district.
    const farDistrict = DistrictCard("example_district_card")

    // Add zones to the far district and then wrap them in walls.
    const farDistrictDeck = DECK_Empty()
    DECK_PutOnBottomOf(WallsCard("addWallWithGatesAndTower", 1), farDistrictDeck)
    DECK_PutOnBottomOf(LayerOfZonesCard("addLayerOfZones", 2), farDistrictDeck)
    DECK_PutOnBottomOf(ZonesCard("addThreeZones", 2), farDistrictDeck)
    DECK_MultiplyBySingle(farDistrictDeck, farDistrict)

    // Place the new district far away from the main district.
    const placeFarFromVillageCenter = PlacementPreferenceCard("placeFarFromVillageCenter")
    DECK_MultiplyBySingle(farDistrict, placeFarFromVillageCenter)
    DECK_PutOnBottomOf(farDistrict, baseDeck)

    // Add the requests for the new district to the base deck.
    DECK_PutOnBottomOf(farDistrictDeck, baseDeck)

    OUTPUT_SetNamedDeck("instantBuildDeck" + villageID, baseDeck)
```

---

### Zones

The zones that belong to the village districts organize the area inside the village and they provide the space to place village features like buildables. To add zones to a village district, there are two types of cards available, `zones_cards` and `layer_of_zones_cards`.

---

#### Zone Cards

* **Card Library Name**: `zones_cards`

These cards will add one or more zones to a village. The first zone added will use any placement preferences multiplied with the zone card to find the best zone (the zone with the highest score). If the number of zones requested is greater than one, we’ll continue trying to add zones that are adjacent to the first zone added until we’ve either run out of connected zones that aren’t already part of the village, or we’ve added the requested number of zones.

**Settings**:

* **`number_of_zones`**: The number of zones that should be added to the village.

**Example**:

```json
// JSON
    // ZONES
    {
        "zones_cards": [
            {
                "cost": 0,
                "tags": ["add24Zones"],
                "number_of_zones": 24
            },
            {
                "cost": 0,
                "tags": ["add18Zones"],
                "number_of_zones": 18
            }
        ]
    }
```

```jsx
// B#
    // INITIAL ZONES
    const initialSmallZonesPartOne = ZonesCard("addZone", 5);
    DECK MultiplyBySingle(initialSmallZonesPartOne, ZoneHeightChangeCard("4DownRelativeToCentre"));
```

* _Notes_:
  * Zones are always added to a district. If no explicit district is specified, the main district is used by default.
  * Placement preferences can be used to specify where zones should be added to the village.
  * If `number_of_zones` is greater than 1, after the first zone is added, the zones neighboring that zones will be added until the amount requested has been added.
  * Only zones that aren’t already part of the existing village will be considered.
  * `zone_tag_cards` can be combined with `zones_cards` to reserve zones for particular features like paths, structures, etc.

---

#### Layer of Zones Cards

* **Card Library Name**: `layer_of_zones_cards`

When this card is handled, it will add all the zones that border the existing village district that this card is being applied to. If no district card is specified, the default main district will be used.

**Example**:

```json
// JSON
    {
        "layer_of_zones_cards": [
            {
            "cost": 0,
            "tags": ["addLayerOfZones"]
            }
        ]
    }
```

```jsx
// B#
const ring1Zones = LayerOfZonesCard(
  "addLayerOfZones",
  _GetLayerOfZoneSize(villageId, 1)
);
DECK_MultiplyByMultipleRules(ring1Zones, [
  ZoneTagCard("attackRing1"),
  ZoneHeightChangeCard("3DownRelativeToCentre"),
]);
```

* _Notes_:
  * Layers of zones are always added to a district. If no explicit district is specified, the main district is used by default.
  * The outer layer of zones around all the district zones that aren’t part of the village yet will be added to the district.
  * `zone_tag_cards` can be combined with `layer_of_zones_cards` to reserve zones for particular features like paths, structures, etc.
  * If your layers of zones get weird zones that stick out from them, look into the `minimum_loz_connection_width` setting. (Found in the village_zone component. e.g `badger:village_zone` within `piglin_obstacle_large.json`)
  * Layers of zones by default try to avoid having tight pinch points in the ring it creates, and tries to add zones to pad out thin sections.  It can cause issues specifically with hex based zones.

![Example Visual](images/village_generation/image08.png)

```jsx
// B#
  const northZone = ZonesCard("addZone", 1);
  DECK_MultiplyByMultipleRules(northZone, [
    ZoneHeightChangeCard("20Height"),
    PlacementPreferenceCard("placeInDirectionNorthWithWedgeBrush"),
  ]);

  const southZone = ZonesCard("addZone", 1);
  DECK_MultiplyByMultipleRules(southZone, [
    ZoneHeightChangeCard("SHeight"),
    PlacementPreferenceCard("placeInDirectionSouthWithWedgeBrush"),
  ]);

  const eastZone = ZonesCard("addZone", 1);
  DECK_MultiplyByMultipleRules(eastZone, [
    ZoneHeightChangeCard("10Height"),
    PlacementPreferenceCard("placeInDirectionEastWithWedgeBrush"),
  ]);

  const westZone = ZonesCard("addZone", 1);
  DECK_MultiplyByMultipleRules(westZone, [
    ZoneHeightChangeCard("15Height"),
    PlacementPreferenceCard("placeInDirectionWestWithWedgeBrush"),
  ]);
```

_In this example, four raised zones are added to the north, south, east, and west using directional placement preference cards._

---

#### Zone Tag Cards

* **Card Library Name**: `zone_tag_cards`

When zones are added to a village district, they can be given one or more tags so that they can be identified later with a card from the `zone_filter_cards` library. Multiply a card from the `zones_cards` library or the `layer_of_zones_cards` library with a card from the `zone_tag_cards` library to add that tag to the zone tag set.

**Settings**:

* **`zone_tag`**: The name of the tag that should be added to the tag set of the zone(s) that it's applied to.

---

#### Zone Filter Cards

**Card Library Name**: `zone_filter_cards`

When a village feature needs to be placed in zones that have been given a specific zone tag (see `zone_tag_cards`), a card from the `zone_filter_cards` library with a `zone_filter` that matches the zone tag should be multiplied with the village feature card. For example, if a group of zones have been multiplied with a zone tag card with a `zone_tag` of `tower_town`, multiplying a card from the `buildable_cards` library with a `zone_filter_cards` card that has a `zone_filter` of `tower_town` (and exclude set to false) will limit the zones that the buildable can place in to the `tower_town` zones.

**Settings**:

* **`zone_filter`**: The zone tag name to filter zones by.

* **`exclude`**: Specifies where the `zone_filter` zone tag name should be used to exclude or include zones. This can be true or false.

---

#### Zone Height Change Cards

**Card Library Name**: `zone_height_change_cards`

When multiplied with cards from the `zones_cards` and `layer_of_zones_cards` libraries, the zones added to the village will raise or lower the height of the terrain inside the zone depending on the settings specified on the zone height change card.

**Settings**:

* **`zone_height_change`**: The distance in blocks that the terrain inside this zone will be raised or lowered. This value can be positive or negative.

* **`biome`**: The name of the biome that this zone should be changed to. This is optional.

* **`zone_height_control`**: This specifies how the zone_height_change should be applied. The options are:

  * **`height_control_centered`**: Relative to the original zone height of the first zone that was added to the village.
  * **`height_control_averaged`**: Relative to the village average zone height of all the zones in the entire village map. This includes zones that haven’t been added to the village.
  * **`height_control_lowest`**: Relative to the lowest zone in the group of zones being added with this particular request.
  * **`height_control_none`**: Relative to the original zone height of the zone being raised or lowered.

---

#### Moats and Pools

Moat cards can be used to either create a moat that encircles a village district or a bunch of individual lava pools.

#### Moat Cards

**Card Library Name**: `moat_cards`

**Settings**:

* **`biome`**: The name of the biome that will exist inside of the moat zones that get added when this card is played. The particular types of blocks that will exist inside of these moat zones will be determined by the biome.

* **`width_in_blocks`**: How wide the moat should be in blocks. The moat will claim as many zones as it needs to meet the requested block width and then the edges of the moat will be displaced inward to reduce the width of the moat if the zone additions resulted in a width that's larger than the requested width.

There are a number of other village generation settings that affect the zone sizes (grid shape, zone jitter) and the moat edges (edge noise) so this new setting will need to be tuned with those others in mind.

* **`distance_in_zones`**: The distance in zones from the district that the moat will be placed around.

* **`filling_depth`**: [**DEPRECATED**] The **`biome`** now determines what blocks go where inside the moat zone.

**Example**:

```json
// JSON
  {
    "moat_cards": [
      {
        "cost": 0,
        "tags": ["DBBMoat"],
        "biome": "lava moat",
        "width_in_blocks": 10,
        "filling_depth": 5,
        "distance_in_zones": 1
      }
    ]
  }
```

* _Notes_:
  * If `distance_in_zones` is 0, a moat will not be placed. Instead, all the zones that have been tagged (using `zone_tag_cards`) with a `lava_option` tag will be turned into a pool.
  * If `distance_in_zones` is greater than 0, an external moat will be placed around the district specified. If no district has been explicitly specified using one of the `district_cards`, the main district will be used by default.
  * Use `zone_height_change_cards` to control the height of the moat zones.

---

### Walls

**Card Library Name**: `wall_cards`

Wall cards can be used to place walls along the edges of village zones.

#### Wall Cards

To place walls, first multiply all the zone cards that you want to place walls around with a zone tag card. Then multiply a wall card with a zone tag filter card with the same `zone_tag_filter` tag.

* (Optional): If you want wall fragments / gaps in the wall, you can multiply the wall card with a placement preference card(s) and a threshold card. This will score the zones that the wall would be placed around and if a zone score is less than the threshold, that zone won’t get a wall.

The best way to create a gate or entrance in a set of walls is to request a path that connects the zones inside the walls (using the same zone tag) to some zones that are outside of the walls. The wall that the path crosses will be replaced with a gate.

* _Note_: When terrain weathering is applied, walls can slip off the side of weathered edges. To avoid this, there is a `wall_offset` setting in the `badger:village_wall` component. This offset will move walls away from the edge. If it’s tuned greater than the maximum terrain weathering amount, that should ensure walls don’t slip.

```json
// JSON
  {
    "badger_village_wall": {
      "wall_offset": 3.0
    }
  }
```

**Settings**:

* `wall_buildable`: The name of the buildable to place for the wall.

* `path_entrance`: The name of a buildable. If a path crosses this wall, the wall segment that’s crossed will be removed and replaced with this entrance buildable. Usually this is a gate buildable.

* `embedded_buildables`: A list of buildable names. When the wall is placed, each one of them will be placed at random locations along the wall if there’s room. It’s not recommended to use these for entrances. It’s better to use path cards so that gates will be placed in locations that can connect with the path.

---

### Paths

Path cards are used to place paths that connect village zones. To connect a buildable to a path, see the `build_from_district_path` and `connect_to_path` placement preference cards.

#### Path Cards

**Card Library Name**: `path_cards`

`path_cards` need to be set up in a particular way before they’re added to the deck so that they’re handled correctly later when the deck is processed. For this reason, there is a special `B#` helper method to request a path that will connect two village zones, `CreatePathRequestOnBottomOf`.

```jsx
// B#

  const southPathStartRules = [PlacementPreferenceCard("closeToDistrictStart")];
  const southPathEndRules = [
    ZoneFilterCard("southPathZone"),
    PlacementPreferenceCard("closeToDistrictStart"),
    PlacementPreferenceCard("placeInDirectionSouthWithRectangleBrush"),
  ];
  CreatePathRequestOnBottomOf(
    "defend_district_path",
    southPathStartRules,
    southPathEndRules,
    baseDeck
  );
```

The first parameter takes a tag identifier for the path card that will be used. In this case `defend_district_path`. The second and third parameters take arrays of rule cards that will be used to identify the best start and end zone for the path. The final parameter takes the deck which this new path request will be put on the bottom of.

The rule cards function in the same way as when they’re multiplied with zone, district, or buildable request cards. Zones will first be filtered and any placement preferences will be used to score the remaining zones so that we can find the best / highest scoring zone. This is done for the path start and path end.

There is also another special B# helper function if you want a path that connects a village zone to the closest path to that zone, `CreatePathFromZoneRequestOnBottomOf`.

```jsx
// B#
  const northwestPathStartRules = [
    ZoneFilterCard("outsideBaseZone"),
    PlacementPreferenceCard(PLACEMENT_CLOSE_TO_VILLAGE_START),
    PlacementPreferenceCard("placeInDirectionNorthWestWithRectangleBrush"),
  ];

  CreatePathFromZoneRequestOnBottomOf(
    "attack_district_path",
    northwestPathStartRules,
    baseDeck
  );
```

The first parameter takes a tag identifier for the path card that will be used. In this case `defend_district_path`. The second parameter takes an array of rule cards that will be used to identify the best zone for the path to start in. Since the path will connect to the closest existing path, there’s no need for the additional array of rule cards that the `CreatePathRequestOnBottomOf` method takes. The final parameter takes the deck which this new path request will be put on the bottom of.

* _Notes_:
  * Bridges will be placed when the path connects two zones with a body of water or when the path connects two zones that have a large enough height change.
  * Gates will be placed when the path crosses a wall.
  * Bridges failing to place can cause paths to fail too.  Adding a `force_building_placement_cards` card on either set of path rules passed into the CreatePathRequestOnBottomOf call will place the path without bridges if it would fail otherwise.  This is currently used by defend bases to ensure gates appear even if bridges won’t.

**Settings**:

* `is_district_path`: If this is set to true, the path created with this request will use the path settings defined in the `village_district_path` component on the village entity. Otherwise the settings in the `village_building_path` will be used. An assert will be triggered if the component needed doesn't exist.

**Example**:

```json
// JSON
  {
    "path_cards": [
      {
        "cost": 0,
        "tags": ["village_district_path"],
        "is_district_path": true
      },
      {
        "cost": 0,
        "tags": ["village_building_path"],
        "is_district_path": true
      }
    ]
  }
```

![Old Dev Village](images/village_generation/image14.png)
![Old Dev Path](images/village_generation/image15.png)

---

### Terrain Weathering

**Card Library Name**: `terrain_weathering_cards`

Without terrain weathering, zone height changes will result in a lot of straight and perfectly smooth cliffs. Village weathering cards help rough them up a bit. The effect is a combination of a sawtooth wave layered with gradients and a lot of noise.

#### Terrain Weathering Cards

Adding this card to a village generation deck will apply terrain weathering to all the edges of zones that have been raised or lowered with height change or moat cards. The terrain weathering card doesn’t have any settings. See the `badger:village_weathering` component for the terrain weathering settings.

**Example**:

![Terrain Weathering](images/village_generation/image17.png)
![Terrain Weathering 2](images/village_generation/image18.png)

---

### Buildables

Buildables (aka structures or buildings) are requested using buildable cards.

#### Buildable Cards

**Card Library Name**: `buildable_cards`

Cards from the `buildable_cards` library are often multiplied with district, placement preference, and zone filter cards to control where they get placed in the village.

* _Notes_:
  * It’s best practice to add the largest, most important buildable cards to the deck first. Buildables will be placed in the order that the cards are pulled from the deck. If smaller non-critical structures are placed first, they might space out and take up the room needed to place the larger structures.
  * Buildable cards are the only kind of village feature card that can be handled after a village is done in generating for the first time. For example, when piglin bases respond to player attacks in campaign or when they rebuild in PvP.

**Settings**:

* `buildable`: The identifier of the buildable.

---

## Placement Preference Cards

**Card Library Name**: `placement_preference_cards`

In general, placement preference cards should be multiplied with other cards like district, path, wall, zone, and buildable cards to change how those cards get handled.

---

### Circle Sine Wave

**Card Name**: `circle_sine_wave`

This placement preference will use the angle between the zone positions and the district start position to sample from a sine wave. The result from that sine wave is shifted so that the placement preference will by default add a score between `0` and `1` to the zone being scored.

**Settings**:

* **`amplitude`**: By default, the `circle_sine_wave` placement preference will add a score between 0 and 1. This score will be multiplied with the amplitude. As a result, the amplitude can be used to increase or decrease the effect that this placement preference has on the final score.

* **`frequency`**: The frequency determines the number of times the sine wave oscillates between 0 and 1 over the 0 to 360 degree range.

**Example**:

```json
//JSON
  {
    "cost": 0,
    "tags": ["example_circle_sine_wave"],
    "placement_preference": "circle sine wave",
    "amplitude": 0.45,
    "frequency": 5.0
  }
```

![Circle Sine Wave Example](images/village_generation/image20.png)

_When combined with a wall request card and a threshold card, `circle_sine_wave` can be used to create fragmented walls. The yellow lines in the example image above are an easy way to visualize the sine wave as it affects the scores of the zones._

---

### Close to Buildable

**Card Name**: `close_to_buildable`

This placement preference will score zones higher if they are close to the specified buildable area.

**Settings**:

**`buildable`**: The tag for the buildable to place close to. This should match one of the tags in the `tags` list in the `badger:tags` component for the buildable entity being used.

**Example**:

```json
// JSON
  {
    "cost": 0,
    "tags": ["example_close_to_pig_tower"],
    "placement_preference": "close_to_buildable",
    "buildable": "pigTower"
  }
```

![Close to Buildable Example](images/village_generation/image22.png)

_In this example, the towers are requested close to the buildable with the `pigGate` tag._

---

### Close to District Center

**Card Name**: `close_to_district_center`

This placement preference will score zones higher if they're closer to the center point of all the district zones. The closest zone(s) will receive a score of 1, the farthest zone(s) will receive a score of 0, and all other district zones will receive a score in between 0 and 1 based on their distance from the center point.

* _Note_: The district center will update each time a new zone is added to the district. That means the center will be continuously changing until all the zones have been added. Using the `close_to_district_center` before all the zones have been added may have some surprising results.

**Example**:

```json
// JSON
  {
    "cost": 0,
    "tags": ["example_close_to_district_center"],
    "placement_preference": "close to district center"
  }
```

![close_to_district_center Example](images/village_generation/image24.png)

_This example base has a main district in the center, a separate district on the left that's lower than the main district, and a separate district on the right that’s higher than the main district. All the zones in the district on the left were added with the `close_to_district_start` placement preference. All the zones in the district on the right were added with the `close_to_district_center` placement preference. Likewise, the tower in the district on the left was requested with the `close_to_district_start` placement preference and the tower in the district on the right was requested with the `close_to_district_center` placement preference._

---

### Close to District Start

**Card Name**: `close_to_district_start`

The `close_to_district_start` placement preference will score zones higher if they’re closer to the zone that was added first to the district. The zone that was added first will contain the starting position and will be given a score of 1. The farthest zone(s) from the starting position will be given a score of 0 and all the zones in between will be given a score between 0 and 1 depending on how far they are from the start.

**Example**:

```json
// JSON
  {
    "cost": 0,
    "tags": ["example_close_to_district_start"],
    "placement_preference": "close_to_district_start"
  }
```

---

### Close to Influence

**Card Name**: `close_to_influence`

This placement preference will score zones higher if they’re close to a particular unit that has the `badger:village_influence` component. This can be used to place buildables near attacking enemy mobs or friendly mobs during gameplay.

**Settings**:

**`alliance_rule_filter`**: Indicates what units should influence this placement preference. Possible values include “enemy”, “friendly”, and “any_team”.

---

### Close to Village Start

**Card Name**: `close_to_village_start`

The placement preference is the same as the close_to_district_start placement preference. It exists because it was added before it was possible to have multiple districts in a village.

* _Miclee Note_:
  * I have been using `close_to_village_start` a lot in the past, is it really the same? I need to test.

---

### Close to Walls

**Card Name**: `close_to_walls`

This placement preference scores zones higher based on the number of walls bordering them.

* _Note_: The `close_to_buildable` placement preference can be used for the same purpose if a wall tag is specified for the `buildable` setting. This `close_to_wall` placement preference will only score zones higher if the zones actually have walls bordering them whereas the `close_to_buildable` placement preference will also score nearby zones higher even if they don’t have walls actually bordering them.

---

### Close to Water

**Card Name**: `close_to_water`

This placement preference scores zones higher when they are in proximity to a zone containing water.

**Example**:

```json
// JSON
  {
    "cost": 0,
    "tags": ["example_close_to_water"],
    "placement_preference": "close_to_water"
  }
```

![Close to Water Example](images/village_generation/image27.png)

In this example, the nether spreaders were requested outside the village and close to water.

---

### Connect to Path

**Card Name**: `connect_to_path`

This placement preference card can be multiplied with a buildable request card. Buildable request cards that have the `connect_to_path` placement preference will try to generate a path from the position of the buildable to the nearest path.

* _Notes_:
  * Since the buildable is placed and then the path is created, it’s common for the path to not be able to find or connect to an existing path because of the buildables placement. For instance, if the buildable is placed such that it’s facing another buildable, it’s likely the path won’t have enough space to place.
  * The `build_from_district_path` placement preference is generally a better method for connecting buildables to paths.

**Example**:

```json
//JSON
    {
        "cost": 0,
        "tags": ["connectToPath"],
        "placement_preference": "connect_to_path"
    }
```

---

### Clear Resources In Zone

**Card Name**: `clear_resources_in_zone`

This placement preference can be multiplied with a zone request card. Zone request cards with this placement preference will remove all the world features that overlap the zone(s) that get added to the village when the zone request card is handled.

* _Note_: It is **not** necessary to use this card to clear world features for buildables. World features will be removed if they overlap buildables regardless by default.

---

### Disable Spacing

There is a default spacing behavior that tries to distribute structures so that they don’t bunch up too much when they have a lot of room to place in. That spacing behavior is turned off for a particular buildable request if the `disable_spacing` placement preference is multiplied with that buildable request.

**Card Name**: `disable_spacing`

---

### Distance From District Start

**Card Name**: `distance_from_district_start`

This placement preference will score zones higher if the distance between the zone site and the district start is close to a specified distance. This placement preference can be multiplied with district, buildable, and path requests.

* _Note_: This preference will be most reliable in low jitter bases. This is especially true if you are using smaller distances and tight tolerances with this preference.

**Settings**:

* **`distance_from_district_start`**: The distance (range or single value) in blocks from the district starting position that the placement should ideally be placed.

* **`distance_to_zero_score`**: The distance from the `distance_from_district_start` until the score added to zones by this placement preference is reduced to 0. For the example card below, with placement preference will add 1 to the zones that are between 80-90 blocks from the district start. At 70-80 and 90-100 blocks the score added will be a value between 0 and 1 depending on how far it is from 80 and 90 blocks away.

**Example**:

```json
// JSON
  {
    "unique_card_id": "example_distance_district_start",
    "cost": 0,
    "tags": ["example_distance_district_start"],
    "placement_preference": "distance_from_district_start",
    "distance_from_district_start": [80, 90],
    "distance_to_zero_score": [10, 10]
  }
```

```jsx
// B#
  // North District
  const districtNorth = DistrictCard("district1");
  DECK_MultiplyByMultipleRules(districtNorth, [
    PlacementPreferenceCard("example_distance_from_district_start"),
    PlacementPreferenceCard("placeInDirectionNorthwithRectangleBrush"),
    PlacementPreferenceCard("withScoreThresholdSmall")
  ]);
  DECK_PutOnBottomOf(districtNorth, baseDeck);
```

_In this example the placement preference is being used to place a district a specific distance from the village start/center._

![distance_from_district_start Visual](images/village_generation/image31.png)

---

### Elevation Range

**Card Name**: `elevation_range`

This placement preference will score zones higher if the normalized height at the zone site is close to the specified target height.

* _Note_:
  * If you aren't using a score threshold, zones that fall outside the range can still be used once good zones are exhausted, so be careful.

**Settings**:

* **`elevation_target`**: This can be a value between 0 and 1. If a zone has a normalized height of 1, that zone is the highest zone in the village.

* **`elevation_max`**: This can be a value between 0 and 1. It’s the upper limit for zone heights. If a zone has a normalized height that’s higher, this placement preference won’t add any score.

* **`elevation_min`**: This can be a value between 0 and 1. It’s the lower limit for zone heights. If a zone has a normalized height that’s lower, this placement preference won’t add any score.

**Example**:

```json
// JSON
  {
    "cost": 0,
    "tags": ["elevationTargetMax"],
    "placement_preference": "elevation_range",
    "elevation_target": 1.0
  },
  {
    "cost": 0,
    "tags": ["elevationRangeLow"],
    "placement_preference": "elevation_range",
    "elevation_target": 0.1,
    "elevation_max": 0.4,
    "elevation_min": 0.0
  }
```

![elevation_range Visual](images/village_generation/image33.png)

---

### Facing Buildable

**Card Name**: `facing_buildable`

This placement preference can be multiplied with buildable request cards. Buildable request cards with this placement preference will try to place with an orientation that faces the nearest specified buildable if one exists.

* _Notes_:
  * Buildables can only face cardinal directions due to their blocky nature. If the buildable to face is north west of the buildable being placed, the placed buildable should face north or west.
  * Without an orientation placement preference, buildables will choose their facing direction randomly.

**Settings**:

* **buildable** The tag for the buildable to face towards. For example, if we wanted to create a `facing_buildable` card that makes buildables face the nearest fountain, we would set this setting to `fountain` because the fountain buildable has the `fountain` tag in its `badger:tags` component `tags` list.

**Example**:

![facing_buildable Example](images/village_generation/image34.png)

_All these buildings have a `facing_buildable` placement preference with fountain specified as the building to face._

---

### Facing Influence

**Card Name**: `facing_influence`

This placement preference can be multiplied with buildable request cards. Buildable request cards with this placement preference will try to place with an orientation that faces the nearest source of influence that passes the specified alliance rule filter. Sources of influence will have the `badger:village_influence` component. This can be used to place buildables that face towards attacking enemy mobs or friendly mobs during gameplay.

**Settings**:

* **`alliance_rule_filter`**: Indicates what units should influence this placement preference. Possible values include `enemy`, `friendly`, and `any_team`.

---

### Facing Water

**Card Name**: `facing_water`

This placement preference can be multiplied with buildable request cards. Buildable request cards with this placement preference will try to place with an orientation that faces the nearest body of water if one exists.

* _Notes_:
  * Buildables can only face cardinal directions due to their blocky nature. If the body of water to face is north west of the buildable being placed, the placed buildable should face north or west.
  * Without an orientation placement preference, buildables will choose their facing direction randomly.

**Example**:

![facing_water Example](images/village_generation/image35.png)

_All of these buildings have the `facing_water` placement preference._

---

### Facing Cardinal Direction

**Card Name**: `facing_cardinal_direction`

This placement preference card can be multiplied with a buildable request card. Buildable request cards that have the `facing_cardinal_direction` placement preference will be rotated to face the specified cardinal direction.

**Settings**:

* **`cardinal_direction`**: The cardinal direction that the buildable should face. The options are north, south, east, or west.

**Example**:

```json
// JSON
  {
    "cost": 0,
    "tags": ["facingNorth"],
    "placement_preference": "facing_cardinal_direction",
    "cardinal_direction": "north"
  }
```

```jsx
// B#
  DECK_MultiplyBySingle(WoF, PlacementPreferenceCard("facingNorth"))
```

---

### Far From Buildable

**Card Name**: `far_from_buildable`

The `far_from_buildable` placement preference is the inverse of the `close_to_buildable` placement preference.

**Settings**:

* **`buildable`**: The tag for the buildable to place close to. This should match one of the tags in the `tags` list in the `badger:tags` component for the buildable entity desired.

**Example**:

![far_from_buildable Example](images/village_generation/image38.png)

_The barracks in this example were requested far from buildables with the tag `pigTower`._

---

### Far From District Center

**Card Name**: `far_from_district_center`

The `far_from_district_center` placement preference is the inverse of the `close_to_district_center` placement preference.

**Example**:

```json
// JSON
  {
    "cost": 0,
    "tags": ["example_far_from_district_center"],
    "placement_preference": "far_from_district_center"
  }
```

---

### Far From District Start

**Card Name**: `far_from_district_start`

The `far_from_district_start` placement preference is the inverse of the `close_to_district_start` placement preference.

**Example**:

```json
// JSON
  {
    "cost": 0,
    "tags": ["example_far_from_district_start"],
    "placement_preference": "far_from_district_start"
  }
```

---

### Far From Influence

**Card Name**: `far_from_influence`

The `far_from_influence` placement preference is the inverse of the `close_to_influence` placement preference.

**Settings**:

* **`alliance_rule_filter`**: Indicates what units should influence this placement preference. Possible values include `enemy`, `friendly`, and `any_team`.

---

### Far From Water

**Card Name**: `far_from_water`

The `far_from_water` placement preference is the inverse of the `close_to_water` placement preference.

---

### Far From Village Start

**Card Name**: `far_from_village_start`

The placement preference is the same as the `far_from_district_start` placement preference. It exists because it was added before it was possible to have multiple districts in a village.

---

### Noise

**Card Name**: `noise`

The noise placement preference will sample a 2D noise map at village zone positions and add the result to the zone scores.

**Settings**:

* **`amplitude`**: By default, the `noise` placement preference will add a score between 0 and 1. This score will be multiplied with the amplitude. As a result, the `amplitude` can be used to increase or decrease the effect that this placement preference has on the final score.

* **`scale`**: A lower value will result in a noise map with more gradual changes. A higher value will have the opposite effect.

**Example**:

```json
{
    "cost": 0,
    "tags": [ "example_noise" ],
    "placement_preference": "noise",
    "amplitude": 0.45,
    "scale": 0.02
}
```

![noise placement_preference](images/village_generation/image42.png)

_When combined with a wall request card and a threshold card, noise can be used to create fragmented walls._

---

### Build From District Path

**Card Name**: `build_from_district_path`

This placement preference card can be multiplied with a buildable request card. Buildable request cards that have the `build_from_district_path` placement preference will place buildables that are connected to a specified district path. This guarantees that the placed buildable is connected to a path and not facing the side of another buildable.

* _Notes_:
  * In `village.json`, there is a setting for the maximum distance allowed from the starting path zone.  This is to keep the buildable from running too far away, but tags and filters will usually kick in first.

    ```json
    {
        "village_building": {
            "build_from_path_max_range_in_blocks": 100
        }
    }
    ```

  * In the village definition json (eg `villager_village_001.json`) the district paths need to "prevent buildable placement".  If this is turned off, paths will start cutting through buildables and removing the bottom layers of blocks.

    ```json
    {
        "prevent_buildable_placement": true
    }
    ```

  * The card is just a placement preference, and is multiplied like usual.  The main things to watch out for are that you have a valid district path to connect to, and that your tag filters/tags all agree on that.

**Example**:

```json
// JSON:
    {
        "cost": 0,
        "tags": [ "example_build_from_district_path" ],
        "placement_preference": "build_from_district_path"
    }
```

```jsx
//B#
    // North Houses
    DECK_MultiplyByMultipleRules(comboVillageDeckNorth, [
    PlacementPreferenceCard("example_build_from_district_path"),
    DistrictCard("district1"),
    ]);
    DECK_PutOnBottomOf(comboVillageDeckNorth, villageDeck);

```

![build_from_district_path example](images/village_generation/image47.png)

---

### Across Path

**Card Name**: `across_path`

This placement preference card can be multiplied with a buildable request card. Buildable request cards that have the `across_path` placement preference will place buildables on the path.

* _Notes_:
  * This placement preference lets buildables place on top of paths. Usually our collision checks prevent that.
  * Make sure you have a path requested.

**Example**:

```json
// JSON
    {
        "cost": 0,
        "tags": [ "example_across_path" ],
        "placement_preference": "across_path"
    }
```

![Arches use across_path](images/village_generation/image49.png)

_The arches use the `across_path` placement preference._

### Along Path

**Card Name**: `along_path`

This placement preference card can be multiplied with a buildable request card. Buildable request cards that have the `along_path` placement preference will place buildables next to a path.

* _Notes_:
  * This placement preference lets buildables place closer to paths than our collision checks would usually allow.
  * Make sure you have a path requested.

**Settings**:

* **`offset_from_path`**: The distance from the path in blocks that the buildable should place.

**Example**:

```json
// JSON
    {
        "cost": 0,
        "tags": [ "example_along_path" ],
        "placement_preference": "along_path"
    }
```

![Benches use along_path](images/village_generation/image49.png)

_The benches use the `along_path` placement preference._

---

### Ignore Zone Filter For Overlapping Zones

**Card Name**: `ignore_zone_filter_for_overlapping_zones_card`

This placement preference can be multiplied with buildable request cards. By default, when we’re checking if a buildable request with a zone filter card can be placed in a zone, if that buildable overlaps a neighboring zone that doesn’t pass the filter, the buildable won’t be placed in that location.

* _Notes_:
  * If the structures are failing to place, but the village looks like it has a lot of open space where they intuitively should be placed, this placement preference might help.
  * The placed buildable center will still be inside a zone that passes the filter. It’s just the overlapping zones that might not pass the filter.

---

### In Direction Of Influence

**Card Name**: `in_direction_of_influence`

This placement preference can be multiplied with buildable request cards. Buildable request cards with this placement preference will try to place in the direction of a source of influence that passes the specified alliance rule filter. Sources of influence will have the `badger:village_influence` component. This is similar to the `wedge_brush` placement preference except the direction is going to be towards the source of influence. This can be used to place buildables in the direction of an enemy attack.

**Settings**:

* **`alliance_rule_filter`**: Indicates what units should influence this placement preference. Possible values include `enemy`, `friendly`, and `any_team`.

* **`wedge_angle`**: The angle between the two sides of the wedge. This determines how wide the wedge area is. See the wedge_brush placement preference for an example.

---

### Random

**Card Name**: `random`

The random placement preference will sample a random value between 0 and 1 and add the result to the zone scores.

---

### Rectangle Brush

**Card Name**: `rectangle_brush`

Zones contained inside the rectangle brush will be scored higher the closer the zone is to the middle of the rectangle. The score added is between 0 and 1.

* _Notes_
  * The rectangle has an infinite length. Use this placement preference with the distance_from_district_start to limit the distance.
  * Multiply this placement preference with a district card to specify the starting position of the rectangle.
  * Don’t make the width smaller than the village zone_spacing or it might be possible for no zone sites to exist inside the area.

**Settings**:

* **`direction_angle`**: The direction that the rectangular area will extend towards.

* **`rectangle_width`**: The width of the rectangular area.

**Example**:

```json
// JSON
  {
    "cost": 0,
    "tags": ["placeInDirectionWestWithRectangleBrush"],
    "placement_preference": "rectangle_brush",
    "direction_angle": 180.0,
    "rectangle_width": 60.0
  }
```

![rectangle_brush](images/village_generation/image53.png)

---

### Score Threshold

**Card Name**: `score_threshold`

Zones with a score less than the threshold specified on this placement preference will not be considered for the buildable, district, path, or wall being requested. If no zone exists that passes the threshold, the request will be canceled.

* _Note_:
  * Only use this placement preference if it’s ok for the request to fail.

**Settings**:

**`threshold`**: Only zones that have a score that is higher than this threshold value will be considered.

**Example**:

```json
// JSON
  {
    "cost": 0,
    "tags": ["withScoreThresholdSmall"],
    "placement_preference": "score_threshold",
    "threshold": 0.3
  }
```

---

### Wedge Brush

**Card Name**: **`wedge_brush`**

Zones contained inside the wedge brush will be scored higher if the direction aligns with the requested direction. The score added is between 0 and 1.

* _Notes_:
  * The wedge has an infinite length. Use this placement preference with the distance_from_district_start to limit the distance.
  * Multiply this placement preference with a district card to specify the starting position of the wedge.
  * Don’t make the wedge_angle so small that the distance between the wedge sides might be smaller than the village zone_spacing or it might be possible for no zone sites to exist inside the area.

**Settings**:

**`direction_angle`**: The direction that the triangular area will extend towards.

**`wedge_angle`**: The angle between the two sides of the wedge. This determines how wide the wedge area is.

**Example**:

```json
// JSON
  {
    "cost": 0,
    "tags": ["placeInDirectionEastWithWedgeBrush"],
    "placement_preference": "wedge_brush",
    "direction_angle": 0.0,
    "wedge_angle": 45.0
  }
```

![wedge_brush](images/village_generation/image56.png)

---

### Facing Invasion Target

**Card Name**: **`facing_invasion_target`**

This placement preference card can be multiplied with a buildable request card. Buildable request cards that have the `facing_invasion_target` placement preference will be rotated to face the village being invaded.

* _Note_:
  * This placement preference can only be used in villages that are participating in an invasion. (they need the `InvasionAttack_OwnedComponent`)

---

## Other Cards

### Force Building Placement Cards

**Card Library Name**: `force_building_placement_cards`

Building request cards that have been multiplied with a force building placement card will ignore these constraints:

* Buildables can’t place in water
* Buildables with a zone filter card cannot place such that they overlap zones that don’t pass the filter (see `ignore_zone_filter_for_overlapping_zones_card`)
* Buildables can’t overlap paths if the path has `prevent_buildable_placement` set to true

When creating a path request with the `CreatePathRequestOnBottomOf` helper, if a force building placement card is added to either set of path rules, gates will still be placed along the path where the path crosses walls even if bridges fail to place. By default, if bridges fail to place, that will cause the entire path to fail which results in no gates.

**Example**:

```jsx
// B#
  // determine portal size
  if (IsSmall(baseSize)) {
      portal = BuildableCard("addPortalSmall");
      DECK_MultiplyBySingle(portal, ZoneHeightChangeCard("4Height"));
      DECK_MultiplyBySingle(portal, ForceBuildingPlacementCard("forcePlacement"));
    } else if (IsMedium(baseSize)) {
      portal = BuildableCard("addPortalMedium");
      DECK_MultiplyBySingle(portal, ZoneHeightChangeCard("9Height"));
      DECK_MultiplyBySingle(portal, ForceBuildingPlacementCard("forcePlacement"));
    } else if (IsLarge(baseSize)) {
      portal = BuildableCard("addPortalLarge");
      DECK_MultiplyBySingle(portal, ZoneHeightChangeCard("16Height"));
      DECK_MultiplyBySingle(portal, ForceBuildingPlacementCard("forcePlacement"));
    }
```

### Heart Cards

**Card Library Name**: `heart_cards`

Multiplying a buildable card with a heart card will make that structure a village heart. If all the village hearts get destroyed, that will destroy the village.

### Appearance Override Cards

**Card Library Name**: `appearance_override_cards`

Multiplying a buildable card with an appearance override card will override the visuals of the buildable. Multiplying a path card with an appearance override card will override the visuals of any bridge buildables placed by the path.

**Settings**:

* **`horde`**: The horde visuals that should be applied.

**Example**:

![appearance_override_cards](images/village_generation/image58.png)

_The bridge belongs to the `DBB` horde, but the appearance is overridden with the `obstacle` horde visuals._

### Hanging Decoration Cards [Not used, may still function]

**Card Library Name**: `hanging_decoration_cards`

Could be used to decorate the sides of zones that have been raised or lowered.

**Settings**:

* **`hanging_decoration`**: The name of the block type to place on the side of the raised zone.

### Tag Cards

**Card Library Name**: `tag_cards`

Multiplying a tag card with a buildable card will add the specified tag to the buildable when it’s placed in the world. This tag card can also be multiplied with wall cards to add the tag to the wall and gate buildables when they’re placed.

**Settings**:

* **`tag`**: The name of the tag to add to the buildable tag set when the buildable is placed.

### Entity Clearing Cards

**Card Library Name**: `entity_clearing_cards`

The entity clearing card will remove entities that have the right tags. Only entities inside the village bounding rectangle will be considered.

**Settings**:

* **`entity_include_tag_set`**: Entities with these tags will be removed.

* **`entity_exclude_tag_set`**: Entities with these tags won’t be removed.

```json
//JSON
  {
    "entity_clearing_cards": [
      {
        "cost": 0,
        "tags": ["debugEntityClearing"],
        "entity_include_tag_set": ["animal"],
        "entity_exclude_tag_set": []
      }
    ]
  }
```

```jsx
// B#
  const baseDeck = DECK_Empty();
  DECK_PutOnBottomOf(EntityClearingCard("debugEntityClearing"), baseDeck);
```

## Village Components

Village components contain settings that often affect all the village features and placement preferences for a particular type of village.

### Village Zone

**Component Name**: `badger:village_zone`

The `badger:village_zone` component contains settings that will determine the amount of zones in the village map and the size and shape of each zone.

---

**Settings**:

* **`zone_jitter_min`**: The maximum amount that a village zone will be offset in meters.

* **`zone_jitter_max`**: The maximum amount that a village zone will be offset in meters.

* **`is_zone_jitter_bell_curve_enabled`**: The generation of jitter values between the min and max will have a probability distribution that approximates a normal distribution.

* **`zone_spacing`**: The default distance between village zones before jitter is applied.

* **`expansion_distance`**: The maximum distance that villages can expand from the central starting position.

* **`water_search_resolution`**: The distance between each check for water while scanning the expansion area for bodies of water.

* **`is_hexagonal_grid_enabled`**: If false, the village map will be grid with square zones. If true, the village map will be a hexagonal grid with hexagon zones.

* **`minimum_loz_connection_width`**: The minimum edge width between zones in a layer request before padding is added.

---

### Village Wall

**Component Name**: `badger:village_wall`

The `badger:village_wall` component contains settings for how walls get placed when the wall cards are handled.

**Settings**:

* **`wall_offset`**: How far in meters to move the walls inward and away from the zone edges that they place along by default.

---

### Village Height Change

**Component Name**: `badger:village_height_change`

The `badger:village_height_change` component influences the terrain changes that are applied when the village height change cards are handled.

**Settings**:

* **`clamp_size`**: The number of blocks above a height change that are not changed to air.

---

### Village Bridge

**Component Name**: `badger:village_bridge`

The `badger:village_bridge` component determines how bridges are built when the village path cards are handled.

**Settings**:

* **`diagonal_bridge_degree_tolerance`**: The number of degrees a bridge can deviate from right angles before being considered diagonal.

* **`bridge_horizontal_distance_min`**: The smallest XZ difference allowed between the start and end of the bridge.

* **`bridge_horizontal_distance_max`**: The largest XZ difference allowed between the start and end of the bridge.

* **`bridge_vertical_distance_max`**: The largest Y difference allowed between the start and end of the bridge.

* **`bridge_cost_per_meter`**: Used to compare the cost of the bridge to the cost of a path without a bridge (paths have a fixed cost of 1 per meter). If the cost is low, there’s a greater risk of getting bridges placed in weird places.

* **`bridge_id`**: The entity identifier for the bridge that should be placed.

---

### Village Hanging Decoration Placement [not used, may still work]

**Component Name**: `badger:village_hanging_decoration_placement`

 The `badger:village_hanging_decoration_placement` contains settings for placing hanging decorations between village zones when the hanging decoration cards are handled.

**Settings**:

* **`minimum_edge_width`**: The minimum shared edge width between two zones considered for placement.

* **`minimum_height_delta`**: The minimum height difference between two zone considered for placement.

---

### Building Placement

**Component Name**: `badger:village_building_placement`

The `badger:village_building_placement` contains settings for placing buildables inside of village zones.

**Settings**:

* **`max_placement_attempts_with_jitter`**: The maximum number of times that a placement will try placing with jitter, per village zone.

* **`is_placement_jitter_bell_curve`**: The generation of jitter values between the min and max will have a probability distribution that approximates a normal distribution.

* **`placement_jitter_min`**: The minimum distance that the placement can be offset from a village zone face site. This value will be clamped if it's large enough to push the placement outside of a zone.

* **`placement_jitter_max`**: The maximum distance that the placement can be offset from a village zone face site. This value will be clamped if it's large enough to push the placement outside of a zone.

* **`minimum_distance_between_buildings`**: Buildings cannot be placed closer than this distance.

---

### Zone Scoring

**Component Name**: `badger:village_zone_scoring`

The `badger:village_zone_scoring` component houses settings that will adjust the weights and easing functions of different placement preferences.

**Settings**:

* The easing function for each placement preference will be `linear` by default.
  * As an example, if the `far_from_district_start` placement preference is used, the farthest zone from the district start position will be assigned a score of 1 and the closest zone will be assigned a score of 0. With the easing set to `linear`, the zone halfway between the two will receive a score of 0.5.
  * If instead set the easing function to `ease_in_cubic`, the score assigned will be much lower since the easing is more gradual before quickly ramping up (see [https://easings.net/](https://easings.net/) for options and examples).

* The weight for each placement preference is `1.0` by default.
  * Placement preferences usually each add a score between 0 and 1 to a zone being scored. The weight will be multiplied with that initial score.
  * As an example, if the random placement preference and the close to district start placement preference is used, but we wanted the random placement preference to have less influence on the zones prioritized for placement, we could give the `random_weight` a lower value than the `close_to_weight`.

**Example**:

```json
//JSON
  {
    "badger:village_zone_scoring": {
      // Easing Function
      "close_to_easing": "linear",
      "far_from_easing": "linear",
      "direction_easing": "linear",
      "close_to_wall_easing": "linear",
      "spacing_easing": "linear",
      "elevation_easing": "linear",
      "distance_from_easing": "linear",
      // Weights
      "close_to_weight": 1.0,
      "far_from_weight": 1.0,
      "direction_weight": 1.0,
      "close_to_wall_weight": 1.5,
      "random_weight": 0.7,
      "elevation_weight": 1.0,
      "distance_from_weight": 1.0,
    }
  }

```

---

### Weathering

This component contains all the terrain weathering settings. This component can be given to any of the village archetypes.

**Component Name**: `badger:village_weathering`

**Settings**:

* **`is_overhanging_edge`**: Determines if the weathering effect will taper up or down.

* **`position_noise_scale`**: Lower values will apply smoother xz jitter to weathering effects.

* **`wave_max_depth`**: The maximum distance in blocks, before noise, that the weathering wave can remove blocks from an edge.

* **`wave_min_depth`**: The minimum distance in blocks, before noise, that the weathering wave can remove blocks from an edge.

* **`wave_frequency_scale`**: A smaller scale factor will reduce the frequency of the weathering wave for a smoother effect.

* **`wave_noise_scale`**: The magnitude of noise used to break up the wave function.

* **`gradient_passes`**: The system supports multiple gradient passes, which determine how many blocks to remove and what height.

  * **`gradient_depths`**: A list of how many blocks deep into the terrain should be removed. These are expected to be in descending order. Each of these values corresponds to a value in the `gradient_weight_heights` list.

  * **`gradient_weight_heights`**: A list of heights for each gradient depth to be applied. These values should be in descending order for non-overhanging edges, and ascending order for overhanging edges. 1.0 means the top of the edge and 0.0 means the bottom. In the example, at a height of 0.8 and above, the terrain should be removed at a depth of 3.0. At a height of 0.6 and above, the terrain should be removed at a depth of 2.5.

  * **`gradient_2d_noise_scale`**: The magnitude of vertical noise applied to the gradient pass.  Lower value noise will look more continuous.  Higher will look more random.

  * **`gradient_3d_noise_scale`**: The magnitude of horizontal noise applied to the gradient pass.

  * **`gradient_affected_by_wave`**: Sets whether or not the particular gradient pass should be affected by the sawtooth wave. If they are affected, the gradient is applied only in spots where the sawtooth wave would remove blocks. If they are not, the gradient is applied consistently over the whole edge. Using a combination allows for some interesting weathering effects.

**Example**:

```json
// JSON
  {
    "badger:village_weathering": {
      "is_overhanging_edge": false,
      "position_noise_scale": 0.6,
      "wave_depth_max": 3,
      "wave_depth_min": 0.2,
      "wave_period_scale": 0.4,
      "wave_noise_scale": 0.6,
      "gradient_passes": [
        {
          "gradient_depths": [3.0, 2.5, 2.0, 0.8],
          "gradient_weight_heights": [0.8, 0.6, 0.3, 0.1],
          "gradient_2d_noise_scale": 2.8,
          "gradient_3d_noise_scale": 1.0,
          "gradient_affected_by_wave": true
        },
        {
          "gradient_depths": [1.2, 1.0, 0.5],
          "gradient_weight_heights": [0.7, 0.3, 0.2],
          "gradient_2d_noise_scale": 1.8,
          "gradient_3d_noise_scale": 1.0,
          "gradient_affected_by_wave": false
        }
      ]
    }
  }
```

---

### Path (Building and District)

**Component Name(s)**: `badger:village_building_path` and `badger:village_district_path`

These components contain all the village path settings for building paths and district paths respectively.

**Settings**:

* **`path_blocks`**: A list of block names and their relative weights to be placed for the central portion of the path. Changing the weight influences which block is randomly selected.
* **`edge_block`**: A list of block names and their relative weights to be placed along the outer edges of the path.
* **`edge_decoration_blocks`**: A list of block names and their relative weights to be placed on top of the path edge blocks.
  
* **`inner_blocks`** and **`outer_blocks`**: `path_blocks`, `edge_blocks`, and `edge_decoration_blocks` can all be divided into `inner` and `outer` sections, as shown in the example for `edge_decoration_blocks`. This allows the inner and outer portions of the path and path edge to have different blocks with different weights.
  
  * **`inner_blocks`**: A list of block names and their relative weights for the inner portion of the path or path edge.

  * **`outer_blocks`**: A list of block names and their relative weights for the outer portion of the path or path edge

* **`path_width`** The number of meters across the main portion of the path should be.

* **`edge_width`**: The number of meters across the edge on either side of the main path should be.

* **`path_noise_scale_factor`**: A small scale factor will produce more gradual changes in a building path's weathering.

* **`path_deco_noise_scale_factor`**: A smaller scale factor will cause path edge decorations to alternate less frequently.

* **`path_block_noise_scale_factor`**: A smaller scale factor will cause path blocks to alternate less frequently.

* **`path_edge_block_noise_scale_factor`**: A smaller scale factor will cause path edge blocks to alternate less frequently.

* **`path_noise_amplitude`**: The noise amplitude affects the severity of a building path’s weathering.  This affects both the edge and main path, but is capped by the path width + edge width + edge noise amplitude.

* **`edge_noise_amplitude`**: How far noise can extend past the building path’s edge in meters.  

* **`diagonal_path_degree_tolerance`**: The number of degrees a building path can deviate from right angles before being considered diagonal.

* **`prevent_buildable_placement`**: Setting this to false will allow buildables to place so that they overlap paths.

* **`can_place_under_buildables`**: Setting this to false will force paths to generate around buildables instead of underneath them.

* **`can_resuse_existing_paths`**: Setting this to true will encourage paths to reuse existing paths and bridges. If a path step is reused, it won’t add any additional cost to the path being planned. If this is false, it’s possible for a path to overlap another, but the cost of the path will still be added to the path being planned as normal. If this is false, paths won’t be able to reuse existing bridges.

* **`height_change_needs_bridge`**: If the height change between zones is equal to or larger than this value, a bridge must be placed to connect the two zones.

* **`outer_edge_threshold`**: Sets the proportion of the inner and outer path. A lower setting means a larger outer edge and a higher setting means a larger inner edge.

**Example**:

```json
// JSON
  {
    "badger:village_building_path": {
      "path_blocks": [
        ["block_path_mud_04", 2],
        ["block_mud_var04", 1]
      ],
      "edge_blocks": [
        ["block_path_mud_path", 3],
        ["block_mud_path", 2],
        ["grass", 1]
      ],
      "edge_decoration_blocks": [
        {
          "inner_blocks": [
            ["block_deco_fol_moss_01", 2],
            ["block_deco_zombie_mushroom_small", 1],
            ["block_deco_fol_wofgrass_01", 2],
            ["block_deco_fol_wofgrass_02", 2],
            ["block_deco_fol_wofgrass_03", 2],
            ["air", 6]
          ],
          "outer_blocks": [
            ["block_deco_fol_moss_01", 2],
            ["block_deco_zombie_mushroom_small", 1],
            ["block_deco_fol_wofgrass_01", 2],
            ["block_deco_fol_wofgrass_02", 2],
            ["block_deco_fol_wofgrass_03", 2],
            ["block_deco_grass_clump_02", 1],
            ["block_deco_grass_clump_03", 1],
            ["air", 6]
          ]
        }
      ],
      "path_width": 2.0,
      "edge_width": 2.0,
      "path_noise_scale_factor": 3.0,
      "path_deco_noise_scale_factor": 3.0,
      "path_block_noise_scale_factor": 3.0,
      "path_edge_block_noise_scale_factor": 5.0,
      "diagonal_path_degree_tolerance": 45,
      "prevent_buildable_placement": false,
      "path_noise_amplitude": 1.0,
      "edge_noise_amplitude": 1.0,
      "can_place_bridge_for_height_changes": false,
      "height_change_needs_bridge": 8.0,
      "outer_edge_threshold": 0.5
    }
  }
```

---

## Village Service Parsed Data

**File Location**: `data/behavior_packs/badger/gamelayer/server/village/villages.json`

The village service parsed data contains global settings that often affect all villages.

### Map Settings

* **`terrain_sampling_fill_step_distance`**: When we sample the terrain data before planning the village, I don’t think we can sample the entire area at once (it used to cause a crash at least) so we do it in pieces that have a width and height equal to this step distance.

* **`block_query_step_distance`**: This distance defines the resolution that we query the terrain for water and then markup zones with water inside them. Water detection should become more precise if this is a smaller value, but it’s also more expensive.

### Building Settings

* **`approximate_block_budget_per_tick`**: [not used], _Miclee note_: may be worth looking into.

* **`build_from_path_max_range_in_blocks**`**: When the `build_from_district_path` placement preference is used, zones won’t be considered if their distance to the starting path zone exceeds this max distance.

### Navigation Map Settings

The village navigation system hasn’t been used in a long time.

* **`crossable_edge_minimum_length`**: [not used]

* **`path_padding_width`**: [not used]

* **`path_destination_arrival_distance**`**: [not used]

### Influence Map Settings

* **`planning_influence_map_settings`**: These settings affect the `close_to_buildable`, `far_from_buildable`, `close_to_water`, `far_from_water`, `facing_buildable`, and `facing_water` placement preferences.

* **`alliance_influence_map_settings`**: These settings affect the `close_to_influence`, `far_from_influence`, `facing_influence`, and `in_direction_of_influence` placement preferences.

* **`decay`**: How much influence is lost as it spreads.

* **`resolution`**: The dimensions of the grid that influence will spread through.

* **`initial_influence`**: The initial amount of influence before it decays.

### Placement Preference Settings

* **`buildable_tag_for_spacing`**: By default, every buildable request will be given a `far_from_buildable` placement preference with this tag to help ensure buildables space themselves out; However, if the buildable request has the `disable_spacing` placement preference, this tag won’t be used.

### Heart Destruction Settings

* **`world_request_type`**: When a village has been destroyed (e.g. portal destroyed), this world request type is used to load the world. It will determine the priority of the world request.

* **`tag_for_village_heart_destruction_opt_out`**: [not used]

### Wall Settings

* **`max_number_of_gate_placement_attempts`**: If a gate collides with another structure (usually a bridge/staircase) we’ll move the gate position 1 meter in the direction of the path entering the wall and then the gate will try to place again. This is the max number of attempts before the gate will fail to place.

* **`minimum_segment_length`**: When a structure is being embedded in a wall, we’ll extend the wall if the original wall isn’t long enough. We’ll skip walls that are less than this length when we’re creating the extended wall.

* **`remove_wall_on_placement_failure`**: Specifies if a gap in the village walls should be left when we fail to embed a structure in the walls.

### Moat Settings

* **`noise_levels`**: This controls how many levels of detail the moat noise is going to have. At each level, the frequency of noise is doubled and the amplitude is halved. This setting will affect all villages that have the new `badger:village_moat` component. It’s probably not worth having too many levels because the finer details are going to be lost when converting to block space. Also something to note is that because the end result is normalized to a value between 0 and 1, more levels of noise will have kind of a smoothing effect, mellowing out some of the interesting outliers when all the levels are averaged.

### Performance Settings

* **`building_time_budget_in_ms`**: The village building system will handle pending placements until this budget is reached or passed.

* **`planning_time_budget_in_ms`**: The village planning system will handle village generation requests until this budget is reached or passed.

## Entity Removal Service Parsed Data

Specifies what entities should be removed inside the village bounding area and inside the world reload areas.

File Location: `data/behavior_packs/badger/services/entity_removal.json`

**Settings**:

* **`include_any`**: Entities with these tags should be removed.

* **`exclude_any`**: Entities with these tags won’t be removed.

**Examples**:

* Before (floating chests)

![Before (floating chests)](images/village_generation/image65.png)

* After (no floating chests)

![After (no floating chests)](images/village_generation/image66.png)
