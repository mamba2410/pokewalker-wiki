# Walk End

## Procedure

- Normal handshake, DS asserts master.
- DS sends `CMD_IDENTITY_REQ 0x20`, no data
- Walker sends `CMD_IDENTITY_RSP 0x22`, contains `walker_info_t (IdentityData)`
- DS sends `0x40`, contains `walker_info_t (IdentityData)` idk what to do with it
- Walker sends `0x42`, no data. Strange since this is apparently `CMD_IDENTITY_SEND_ALIAS1`
- DS does a bunch of reads.
- DS ping
- Walker pong
- DS sends `CMD_WALK_END_REQ 0x4e`
- Walker sends `CMD_WALK_END_RSP 0x50`
- End of connection


## What the walker does

- End walk action (after 0x4e)
    - clear special route bit in `RamCache_settingsByte`
    - clear walker has poke bit in `walker_status_flags`
    - set log write pointer to top of stack
    - set current watts to zero
    - copy ram health data to reliable `HEALTH_DATA_1/2`
    - reads `IDENTITY_DATA_1/2`
    - zeros `walker_info_t.unk1, unk3, flags:[0..1]`
    - writes to `IDENTITY_DATA_1/2`
    - zeros 0x64 bytes of eeprom at 0xcf0c (obtained items/pokemon/peer items)
    - 'zeros' 24 elements, each size 0x88 bytes, in eeprom starting at 0xcf0c (see eventLogInvalidate:0x188c)
    - zeros 0x6c8 bytes of eeprom at 0xb800 (event data)
    - zeros 0x1568 bytes of eeprom at 0xde24 (met peer data)
    - zeros 0x10 bytes of eeprom at 0x8f00 (current pokemon summary)

## DS checks

In order for the DS to accept the walk end, the eeprom needs to contain:

- valid `walker_info_t (IdentityData)`
    - `protocol_ver = 2`, `protocol_subver=0`
    - `le_unk1=1`, `le_unk3=7`
    - correct `unique_identity_data_t`
- The correct species in `route_info_t.pomeon_summary.le_species`
