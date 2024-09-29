# Biomes in Minecraft Legends

Biomes describe how a local patch of the world should look and behave. By writing custom biome data you could:

1) Change the general shape of terrain for a biome.
2) Change the ratio of frequency of biome types.
3) Change the blocks that make up the biome, both at the surface and below.
4) Change the distribution of decorative features (e.g. trees, grass, etc.) for a biome.
5) Change the settings for a biome's sub-biomes.
6) Set UI-related information(e.g. `biome_banner`, `biome_display_name`) for a biome

## JSON format

All biomes should specify the version that they target via the "format_version" field. The remainder of the biome data is divided up into independent JSON sub-objects, or components. In general you can think of the presence of a component as defining what game behaviors a biome participates in with the component fields defining how it participates. Broadly there are two categories of components:

1) Namespaced components (i.e. those with a 'name:' prefix) map to specific behaviors in-game; they may have member fields that parameterize that behavior; only names that have a valid mapping are supported.
2) Components with no namespace are treated as 'tags': any name consisting of alphanumeric characters, '.' and '_' is permitted; the tag is attached to the biome so that either code or data may check for its existence; tag components may not have member fields.

See the full biome schema below for additional details and the full list of namespaced components.

**Here is a sample biome**:

```json
{
   "format_version" : "1.13.0",
   "minecraft:biome" : {
      "components" : {
         "badger:overworld_height" : {
            "height_params" : {
               "average_height" : 21,
               "texture_heights": [15, 24],
               "clamp_heights": [17, 22],
               "clamp_scales": [1.0, 0.5]
            },
            "noise_type" : "beach"
         },
         "badger:rivers" : {
            "river_biome" : "beach_rivers"
         },
         "badger:sub_biomes" : {
            "borders" : [
               {
                  "allowlist" : [ "mud_pit", "mud_pit_border" ],
                  "biome" : "forest_clamp_lower",
                  "width" : 8
               },
               {
                  "allowlist" : [ "grasslands_mud_pit", "grasslands_mud_pit_border" ],
                  "biome" : "grasslands_clamp_lower",
                  "width" : 8
               }
            ]
         },
         "badger:visual_biome_attributes" : {
            "grass" : {
               "normal" : {
                  "grass_noise_intensity" : 0.50,
                  "grass_noise_scale" : 0.50,
                  "grass_surface_color" : "#BBD669"
               },
               "overlay" : {
                  "grass_overlay_noise_bias" : 0.0,
                  "grass_overlay_noise_intensity" : 0.0,
                  "grass_overlay_noise_scale" : 0.0,
                  "grass_overlay_surface_color" : "#000000"
               }
            },
            "map" : {
               "biome_banner" : "",
               "biome_display_name" : "beach",
               "common_resource_icon" : "",
               "map_color" : "#916d2f",
               "map_color_odd" : "#916d2f",
               "map_pattern_index" : 11
            },
            "particulates" : {
               "nether_spores" : 0.0,
               "sparkles" : 0.0
            },
            "rain" : {
               "downfall" : 10.0,
               "lower_limit" : 0.4000000059604645,
               "rain_to_snow_height" : 120,
               "rain_to_snow_transition_size" : 5,
               "snow_to_rain_density_ratio" : 0.75,
               "upper_limit" : 1.0
            },
            "water" : {
               "water_blur_distance" : 16.0,
               "water_color_blend_distance" : 6.0,
               "water_depths_color" : "#008D98",
               "water_surface_color" : "#00CBCB",
               "water_surface_transparency" : 1.0
            }
         },
         "beach" : {},
         "minecraft:surface_parameters" : {
            "default_surface" : {
               "mid_material" : "badger:block_sand",
               "top_material" : "badger:block_sand"
            },
            "foundation_material" : "badger:block_sand",
            "sea_floor_depth" : 7,
            "sea_floor_material" : "badger:block_sand",
            "sea_material" : "badger:block_water"
         },
         "monster" : {},
         "warm" : {}
      },
      "description" : {
         "identifier" : "beach"
      }
   }
}
```

## Adding biomes

