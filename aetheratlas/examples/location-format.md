# Location display settings

Created by Steve Sheets on November 3, 2026.

These settings are supported by the AetherAtlas app and CLI. Use the latest
versions when authoring numbered HUD entries and location icons.

## Panorama coverage

A panorama's `media.degrees` is its horizontal angular coverage, greater than
zero and at most 360 degrees. When omitted, use
`details.defaults.panoramadegrees`, then the app default of 270.
Vertical coverage is derived from the image proportions.
The Yosemite Glacier Point example explicitly uses the agreed 270-degree
default; this is a display setting, not a verified measurement of the photograph.

## Background environment

Resolve `location.environment`, then `details.defaults.environment`, then
`{"type":"color","color":"black"}`.
An explicit environment replaces the entire inherited environment object.
Keep the background behind ordinary images, videos, and panoramas, including
panoramas with 360-degree horizontal coverage that leave vertical gaps.
Full spherical media obscures the background.

| type | Fields |
| --- | --- |
| passthrough | No additional fields |
| color | color: named color or opaque #RRGGBB |
| image360 | image: relative path or HTTP(S) URL |
| room | material: built-in material or image reference |
| cylinderroom | material: built-in material or image reference |

Preferred color names: black, charcoal, midnight, navy, teal, gray, white.
Exact named color values and the shipped material inventory will be established
in app code. Candidate built-ins include dark-walnut, blue-fabric,
brushed-steel, sandstone, and carrara-marble.

Material examples:

```json
{"type":"cylinderroom","material":{"builtin":"dark-walnut"}}
```

```json
{"type":"room","material":{"image":"materials/stone-wall.jpg"}}
```

A material specifies either builtin or image, not both. Image references can be
relative paths or HTTP(S) URLs. Resolve every relative resource against the
.aetheratlas descriptor URL, not the HTML page. Background colors are opaque.

## Shared defaults used by all three examples

```json
"defaults": {
  "panoramadegrees": 270,
  "environment": {
    "type": "color",
    "color": "black"
  }
}
```

Location overrides are optional. The current examples inherit black backgrounds;
Oz's full spherical images naturally cover them. DC Mall retains its external
public-domain image URLs.


## Numbered HUDs and location navigation

Use independent string fields `hud-1`, `hud-2`, `hud-3`, and so on in
`details.defaults` and/or a location. There is no fixed number of entries.
Numbers must be positive canonical decimal integers (`hud-1`, not `hud-01`).
Entries display in numeric order, so `hud-2` precedes `hud-10`; gaps are allowed.
Each location field replaces the same numbered default. Other numbered defaults
remain inherited. If neither scope supplies HUD entries, the app supplies exit
and list for compatibility with existing tours.

The app renders the effective entries using one upper-left action HUD, with
the helper's default maximum of three icons per row. The ordinary Home screen
uses its existing app UI.

| HUD value | Behavior | Icon |
| --- | --- | --- |
| `exit` | Ask whether to leave the map and return Home | `home` |
| `list` | Change Location popup with other locations' full `title` values | `list` |
| `shortlist` | Same popup, using `shortitle` where supplied and `title` otherwise | `list` |
| `mapswitch` | Toggle the map window; hide/collapse restores HUD in exclusive mode | `globe` |
| `loc location-key` | Go directly to that location | Location's `icon`, otherwise `play` |

`loc` has exactly one ASCII space before its target key. The target must exist
in the tour. The current location is excluded from list popups. A shortcut to
the current location is disabled. A list with no other locations is disabled.

### Exit text

Resolve `exitstring` from the location, then `details.defaults`, then the exact
app default `Do you wish to exit the map?`. No preserves the room and disk cache.
Yes returns to the app Home screen and removes the tour's cache files.

Oz uses:

```json
"defaults": {
  "hud-1": "exit",
  "hud-2": "shortlist",
  "exitstring": "Do you wish to leave the World of Oz?"
}
```

Existing panorama and environment defaults may appear in the same object.

### Optional location fields

