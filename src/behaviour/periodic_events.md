# Periodic events

## Function `0xa34a:regular_processing`

Checks if each of the `RTC_EVENTS` bits are set, and if they are, perform that task.
Only does anything for minute, hour and day.

Minute just increments `walk_minute_counter`.
Hour and day have separate functions, seen below.

## Function `0xa682:rtc_sec` interrupt routine

- Sets `current_second`
- Increments `ram_health_data.last_sync` seconds counter
- increments `seconds_counter`, clamping to 3600 seconds
- Decrements `sleep_mode_countdown`
- Decrements `deep_sleep_countdown`
- Clears RTC interrupt bit

## Function `0xa3aa:perform_every_hour_tasks`

- sets bit 2 in `common_bit_flags`
- checks if total steps < 9999999 (7 9s)
    - If not, skip today steps checking
- Checks if `ram_health_data.today_steps` < 9999999 (7 9s)
    - If not, skip increment
- Increment `ram_health_data.today_steps`
- Write `ram_health_data` to eeprom
- Check if we have a pokemon (`walker_status_flags.2` is set)
    - If we don't, skip to end, setting watts to 0
- Read `route_info` from eeprom
- Check if we're on a special route
- Log event with type `0x1B_EVENT_TYPE_FELL_ASLEEP`
- Zero `0xf7a0:current_watts`
    - This is watts gained since last write? Not actual current held watts
- Check if time if at end of day hour in BCD
    - If so, set `RTC_EVENTS` bit 2 (every day?)

## Function `0xa45e:perform_every_day_tasks`

- Increment `ram_health_data.total_days`, clamping to `9999`
- Write `ram_health_data` to eeprom
- Read eeprom `0xcef0:historic_step_count`
- Shift the step count array, deleting 7th day and making room for today
- Write today's steps at index 0
- Write back to `0xcef0:historic_step_count`
- Fills 10 slots in `0xde24:MET_PEER_DATA` with 40 bytes of 0xff

