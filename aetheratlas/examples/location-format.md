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
| `list` | Title-free location popup with other locations' full `title` values | `list` |
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
| `showmap` | `false` | Initial map visibility from defaults; an explicit location value hides or expands the map on entry. |
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
object. An explicit location `showmap` is reapplied on entry; a default is only the\ninitial visibility, not a command to reset the user’s window on every move.

Default `showmap` establishes visibility when a tour opens. While moving between\nlocations, preserve the user’s expanded, hidden, or collapsed map state unless the\ndestination explicitly supplies `showmap`. A location `showmap: true` expands it;\n`false` hides it. Window position, rotation, and the selected map also carry over\n(subject to an authored `initialmap` override). A new tour starts from its own defaults.\n\n## Maps, initial selection, and marker names

Each `maps[]` object supports these fields:

| Key | Description |
| --- | --- |
| `id` | Optional stable lowercase map key; required when selecting it by `initialmap`. |
| `title` | Required display title, also used as the movable window's title. |
| `image` | Required relative path or public HTTP(S) image URL. |
| `pinstyle` | Default marker appearance: `pin`, `dot`, or legacy `flag`. |
| `pins` | Location markers with `location`, normalized `x`/`y`, optional `name` and `pinstyle`. |
| `locationname` | Optional Boolean, default `false`; displays each marker's title when true. |

Pin coordinates run from 0 to 1, starting at the image's upper-left corner.
Each pin targets an existing location. Its visible label uses `name` when supplied,
otherwise that location's full `title`. Built-in location icons are inherited;
otherwise a dot appears in the marker. `dot` selects a circle; `pin` and legacy
`flag` use the current teardrop marker. Markers have 3D depth over the flat image.
The marker for the current location is visible but disabled.

`details.initialmap` selects the first map by its `id`. When omitted, the first
array entry is selected, including for older map entries without IDs. Map IDs,
when supplied, must be unique and follow the normal single-token identifier rule.
`initialmap` may also appear in defaults or locations. A local value overrides a
default, and a default overrides the atlas-wide initial choice. Location/default
overrides are applied on entry; otherwise the user's selected map is retained
while moving between locations.

```json
"details": { "initialmap": "classic" },
"maps": [
  {"id":"classic", "title":"Classic Map", "image":"classicmap.png", "locationname":true},
  {"id":"modern", "title":"Modern Map", "image":"modernmap.png", "locationname":false}
]
```

The example above shows only the map-related fields; normal required atlas and
details fields still apply. Oz contains a pin for all 16 locations on each map;\nClassic starts first with labels on, and Modern has labels off. National Mall and\nYosemite use their existing NPS map images, with 11 and 8 destination pins\nrespectively, plus exit, shortlist, and mapswitch HUD defaults. Their map images\nalready contain labels, so marker labels are off. Pin positions indicate the\nlandmark/area; nearby photos of the same landmark have small display offsets.\nNASA Planets has no map image and keeps its exit/list HUD without mapswitch.

### Window controls

Only one map window is shown. Its content is an `AetherMapArrangement` containing
the selected image and tappable markers. It can be moved with the normal bar.

- Left, first: Home icon asks to leave the tour, using `exitstring` with no dialog title.
- Left, second: List icon opens the selected map's marked destinations using full location titles, with no popup title. Duplicate destinations appear once; the current location is omitted. The button is disabled when no other destinations remain.
- Right: Hide removes the window and restores the HUD in exclusive mode.
- Right, before Collapse: Play cycles first-to-last-to-first, shown only with multiple maps.
- Collapse/Restore changes the actual window state and updates HUD visibility accordingly.

HUD list and shortlist popups also have no title. Shortlist keeps its existing
`shortitle` fallback behavior; the map bar always uses full location names.

Map image files share the tour's disk cache. Play prepares the new image before
replacing the current map; failure keeps the existing image, title, and selection.
Only the displayed map image is registered in memory. Home clears that image
registration and removes the tour's descriptor/media/map cache directory.

### Navigation validation

`showmap: true` and authored `mapswitch` commands require at least one map.
The map bar provides navigation to the selected map's marked destinations and an exit.
Cycling maps refreshes the popup's destinations; an open map popup is rebuilt, and
obsolete menu actions are removed. HUD `list` and `shortlist` still cover all tour
locations. Navigation checks count the union of marker destinations across the
maps reachable through Play, so an empty map cannot invent a location route.
`mapswitch` also provides a route to Home. Checks still reject settings that
can hide all navigation or leave no reachable exit after the map is hidden.
Existing visibility inheritance and `hudormap` rules remain unchanged.
