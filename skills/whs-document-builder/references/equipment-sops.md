# Individual Equipment SOPs

One document per piece of equipment, each following an identical 9-section structure. This makes the library easy to extend — add a new SOP by providing the data arrays and calling the shared `doc()` function.

---

## The 9-Section SOP Structure

Every equipment SOP contains these sections in this order:

1. **Document Information** — version, dates, approved by, applies to, legislation, risk level, licence required
2. **Responsibilities** — employer obligations + employee/contractor obligations (both as bullet lists)
3. **Hazard Identification & Risk Controls** — 3-column table: hazard/risk | potential harm | control measures
4. **Personal Protective Equipment (PPE)** — 2-column table: PPE item | standard/specification
5. **Pre-Start Inspection Checklist** — tick-box table with inspector sign-off block
6. **Safe Operating Procedure** — numbered step-by-step instructions
7. **Maintenance & Inspection Schedule** — 3-column table: frequency | task | responsible
8. **Incident Reporting** — brief para with state regulator number
9. **Worker Acknowledgement** — 5-row sign-off table + management signature line

---

## Shared `doc()` Function Pattern

Build one shared function that takes data arrays and produces a complete SOP. This avoids repeating the 9-section structure 20+ times:

```javascript
function doc(title, code, riskLevel, licence, description, hazards, ppeRows, preStart, safeOp, maintenance, extra) {
  const riskColour = 
    riskLevel === "EXTREME" ? "CC0000" : 
    riskLevel === "HIGH"    ? "B85C00" : 
    riskLevel === "MEDIUM"  ? "B8860B" : C.PRIMARY;
  
  return new C.Document({
    numbering: C.numbering, styles: C.styles,
    sections: [{
      properties: C.pageProps,
      headers: { default: C.makeHeader(title, code) },
      footers: { default: C.makeFooter("1.0 | [Month Year]") },
      children: [
        // Section 1 — Document Info
        C.sectionBanner("Document Information"), C.emptyRow(100),
        // info table with risk level and licence rows...
        
        // Important notice banner (yellow background)
        
        // Section 2 — Responsibilities (heading2 + bullets for each party)
        
        // Section 3 — Hazard Table
        C.sectionBanner("3. Hazard Identification & Risk Controls"), C.emptyRow(80),
        hazardTable(hazards),
        
        // Section 4 — PPE Table
        C.sectionBanner("4. Personal Protective Equipment (PPE)"), C.emptyRow(80),
        ppeTable(ppeRows),
        
        // Section 5 — Pre-Start Checklist
        C.sectionBanner("5. Pre-Start Inspection Checklist"), C.emptyRow(80),
        preStartTable(preStart),
        
        // Section 6 — Safe Operating Procedure (numbered list)
        C.sectionBanner("6. Safe Operating Procedure"), C.emptyRow(80),
        ...safeOp.map(step => C.numbered(step)),
        
        // Section 7 — Extra content (warning banners etc.) if provided
        ...(extra || []),
        
        // Section 8 — Maintenance Table
        C.sectionBanner("7. Maintenance & Inspection Schedule"), C.emptyRow(80),
        maintenanceTable(maintenance),
        
        // Section 9 — Incident Reporting + Worker Acknowledgement
      ]
    }]
  });
}
```

### Hazard Table Helper

```javascript
function hazardTable(rows) {
  return new C.Table({
    width: { size: 9360, type: C.WidthType.DXA },
    columnWidths: [3120, 3120, 3120],
    rows: [
      C.tableHeader([["Hazard / Risk",3120],["Potential Harm",3120],["Control Measures",3120]]),
      ...rows.map(([haz, harm, ctrl], i) => new C.TableRow({
        children: [[haz,3120],[harm,3120],[ctrl,3120]].map(([t,w]) => new C.TableCell({
          width: { size: w, type: C.WidthType.DXA },
          shading: { fill: i % 2 === 0 ? C.GRAY : C.WHITE, type: C.ShadingType.CLEAR },
          margins: { top: 80, bottom: 80, left: 120, right: 120 }, borders: C.borders,
          children: [new C.Paragraph({ children: [new C.TextRun({ text: t, size: 19, font: "Arial", color: C.TEXT })] })]
        }))
      }))
    ]
  });
}
```

