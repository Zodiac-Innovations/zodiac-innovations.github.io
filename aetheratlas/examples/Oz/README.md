# Oz

Created by Steve Sheets on November 1, 2026.

- [`oz.aetheratlas`](oz.aetheratlas): version-1 JSON tour descriptor, using the format checked by the AetherAtlas CLI.
- `classicmap.png`: the active map, referenced relative to the descriptor.
- `modernmap.png`: an alternate map image; change `map.image` to use it.

The tour contains all sixteen destination images below, starts in Munchkin Country, and follows their listed order for Previous/Next navigation. Map pins remain unassigned.

The map is a new AI-generated interpretation inspired by L. Frank Baum's 1900 *The Wonderful Wizard of Oz* and W. W. Denslow's illustrations. It is not an original historical map or a reproduction of book artwork. Its geography is illustrative rather than an authoritative reconstruction.

## Sample destinations

Sixteen AI-generated, book-inspired 360-style environment images (1774 × 887 pixels, 2:1) are referenced by `oz.aetheratlas`. Paths resolve relative to that file. The descriptor uses `formatversion: 1`, `initiallocation`, and `locations[].media` with `type: "image360"` and `url` fields.

| Location | Image |
| --- | --- |
| Munchkin Country | [munchkin-country-360.png](munchkin-country-360.png) |
| The Scarecrow’s Cornfield | [scarecrow-cornfield-360.png](scarecrow-cornfield-360.png) |
| The Tin Woodman’s Forest | [tin-woodman-forest-360.png](tin-woodman-forest-360.png) |
| The Lion’s Forest | [lion-forest-360.png](lion-forest-360.png) |
| The Great Ravine | [great-ravine-360.png](great-ravine-360.png) |
| The River Crossing | [river-crossing-360.png](river-crossing-360.png) |
| The Deadly Poppy Field | [poppy-field-360.png](poppy-field-360.png) |
| Emerald City Approach | [emerald-city-approach-360.png](emerald-city-approach-360.png) |
| The Gates of Emerald City | [emerald-city-gates-360.png](emerald-city-gates-360.png) |
| Emerald City Street | [emerald-city-street-360.png](emerald-city-street-360.png) |
| The Wizard’s Throne Room | [wizard-throne-room-360.png](wizard-throne-room-360.png) |
| The Wicked Witch’s Castle | [witch-castle-360.png](witch-castle-360.png) |
| The Fighting Trees | [fighting-trees-360.png](fighting-trees-360.png) |
| The Dainty China Country | [china-country-360.png](china-country-360.png) |
| The Hammer-Heads’ Hill | [hammer-heads-hill-360.png](hammer-heads-hill-360.png) |
| Glinda’s Palace | [glinda-palace-360.png](glinda-palace-360.png) |

These are artistic interpretations, not historical illustrations or photographs. Although generated for equirectangular viewing, spherical projection accuracy, edge continuity, and pole artifacts have not been verified in a VR viewer. They are suitable as initial sample assets, not validated seamless production panoramas. Map pins remain unassigned.

## Check the tour

From the AetherAtlasExamples repository root, after pulling the current files:

```sh
git pull
# Check the description only:
aetheratlas check Oz/oz.aetheratlas
# Check the description and every referenced asset in the folder:
aetheratlas validate Oz
```

Or let the tool discover the descriptor inside the Oz folder:

```sh
cd Oz
aetheratlas check
aetheratlas validate
```

The descriptor references sixteen location images and one active map (seventeen assets). The alternate map is available in the folder but is not referenced until selected. Validation checks referenced assets, not unused extra files or panorama quality.

## Maps

- **Classic Map** — `classicmap.png`
- **Modern Map** — `modernmap.png`

Both maps appear in the atlas’s `maps` array and in its HTML resource list.
