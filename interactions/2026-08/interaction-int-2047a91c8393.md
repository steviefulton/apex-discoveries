# Novel interaction: rapamycin + creatine

APEX Orchestrator - Copyright 2026 Steven Charles Fulton. All rights reserved.
Licensed under BSL-1.1. Non-commercial use permitted. Commercial use requires a licence.
Contact: stevencharlesfulton (GitHub). Originated in Glasgow, Scotland.

- **Author:** Steven Charles Fulton
- **System:** APEX Orchestrator
- **Date:** 2026-08-16T06:10:31.039808+00:00
- **Severity:** low
- **Status:** needs_review

## Compounds
- A: rapamycin
- B: creatine

## Mechanism chain
rapamycin targets overlap with creatine targets at 2 shared proteins (avg STRING score 0.759). Direct shared targets: P08684.

## Pathway overlap
```json
{"shared_proteins": ["9606.ENSP00000498939", "CYP3A4"], "shared_count": 2, "direct_targets_shared": ["P08684"], "avg_string_score": 0.759, "targets_a": ["AAG24257", "P62942", "P42345", "P08684", "P31750", "P68106", "Q8TDX7", "Q9P1W9", "P41240", "O43781", "P27361", "Q9H422", "P45983", "O75582", "P53778", "P51812", "Q9NQU5", "P27448", "P11309", "Q14012"], "targets_b": ["P06732", "Q7Z2H8", "O76082", "P0DTD1", "Q9UBN7", "P48029", "P08684", "P10635", "P11712", "P33261", "P05177", "O75469", "P22303", "P0DMS8", "P35348", "P08913", "P10275", "P11229", "P08172", "P21728"]}
```

## FAERS signal
```json
{"both": 0, "a": 643, "b": 1051, "prr": null, "signal": "none"}
```

## Confidence
- Raw: 0.6636
- Calibrated: 0.6636

---
*SAFETY: Computational prediction only. Not clinical evidence.
Not medical advice. Critical findings require human review and
independent validation before any clinical action.*