### PPE Table Helper

```javascript
function ppeTable(rows) {
  return new C.Table({
    width: { size: 9360, type: C.WidthType.DXA },
    columnWidths: [4680, 4680],
    rows: [
      C.tableHeader([["PPE Item",4680],["Standard / Specification",4680]]),
      ...rows.map(([item, spec], i) => new C.TableRow({
        children: [[item,4680],[spec,4680]].map(([t,w]) => new C.TableCell({
          width: { size: w, type: C.WidthType.DXA },
          shading: { fill: i % 2 === 0 ? C.GRAY : C.WHITE, type: C.ShadingType.CLEAR },
          margins: { top: 80, bottom: 80, left: 120, right: 120 }, borders: C.borders,
          children: [new C.Paragraph({ children: [new C.TextRun({ text: t, size: 19, font: "Arial", color: C.TEXT })] })]
        }))
      }))
    ]
  });
}
```

### Maintenance Table Helper

```javascript
function maintenanceTable(rows) {
  return new C.Table({
    width: { size: 9360, type: C.WidthType.DXA },
    columnWidths: [2000, 5160, 2200],
    rows: [
      C.tableHeader([["Frequency",2000],["Inspection / Maintenance Task",5160],["Responsible",2200]]),
      ...rows.map(([freq, task, resp], i) => new C.TableRow({
        children: [[freq,2000],[task,5160],[resp,2200]].map(([t,w]) => new C.TableCell({
          width: { size: w, type: C.WidthType.DXA },
          shading: { fill: i % 2 === 0 ? C.GRAY : C.WHITE, type: C.ShadingType.CLEAR },
          margins: { top: 80, bottom: 80, left: 120, right: 120 }, borders: C.borders,
          children: [new C.Paragraph({ children: [new C.TextRun({ text: t, size: 19, font: "Arial", color: C.TEXT })] })]
        }))
      }))
    ]
  });
}
```

---

## Equipment Data Format

Each SOP is defined as an array of data passed to the `doc()` function:

```javascript
const policies = [
  ["Chainsaw — Safe Operating Policy", "PREFIX-SOP-CS-001", "EXTREME",
    "AHCMOM213 or equivalent competency required. Tree felling may require additional licence.",
    "This policy establishes safe operating requirements for all chainsaw use...",
    // hazards: [hazard, harm, control] rows
    [
      ["Kickback","Severe laceration, amputation, fatality","Only trained operators; never cut with tip; sharp chain; kickback guard must function"],
      ["Falling trees and limbs","Crush injury, fatality","2x tree-height exclusion zone; planned escape routes; never fell toward personnel"],
      // ...
    ],
    // ppeRows: [item, standard] rows
    [
      ["Chainsaw chaps (cut-resistant)","Mandatory — Class 1+ to AS 4453.3"],
      ["Chainsaw helmet with face shield and hearing protection","Mandatory — full helmet unit"],
      // ...
    ],
    // preStart: string[] — one item per checklist row
    ["Chain tension correct — no sag but pulls freely by hand",
     "Chain oil reservoir is full",
     "Chain brake operates — push guard forward, chain stops",
     // ...
    ],
    // safeOp: string[] — numbered procedure steps
    ["NEVER operate without current, documented chainsaw competency — no exceptions",
     "Conduct site assessment — dead branches, lean direction, escape routes, power lines",
     // ...
    ],
    // maintenance: [frequency, task, responsible] rows
    [["Before each use","Chain tension, oil, brake, fuel, guards, throttle","Operator"],
     ["Weekly","Chain sharpness, bar wear, fasteners, anti-vibration mounts","Supervisor"],
     ["Annually","Full professional service — chain, bar, sprocket, anti-vibration system","Management"]],
    // extra: additional content (warning banners etc.)
    [C.warningBanner("CHAINSAW KICKBACK IS INSTANTANEOUS AND FATAL...", "CC0000"), C.emptyRow(80)],
    // filename
    "PREFIX_SOP_Chainsaw.docx"
  ],
  // ... more equipment ...
];
```

