# Walk Start

## Procedure

- Normal handshake, DS asserts master

## What the walker does

On rx 0x5a:

- runs `start_walk`
- zeroes 0x6c8 bytes at 0xb800
- go to animation

On rx 0x38:

- runs `start_walk`
- go to animation

### Function `start_walk`

- Write 0xa5 to eeprom reliable data area copy markers
- (start of function `eeprom_unfinished_copy`)
- copy 0x2900 bytes from eeprom 0xd700 to 0x8f00
- copy 0x280 bytes ftin eeprom 0xd480 to 0xcc00 (my team data)
- clears copy marker in eeprom reliable data area
- (end of function `eeprom_unfinished_copy`)
- zero event log (same as walk start)
- zero peer team data, 0x1568 bytes of eeprom at 0xde24
- zero `current watts`
- zero `health_data.walk_minute_counter`
- zero `health_data.event_log_idx`
- zero `health_data.current_watts`
- write ram cache health data to eeprom
- reads walker_info_t from eeprom
- set walker status flag bits [0..1], clears bit [2]
- copy `unk{0..3}` in `walker_info` from `peer_walk_info`
- copy `walker_info.{tid,sid}` from `peer_walk_info`
- copy `trainer_name`, `protocol_ver`, `protocol_subver`, `unk5` from `peer_walk_info`
- sets `walker_info.unk8` to `2`
- reliable write `walker_info_t` to eeprom
- sets up walk start log event
- writes walk start log event
- clears obtained items/pokemon/peer items (zeros 0x64 bytes at 0xce8c)

### Log

- calls [logEvent](./event_log.md)
- e1 = 0x0000
- [sp] = settings byte u8 and settings byte&0x01 u8
- event type 0x19
- isOnSpecialRoute = settings byte&0x01
