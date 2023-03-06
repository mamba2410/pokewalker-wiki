# Walk Start

## What the walker does

- Start walk action (after 0x5a)
    - Write 0xa5 to eeprom reliable data area copy markers
    - copy 0x2900 bytes from eeprom 0xd700 to 0x8f00
    - copy 0x280 bytes ftin eeprom 0xd480 to 0xcc00 (my team data)
    - clears copy marker in eeprom reliable data area
    - zero event log (same as walk start)
    - zero peer team data, 0x1568 bytes of eeprom at 0xde24
    - clear current watts
    - write ram cache health data to eeprom
    - reads walker_info_t from eeprom
    - set walker status flag bits [0..1]
    - sets unk[0..3] in walker_info_t
    - write walker_info_t to eeprom
    - sets up walk start log event
    - writes walk start log event
    - clears obtained items/pokemon/peer items

