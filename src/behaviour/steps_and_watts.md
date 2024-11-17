# Steps and Watts

This is more "health data" focussed, which contains steps and watts.

## Function `0x24ac:process_step_taken`

- Compares two bytes, if they're equal then exit function
    - Probably how many steps are left to process
- Increment byte `0xf7b4`
- If its less than 64, exit function
- Increment `0xf7a4:watts_gained_since_last_sync`, clamping to 9999
- Increment 4-byte `0xf79c:current_steps`, clamping to 99999
- Increment 4-byte `ram_health_data.total_steps`, clamping to 9999999
- Increment `ram_health_data.steps_this_watt`
- If >= 20, subtract 20 and increment `ram_health_data.current_watts`, clamping to 9999
- increment byte `0xf7b3` and compare to `0xf7b2`
    - If `0xf7b3` <= `0xf7b2` then skip next step
- set `0xf7b3` = `0xf7b2`
- Decrement byte `0xf7b4` by 63


## Writing  `ram_health_data` to eeprom

- In `add_watts`
- On receive command `0x20_IDENTITY_DATA_REQ`
- On receive commands `0x32`, `0x40`, `0x52`
- On receive command `0x66`
- On receive commands `0xc6`, `0xd6`
- On walk start
- On walk end
- On battle lost
- On log event
- On pokemon join route
- On update settings
- Before entering dowsing or radar (to subtract watts)
- On initialising eeprom
- 
