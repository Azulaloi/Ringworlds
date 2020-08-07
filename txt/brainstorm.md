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
 -- they also have 'airless' which toggles the orbital horizon clouds, BUT ALSO forces no atmosphere stuff to render while on the planet, so it's always black sky with stars visible. kill me 
 
 # | Parallax
 - only necessary if your biomes want a new parallax
 - define parallax in .parallax file
 -- .parallax defines multiple layers of images, like landscapes, trees, and clouds. biomes reference a .parallax file from absolute path
 --- Parallax Layer Parameters
 ---- kind (this is the directory in /parallax/images)
 ---- baseCount (this may be the number of images in /type/base, and seems to default to 1 because it's defined as 1, only as 2 or more for kinds that have multiple bases)
 ---- offset ([f, f] no idea if this is a range or position), 
 ---- parallax (this defines the z layer (higher is further back), and movement ratio, where 1 is no parallax, cave backgrounds are 1.08, and the most distant clouds are 9. presumably, below 1 would move faster that the foreground, such that it appears closer, and 0 seems to just not display at all, neither did 999?)
 ---- timeOfDayCorrelation (values I found are: dayVisible, nightVisible, dayCloudVisible, nightCloudVisible) 
 ---- unlit (I think this like the inverse of fullbright, because night clouds are false and day clouds are true)
 ---- fadePercent, directives, lightMapped, nohueshift, repeatY,  tileLimitTop, minSpeed, maxSpeed
 
 - for any new layer kinds, add their directory and images to /parallax/images
 -- the layer 'kind' looks for a directory of that type, then goes into /type/base/ and grabs x.png where x is 1..? 
 -- but there are some kinds of modifiers? like in forest, forestcanopy is defined by foliage/forestcanopy and forestsmalltree is stem/forestsmalltree, which do exist, but just have a single base image, and not any stem versions? and there is no stem directory. I would assume that stem and foliage are determined by the biome, because in /plants/trees/_TYPE_/ there are /stem/ and /foliage/ which have images for the parts of the tree, but how are those related to parallax? are the /images/forestcanopy/ etc folders just defaults? and the stem and foliage parallax types are special and construct themselves in some hardcoded way from the foliage and stem types of the biome?
 --- wait no in /plants/trees/forest/foliage/ there are multiple types, then in the type there are branch images, a .modularfoliage file, and a parallax folder!!! and in parallax there are subdirs forestcanopy, treeback, and treefront, which are each normal parallax image folders. so stem and foliage are keywords for /plants/trees/_BIOME_/foliage|stem/_TYPE_/parallax and _BIOME_ is ignored. fuck how confusing
 --- UPDATE, it does NOT ignore _BIOME_, so if I want my own forest biome I will need to duplicate all of the parallax for each tree type... except treestems and foliages appear to be defined in the biome under placeables, of type tree, where it has a list for each... and those trees are working in the rw-forest biome, so I guess it doesn't matter for what biome the trees are defined, it only matters what biome the trees parallax is defined for... sounds like they just organized the trees by category, then later when building the tree parallax system accidentally hardcoded those categories. hey at least there are defaults 
 -- then it goes to /type/base/mod1/ and grabs an image, then to mod2, which I assume are places on top of each other? but the base images are like x800 and the mods are x1024, and they don't line up 
 -- plus no idea how the mod type would be saved to the world? it might determine mod types per primary regions main biomes parallaxes mods but the parallax doesnt even define its mods anywhere? so what would happen if the mod images changed, the parallax layers changed, the biomes parallax changed? 
 
 -- ALSO parallaxes are saved to the world, so if you change the parallax, you will need to delete the world file
 
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


# PROBLEMS

- horizon clouds, their pos/movement is wrong for a ringworld from orbit. using '"airless" : true' in the biome disables these, but also changes the atmospheric rendering from the surface, making it basically always look like night and disabling sunrise/set effects etc.
- world wrapping, since ringworlds have walls you shouldn't be able to wrap. this seems to be hardcoded even for dungeons and shipworlds. I'm planning to solve it by somehow using world.placeDungeon at the world edges and having a giant wall as a dungeon thick enough that you can't see the wrap (will have to be generous for wide monitors)
- parallax modifiers based on the subregions/subbiomes of those subregions, since you can see tons of background terrain, it would be neat if it had different modifiers to match the subbiomes that appear in the world
- when the parallax is really high, it seems to offset its default location, so like with parallax 20 and offset 0,0, a 600x800 image was up and to the right a bunch... so if I want a functionally static parallax of like 999 or whatever, I'll need to offset the image a LOT, but I'm not sure how to calculate that offset
- since atmosphere/space is a biome, going up enough will fade out the background ring... I could solve this maybe by adding the same thing to a az-rw_space biome, but then I'd need an atmosphere and space biome for every ring primary biome that has a different ring background...

most of the problems seem solvable, though a huge pita, except for horizon clouds...