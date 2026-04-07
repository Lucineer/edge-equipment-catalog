# Edge Equipment Catalog

You run edge hardware. You know the quiet problem: agent workloads can fail because hardware capabilities aren't formally documented.

This catalog provides a shared, structured language for edge hardware and software within distributed fleets, reducing reliance on tribal knowledge.

## What this is
A repository of JSON schemas and TypeScript types that define edge equipment (sensors, gateways, SBCs) and their compatibility rules. Agents can use this to validate workloads against node capabilities. It's designed for the Cocapn Fleet agent runtime.

## What makes this different
- **Fork-first workflow:** You start by forking. Modify only what you need, merge upstream updates when it suits you.
- **No vendor lock:** It's a schema repository, not a service. There are no runtime dependencies.
- **Extensible base:** Designed for you to add custom device profiles and site-specific rules.

## Try the public catalog
A live reference deployment is available:
https://the-fleet.casey-digennaro.workers.dev/catalog

## Quick start
1. **Fork** this repository.
2. Deploy the `schemas/` directory to any static host or import the TypeScript types directly.
3. Extend the equipment profiles with your own hardware.

## Key features
* **Structured equipment definitions:** Machine-readable types for common edge devices.
* **Compatibility matrix:** Rules defining which agent software can run on which hardware.
* **Portable schemas:** Core definitions are JSON Schema and TypeScript types.
* **Base for extension:** Add your own device profiles and driver interfaces.

## Limitations
The base catalog covers common patterns. For highly specialized hardware, you will need to define and maintain your own profiles.

## Architecture
This is a schema-first specification. It defines the shape of equipment data and compatibility rules. Agents or schedulers can load these schemas to validate deployments.

## Extending the catalog
Fork the repository and add your equipment profiles under `schemas/equipment/`. You can update from the upstream main branch when you choose to pull in new base definitions.

## Contributing
Corrections and new equipment profiles for common hardware are welcome. Please open an issue to discuss significant changes first.

## License
MIT License.

Superinstance & Lucineer (DiGennaro et al.).

---

<div align="center">
  <a href="https://the-fleet.casey-digennaro.workers.dev">The Fleet</a> • <a href="https://cocapn.ai">Cocapn</a>
</div>