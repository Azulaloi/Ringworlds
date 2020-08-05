# Brainstorming for Ringworlds

Different "Flavours"
- vanilla equivalents
-- standard planets like forest/jungle, savannah/desert, jungle, ocean
-- extreme planets like molten/volcanic/magma, snowy/tundra/arctic
-- spicy planets like midnight, alien
- different kinds of rings
-- climates
-- theta/gamma etc with different styles? as if created by different species?
--


Lifeforms
- Automatons like sentinels

Dungeons
- Forerunner esque structures
-- something that is a clear homage but with a new take, like the iconic forerunner shooty uppy things
-- Analogues to structures like library, control room, etc
-- underground mechanisms
- Villages?
- Ruins like on Delta Halo
- Research stations
 

--


# NOTES FOR SELF ON PLANET TYPES

How defining a planet type works

#1 | Celestial Config:
 - add planet type to graphics tables (horizon, baseimage, dynamics)
 - add planet type to starsys tables (planets, satellites, per tier)
 
#2 | Terrestrial Worlds Config
 - define planet type in planetTypes
 -- define layer properties (primary and secondary regions)
 
 - define region types in regionTypes
 -- regions have biomes and subregions (not to be confused with a layer's secondary regions)
 -- subregions are just regions: no idea what happens if a subregion has its own subregions, maybe
    they are ignored when a region is itself a subregion? esp to prevent recursion. all the vanilla subregions
	have no subregions of their own. maybe testing is needed

#3 | Biomes
 - add .biome file to biomes (no idea if the subdirs are important, best to be safe and place them appropriately)
 -- biomes define: entity spawns, weather, skytime colors, blocks/ores, parallax, ambient noise/music tracks, 
    and objects for surface/underground (grasses, bushes, trees, crops, vines/stala__ites/spikes, capsules/pots/chests, and microdungeons) 
 
 - define parallax in .parallax file
 -- a .plx defines multiple layers of images, like landscapes, trees, and clouds. 
 --- (!!!!) what I don't yet understand is that each layer, rather than an image, has a "kind", but I don't know where layer kinds are defined
 --- each image has a parallax value that defines their movement ratio, where 1 is no parallax, cave backgrounds are 1.08, and the most distant clouds are 9. presumably, below 1 would move faster that the foreground, such that it appears closer, and 0 would be infinite (so don't do that).
 --- they also have parameters like offset, time correlation, light settings, tile/repeating, directives, fade, min/max speed, count of
 
# MORE DETAILS
 
- planetType properties
-- these values are per size {verysmall, small, medium, large}
-- dayLengthRange, gravityRange, threatRange, layers 

- planet layers (table below is "layername" : "defaultprimaryregion")
-- {space : asteroids, atmosphere : atmosphere, surface : barren, subsurface : subsurface
-- underground1 : shallowunderground, underground2 : midunderground, underground3 : deepunderground, core : core}

- layer properties:
-- enabled, layerlevel, baseheight(center)
-- primaryregion, 
-- secondaryregions, secondaryregionrange, secondaryregionsizerange
-- dungeons, dungeoncountrange, dungeonxvariance

- region properties:
-- fgCaveSelector, bgCaveSelector, fgOreSelector, bgOreSelector, subBlockSelector
-- oceanLiquid, oceanLevelOffset, encloseLiquids, fillMicrodungeons, caveLiquid, caveLiquidSeedDensityRange
-- subRegion