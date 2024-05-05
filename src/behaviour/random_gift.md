# Random gift

In sleep mode (and only in sleep mode), calls `choose_random_event` if `walker_status_flags.set_on_startup` and not `walker_status_flags.poke_joined`.
`choose_random_event` does this exact check again before doing anything.
Does `walker_status_flags.poke_joined` have another usage?

There's a `random_event_table` which is an array of 7(*) pointers to 12-byte data for each event type.
- (*) since event types start at 1, index 0 is not part of the array. event type 6 doesn't exist so its entry contains a null pointer.

## Analysis of `0x5d52 handle_random_splash_event`

`handle_random_splash_event` takes parameter `event_type` in `r0l`.
`interaction_handler_splash` zeroes `substate_z`, keeps `substate_y` as `event_type` and passes `event_type` into function.

- Sets `substate_b` to `random_event_table[event_type*2]` - related to drawing?
- Sets current state to `0x0c` (random gift)
- Sets `seconds_counter` to 0

```c
enum event_type {
    // type 0 doesn't exist
    EVENT_TYPE_ITEM = 1,
    EVENT_TYPE_50_WATTS = 2,
    EVENT_TYPE_20_WATTS = 3,
    EVENT_TYPE_10_WATTS = 4,
    EVENT_TYPE_NO_GIFT = 5, // smiled/is happy etc
    // type 6 doesn't exist
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
    - this leads to a "random" message displayed on screen
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

## Analysis of `0x5fc2 choose_random_event`

checks walker flags, if set on startup and not poke joined then do the following:

- if we aren't in splash state, go to default
- if walker flags bit zero is not set, go to default
- clear walker flags bit 0
- clear substate y, z
- put current watts on stack
- 40% rand check, if failed go to default
- check walker flags, if we dont have pokemon:
    - if watts > 300 go to default
    - if not, move `7` to substate y (probably `EVENT_TYPE_JOIN_WALK`)
    - move `0x30` into substate z
    - exit
- check walker flags, if we do have pokemon
    - if seconds counter > 3600, go to default
    - reset heap, malloc and read `route_info`
    - put happiness inro `r5l`
    - reset heap, malloc and read 12 bytes for obtained items
    - if we have free slot, happiness > 90 and watts > 500, substate_y = `EVENT_TYPE_ITEM`
    - else if no free slot, happiness > 80 and watts > 250, substate_y = `EVENT_TYPE_50_WATTS`
    - else if watts > 200 `EVENT_TYPE_20_WATTS`
    - else if watts > 100 `EVENT_TYPE_10_WATTS`
    - else if watts > 50 and `ran_health_data.walk_minute_counter` > 60 `EVENT_TYPE_5`
    - else exit (with y=z=0)
    - y is event type, set z to 0x30, then exit

    
## Analysis of `0x5edc draw_random_gift_state`

- `substate_b` seems to be a pointer to an array/struct of data, outlined below. 12-bytes wide
- `substate_z` seems to be what was found, depending on the flags in `substate_b`
- `substate_a` gets added to `substate_b[3]` to make a "random" large message index

```c
struct random_event_data {
    uint8_t  flags;         // [0]=go to splash, [1]=draw pokemon, [2]=draw item, [3:4]=apply range(?), [5:7]=face index
    uint8_t  sound_id;
    uint8_t  draw_id;       // fc=draw pokemon name, fd = draw item name, fe = draw watts, ff = do nothing, anything else = use as message index and draw it (never used)
    uint8_t  message_id;    // index into large message id table (see eeprom 0x2530)
}
```

- reset heap pointer and malloc `0xc0` bytes
- load `substate_b[0]` bit 1
    - if bit 1 is set, draw pokemon small animated at (32, 4)
- converge, grab `substate_b[0]`, treat top 3 bits like a `u3 idx` (face index)
    - if `idx != 7` then call `draw_talk_faces(idx)`
- converge, grab `substate_b[0]` and load bit 2
    - if set, then call `draw_item_symbol(x=20, y=20)`
- converge, grab `substate_b[2]`
    - if val == 0xfc then draw pokemon name
    - if val == 0xfd then draw `substate_z` as item name index
    - if val == 0xfe then draw `substate_z` as watts
    - if val != 0xff then draw large message `substate_z` is index
- get first byte of `substate_b` array
- if `val&0x18 <= 8` (ie bit 4 == 0) then
    - if `substate_b[2] == 0xff` then move `substate_b[3]` into `r0h` and draw large message with full border, blinking cursor
    - else move `substate_b[3]` into `r0h` and draw large message with partial border, blinking cursor
- else move `substate_b[3] + substate_a` into `r0h` and draw large message with partial border, blinking cursor

## Analysis of `0x5e9e random_gift_state()`

Essentially steps through the 12-byte array on each button press.
Does this 4 bytes at a time, so there's max 3 message states.

- check if enter key pressed, if not then exit
- load `substate_b[0]` bit 0
- if set then call `some_statevar_shuffling` (sets y=z=0, sets a) then send to splash
- else `substate_b` += 4 (moves 4 bytes across the 12-byte array), then grab `substate_b[1]`
    - if val != 0x10 then call `play_sound(substate_b[1])`

## Decoding the event table

- item event 
    - draw pokemon, face_idx=0, [4:3] = 0b01, sound id 0x10, draw name, message id 0x32 "cheered!"
    - draw pokemon, face idx=7, [4:3] = 0b01, sound id 0x10, message id 0x3f "found something"
    - draw pokemon, draw item, go to splash, face idx=7, [4:3] = 0b01, sound id 5, draw item, message id 0x18 "found!"
- 50 watts event
    - draw pokemon, face idx=1, [4:3] = 0b11, sound id 0x10, draw name, message id 0x33 "is very happy!" (+`substate_a`) 
    - draw pokemon, face idx=7, [4:3] = 0b01, sound id 0x10, message id 0x3f "found something"
    - draw pokemon, go to splash, face idx=7, [4:3] = 0b01, sound id 5, draw watts, message id 0x18 "found!"
- joined event
    - face idx 7, [4:3] = 0b01, sound id 0x10, message id 0x40 "what?"
    - go to splash, face idx 6, [4:3] = 0b01, sound id 7, draw name, message id 0x41 "joined!"
- nothing event
    - go to splash, face idx 4, draw pokemon, [4:3]=0b11, sound id 0x10, draw name, message id 0x39 "is looking around" (+`substate_a`)
    

