# Walker Initialisation


## Resetting via IR

Relevant commands are:

| CMD  | Description                         |
|------|-------------------------------------|
| 0xe0 | Clear events and lifetime data      |
| 0x2a | Clear events only                   |
| 0x2c | Don't clear events or lifetime data |

"clear events" means:

- zero `walker_info_t.event_bitmap`
- zero eeprom `received_bitfield -> text_event_item_name`

"clear lifetime data" means:

- Zero `walker_info_t.be_step_count`
- Zero `health_data_t.total_steps`
- Zero `health_data_t.total_days`
- `health_data_t.last_sync = ` 2nd Jan 2008
- Zero `health_data-t.today_steps`
- Zero eeprom `caught_pokemon_summary-0x08 -> current_peer_team_data-0x34`

All commands call `eeprom_init` with their respective flags set.

The one that's actually used by the game is `0x2a` - only clear event data.

## Function `eeprom_init`

- Zero `current_steps`
- Zero `current_watts`
- Clear `status_flags` bits 1 and 2 (`walker_inited`, `walker_has_pokemon`)
- Reliable read `walker_info_t`
    - Zero `unk0, unk1, unk2, unk3`
    - Zero tid/sid
    - Zero trainer name
    - If `clear_events` zero `event_bitmap_t`
    - Zero flags
    - Zero `unk4[2]`
    - Zero `protocol_subver`
    - `unk8 = 2`
    - Reliable read `unique_identity_data_t` into `walker_t.unique_identity_data`
    - if `clear_steps` zero `be_step_count`
    - Reliable write `walker_info_t`
- Clear and write `health_data_t`
  - if `clear_lifetime_data`
    - Zero `total_steps`
    - Zero `total_days`
    - `last_sync = 220924800` 2nd Jan 2008
    - Zero `today_steps`
  - Zero `walk_minute_counter`
  - Zero `current_watts`
  - Zero `steps_this_watt`
  - `settings = 0x24`
  - Reliable write `health_data_t`
- If `clear_steps` zero eeprom `0xce80 - 0xdbcc` (caught_pokemon_summary-8 -> current_peer_team_data-0x34)
- Else only zero inventory, event_log, step_log (note: this only skips clearing the staging area (?))
- If `clear_events` zero eeprom `0xb800 - 0xbec8` (received_bitfield -> text_event_item_name)
- Zero met peer data

## Function `check_and_validate_eeprom`

- Check for "nintendo" string at first 8 bytes of eeprom.
    - if not found:
        - run `eeprom_init(true, true)`
        - reset sound and contrast from ram health_data
        - write "nintendo" to start of eeprom
    - if found:
        - check settings byte and apply a minimum contrast?
      - update status flags from eeprom
- Check for unfinished copy by reliable reading both copy markers
    - if not ok, continue the copy