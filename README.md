# lvGen
 Room based level generation with bitmap auto tiling, room type forcing and level fitness checking made for and in Gamemaker. Uses .yy (standard Gamemaker room files).

---

## Contents

[Todo](README.md#todo)  
[Adding a room](README.md#adding-a-room)  
[Adding a new level prop/actor](README.md#adding-a-new-level-propactor)  
[Adding a prefix](README.md#adding-a-prefix)  
[Adding a forced room](README.md#adding-a-forced-room)  
[Adding a level requirement](README.md#adding-a-level-requirement)  
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
In `obj_lvGen_control > step event` set fail to true on any requirement check to make the level fail inspection.

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
