# Structures

## Walker

```c
/*
 *  size: 0x68 = 104 bytes
 *  Dmitry: struct IdentityData
 */
typedef struct {
    /* +0x00 */ uint32_t le_unk0;
    /* +0x04 */ uint32_t le_unk1;
    /* +0x08 */ uint16_t le_unk2;
    /* +0x0a */ uint16_t le_unk3;
    /* +0x0c */ uint16_t le_tid;
    /* +0x0e */ uint16_t le_sid;
    /* +0x10 */ unique_identity_data_t identity_data;
    /* +0x38 */ event_bitmap_t event_bitmap;
    /* +0x48 */ uint16_t le_trainer_name[8];
    /* +0x58 */ uint8_t unk4[3];
    /* +0x5b */ uint8_t flags;
    /* +0x5c */ uint8_t protocol_ver;
    /* +0x5d */ uint8_t unk5;
    /* +0x5e */ uint8_t protocol_subver;
    /* +0x5f */ uint8_t unk8;
    /* +0x60 */ uint32_t be_last_sync;
    /* +0x64 */ uint32_t be_step_count;
} walker_info_t;

/*
 *  size: 0x18 = 24 bytes
 *  dmitry: struct HealthData
 */
typedef struct {    // strut HealthData
    /* +0x00 */ uint32_t be_total_steps;
    /* +0x04 */ uint32_t be_today_steps;
    /* +0x08 */ uint32_t be_last_sync;
    /* +0x0c */ uint16_t be_total_days;
    /* +0x0e */ uint16_t be_current_watts;
    /* +0x10 */ uint8_t unk[4];
    /* +0x14 */ uint8_t padding[3];
    /* +0x17 */ uint8_t settings;
} health_data_t;

/*
 *  size: 64 bytes
 *  dmitry: struct LcdConfigCmds
 */
typedef struct {
    uint8_t flags;
    uint8_t commands[0x3f];
} lcd_config_t;

/*
 *  size: 256 bytes
 *  dmitry: RELIABLE_DATA
 */
typedef struct {
    uint8_t factory_data[3];
    unique_identity_data_t unique_data;
    uint8_t padding_1;
    lcd_config_t lcd_config;
    uint8_t padding_2; // random 0xbf byte?
    walker_info_t walker_info;
    uint8_t padding_3;
    health_data_t health_data;
    uint8_t padding_4;
    uint8_t copy_marker;
    uint8_t padding[16];
} reliable_data_t;

/*
 *  size: 1 byte
 *
 */
typedef struct {
    uint8_t stamp_heart: 1;
    uint8_t stamp_space: 1;
    uint8_t stamp_diamond: 1;
    uint8_t stamp_club: 1;
    uint8_t special_map: 1;
    uint8_t event_pokemon: 1;
    uint8_t evet_item: 1;
    uint8_t special_route: 1;
} special_inventory_t;

```

## Pokemon

