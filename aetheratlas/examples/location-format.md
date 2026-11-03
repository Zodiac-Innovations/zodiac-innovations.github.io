# Location display settings

Created by Steve Sheets on November 3, 2026.

These format additions are recorded in the examples ahead of the app and CLI
implementation. Existing validators may reject the new fields until updated.

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