```json
{
  "id": "emerald-city-gates",
  "title": "The Gates of Emerald City",
  "shortitle": "City Gates",
  "icon": "play",
  "hud-3": "loc glinda-palace",
  "media": {"type": "image360", "url": "emerald-city-gates-360.png"}
}
```

The field spelling is `shortitle`. It is human-readable text, not an identifier.
It does not change the full title shown by `list` or the HTML resource pages.
An optional `icon` is a portable name from the
[built-in icon catalog](https://github.com/Zodiac-Innovations/ZodiacInnovationsDevelopment/blob/main/Documentation/AetherCircle-Built-In-Icons.md). Direct location shortcuts
and other small location-icon presentations use it; media is unchanged.

### Keys and validation

Every JSON property name and identifier key must be a nonempty lowercase token
without whitespace, including spaces, tabs, line breaks, and Unicode whitespace.
This applies to atlas `id`, location `id`, `details.initiallocation`, creator IDs,
map-pin location references, and `loc` targets. Characters such as `-`, `+`, `_`,
and `.` are allowed. Examples: `magesteve.oz`, `city-gates`, `room_one+two`.
Human-readable titles, short titles, descriptions, and exit questions may contain
spaces. The space separating `loc` from its target is command syntax.

The app and CLI check numbered fields, string types, command syntax, existing
location targets, icon names, nonempty authored text, and identifier whitespace.
Encoding keeps numbered HUD entries as flat fields; it does not create a `hud`
object or array. All three published examples have been audited for this rule.


## Map window and HUD visibility

The Boolean keys `showhud`, `showmap`, and `hudormap` can be authored in
`details.defaults` or a location. Resolve each independently from location,
then defaults, then the built-in value below. File keys remain lowercase.

| Key | Default | Meaning |
| --- | --- | --- |
| `showhud` | `true` | Show the configured HUD when independent mode is active. |
| `showmap` | `false` | Show the map window expanded when entering the location. |
| `hudormap` | `false` | Alternate between expanded map and HUD instead of displaying both. |

With `hudormap: false`, `showhud` controls the HUD independently of map visibility.
With `hudormap: true`, an expanded map hides the HUD; hiding or collapsing the
map reveals the HUD. This exclusive-mode rule takes precedence over `showhud`.

Add `mapswitch` as a numbered HUD value to toggle the map. In independent mode,
a second tap hides the map again. In exclusive mode, use the map window's Hide
or Collapse control to get the HUD back. Tapping Map while the window is collapsed
expands it; the standard Restore control does the same.

Oz's test configuration includes:

```json
"defaults": {
  "showhud": true,
  "showmap": false,
  "hudormap": true,
  "hud-1": "exit",
  "hud-2": "shortlist",
  "hud-3": "mapswitch"
}
```

Existing panorama, environment, and exit-string defaults remain in the same
object. Location overrides are reapplied whenever that location is entered.

### Placeholder window

Only one movable map window can exist at a time. For this stage it contains
the text “Map” and an Exit button. Exit asks before leaving the tour and uses
the location/default `exitstring`. Its lower bar supplies the standard move,
Hide (Close/X), Collapse, and Restore controls. Hide affects this window only;
it does not exit the tour. Collapse keeps the restore bar visible. Window
placement is carried between locations during the tour and resets on Home.

Map images, pin navigation, and selecting among authored `maps[]` entries are
the next stage; the placeholder does not imply a working map-pin route.

### CLI navigation checks

`check` and the app loader resolve effective settings for every location.
They reject configurations where hiding the map leaves all navigation hidden.
In particular, `showhud: false` with `hudormap: false` is unsafe with this
hideable window, even when it starts visible. Use exclusive mode for a map-only
initial view with automatic HUD recovery.

For tours containing multiple locations, each location needs an outgoing HUD
route: `list`, `shortlist`, or `loc` pointing elsewhere. That HUD may be restored
by hiding the map in exclusive mode. `exit`, `mapswitch`, and a shortcut to the
current room do not supply an outgoing location route. Map pins are not counted
until their presentation is implemented.

Every room must also have a reachable exit, directly through `exit` or
`mapswitch` (which reaches the window's Exit button), or by navigating to a room
that provides one. Closed navigation cycles without an exit are rejected.
