# Peer Play

## IR Sequence

Taken from [dmitry's writeup](http://dmitry.gr/?r=05.Projects&proj=28.%20pokewalker#_TOC_ecc9a541ea98ef47d0b4dc3d1f603ac8).

- Normal keyex
- Master sends command `0x10` containing its `identity_data_t`
- Slave checks version compatability and if already played
    - What if these checks fail?
- Slave replies with command `0x12` containing its `identity_data_t`
- Master checks version compatability and if already played
    - What if these checks fail? These should pass if slave replies?
- Master sends small animated sprite to slave's `0xf400`
- Master sends team data to slave's `0xdc00`
- Master reads slave's small animated sprite and writes to `0xf400`
- Master reads slave's team data and writes to `0xdc00`
- Master sends command `0x14` with `peer_play_data_t`
- Slave writes this to `0xf6c0`
- Slave replies with command `0x14` and its own `peer_play_data_t`
- Master writes this to `0xf6c0`
- Master sends command `0x16`
- Slave replies with command `0x16`
- Both play animation
- Both calculate gift as `seed = max(20000, 10*((remote.watts + self.watts) + (remote.steps + self.steps))`
- Checks if there's space in peer play gifts
    - If fails, gift `watts = max(99, seed/200)`
    - If passes, gift `item = TBD`

## Animation

- Self sits on middle right side, other walks in over 8 fast frames to middle-left, both animated
- show message "{other} has arrived" for 8 fast frames (2-high), both animated
- Show message "played a bit" with music note static in top middle, 8 fast frames, 2 high, both animated
- Draw large present in middle, 8 px from top, show message "here's a gift", 4 fast frames, 1 high
- Draw large message "{gift} received!", 4 fast frames, 2 high
- Send to splash


