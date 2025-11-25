---
title: "EDNA – Engineering Design Lineage CLI"
description: "EDNA (eng-dna) tags engineering artefacts with stable DNA tokens and tracks their lineage in a local SQLite store."
---

![EDNA Whiteboard Diagram](/media/edna_whiteboard_image.png)

EDNA (`eng-dna`) is a lightweight CLI that gives engineers a portable, file-system-native memory of how designs, scripts, simulations, and reports evolve. It tags artefacts with stable DNA tokens, writes sidecars instead of mutating source files, and keeps lineage in a local SQLite database—no server required.

**At a glance**

- **What it is:** CLI for engineering lineage with stable DNA tokens and a local SQLite memory store.  
- **Who it’s for:** Engineers, analysts, and simulation teams working in file/Git-centric workflows.  
- **Why it exists:** Traditional PLM/PDM is heavy; EDNA makes lineage as simple as running a CLI in your repo.  
- **Key capabilities:** Tag artefacts, link derivations, trace and graph lineage, export/import bundles, repair sidecars.

## Contents

{{< toc >}}

## Why EDNA? The market gap it fills

- PLM/PDM systems are heavy, centralised, and slow to fit quick local workflows.  
- Most teams reconstruct history from ad-hoc folders, filenames, and chat/email trails.  
- There’s rarely a lightweight, CLI-first way to tag artefacts with stable IDs, capture `derived_from`/`validated_by` lineage across CAD/CFD/FEA/scripts/reports, and ship that lineage with a project via Git—without a server.  
- EDNA is a “Git-like” lineage CLI: local, SQLite-backed, scriptable, and portable via JSON export/import bundles.

## How EDNA works (high level)

- **DNA tokens:** Immutable IDs assigned per artefact; moving or renaming files does not change the DNA.  
- **SQLite lineage store:** `eng_dna.db` holds artefacts, edges, tags, notes, events, and projects.  
- **Sidecars + embedded markers:** Most formats use `filename.ext.edna` JSON sidecars (DNA, hash, type, path). Some text formats can embed metadata in comments, but sidecars keep hashes honest by default.  
- **Lineage graph:** Nodes are artefacts; edges capture relations like `derived_from`, `uses`, `validated_by`, and `supersedes`.  
- **Events:** `created`, `derived`, `moved`, `versioned`, `sidecar_restored`, etc., log the story over time.

## Core workflows

**Initialise a workspace and project**

```bash
edna init
edna project add demo --name "Demo Build"
```

**Tag files with context**

```bash
edna tag cad/wing.sldprt --type cad --project aero --tag baseline --description "Initial loft"
edna tag reports/loads.md --type report --tag review --project aero
edna tag scripts/cleanup.py --type script --tag util
```

**Link derived artefacts**

```bash
edna link results/run1_pressures.csv \
  --from cfd/run1.jou \
  --relation derived_from \
  --reason "post-processing"
```

**Trace lineage**

```bash
edna trace results/run1_pressures.csv
```

**Visualise the graph**

```bash
edna graph cad/wing.sldprt --scope full --direction LR > lineage.mmd
```

**Export/import portable lineage**

```bash
edna export --project demo --output edna_lineage_demo.json
edna import edna_lineage_demo.json --dry-run
edna import edna_lineage_demo.json
```

**Repair and rescan**

```bash
edna rescan ./workspace
```

## Detailed CLI usage

### Search across tags, types, and projects

```bash
edna search --tag baseline --project aero
edna search --type cad
```

### Correct mistaken lineage

```bash
# Remove an incorrect derived_from edge but keep an audit trail
edna unlink results/run1_pressures.csv \
  --from cfd/run1.jou \
  --relation derived_from

# Preview unlinking without DB changes
edna unlink results/run1_pressures.csv \
  --from cfd/run1.jou \
  --relation derived_from \
  --dry-run
```

### Day in the life – designer iterating a geometry

```bash
edna init
edna project add aero --name "Wing Study"
edna tag cad/wing.sldprt --type cad --tag v1 --project aero
edna tag cfd/mesh.jou --type cfd --project aero --tag v1
edna link cfd/mesh.jou --from cad/wing.sldprt --relation derived_from --reason "meshing"

# Designer tweaks geometry (new version auto-linked)
edna tag cad/wing.sldprt --type cad --tag v2
edna tag cfd/mesh.jou --type cfd --tag v2 --force-overwrite
edna link cfd/mesh.jou --from cad/wing.sldprt --relation derived_from --reason "meshing update"

edna graph cad/wing.sldprt --scope full --direction LR > lineage.mmd
```

### Pathology and recovery

```bash
# Missing sidecar → automatic recreation on show/tag/rescan
rm reports/day1.md.edna
edna show reports/day1.md

# Retagging identical content → same DNA
edna tag reports/day1.md --tag review

# Force hash overwrite when needed
edna tag reports/day1.md --force-overwrite --description "hotfix without new version"

# Work-in-progress saves keep the same DNA but log wip events
edna tag reports/day1.md --mode wip

# Corrupted sidecar is ignored and repaired
printf '{bad json' > reports/day1.md.edna
edna show reports/day1.md
```

### Lineage diagram (conceptual)

```
Projects: [aero]

edna_parent1 ---- derived_from ----> edna_childA
     |                                   |
     | linked (relation=notes)           | derived_from
     v                                   v
  events (created, moved, versioned)  edna_childB
```

### Export, trace, and graph

```bash
edna trace results/new.csv
edna graph results/new.csv > lineage.mmd
edna graph results/new.csv --format dot --scope full --direction LR > lineage.dot

edna export --project demo --output edna_lineage_demo.json
edna import edna_lineage_demo.json --dry-run
edna import edna_lineage_demo.json
```

### Project management

```bash
edna project add demo --name "Demo Build"
edna project list
edna project show demo
edna project files demo
edna project delete demo --dry-run
edna project delete demo --force
edna project delete demo --purge-sidecars --dry-run
edna project delete demo --purge-sidecars --force
```

### Rescanning and repair

```bash
edna rescan ./workspace
```

### FAQ

- **Why do DNA tokens never change?** They are immutable IDs; new content creates a new DNA linked to its parent so history stays auditable.  
- **Why is embedding metadata disabled by default?** Embedded markers would mutate files and hashes; sidecars keep content untouched until hashing can ignore embedded markers.  
- **How does EDNA behave when files move?** On show/tag/rescan, EDNA normalises the new path, rewrites the sidecar, and logs a `moved` event without changing DNA.  
- **How does EDNA detect versions?** A hash mismatch creates a new artefact and `derived_from` edge unless `--force-overwrite` is provided.

## Learn more

- Source: [eng-dna on GitHub](https://github.com/JamiA97/eng-dna)  
- Design notes: [`Background.md`](https://github.com/JamiA97/eng-dna/blob/main/Background.md) and [`USAGE.md`](https://github.com/JamiA97/eng-dna/blob/main/USAGE.md)

[Back to top](#contents)
