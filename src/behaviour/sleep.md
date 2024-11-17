# Pokewalker Sleep

Device has two sleep modes, "normal" sleep and "deep" sleep.

"Normal" sleep is triggered after 60 seconds of inactivity.
"Deep" sleep is triggered after 90 seconds of inactivity.

Walker keeps track of what sleep state its in with bits `[3..4]` of `0xf7b6:walker_status_flags`.

When putting the walker to sleep, the main event loop changes to `sleep_mode_event_loop`.

On wake-up, it resets these counters, and checks what sleep state its in.
If it's in "deep" sleep mode, it resets the accel sample index to 0 then does the same as normal sleep.
In "normal" sleep mode (and deep sleep) it sets the `walker_status_flags` to "awake" and tells the LCD to exit power save mode.

On "deep" sleep, it stops `TIMERW`, clears bit 2 of `CKSTPR1`, sets `walker_status_flags` to "deep sleep" and clears bit 0 of `RTC_RTCCR2`.
On "normal" sleep, it sets bit 2 of `CKSTPR1`, sets `walker status flags` to "normal sleep", sets bit 2 of `RTC_RTCCR2`, sets the "deep sleep" countdown to `30` and clears bit 7 of `0xf7b5:common_bit_flags`, likely to indicate that we aren't walking any more.

## Function `0x7882:sleep_mode_event_loop`

TODO
