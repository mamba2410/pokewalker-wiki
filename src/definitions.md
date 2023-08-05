# Definitions

## Pokemon

- `POKEMON` is just a generic name. Should only be used for sizes, not addresses.
- `CURRENT_POKEMON` is the current pokemon on a walk. Unknown if "joined" pokemon are treated the same.
- `EXTRA_POKEMON` is the pokemon gifted as an event by `CMD_C2` or caught on a special route.
- `ROUTE_POKEMON` are the three regular route-available pokemon. Stored in `route_info_t.route_pokemon[]`.
- `GIFTED_POKEMON` exclusively refers to the pokemon gifted by `CMD_C2`.
- `SPECIAL_ROUTE_POKEMON` is the extra pokemon on the special route. Stored in `special_route_info_t.special_pokemon`.

## Items

- `ITEM` is a generic name, only used to refer to `uint16_t le_item`.
- `DOWSED_ITEM` is an item currently in the inventory that was obtained through normal dowsing.
- `EXTRA_ITEM` is an item currently in the inventory that was either obtained though a `CMD_C2` gift or dowsed on a special route.
- `ROUTE_ITEM` is an item *not* currently in the inventory, that is available to be dowsed thorough normal dowsing.
- `GIFTED_ITEM` exclusively refers to the item gifted by `CMD_C2`
- `PEER_PLAY_ITEM` is an item received after engaging in peer play.
- `SPECIAL_ROUTE_ITEM` is an item *not* currently in the inventory, that is only available to be dowsed though special route dowsing.

