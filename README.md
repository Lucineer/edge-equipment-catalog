# Edge Equipment Catalog 📋

You run edge hardware. You've debugged why an agent works on one device but fails on others. The cause is rarely your code; it's undocumented hardware quirks, firmware versions, or resource limits that live only in team chat logs.

This catalog provides a shared, machine-readable specification for edge hardware and software. It replaces tribal knowledge with structured data you can use in your deployment tools.

---

## Why This Exists

Teams repeatedly build internal hardware registries. This rebuilds the same foundation: documenting USB bus limits, RAM overhead for specific OS images, and firmware compatibility. This project provides that common base so you don't have to.

---

## Quick Start

1.  **Fork this repository.** This is now your owned copy.
2.  Use the schemas. Deploy the `schemas/` folder to a static host or import the TypeScript types directly.
3.  Add your own hardware profiles and compatibility rules.

**Live Reference Catalog:**  
[https://the-fleet.casey-digennaro.workers.dev/catalog](https://the-fleet.casey-digennaro.workers.dev/catalog) (Hosted on Cloudflare Workers)

---

## What's Inside

*   **Typed Definitions:** Structured profiles for 50+ common edge device models (e.g., Raspberry Pi, Jetson, industrial gateways).
*   **Compatibility Checks:** Machine-readable rules to validate if an agent's requirements match a device's capabilities before deployment.
*   **Zero Dependencies:** Pure JSON Schema and TypeScript. No runtime services, daemons, or lock-in.
*   **Fork-First Workflow:** You control your catalog. Modify it freely and pull upstream updates when you choose.

---

## How It Works

A schema-first specification. The catalog defines the shape of equipment data and compatibility rules. Your schedulers, provisioning systems, or agent frameworks can load these schemas to validate workloads against hardware capabilities.

---

## Limitations

The base catalog is designed for general-purpose edge devices. **It does not model real-time performance characteristics or dynamic environmental constraints.** For example, it can specify a GPU exists but not its current thermal state or actual inference latency under load. You will add and maintain profiles for highly specialized or custom hardware.

---

## Extending

Add your equipment profiles under `schemas/equipment/` in your fork. You can pull updates from the upstream `main` branch to merge new base definitions when it suits your needs.

---

## Contributing

Corrections and additions for widely-used public hardware are welcome. Please open an issue to discuss significant changes.

## License

MIT License.

<div style="text-align:center;padding:16px;color:#64748b;font-size:.8rem"><a href="https://the-fleet.casey-digennaro.workers.dev" style="color:#64748b">The Fleet</a> &middot; <a href="https://cocapn.ai" style="color:#64748b">Cocapn</a></div>