---

## Risk Level Assignment

Assign risk level based on potential consequence and frequency of use on the trade's typical sites:

| Level | Colour | Typical equipment |
|-------|--------|-------------------|
| EXTREME | Red `CC0000` | Chainsaw, stump grinder, oxy-acetylene, angle grinder (cutting) |
| HIGH | Orange `B85C00` | Ride-on mower, excavator, angle grinder (grinding), circular saw, brush cutter, ladders, plate compactor, auger |
| MEDIUM | Amber `B8860B` | Hedger, push mower, backpack blower, jigsaw, cement mixer, turf cutter |
| LOW | Brand primary | Hand blower, tape measures, spirit levels, basic hand tools |

---

## Content Sourcing from Existing Policies

If the client has an existing machinery policy, extract equipment-specific content from it:

```bash
extract-text /mnt/user-data/uploads/Client_WHS_Policy.docx
```

Then for each piece of equipment listed:
- Use the existing **Key Hazards** as the basis for the hazards table
- Use the existing **Control Measures** as the starting point for hazard controls and safe operating steps
- Expand substantially — existing policies are usually brief summaries; SOPs should be detailed enough to induct a new worker
- Add missing hazards that the existing policy didn't cover
- Keep the client's exact terminology for equipment names

---

## Landscaping Equipment Reference List

The 22 equipment SOPs built for Brazier's Landscaping cover these tools (adapt for other landscaping clients):

**Company-owned equipment:** Hedger, Brush Cutter, Push Mower, Ride-On Mower, Backpack Blower, Hand Blower, Pole Hedger/Saw, Chainsaw, Circular Saw, Jigsaw, Angle Grinder, Hammer Drill, Ladders

**Hired equipment:** Excavator/Digger, Dumper, Plate Compactor, Brick/Paver Saw, Cement Mixer, Auger, Turf Cutter, Rotary Hoe, Stump Grinder

For other trades, the equipment list changes but the 9-section structure and `doc()` function pattern remain identical.

---

## Batch Generation

Use `Promise.all()` to generate all SOPs simultaneously:

```javascript
const jobs = policies.map(([title, code, risk, licence, desc, hazards, ppeRows, preStart, safeOp, maintenance, extra, filename]) =>
  C.Packer.toBuffer(doc(title, code, risk, licence, desc, hazards, ppeRows, preStart, safeOp, maintenance, extra))
    .then(buf => {
      C.fs.writeFileSync(`/mnt/user-data/outputs/${filename}`, buf);
      process.stdout.write(`✓ ${filename}\n`);
    })
);

Promise.all(jobs)
  .then(() => console.log(`\nAll ${policies.length} SOPs complete.`))
  .catch(e => console.error('ERROR:', e.message));
```

---

## Validation

Always validate every SOP after generation:

```bash
for f in /mnt/user-data/outputs/PREFIX_SOP_*.docx; do
  result=$(python /mnt/skills/public/docx/scripts/office/validate.py "$f" 2>&1)
  if echo "$result" | grep -q "PASSED"; then echo "✓ $(basename $f)"
  else echo "✗ $(basename $f)"; echo "$result" | grep "error" | head -3; fi
done
```

The most common failures and their fixes:
- `w:pgNum` in footer → replace `PageNumberElement` with `SimpleField` (see common module)
- Hex colour length error → remove opacity suffix from hex values (must be exactly 6 characters)
- Table rendering issues → ensure both `columnWidths` on table AND `width` on every cell are set
