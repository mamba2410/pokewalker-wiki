# Random gift

## Analysis of disassembly 0x5d52

`handle_random_splash_event` takes parameter `event_type` in `r0l`.

```c
enum event_type {
    EVENT_TYPE_ITEM = 1,
    EVENT_TYPE_50_WATTS = 2,
    EVENT_TYPE_20_WATTS = 3,
    EVENT_TYPE_10_WATTS = 4,
    EVENT_TYPE_JOIN_WALK = 7,
};
```

If event type adds watts, just call `0x1f3e add_watts()`.

If event type was join walk, call `0x5c0a pokemon_join_walk()`

If event type was add item then:

- `item_index` = 9 - floor(`current_watts` / 500)
- `item_id` = `route_item_ids[item_index]`
- if we have slots free add item to slot 
- if we don't have slots free, don't bother 

common to all event types:

- alloc and read route info from eeprom
- check if we're on a special map
- malloc enough for log struct
- log event
    - log type is (16 + `event_type`)
    - say if we're on a special route
- if event type was add watts, `g_substate_a` = `rand()`>>3 % 3
- else `g_substate_a` = 0

## Analysis of `0x5c0a pokemon_join_walk()`

- clear current watts
- set `have_pokemon` flag (bit 2) in `walker_status_flags` ram cache
- malloc and read in `walker_info` from eeprom
- set flags in `walker_info`
    - set walker inited if it wasn't already
    - copy unk0 to unk1
    - copy unk2 to unk3
    - set have pokemon in flags
- reliable write walker info
- reset and malloc 0x180 bytes for images
- copy option c small sprite to current pokemon small sprite
- copy option c large sprite to current pokemon large sprite
- copy option c pokemon name to current pokemon name
- reset, malloc and read route info
- copy option c pokemon summary to route info
- set bit 7 in pokemon flags (idk what this bit does)
- set pokemon happiness to 70
- copy pokemon nickname to route info 
- write route info again
- reliable write ram health data to eeprom
- clear event log