Biomes are read from JSON files in the biomes subfolders of behavior packs. Loading enforces one biome per file; the file name and the actual biome name must match. Adding a file with a new name to the biome data location will make it available for the game to use, while existing biomes can be overridden via files that match their existing name. Note that if you add a new biome you'll need to write component data that allows it to participate in world generation (see full schema below), or else it won't show up in your worlds!

## Schema

****

```json
  {
      object "badger:icewave" : opt // Controls the frostlands snow waves and material transition parameters.
      object "badger:jungle_egg" : opt // Controls the jungle egg rock and material transition parameters.
      object "badger:rivers"[0,6] : opt // Controls for river density and replacement biome.
      {
          array "cell_range"[2] : opt // The range of possible IDs this biome's voronoi cells may take. A wider range means more opportunities for rivers to form in this biome, and a narrow range means fewer opportunities. Rivers will only form across the borders of cells with different IDs.
          {
              int "[0..0]"<0-255>
              int "[1..1]"<0-255>
          }
          float "point_chance"<0.000000-1.000000> : opt // Probability that a river cell spawns in a particular region. Smaller values(e.g: closer to 0.0) lead to few large cells (which means few opportunities for rivers to form), while higher values(e.g: closer to 1.0) lead to many small cells (which means many opportunities for rivers to form).
          biome_reference "river_biome" : opt // The identifier of the biome that this biome's rivers transform into.
          int "river_width"<-*-64> : opt // Average river width for this biome in blocks.
          int "river_border_width"<-*-64> : opt // Average border biome width for this biome's rivers.
          biome_reference "river_border_biome" : opt // The border biome for this biome's rivers.
      }
      object "badger:custom_water_level"[0,1] : opt // Control the height of water, the material used for bottom of water bodies, and the depth of the floor material.
      {
          int "water_level" : opt // Controls the water level in this biome.
      }
      object "decorate_exclusive"[0,1] : opt // Determines which biomes may co-decorate chunks with this biome.
      {
          array "allowed_biome_tags" : opt
          {
              string "<any array element>"
          }
      }
      object "badger:fatefullands_surface"[0,6] : opt // Control the fateful lands hill shape and size.
      {
          int "hill_top_height" // The number of blocks above sea level that the hill peak should protrude
          float "hill_width_scale" // The width factor for the hill. Larger values will result in a wider hill.
          float "hill_noise_amplitude" // A scalar for the terrain noise amplitude. Larger values will result in noisier terrain.
          float "hill_noise_frequency" // The frequency for our terrain noise overlay. Larger values will result in horizontally 'rougher' noise.
          float "river_noise_scale" // The amount of relative variation in the shape of the rivers. Zero means the rivers will be perfectly circular.
          array "rivers" : opt // Settings that describe individual concentric rivers around the hill
          {
              array "<any array element>"[3] : opt
              {
                  float "[0..0]" // River radius in blocks
                  float "[1..1]" // River relative width; larger means wider
                  float "[2..2]" // Maximum river depth in blocks
              }
          }
      }
      object "badger:overworld_height"[0,6] : opt // Noise parameters used to drive terrain height in the Overworld.
      {
          array "noise_params"[2] : opt
          {
              float "[0..0]"
              float "[1..1]"
          }
          string "noise_type"<"stone_beach", "deep_ocean", "default", "default_mutated", "lowlands", "river", "ocean", "taiga", "mountains", "highlands", "mushroom", "less_extreme", "extreme", "beach", "swamp"> : opt
          object "height_params" : opt // Settings for height clamping and texture height ranges
          {
              array "clamp_heights"[2] : opt // The upper and lower heights for clamping
              {
                  int "[0..0]"<0-255>
                  int "[1..1]"<0-255>
              }
              array "clamp_scales"[2] : opt // The degree to which 'natural' heights in this biome are allowed to go beyond the given bound.     0.0 means full clamping (i.e. the height cannot go at all beyond the bound), 1.0 means no clamping,     and >1.0 means the natural height will be amplified beyond the lower bound.
              {
                  float "[0..0]"<0.000000-2.000000>
                  float "[1..1]"<0.000000-2.000000>
              }
              array "texture_heights"[2] : opt // The upper and lower heights for textures that use biome-relative heights
              {
                  int "[0..0]"<0-255>
                  int "[1..1]"<0-255>
              }
              array "geology_heights"[2] : opt // The upper and lower heights for geology textures that appear within this biome
              {
                  int "[0..0]"<0-255>
                  int "[1..1]"<0-255>
              }
              int "average_height"<0-255> // The 'anchor' (midpoint) height used by textures in HEIGHT_ELEVATION_OFFSETS mode that sample this biome
          }
          float "frequency_scale"<1.000000-5.000000> : opt // The scale by which the terrain noise for this biome is compressed. Rounds to 1.0, 1.4, or 5.0.
          bool "mark_as_wet_when_stamped" : opt
          bool "mark_as_wet_in_geology_layer" : opt
      }
      object "badger:sub_biomes"[0,2] : opt // Controls which sub-biomes will will be used in this biome
      {
          array "borders"[1,*] : opt // Settings that determine this biome's border biome transformations and widths
          {
              object "<any array element>"
              {
                  biome_reference "biome" // Which biome this biome should transform into
                  int "width" // The width in blocks for this border
                  bool "fixed_width" : opt // Increases the accuracy/consistency of the border width at the expense of performance
                  array "allowlist" : opt // A list of biomes which, when situated adjacent to this biome, will initiate a transformation to the given border biome
                  {
                      biome_reference "<any array element>"
                  }
                  array "denylist" : opt // A list of biomes which, when situated adjacent to this biome, will prevent a transformation to the given border biome.    If any biomes are specified, the border transformation will occur at the interface of ALL other biomes.
                  {
                      biome_reference "<any array element>"
                  }
              }
          }
          array "scatter_biomes"[1,*] : opt // Settings that determine this biome's sub-biome transformations
          {
              object "<any array element>"
              {
                  biome_reference "biome" : opt // The biome this biome will transform into
                  float "scaleX" : opt // The x-axis noise scaling factor. Smaller values will be 'smoother' and larger values will be 'rougher'.
                  float "scaleY" : opt // The y-axis noise scaling factor. Smaller values will be 'smoother' and larger values will be 'rougher'.
                  float "offSetZ" : opt // The z-axis value at which we will sample the 3D noise map. This effectively gives us access to an infinite number of unique 2D noise maps to sample.
                  float "threshold" : opt // Noise values ([0, 1]) greater than this value will initiate a transformation. Lower values increase the prevalence of the given sub-biome.
              }
          }
      }
      object "minecraft:surface_parameters"[0,14] : opt // Control the blocks used for the default Minecraft Overworld terrain generation.
      {
           "sea_floor_material" // Controls the block type used as a floor for bodies of water in this biome.
           "foundation_material" // Controls the block type used deep underground in this biome.
           "sea_material" // Controls the block type used for the bodies of water in this biome.
           "sea_extra_material" : opt // Controls the block type used for the sea's extra block layer in this biome.
           "air_material" : opt // Controls the block type used for air in this biome.
          int "sea_floor_depth" // Controls how deep the ocean floor layer should be.
          int "wetness_height_min" : opt // We can mark an entire height range (other than 'below sea level') in this biome as 'wet' for the wetness query. This is the minimum height in blocks above sea level.
          int "wetness_height_max" : opt // Maximum height in blocks above sea level
          bool "use_lower_wetness_bits" : opt // Whether to use the lower 4 bits of the wetness value -- currently reserved for mud, since mud and water can show up in close proximity to one another.
          bool "override_subchunk_optimization" : opt // If true, this will ensure subchunks in any chunks where this biome occurs will be built all the way up to max level height.
          object "ordered_surface" : opt // A surface type with many layers
          {
              array "stacks"[1,*] // A list of unique block layerings (stacks) corresponding to particular height ranges
              {
                  object "<any array element>" : opt
                  {
                      array "height_range"[2] // The height range (in blocks above sea level) that should use this surface stack
                      {
                          int "[0..0]"
                          int "[1..1]"
                      }
                      bool "repeat_block_sequence" : opt // Should this block sequence repeat over and over? (Or should we switch to the foundation block once we've iterated the stack once?)      Not compatible with 'bottom-up' order -- bottom-up always repeats the last block in the stack.
                      bool "top_down" : opt // Does this stack start from the top down? (Default is 'bottom up')
                      array "layers"[1,*] // A list of weighted blocks and associated depths ('layers')
                      {
                          object "<any array element>"
                          {
                              array "depth_range"[1,2] : opt // The depth for this layer. If two values are provided, a uniform random value in the range will be selected at each block column in the biome.
                              {
                                  int "[0..0]"
                                  int "[1..1]" : opt
                              }
                              array "blocks_weighted"[1,*]
                              {
                                  array "<any array element>"[1,2] // A list of weighted blocks to choose from
                                  {
                                       "[0..0]" // Block name
                                      int "[1..1]" : opt // Relative weight (defaults to 1)
                                  }
                              }
                          }
                      }
                  }
              }
          }
          object "default_surface" : opt // A surface type with only top, middle, and foundation layers
          {
               "mid_material" // Controls the block type used in a layer below the surface of this biome.
               "top_material" : opt // Controls the block type used on the surface of this biome.
              array "weighted_top_materials" : opt // A list of weighted blocks to choose from
              {
                  array "<any array element>"[1,2] : opt // A list of weighted blocks to choose from
                  {
                       "[0..0]" // Block name
                      int "[1..1]" : opt // Relative weight (defaults to 1)
                  }
              }
          }
          array "above_top"[1,*] : opt // A list of block options associated with one or more placement conditions
          {
              object "<any array element>"
              {
                  array "weighted_blocks"[1,*]
                  {
                      array "<any array element>"[1,2] // A list of weighted blocks to choose from if the given conditions are met
                      {
                           "[0..0]" // Block name
                          int "[1..1]" : opt // Relative weight (defaults to 1)
                      }
                  }
                  object "conditions" // A set of conditions that, if evaluating to true at a given surface coordinate, will enable the placement of the associated block(s)
                  {
                      object "noise" : opt // Conditions pertaining to a 2D noise map
                      {
                          float "scale" : opt // Horizontal scaling factor for noise map queries. 40.0 is a good starting value; larger values will result in smoother noise.
                          object "range" : opt // Acceptable noise range for block placement to succeed
                          {
                              array "min"[1,2] // Minimum 'success' noise value. If two floats are provided,      a uniform random float in that range will be selected as the minimum value at each block column in the biome.
                              {
                                  float "[0..0]"
                                  float "[1..1]" : opt
                              }
                              array "max"[1,2] // Maximum 'success' noise value. If two floats are provided,      a uniform random float in that range will be selected as the maximum value at each block column in the biome.
                              {
                                  float "[0..0]"
                                  float "[1..1]" : opt
                              }
                          }
                          array "bands"[1,*] : opt // A list of noise 'bands' in the form [min, max] for which the given block option should succeed
                          {
                              array "<any array element>"[2]
                              {
                                  float "[0..0]" // Minimum noise band value
                                  float "[1..1]" // Maximum noise band value
                              }
                          }
                          bool "abs" : opt // If 'true', the absolute value of the noise query will be used instead.      This is useful for 'mirrored' bands, e.g. [[-0.7, -0.6], [0.6, 0.7]] -- you can just provide the positive band      and the 'abs' flag to achieve the same result.
                          array "wiggle"[2] : opt // A uniform random value to add to the noise query result
                          {
                              float "[0..0]"
                              float "[1..1]"
                          }
                      }
                      object "wetness" : opt // [DEPRECATED] A wetness (distance to nearest water, or 'wet' terrain) range
                      {
                          array "min"[1,2] // Minimum distance. If two values are provided, the minimum at each      block column will be uniform randomly selected from the range.
                          {
                              int "[0..0]"
                              int "[1..1]" : opt
                          }
                          array "max"[1,2] // Minimum distance. If two values are provided, the minimum at each      block column will be uniform randomly selected from the range.
                          {
                              int "[0..0]"
                              int "[1..1]" : opt
                          }
                      }
                      array "height"[2] : opt // A height range in which the given block should successfully place
                      {
                          int "[0..0]" // Minimum height
                          int "[1..1]" // Minimum height
                      }
                      array "random"[2] : opt // A range within which a uniform random number [0, 1) should indicate a successful placement
                      {
                          float "[0..0]" // Minimum value
                          float "[1..1]" // Maximum value
                      }
                  }
              }
          }
          array "conditional_top"[1,*] : opt // A list of block options associated with one or more placement conditions
          {
              object "<any array element>"
              {
                  array "weighted_blocks"[1,*]
                  {
                      array "<any array element>"[1,2] // A list of weighted blocks to choose from if the given conditions are met
                      {
                           "[0..0]" // Block name
                          int "[1..1]" : opt // Relative weight (defaults to 1)
                      }
                  }
                  object "conditions" // A set of conditions that, if evaluating to true at a given surface coordinate, will enable the placement of the associated block(s)
                  {
                      object "noise" : opt // Conditions pertaining to a 2D noise map
                      {
                          float "scale" : opt // Horizontal scaling factor for noise map queries. 40.0 is a good starting value; larger values will result in smoother noise.
                          object "range" : opt // Acceptable noise range for block placement to succeed
                          {
                              array "min"[1,2] // Minimum 'success' noise value. If two floats are provided,      a uniform random float in that range will be selected as the minimum value at each block column in the biome.
                              {
                                  float "[0..0]"
                                  float "[1..1]" : opt
                              }
                              array "max"[1,2] // Maximum 'success' noise value. If two floats are provided,      a uniform random float in that range will be selected as the maximum value at each block column in the biome.
                              {
                                  float "[0..0]"
                                  float "[1..1]" : opt
                              }
                          }
                          array "bands"[1,*] : opt // A list of noise 'bands' in the form [min, max] for which the given block option should succeed
                          {
                              array "<any array element>"[2]
                              {
                                  float "[0..0]" // Minimum noise band value
                                  float "[1..1]" // Maximum noise band value
                              }
                          }
                          bool "abs" : opt // If 'true', the absolute value of the noise query will be used instead.      This is useful for 'mirrored' bands, e.g. [[-0.7, -0.6], [0.6, 0.7]] -- you can just provide the positive band      and the 'abs' flag to achieve the same result.
                          array "wiggle"[2] : opt // A uniform random value to add to the noise query result
                          {
                              float "[0..0]"
                              float "[1..1]"
                          }
                      }
                      object "wetness" : opt // [DEPRECATED] A wetness (distance to nearest water, or 'wet' terrain) range
                      {
                          array "min"[1,2] // Minimum distance. If two values are provided, the minimum at each      block column will be uniform randomly selected from the range.
                          {
                              int "[0..0]"
                              int "[1..1]" : opt
                          }
                          array "max"[1,2] // Minimum distance. If two values are provided, the minimum at each      block column will be uniform randomly selected from the range.
                          {
                              int "[0..0]"
                              int "[1..1]" : opt
                          }
                      }
                      array "height"[2] : opt // A height range in which the given block should successfully place
                      {
                          int "[0..0]" // Minimum height
                          int "[1..1]" // Minimum height
                      }
                      array "random"[2] : opt // A range within which a uniform random number [0, 1) should indicate a successful placement
                      {
                          float "[0..0]" // Minimum value
                          float "[1..1]" // Maximum value
                      }
                  }
              }
          }
      }
      object "badger:biome_tags"[0,1] : opt // Assign an arbitrary collection of tags to this biome.
      {
          array "tags" : opt // A list of tags this biome should have
          {
              string "<any array element>" : opt
          }
      }
      object "minecraft:overworld_generation_rules"[0,1] : opt // Control how this biome is instantiated (and then potentially modified) during world generation of the overworld.
      {
          biome_reference "shore_transformation" : opt
          array "shore_transformation"[1,*] : opt
          {
              biome_reference "<any array element>" : opt
              array "<any array element>"[2] : opt
              {
                  biome_reference "[0..0]"
                  int "[1..1]"
              }
          }
      }
      object "badger:visual_biome_attributes"[0,6] : opt // Client Side information for Map color, Grass color/noise, Water color/transparency, and Sky parameters
      {
          object "map" : opt // World map settings for this biome
          {
              string "map_color" : opt // [LEGACY] RGB color for this biome on the map
              string "map_color_odd" : opt // [LEGACY] RGB color for this biome on the map for odd tiles
              int "map_pattern_index"<0-*> : opt // Index for this biome's world map pattern from the texture atlas
              string "biome_display_name" // The player-facing name for this biome in the map tooltip
              string "biome_banner" : opt // File location for the PNG this biome should use as its tooltip banner image
              string "common_resource_icon" : opt // File location for the PNG this biome should use as its primary resource icon
          }
          object "water" : opt // Water rendering settings for this biome
          {
              string "water_surface_color" : opt // RGB value defining the surface color
              float "water_surface_transparency"<0.000000-1.000000> : opt // Surface transparency value [0.0, 1.0]
              string "water_depths_color" : opt // RGB value defining the color of the water when it's deep
              float "water_blur_distance"<0.000000-16.000000> : opt // Distance under water after which the ground becomes maximally blurred
              float "water_color_blend_distance"<0.000000-16.000000> : opt // Distance at which the water color fully transitions from 'water_surface_color' to 'water_depths_color'; any distance in between is a mix of the two colors
          }
          object "grass" : opt // Grass rendering settings for this biome
          {
              object "normal" : opt
              {
                  string "grass_surface_color" : opt // Grass color
                  float "grass_noise_intensity"<0.000000-1.000000> : opt // Amount of noise to add to the default color
                  float "grass_noise_scale"<0.000000-5.000000> : opt // Noise scaling factor
              }
              object "overlay" : opt
              {
                  string "grass_overlay_surface_color" : opt // The secondary grass color that can be blended in
                  float "grass_overlay_noise_intensity"<0.000000-*> : opt // Noise strength; higher values means the noise is more broken up, low it's smoother
                  float "grass_overlay_noise_scale"<0.000000-5.000000> : opt // Horizontal scaling factor for the overlay color noise
                  float "grass_overlay_noise_bias"<0.000000-1.000000> : opt // The blending bias between the 'normal' grass color and the overlap grass color
              }
          }
          object "world" : opt // Sky and fog rendering settings for this biome
          {
              float "fog_near" : opt
              float "fog_far" : opt
              float "fog_rain_near" : opt
              float "fog_rain_far" : opt
              string "color_LUT" : opt
              float "sky_weight"<0.000000-*> : opt // Relative weight for this biome's contribution to sky rendering settings
          }
          object "rain" : opt // Precipitation settings for this biome
          {
              float "lower_limit"<-1.000000-1.000000> : opt // The lower bound for when rain should stop.     This is sampled from a noise value in [-1, 1]; any time the value is below lower_limit, the rain stops.
              float "upper_limit"<-1.000000-1.000000> : opt // The upper bound for when rain should be at maximum intensity.     This is sampled from a noise value in [-1, 1]; any time the value is above upper_limit, the rain is set to the maximum level.
              int "rain_to_snow_height"<0-*> : opt // The world height at which rain transitions to snow
              float "downfall"<0.000000-60.000000> : opt // How much precipitation should be created at maximum levels
              float "snow_to_rain_density_ratio"<0.000000-2.000000> : opt // The ratio of snow to rain, used to dampen snow in heavy rain locations (or boost if desired)
              int "rain_to_snow_transition_size"<0-255> : opt // The vertical size in blocks, centered at the rain_to_snow_height, to blend between snow and rain
          }
          object "particulates" : opt // Ambient particle settings for this biome
          {
              float "nether_spores"<0.000000-1.000000> : opt // Intensity of nether spores
              float "sparkles"<0.000000-1.000000> : opt // Intensity of sparkles
          }
      }
      object "[a-z0-9_.:]+" : opt // Attach arbitrary string tags to this biome
  }

```
