# Event logs

## Notes

- dmitry's struct is 0x5e bytes, missing route_name

## walker function sig

r0 = TeamAndrouteInfo in ram ptr (0xBE bytes from 0x8f00)
e0 = tmpBufPtr (0x88 bytes big) can be pre-initialized with data
r1l = eventTYpe
r1h = boolean isOnSpecialEventRoute
e1 = extraInfo (eg: item type)
[sp] = u8 pushed as u16, availablePokeIdx(1..3 are route availables, 4 is event poke, 0 for N/A)

## Better notes on log_event

- Reads next event write location(index stored in `ram_health_data.event_log_idx`)
- If read event is zero, write new event
- If this event is "fell asleep", then drop it
- Otherwise, log event. So all events are valid, unless you're logging a "fell asleep" event after a nonzero event
- If we're going to overwrite the walk start event, don't do that and increment index (r4) again
- Zero event struct when incoming event type is <= 10
- Copies the following information into the struct:
  - incoming event type
  - extra info from e1
  - `ram_health_data.last_sync`
  - `watts_gained_since_last_sync`
  - `current_steps`
  - walking pokemon's species from `route_info`
  - walking pokemon's nickname from `route_info`
  - walking pokemon's friendship from `route_info`
  - Pokemon flag shenanegans (best I can decipher, there's a lot of redundant stuff?)
    `lip.opf = (lip.opf&0xe0) | (ri.ps.f1&0x1f)`
    `insert_val = ((ri.ps.f1>>5)&0x03)`
    `bitfield_insert(le.opf, insert_val, 2 bits, << 1)`
    `lip.opf |= (ri.ps.f2&0x02)<<6`
  - If not on special route:
    - route image index
    - route name
  - Else
    - copy route image index from `0xbf06`
    - copy route name from `0xbf50`
  - If `pokemon_idx` is a route pokemon:
    - copy species to `lip.their_species`
    - do same flags shenanegans as for ours above, without orring f2
  - Else (is either our pokemon again, likely joined, or a special pokemon)
    - If its ours, do nothing?
    - We know its a special pokemon, so:
    - If event type is 0x0f or 0x10 (pokemon ran or lost)
      - Copy species from 0xbf08
    - Else
      - Copy species from 0xba44
    - read special pokemon flags from 0xbf0d
    - do same shenanegans, withour orring f2
  - Write to address using existing index
  - circularly increment `ram_health_data.event_log_index` (with 23 as denominator)
  - reliably write `ram_health_data`

## notes on the decomp of logEvent

- we have a log set up to write (apparently)
- reads written event
- if 0x1b, drop this log
- if 0x19, increment index `idx = (idx+1)%23`
- if our event is <0x0a, skip the zero part (this will be peer play events)
- zero 0x88 bytes of our log buf
- (buf+0x84) = our event type (u8)
- (buf+0x0e) = extra data (u16)
- (buf+0x00) = current time (u32, be)
- (buf+0x78) = current watts (u16, be)
- (buf+0x7c) = current steps (u32, be)
- (buf+0x0a) = (route_info) (current species) (u16)
- (buf+0x20) = (route_info+0x10) (current nickname) 22 bytes
- (buf+0x77) = (route_info+0x26) pokemon happiness u8
- (buf+0x85) = (pokemon_flags_1&0x1f) | (buf+0x85)&0xe0, u8
- (buf+0x85):7 = are we on special route, u8
- on normal route: (buf+0x76) = route_image_index u8
- on normal route: (buf+0x4c) = route_name 0x2a bytes (21*u16)
- on special route: (buf+0x76) = special route_image_index (eeprom:0xbf06) (u8)
- on special route: (buf+0x4c) = special route_name 0x2a bytes (eeprom:0xbf50)
- check if we have 1-3:normal_pokemon, 4:special_pokemon, else:no_pokemon (idk)
  - probably for caught pokemon
- if we were normal pokemeon: (buf+0x0c) = caught species
- normal pokemon: (buf+0x86) = caught gender/form &0x1f | (buf+0x86)&0xe0 + funky bitfield insert
- event pokemon:
  - if log type is 0x0f or 0x10: (buf+0x0c) = (eeprom:bf08) event caught species (u16)
  - else: (buf+0x0c) = (eeprom:ba44)  gifted pokemon species (u16)
- event pokemon: (buf+0x86) = 1f&(eeprom:bf0d) (should be bf15) special pokemon flags (u8)
  - also orred with (buf+0x86)&0xe0 + funky bitfield insert
- all routes: (no pokemon jumps straight here)
- write 0x88 bytes to eeprom at `0xcf0c+0x88*idx`
- increment global index `idx = (idx+1)%23`
- write total steps to reliable area

## notes on decomp of logPeerPlay

- Also grabs 0x88 bytes for log item
- (buf+0x04) = peer_play+0x08 u32
- (buf+0x08) = peer_play+0x0c u16
- (buf+0x0c) = peer_play+0x0e u16
- (buf+0x86) = peer_play+0x36 u8 + bitfield inserts again | peer_play:7
- (buf+0x7a) = peer_play+0x04 (peer watts) u16
- (buf+0x80) = peer_play+0x00 (peer steps) u32
- (buf+0x36) = peer_play.nickname 22 bytes (walker copies 24)
- (buf+0x10) = peer_play.trainer_name 16 bytes (walker copies 18)
- pushes `(2*number_of_peers_met)&0xff00` u16 to the stack??
- event type is number of peers met so far
- extra data = route_info.items[n_peers_met] u16:w
