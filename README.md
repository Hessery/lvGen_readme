# lvGen
 Room based level generation with bitmap auto tiling, room type forcing, weighted room alternatives and level fitness checking made for and in Gamemaker. Uses .yy (standard Gamemaker room files).

---

## Contents

[Todo](README.md#todo)  
[Adding a room](README.md#adding-a-room)  
[Adding a new level prop/actor](README.md#adding-a-new-level-propactor)  
[Adding a prefix](README.md#adding-a-prefix)  
[Adding a forced room](README.md#adding-a-forced-room)  
[Adding a level requirement](README.md#adding-a-level-requirement)  
[Adding a weighted room alternative](README.md#adding-a-weighted-room-alternative)  
[Change tile size](README.md#change-tile-size)  
[Troubleshooting](README.md#troubleshooting)  

---

## Adding a room
1. Create a new blank room in gamemaker. 
2. Add the room to the corresponding group in the IDE.
3. Add level instance, asset and tile layers and contents.
4. Add the room by name to the `_lvGen_data` file. You may need to edit one of the `_lvGen_pool_rooms_add_*` functions to place it in the appropriate location.
5. Update the included files to reflect the changes made by right clicking the room in the Asset Browser and then clicking Open In Explorer.
6. Copy the rooms folder into the datafiles folder.

## Adding a new level prop/actor
Add the object to the `global._lvGen_objects` array in `_lvGen_globals`.

## Adding a prefix
1. In `_lvGen_globals` add the desired prefix.
2. Add a function called `_lvGen_pool_rooms_add_*prefix*`.
3. In `_lvGen_data` add the function and the `global.gen_map` keys for each direction with the prefix. I.e. `global.gen_map[? “*prefix*_right”] = [ ];`.
4. When adding a room width the new prefix, the room name should follow the convention: `rp_*prefix*_*direction*_*number*`. In `_lvGen_pool_rooms_add_*prefix*` the room type should be prepended with the prefix like so `*prefix*_right`.

## Adding a forced room
1. Before `lvGen_generate` is called `global._lvGen_roomMap` should be cleared.
2. After `global._lvGen_roomMap` is cleared `lvGen_room_force` can be called with any predefined prefix.

## Adding a level requirement
1. When calling `lvGen_level_generate` an array should be added for fitness checks, within which all level requirement checks should be added.
2. When adding a requirement, it should be added as an array, starting with the function to call to check followed by the appropriate arguments.
i.e `[[lvGen_instance_exists, obj_lvGen_finish], [lvGen_instance_min, obj_lvGen_powerUp, 1]]`

## Adding a weighted room alternative
1. When calling `lvGen_level_generate` a weight array should be added, within which all weighted room alternatives should be added.
2. When adding a weighted alternative, it should be added as an array, starting with the prefix to replace, followed by the prefix to replace it with and then the probability that it should be replaced.
i.e `[["", "adv", global.score]]` will attempt to replace a basic room with an `adv` room 1/4 of the time if `global.score` is 25.

## Change tile size
Edit `#macro lvGen_tile_size` in `lvGen_macros_globals_and_enums`. 

---

## Troubleshooting
<details><summary>Branches are terminating with open doorways.</summary>
 
- Make sure that you have copied the rooms to the included files.</details>

<details><summary>Too many straight corridors.</summary>
 
- Room pools requires more corner rooms.</details>

<details><summary>Instances persist between level generation attempts.</summary>
 
 - Ensure that the object has beed added to global._lvGen_objects array in _lvGen_globals.</details>

<details><summary>Alternative rooms not being placed acording to probability.</summary>
 
 - Ensure that the pool of replacement rooms has enough variety of shape and size. If a room fails to replace, then whether the room changes to the alternate prefix is calculated again.</details>
 
 <details><summary>Level gen always fails after first attempt</summary>
 
 - Ensure that all instances to be removed before level generation are included in the `global._lvGen_objects` array in `_lvGen_globals`.
