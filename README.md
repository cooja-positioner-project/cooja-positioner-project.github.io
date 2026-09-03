# Cooja Positioner — Live Application

Cooja Positioner is a client-side, browser-based editor for authoring geographically grounded WSN and IoT scenarios and exporting simulator-specific artifacts.

- **Live application:** https://cooja-positioner-project.github.io/
- **Source, tests, and reproducibility artifacts:** https://github.com/cooja-positioner-project/cooja-positioner

No application server is required. Leaflet is bundled locally; OpenStreetMap tiles and Nominatim place search still require network access.

## Current features

### Geographic scenario authoring

- Manual point placement with stable node identifiers.
- Static multi-node deployments and timestamped mobile routes.
- Approximately hexagonal polygon deployments and horizontal/vertical scan paths.
- First-waypoint or explicit WGS84 origin selection.
- WGS84 → ECEF → local ENU conversion independent of map pan and zoom.
- Session persistence, duplicate-ID guards, undo/redo, multi-selection, keyboard shortcuts, and Cooja position import.

### Mobility and position exports

| Target | Generated artifact | Supported scope |
|---|---|---|
| Cooja Mobility | `positions.dat` | Planar mobile trace with explicit zero-based mote-array indexing and cyclic-plugin warnings. |
| ns-2 | `mobility-ns2.tcl` | Static initialization and planar `setdest` mobility. |
| ns-3 | `mobility-ns3.tcl` | Static and planar mobile input for `Ns2MobilityHelper`. |
| INET/OMNeT++ | `mobility-bonnmotion.movements` | Planar BonnMotion `t x y` triplets. |

Each adapter validates its own dimensionality, identifier, and timing restrictions before export. A file-format check is not presented as proof of compatibility with arbitrary simulator releases.

### Provenance-aware propagation initialization

The bundled propagation catalog represents five deployment-environment families plus a free-space baseline through six cited profile definitions: urban open-street LoS, urban street-canyon NLoS, 2.4-GHz agriculture, forest/vegetation, 2.4-GHz coastal over-water, and Friis free space. Profiles retain their source, frequency applicability, required context, caveats, parameter origins, and BibTeX provenance.

The shared adapter library assesses Cooja, ns-2, ns-3, and INET mappings as `native`, `parameterized-approximation`, or `unsupported`. Unsupported is an evidence-preserving result: the code does not invent a coefficient when required environmental or calibration inputs are absent. Geometry/mobility and propagation remain separate artifacts so that a geographic scenario can be reused under controlled radio assumptions without implying cross-simulator equivalence.

The current live interface provides a dedicated Cooja workflow with 18 cited condition-level choices. A user selects an existing researcher-created `.csc`; the browser replaces its radio medium with an inspectable LogisticLoss configuration and updates the simulation seed while preserving mote types, firmware references, positions, Mobility and ScriptRunner settings, and unrelated plugins. Browser permissions produce a downloaded `*-pathloss.csc` rather than silently overwriting the selected file.

Cooja's `transmitting_range` is both a strict candidate-receiver cutoff and the distance at which mean RSSI is anchored to `rx_sensitivity`; it is not a simple antenna-range control. Likewise, source-reported spatial shadowing is not automatically equivalent to Cooja's independent per-reception AWGN term. These settings are literature-informed initialization assumptions, not automatic environment detection, site calibration, or guaranteed packet reception.

## Verification boundary

The source artifact records successful loading of 13 pinned propagation configurations: three Cooja, three ns-2.35, four ns-3.47, and three OMNeT++ 6.4/INET 4.7 cases. This verifies configuration loading and model instantiation in those versions, not field accuracy or numerical equivalence across simulators.

A deterministic Zolertia Z1 characterization executed 216 Cooja scenarios: 18 condition-level profiles × two RX anchors × six distances from 10 to 60 m. All cases completed. Of 108 paired RX-anchor comparisons, 107 produced identical PRR and conditional RSSI; the sole difference, Forest Guava at 20 m, resulted from the strict LogisticLoss candidate cutoff. The sweep uses one seed and includes source-domain extrapolations, so it is reported as simulator-model characterization rather than inferential or hardware-calibration evidence.

Detailed equations, mappings, source citations, automated tests, pinned-toolchain reports, and evidence limitations are maintained in the [anonymous source repository](https://github.com/cooja-positioner-project/cooja-positioner).

## Quick use

1. Open the [live application](https://cooja-positioner-project.github.io/).
2. Choose a static or mobile scenario and a point or polygon workflow.
3. Select the local-origin policy and author or paste the geographic positions.
4. Select Cooja, ns-2, ns-3, or INET/OMNeT++ and generate the mobility/position artifact.
5. Optionally enable the Cooja propagation panel, select a cited condition, review its assumptions, choose an existing `.csc`, and download the preserved LogisticLoss version.

Keep the visible OpenStreetMap attribution when capturing or redistributing map-based screenshots or videos.

## Anonymous citation

> Cooja-Positioner Project, “Cooja-Positioner: A provenance-aware web-based scenario synthesizer for geo-grounded WSN simulations in Cooja, ns-2, ns-3, and OMNeT++,” software and reproducibility artifact, 2026. Source: https://github.com/cooja-positioner-project/cooja-positioner. Live application: https://cooja-positioner-project.github.io/

License and complete attribution information are provided in the [source repository](https://github.com/cooja-positioner-project/cooja-positioner).
