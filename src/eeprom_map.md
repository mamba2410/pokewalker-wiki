# EEPROM Map

| name | addr | size | comments | changed? |
|:-----|:-----|-----:|:---------|:---|
| NINTENDO | 0x0000 | 8 |  "nintendo" string as a magic marker. if rom does not find this at boot, it will consider the walker empty and uninitialized | |
| PERSONALISATION | 0x0008 | 8 |  some value written during personalization. never read. | |
|  | 0x0010 | 98 |  ??? | |
| WATCHDOG_RESETS | 0x0072 | 1 |  number of watchdog resets | |
|  | 0x0073 | 13 |  ??? | |
| FACTORY_DATA_1 | 0x0080 | 2 |  factory-provided adc calibration data. (reliable data format, copy at 0x0180) | Y |
| UNIQUE_IDENTITY_DATA_1 | 0x0083 | 40 |  struct uniqueidentitydata. provisioned at game pairing time (reliable data format, copy at 0x0183) | Y |
| LCD_COMMANDS_1 | 0x00ac | 64 |  struct lcdconfigcmds. provisioned at game pairing time (reliable data format, copy at 0x01ac) | Y |
| IDENTITY_DATA_1 | 0x00ed | 104 |  struct identitydata. provisioned at walk start time (reliable data format, copy at 0x01ed) | Y |
| HEALTH_DATA_1 | 0x0156 | 24 |  struct healthdata. provisioned at walk start time (reliable data format, copy at 0x0256) | Y |
| COPY_MARKER_1 | 0x016f | 1 |  struct copymarker. used at walk init time (reliable data format, copy at 0x26f) | Y |
|  | 0x0170 | 16 |  unused | Y |
| FACTORY_DATA_2 | 0x0180 | 2 |  factory-provided adc calibration data. (reliable data format, copy at 0x0080) | Y |
| UNIQUE_IDENTITY_DATA_2 | 0x0183 | 40 |  struct uniqueidentitydata. provisioned at game pairing time (reliable data format, copy at 0x0083) | Y |
| LCD_COMMANDS_2 | 0x01ac | 64 |  struct lcdconfigcmds. provisioned at game pairing time (reliable data format, copy at 0x00ac) | Y |
| IDENTITY_DATA_2 | 0x01ed | 104 |  struct identitydata. provisioned at walk start time (reliable data format, copy at 0x00ed) | Y |
| HEALTH_DATA_2 | 0x0256 | 24 |  struct healthdata. provisioned at walk start time (reliable data format, copy at 0x0156) | Y |
| COPY_MARKER_2 | 0x026f | 1 |  struct copymarker. used at walk init time (reliable data format, copy at 0x16f) | Y |
|  | 0x0270 | 16 |  unused | Y |
| IMG_DIGITS | 0x0280 | 416 |  numeric character images: "0123456789:-/", 8x16 each, in this order | |
| IMG_WATTS | 0x0420 | 64 |  watt symbol image 16x16 | |
| IMG_BALL | 0x0460 | 16 |  pokeball 8x8 | |
| IMG_BALL_LIGHT | 0x0470 | 16 |  pokeball light grey 8x8 (used for event pokemon) | |
|  | 0x0480 | 8 |  unused | |
| IMG_ITEM | 0x0488 | 16 |  item symbol 8x8 | |
| IMG_ITEM_LIGHT | 0x0498 | 16 |  item symbol light grey 8x8 (used for event items) | |
| IMG_MAP_SMALL | 0x04a8 | 16 |  tiny map icon 8x8 (used for "special map" reception) | |
| IMG_CARD_SUITS | 0x04b8 | 64 |  card faces: heart, spade, diamond, club, 8x8 each (used for "stamp" reception) | |
| IMG_ARROWS | 0x04f8 | 192 |  arrows (up down left right), each in 3 configs (normal, offset, inverted) 8x8 each | |
| IMG_MENU_ARROW_LEFT | 0x05b8 | 32 |  left arrow for menu 8x16 | |
| IMG_MENU_ARROW_RIGHT | 0x05d8 | 32 |  right arrow for menu 8x16 | |
| IMG_MENU_ARROW_RETURN | 0x05f8 | 32 |  "return" symbol for menu 8x16 | |
|  | 0x0618 | 40 |  unused | |
|  | 0x0638 | 16 |  symbol for "have more message" in the bottom right of messages. orred into last 8 columns (thus 16 bytes). applied after 0x0648 | |
|  | 0x0648 | 8 |  symbol for "have more messages" in the bottom right of messages. each byte here is anded with each col of last 8 in the message. both bitplanes (so you can make it black or keep as is). applied before 0x0638 | |
|  | 0x0650 | 16 |  medicine vial (?) icon 8x8 | |
| IMG_LOW_BATTERY | 0x0660 | 16 |  low battery icon 8x8 | |
| IMG_TALK_FACES | 0x0670 | 576 |  large talk bubbles from bottom right with pokemon feeling icon (exclamation, heart, music note, smile, neutral face, ellipsis) 24x16 each, 6 of them | |
| IMG_TALK_EXCLAMATION | 0x08b0 | 96 |  large talk bubble from bottom left with exclamation point in it 24x16 | |
| IMG_MENU_TITLE_POKERADAR | 0x0910 | 320 |  "pok&eacute; radar" menu heading in a box. 80x16 | |
| IMG_MENU_TITLE_DOWSING | 0x0a50 | 320 |  "dowsing" menu heading in a box. 80x16 | |
| IMG_MENU_TITLE_CONNECT | 0x0b90 | 320 |  "connect" menu heading in a box. 80x16 | |
| IMG_MENU_TITLE_TRAINER_CARD | 0x0cd0 | 320 |  "trainer card" menu heading in a box. 80x16 | |
| IMG_MENU_TITLE_INVENTORY | 0x0e10 | 320 |  "pok&eacute;mon &amp; items" menu heading in a box. 80x16 | |
| IMG_MENU_TITLE_SETTINGS | 0x0f50 | 320 |  "settings" menu heading in a box. 80x16 | |
| IMG_MENU_ICON_POKERADAR | 0x1090 | 64 |  "poke-radar" icon for main menu 16x16 | |
| IMG_MENU_ICON_DOWSING | 0x10d0 | 64 |  "dowsing" icon for main menu 16x16 | |
| IMG_MENU_ICON_CONNECT | 0x1110 | 64 |  "connect" icon for main menu 16x16 | |
| IMG_MENU_ICON_TRAINER_CARD | 0x1150 | 64 |  "trainer card" icon for main menu 16x16 | |
| IMG_MENU_ICON_INVENTORY | 0x1190 | 64 |  "pokemon & items" icon for main menu 16x16 | |
| IMG_MENU_ICON_SETTINGS | 0x11d0 | 64 |  "settings" icon for main menu 16x16 | |
| IMG_PERSON | 0x1210 | 64 |  "person" icon for trainer card screen 16x16 | |
| IMG_TRAINER_NAME | 0x1250 | 320 |  trainer's name rendered as an image 80x16 | |
| IMG_ROUTE_SMALL | 0x1390 | 64 |  small route image for "trainer card" screen 16x16 | |
| IMG_STEPS_FRAME | 0x13d0 | 160 |  "steps" in frame for second screen of trainer card 40x16 | |
| IMG_TIME_FRAME | 0x1470 | 128 |  "time" in frame for second screen of trainer card 32x16 | |
| IMG_DAYS_FRAME | 0x14f0 | 160 |  "days" in frame for second screen of trainer card 40x16 | |
| IMG_TOTAL_DAYS_FRAME | 0x1590 | 256 |  "total days:" in frame for second screen of trainer card 64x16 | |
| IMG_SOUND_FRAME | 0x1690 | 160 |  "sound" in frame for preferences screen 40x16 | |
| IMG_SHADE_FRAME | 0x1730 | 160 |  "shade" in frame for preferences screen 40x16 | |
| IMG_SPEAKER_OFF | 0x17d0 | 96 |  speaker icon with no waves (no sound) for preferences screen 24x16 | |
| IMG_SPEAKER_LOW | 0x1830 | 96 |  speaker icon with one wave (low sound) for preferences screen 24x16 | |
| IMG_SPEAKER_HIGH | 0x1890 | 96 |  speaker icon with two waves (high sound) for preferences screen 24x16 | |
| IMG_CONTRAST_DEMONSTRATOR | 0x18f0 | 32 |  contrast demonstrator (drawn a bunch of times over) 8x16 | |
| IMG_TREASURE_LARGE | 0x1910 | 192 |  large treasure chest icon for item view 32x24 | |
|  | 0x19d0 | 192 |  large map scroll thingie 32x24 | |
| IMG_PRESENT_LARGE | 0x1a90 | 192 |  large present icon for item view 32x24 | |
| IMG_DOWSING_BUSH_DARK | 0x1b50 | 64 |  small bush dark-colored, for dowsing 16x16 | |
| IMG_DOWSING_BUSH_LIGHT | 0x1b90 | 64 |  small bush light-colored, for dowsing 16x16 | |
|  | 0x1bd0 | 128 |  "left: "string on white background. seems unreferenced 32x16 | |
|  | 0x1c50 | 96 |  blank image 16x24 | |
| IMG_RADAR_BUSH | 0x1cb0 | 192 |  bush dark 32x24 | Y |
| IMG_RADAR_BUBBLE_ONE | 0x1d70 | 64 |  word bubble with one exclamation point (for poke hunting) 16x16 | |
| IMG_RADAR_BUBBLE_TWO | 0x1db0 | 64 |  word bubble with two exclamation points (for poke hunting) 16x16 | |
| IMG_RADAR_BUBBLE_THREE | 0x1df0 | 64 |  word bubble with three exclamation points (for poke hunting) 16x16 | Y |
| IMG_RADAR_CLICK | 0x1e30 | 64 |  three lines radiating from bottom left (for bush we just clicked) 16x16 | |
| IMG_RADAR_ATTACK_HIT | 0x1e70 | 128 |  skewed small 7-pointed star (attack) 16x32 | |
| IMG_RADAR_CRITICAL_HIT | 0x1ef0 | 128 |  skewed large 7-pointed star (critical hit attack) 16x32 | |
| IMG_RADAR_APPEAR_CLOUD | 0x1f70 | 192 |  cloud "for pokemon appeared" 32x24 | |
| IMG_RADAR_HP_BLIP | 0x2030 | 16 |  "hp" item (4 of these make up an hp bar) 8x8 | |
| IMG_RADAR_CACH_EFFECT | 0x2040 | 16 |  a little 5-pointed star image for when we catch something 8x8 | |
| TEXT_RADAR_ACTION | 0x2050 | 768 |  "attack/evade/catch" directions placard for battles 96x32 | |
| IMG_POKEWALKER_BIG | 0x2350 | 256 |  pokewalker image, blank screen, 32x32 | |
| IMG_IR_ARCS | 0x2450 | 32 |  ir xmit icon (like wifi arcs) 8x16 | |
| IMG_MUSIC_NOTE | 0x2470 | 16 |  music note icon 8x8 | |
|  | 0x2480 | 16 |  blank icon 8x8 | |
| IMG_HOURS_FRAME | 0x2490 | 160 |  "hours" in a pretty frame. appears unused 40x16 | |
| TEXT_CONNECTING | 0x2530 | 384 |  "connecting..." string for comms 96x16 | |
| TEXT_NO_TRAINER | 0x26b0 | 384 |  "no trainer found" string for comms 96x16 | Y |
| TEXT_CANNOT_COMPLETE | 0x2830 | 768 |  "cannot complete thisconnection" string for comms 96x32 | |
| TEXT_CANNOT_CONNECT | 0x2b30 | 384 |  "cannot connect" string for comms 96x16 | |
| TEXT_TRAINER_UNAVAILABLE | 0x2cb0 | 768 |  "other trainer isunavailable" s60tring for comms 96x32 | |
| TEXT_ALREADY_RECV_EVENT | 0x2fb0 | 768 |  "already received this event" string for comms 96x32 | Y |
| TEXT_CANNOT_CONNECT_AGAIN | 0x32b0 | 768 |  "canont connect to trainer again" string for comms 96x32 | Y |
| TEXT_COULD_NOT_RECV | 0x35b0 | 384 |  "could not receive..." string for comms 96x32 | Y |
| TEXT_HAS_ARRIVED | 0x38b0 | 384 |  "has arrived!" string for comms 96x16 | Y |
| TEXT_HAS_LEFT | 0x3a30 | 384 |  "has left." string for comms 96x16 | |
| TEXT_RECV | 0x3bb0 | 384 |  "received!" string for comms 96x16 | Y |
| TEXT_COMPLETED | 0x3d30 | 384 |  "completed!" string for comms 96x16 | |
| TEXT_SPECIAL_MAP | 0x3eb0 | 384 |  "special map" string for comms 96x16 | Y |
| TEXT_STAMP | 0x4030 | 384 |  "stamp" string for comms 96x16 | |
| TEXT_SPECIAL_ROUTE | 0x41b0 | 384 |  "special route" string for comms 96x16 | Y |
| TEXT_NEED_WATTS | 0x4330 | 384 |  "need more watts." string 96x16 | |
| TEXT_NO_POKEMON_HELD | 0x44b0 | 384 |  "no pokemon held!" string 96x16 | |
| TEXT_NOTHING_HELD | 0x4630 | 384 |  "nothing held!" string 96x16 | |
| TEXT_DISCOVER_ITEM | 0x47b0 | 384 |  "discover an item!" string 96x16 | |
| TEXT_FOUND | 0x4930 | 384 |  "found!" string 90x16 | |
| TEXT_NOTHING_FOUND | 0x4ab0 | 384 |  "nothing found!" string 90x16 | |
| TEXT_ITS_NEAR | 0x4c30 | 384 |  "it's near!" string 90x16 | |
| TEXT_FAR_AWAY | 0x4db0 | 384 |  "it's far away..." string 90x16 | Y |
| TEXT_FIND_POKEMON | 0x4f30 | 384 |  "find a pokemon!" string 90x16 | Y |
| TEXT_FOUND_SOMETHING | 0x50b0 | 384 |  "found something!" string 90x16 | |
| TEXT_GOT_AWAY | 0x5230 | 384 |  "it got away..." string 90x16 | Y |
| TEXT_APPEARED | 0x53b0 | 384 |  "appeared!" string 90x16 | |
| TEXT_WAS_CAUGHT | 0x5530 | 384 |  "was caught!" string 90x16 | Y |
| TEXT_FLED | 0x56b0 | 384 |  "fled..." string 96x16 | Y |
| TEXT_TOO_STRONG | 0x5830 | 384 |  "was too strong." string 96x16 | |
| TEXT_ATTACKED | 0x59b0 | 384 |  "attached!" string 96x16 | |
| TEXT_EVADED | 0x5b30 | 384 |  "evaded!" string 96x16 | |
| TEXT_CRITICAL_HIT | 0x5cb0 | 384 |  "a critical hit!" string 96x16 | |
| TEXT_SPACES | 0x5e30 | 384 |  "       " (yes a bunch of spaces) string 96x16 | |
| TEXT_THREW_BALL | 0x5fb0 | 384 |  "threw a poke ball." string 96x16 | Y |
| TEXT_ALMOST_HAD | 0x6130 | 128 |  "almost had it!" string 96x16 | |
| TEXT_STARE_DOWN | 0x62b0 | 384 |  "stare down!" string 96x16 | |
| TEXT_LOST | 0x6430 | 384 |  "lost!" string 96x16 | |
| TEXT_PEER_HAS_ARRIVED | 0x65b0 | 384 |  "has arrived" (for walker to walker) string 96x16 | Y |
| TEXT_HAD_ADVENTURES | 0x6730 | 384 |  "had adventures!" string 96x16 | |
| TEXT_PLAY_BATTLED | 0x68b0 | 384 |  "play-battled." string 96x16 | |
| TEXT_WENT_RUN | 0x6a30 | 384 |  "went for a run." string 96x16 | |
| TEXT_WENT_WALK | 0x6bb0 | 384 |  "went for a walk." string 96x16 | |
| TEXT_RECV_GIFT | 0x6d30 | 384 |  "played a bit." string 96x16 | |
| TEXT_RECV_GIFT | 0x6eb0 | 384 |  "here's a gift..." string 96x16 | Y |
| TEXT_CHEERED | 0x7030 | 384 |  "cheered!" string 96x16 | |
| TEXT_VERY_HAPPY | 0x71b0 | 384 |  "is very happy!" string 96x16 | |
| TEXT_FUN | 0x7330 | 384 |  "is having fun!" string 96x16 | |
| TEXT_FEEL_GOOD | 0x74b0 | 384 |  "is feeling good!" string 96x16 | |
| TEXT_HAPPY | 0x7630 | 384 |  "is happy." string 96x16 | |
| TEXT_SMILING | 0x77b0 | 384 |  "is smiling." string 96x16 | |
| TEXT_CHEERFUL | 0x7930 | 384 |  "is cheerful." string 96x16 | |
| TEXT_PATIENT | 0x7ab0 | 384 |  "is being patient." string 96x16 | |
| TEXT_SITTING | 0x7c30 | 384 |  "sits quietly." string 96x16 | |
| TEXT_TURN_LOOK | 0x7db0 | 384 |  "turned to look." string 96x16 | |
| TEXT_LOOKING_AROUND | 0x7f30 | 384 |  "is looking around." string 96x16 | Y |
| TEXT_LOOKING_HERE | 0x80b0 | 384 |  "is looking this way." string 96x16 | |
| TEXT_DAYDREAMING | 0x8230 | 384 |  "is daydreaming." string 96x16 | |
| TEXT_INTERACT_FOUND_SOMETHING | 0x83b0 | 384 |  "found something." string 96x16 | |
| TEXT_WHAT | 0x8530 | 384 |  "what?" string 96x16 | |
| TEXT_JOINED | 0x86b0 | 384 |  "joined you!" string 96x16 | |
| TEXT_REWARD | 0x8830 | 384 |  "reward" string 96x16 | |
| TEXT_GOOD_JOB | 0x89b0 | 384 |  "good job!" string 96x16 | |
| TEXT_SWITCH | 0x8b30 | 320 |  "switch?" string 80x16 | |
|  | 0x8c70 | 64 |  ??? | |
| RANDOM_CHECKSUM_INFO | 0x8cb0 | 48 |  random checksum area descriptor addrs	(see 	0x36f2:randomeepromchecksumcheck	, struct struct randomcheckinfo) | |
| RANDOM_CHECKSUM_AREA | 0x8cf0 | 528 |  random garbage data that is checksummed by randomeepromchecksumcheck() | |
| ROUTE_INFO | 0x8f00 | 190 |  struct routeinfo - current route data | |
| IMG_ROUTE_LARGE | 0x8fbe | 192 |  current "area" we are strolling in graphic 32x24 | |
| TEXT_ROUTE_NAME | 0x907e | 320 |  current "area" we are strolling in textual name 80x16 | |
| IMG_POKEMON_SMALL_ANIMATED | 0x91be | 384 |  current pokemon animated sprite for "held items and pokemon" screen, fights, etc. 32 x 24 x 2 frames | |
| IMG_POKEMON_LARGE_ANIMATED | 0x933e | 1536 |  current pokemon large nimated sprite for main screen 64 x 48 x 2 frames | |
| TEXT_POKEMON_NAME | 0x993e | 320 |  cur pokemon name image 80x16 | |
| IMG_ROUTE_POKEMON_SMALL_ANIMATED | 0x9a7e | 1152 |  route available pokemon selected by the same animated small sprites  32 x 24 x 2 frames x 3 pokemon | |
| IMG_ROUTE_POKEMON_LARGE_ANIMATED | 0x9efe | 1536 |  large animated image (like at 0x933e) but of the third (option c) available pokemon on this route. used for "joined your walk" situation 64 x 48 x 2 frame | |
| TEXT_POKEMON_NAMES | 0xa4fe | 960 |  available pokemon name images 80x16 x3 pokemon | |
| TEXT_ITEM_NAMES | 0xa8be | 3840 | available item names as images. 96x16 x 10 images (one per item) | Y |
|  | 0xb7be | 66 |  ??? | |
| RECEIVED_BITFIELD | 0xb800 | 1 |  bitfield of special things received. 0x01 - "heart" stamp received, 0x02 - "spade" stamp received, 0x04 - "diamond" stamp received, 0x08 - "club" stamp received, 0x10 - "special map" received, 0x20 - walker contains an event pokemon (gifted or caught), 0x40 - walker contains event item (gifted or dowsed), 0x80 - walker has received a "special route" | |
|  | 0xb801 | 3 |  unused | |
|  | 0xb804 | 576 |  data for "special map received". format unknown. possibly used by the ds games, but no evidence of this found in the games. | |
| EVENT_POKEMON_BASIC_DATA | 0xba44 | 16 |  gifted event poke, or radar-caught event poke. Basic data struct PokemonSummary | Y |
| EVENT_POKEMON_EXTRA_DATA | 0xba54 | 44 |  extra data. struct eventpokeextradata | |
| IMG_EVENT_POKEMON_SMALL_ANIMATED | 0xba80 | 384 |  small sprite 32 x 24 x 2 frames | |
| TEXT_EVENT_POKEMON_NAME | 0xbc00 | 320 |  name image 80x16 | |
| EVENT_ITEM | 0xbd40 | 8 |  gifted event item, or dowsed event item. item data. 6 bytes of zeroes, then u16 item, LE | Y |
| TEXT_EVENT_ITEM_NAME | 0xbd48 | 384 |  item name image 96x16 | |
|  | 0xbec8 | 56 |  unused | |
| SPECIAL_ROUTE_STRUCT | 0xbf00 | 3260 |  "special route" info (struct specialroute): | Y |
| | 0xbf00 | 6 | 6 bytes of zeroes (part of item struct but unused by DS or walker) | Y |
| ROUTE_IMAGE_IDXNAME | 0xbf06 | 1 |  enum routeimageidx	 | |
|  | 0xbf07 | 1 |  unused | |
| SPECIAL_POKEMON_BASIC_DATA | 0xbf08 | 16 |  special route-available pokemon basic data. struct pokemonsummary | |
| SPECIAL_POKEMON_EXTRA_DATA | 0xbf18 | 44 |  special route-available pokemon extra data. struct eventpokeextradata | |
| SPECIAL_POKEMON_STEPS_REQUIRED | 0xbf44 | 2 |  min steps to encounter this poke on the route. u16 le | |
| SPECIAL_POKEMON_PERCENT_CHANCE | 0xbf46 | 1 |  percent chance to encounter this poke on route after step minimum met	 | |
|  | 0xbf47 | 1 |  unused | |
| SPECIAL_ITEM | 0xbf48 | 2 |  special route-available item. u16 le | |
| SPECIAL_ITEM_STEPS_REQUIRED | 0xbf4a | 2 |  min steps dowse this item. u16 le | |
| SPECIAL_ITEM_PERCENT_CHANCE | 0xbf4c | 1 |  percent chance to dowse this item on route after step minimum met	 | |
|  | 0xbf4d | 3 |  unused | |
| SPECIAL_ROUTE_NAME_NINTENDOENC | 0xbf50 | 42 |  routename u16[21] | |
| SPECIAL_POKEMON_EVENT_INDEX | 0xbf7a | 1 |  "event index" for catching this route's special pokemon	 | |
| SPECIAL_ITEM_EVENT_INDEX | 0xbf7b | 1 |  "event index" for dowsing this route's special item	 | |
| IMG_SPECIAL_POKEMON_SMALL_ANIMATED | 0xbf7c | 1920 |  special route pokemon animates small sprite. 32 x 24 x 2 frames. should be 0x180 bytes big, but it 0x170. no idea why but confirmed	 | |
| TEXT_SPECIAL_POKEMON_NAME | 0xc6fc | 320 |  special route pokemon name image 80x16	 | |
| IMG_SPECIAL_ROUTE_IMAGE | 0xc83c | 192 |  special routes's large image for home screen, like 0x8fbe is for a normal route 32x24	 | |
| TEXT_SPECIAL_ROUTE_NAME_SMALL | 0xc8fc | 320 |  special routes's textual name 80x16 | |
| TEXT_SPECIAL_ROUTE_NAME | 0xca3c | 384 |  special route item textual name 96x16 | |
|  | 0xcbbc | 68 |  unused | |
| TEAM_DATA_STRUCT | 0xcc00 | 548 |  struct teamdata on our whole team, so that any walkers we peer play with transfer it to their ds game and we can be battled in the trainer house | |
|  | 0xce24 | 92 |  also written at walk start time as part of the above. probably just to keep the write a multiple of 0x80 bytes | |
|  | 0xce80 | 8 |  unused | |
|  | 0xce88 | 1 |  if low bit set, game will give player a starf berry once per savefile. used when 99999 steps reached | |
|  | 0xce89 | 1 |  unused | |
|  | 0xce8a | 2 |  current watts written to eeprom by cmd 0x20 before replying (likely so remote can read them directly). u16 be | |
| CAUGHT_POKEMON_SUMMARY | 0xce8c | 48 |  3x route-available pokemon we've caught so far. 3x struct pokemonsummary | |
| OBTAINED_ITEMS | 0xcebc | 12 |  3x route-available items we've dowsed so far. 3x {u16 le item, u16 le unused} | |
| PEER_PLAY_ITEMS | 0xcec8 | 40 |  10x route-available items we've been gifted by peer play. 10x {u16 le item, u16 le unused} | Y |
| HISTORIC_STEP_COUNT | 0xcef0 | 28 |  historic step count per day. u32 each, be, [0] is yesterday, [1] is day before, etc... | |
| EVENT_LOG | 0xcf0c | 3316 |  event log. circularly-written, displayed in time order. 24x struct eventlogitem | |
| TEAM_DATA_STAGING | 0xd480 | 640 |  team data written here before walk start action. struct teamdata	 | |
|  | 0xd700 | 10496 | scenario data written here before walk start action. everything that 0x8F00-0xB7FF would have | Y |
| CURRENT_PEER_TEAM_DATA | 0xdc00 | 548 |  current peer play peer. struct teamdata. uploaded as part of peer play. later shifted to index [0] at 0xde24 list of peers | |
| MET_PEER_DATA | 0xde24 | 5480 |  peers we've met. for battle house info. newest element is first. 10x struct teamdata | |
|  | 0xf38c | 116 |  unused | |
|  | 0xf400 | 760 | peer play temporary data about peer | Y |
| IMG_CURRENT_PEER_POKEMON_ANIMATED_SMALL | 0xf400 | 384 |  medium pokemon animated image of pokemon we are peer-playing with (never erased) 32x24 x 2 frames | Y |
| TEXT_CURRENT_PEER_POKEMON_NAME | 0xf580 | 320 |  rendered text name of pokemon we are peer-playing with 80x16 | |
| CURRENT_PEER_DATA | 0xf6c0 | 56 |  data. struct peerplaydata | |

<!-- vim: nowrap
-->
