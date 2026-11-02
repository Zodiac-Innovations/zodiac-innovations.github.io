# AetherAtlas registry

`atlasmaps.json` is a JSON object whose unique keys are registered atlas IDs and whose values are direct HTTP(S) URLs to `.aetheratlas` descriptors.

`atlascreators/<registeredid>.html` lists a creator's registered maps. Map links target the descriptors directly.

In an atlas descriptor, optional `details.author.registeredid` identifies the registered creator. For Steve Sheets this is `magesteve`. Existing author information remains independent of registration.

When adding or changing a registration, update the map registry and the corresponding creator page together.

The public base URL is https://zodiacinnovations.com/registry/.

`atlascreators.json` contains registered creators in a `creators` array.

## Hosted examples

The complete example tours are maintained in [`/aetheratlas/examples/`](https://zodiacinnovations.com/aetheratlas/examples/), including HTML, descriptors, and images. The registry and creator pages point to these public website URLs.