```c
/*
 *  size: 0x38 = 56 bytes
 *  dmitry: struct PeerPlayData
 */
typedef struct {    // Dmitry struct PeerPlayData
    /* +0x00 */ uint32_t le_current_steps;
    /* +0x04 */ uint16_t le_current_watts;
    /* +0x06 */ uint8_t padding[2];
    /* +0x08 */ uint32_t le_unk0;
    /* +0x0c */ uint16_t le_unk2;
    /* +0x0e */ uint16_t le_species;
    /* +0x10 */ uint16_t pokemon_name[11];
    /* +0x26 */ uint16_t trainer_name[8];
    /* +0x36 */ uint8_t pokemon_flags_1;
    /* +0x37 */ uint8_t pokemon_flags_2;
} peer_play_data_t;

/*
 *  size: 16 bytes
 *  dmitry: struct PokemonSummary
 */
typedef struct {
    /* +0x00 */ uint16_t le_species;
    /* +0x02 */ uint16_t le_held_item;
    /* +0x04 */ uint16_t le_moves[4];
    /* +0x0c */ uint8_t level;
    /* +0x0d */ uint8_t pokemon_flags_1;    // [0..5] = variant (spinda, arceus, unown, etc.) [6] = female
    /* +0x0e */ uint8_t pokemon_flags_2;    // [0] = has form, [1] = shiny
    /* +0x0f */ uint8_t padding;
} pokemon_summary_t;

/*
 *  size: 0x38 = 56 bytes
 *  dmitry: struct TeamPokeData
 */
typedef struct {
    /* +0x00 */ uint16_t le_species;
    /* +0x02 */ uint16_t le_held_item;
    /* +0x04 */ uint16_t le_moves[4];
    /* +0x0c */ uint16_t le_ot_tid;
    /* +0x0e */ uint16_t le_ot_sid;
    /* +0x10 */ uint32_t le_pid;
    /* +0x14 */ uint32_t ivs;   // packed to 5-bits each
    /* +0x18 */ uint8_t evs[6];
    /* +0x1e */ uint8_t pokemon_flags_1;
    /* +0x1f */ uint8_t source_game;
    /* +0x20 */ uint8_t ability;
    /* +0x21 */ uint8_t happiness;
    /* +0x22 */ uint8_t level;
    /* +0x23 */ uint8_t padding;
    /* +0x24 */ uint16_t nickname[10];
} pokemon_info_t;

/*
 *  size: 0x2c = 44 bytes
 *  dmitry: struct EventPokeExtraData
 */
typedef struct {
    /* +0x00 */ uint32_t le_unk0;
    /* +0x04 */ uint16_t le_ot_tid;
    /* +0x06 */ uint16_t le_ot_sid;
    /* +0x08 */ uint16_t le_unk1;
    /* +0x0a */ uint16_t le_location_met;
    /* +0x0c */ uint16_t le_unk2;
    /* +0x0e */ uint16_t ot_name[8];
    /* +0x1e */ uint8_t encounter_type;
    /* +0x1f */ uint8_t ability;
    /* +0x20 */ uint16_t le_pokeball_item;
    /* +0x22 */ uint8_t unk3[10];
} special_pokemon_info_t;

```
Note: `special_pokemon_info_t` cannot contain PID because ability, gender, shininess, spinda spots all depend on PID.
So PID must be generated/inferred from the data given.
Unsure if it's random each time, or if the same gifted pokemon gives the same PID.
The `le_unk1` and `le_unk2` change PV somehow, since I can see nature and characteristics changes.
Nothing seems to grant ribbons, marks or pokerus.

## Routes

```c

/*
 *  size: 0xbe = 190 bytes
 *  dmitry: struct RouteInfo
 */
typedef struct {
    /* +0x00 */ pokemon_summary_t pokemon_summary;
    /* +0x10 */ uint16_t pokemon_nickname[11];
    /* +0x26 */ uint8_t pokemon_happiness;
    /* +0x27 */ uint8_t route_image_index;
    /* +0x28 */ uint16_t route_name[21];
    /* +0x52 */ pokemon_summary_t route_pokemon[3];
    /* +0x82 */ uint16_t le_route_pokemon_steps[3];
    /* +0x88 */ uint8_t route_pokemon_percent[3];
    /* +0x8b */ uint8_t padding;
    /* +0x8c */ uint16_t le_route_items[10];
    /* +0xa0 */ uint16_t le_route_item_steps[10];
    /* +0xb4 */ uint8_t route_item_percent[10];
} route_info_t;

typedef struct {
    uint8_t item_info[6];
    uint8_t route_image_index;
    uint8_t padding_1;
    pokemon_summary_t special_pokemon;
    special_pokemon_info_t special_pokemon_extra;
    uint16_t le_special_pokemon_steps;
    uint8_t special_pokemon_percent;
    uint8_t padding_2;
    uint16_t le_special_item;
    uint16_t le_special_item_steps;
    uint8_t special_item_percent;
    uint8_t padding_3[3];
    uint16_t route_name[21];
    uint8_t pokemon_event_number;
    uint8_t item_event_number;
    uint8_t special_pokemon_sprite_data[0x170]; // should be 0x180 bytes, truncated
    uint8_t special_pokemon_name_image[0x140];
    uint8_t special_area_icon[0xc0];
    uint8_t special_area_name_image[0x140];
    uint8_t special_item_name_image[0x180];
} special_route_info_t;
```