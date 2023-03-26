# Wild Pokemon Join

## Notes

## Walker stuff

In pokeJoinEmptyWalkerWalk

- set global status has_walking_poke [2]
- read reliable identity data
- if walker_info_t.flags.1 (have pokemon) then break
- set walker_info_t.flags.1
- set walker_info_t.unk0 and walker_info_t.unk2
- set walker_info_t.flags.2 (pokemon joined walk)
- write back to reliable area
- read small image for pokemon option c
- write to current small pokemon sprite
- read large image for pokemon option c
- write to current large pokemon sprite
- read pokemon option c name
- write to current pokemon name
- read route_info_t
- copy route_info_t.route_pokemon[2]  to route_info_t.current_pokemon
- clear pokemon_summary_t.flags[7]
- write 0x46 to route_info_t.current_pokemon.happiness
- zero 0x16 bytes of route_info_t.pokemon_nickname 
- write route_info_t
- reliable write global health data
- clear event logs (write zero in each entry type)

In addRandomGift

- (skipped some stuff)
- standard log event, with event_type=(0x10+ addRandomGift param)
  - should be event type 0x17
  