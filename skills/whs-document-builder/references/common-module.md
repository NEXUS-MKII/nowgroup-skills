# Common Module Reference

The common module (`common_[client].js`) is a shared Node.js file that all document builder scripts `require()`. It must be created and validated before writing any documents.

---

## Colour Extraction from Logo

Always extract colours from the client's logo before defining the common module:

```python
from PIL import Image
import numpy as np
from collections import Counter

img = Image.open('/mnt/user-data/uploads/logo.png').convert('RGBA')
arr = np.array(img)

# Only non-transparent pixels
mask = arr[:,:,3] > 200
rgb = arr[mask][:,:3]

# Quantize to groups of 16 for clustering
quant = [(int(p[0])//16*16, int(p[1])//16*16, int(p[2])//16*16) for p in rgb]
top = Counter([f'{r:02X}{g:02X}{b:02X}' for r,g,b in quant]).most_common(15)

# Print with classification
for hex_col, count in top:
    r = int(hex_col[0:2],16); g = int(hex_col[2:4],16); b = int(hex_col[4:6],16)
    kind = 'GREEN' if g > r*1.1 else ('DARK' if r+g+b < 150 else 'WHITE/LIGHT')
    print(f'  #{hex_col}  ({count}px) {kind}')
```

From the output, identify:
- **Primary brand colour** — the dominant non-white colour for section banners and header backgrounds
- **Secondary/accent colour** — a lighter or brighter variant for highlights
- **Light tint** — a very pale version for alternate table rows

---

## Logo Base64 Module

```python
import base64
with open('/mnt/user-data/uploads/logo.png', 'rb') as f:
    data = base64.b64encode(f.read()).decode()
with open('/home/claude/whs/logo_b64.js', 'w') as f:
    f.write(f'module.exports = "{data}";')
```

Get correct pixel dimensions for header scaling:
```python
from PIL import Image
img = Image.open('/mnt/user-data/uploads/logo.png')
w, h = img.size
target_w = 162  # target width in pixels at 96dpi — adjust to fit header row
target_h = int(h / w * target_w)
print(f'Logo transformation: width={target_w}, height={target_h}')
```

---

## Full Common Module Template

Save as `/home/claude/whs/common_[client].js`:

```javascript
const {
  Document, Packer, Paragraph, TextRun, Table, TableRow, TableCell,
  Header, Footer, AlignmentType, HeadingLevel, BorderStyle, WidthType,
  ShadingType, VerticalAlign, SimpleField, LevelFormat, PageBreak, ImageRun
} = require('docx');

// ── Brand Colours ─────────────────────────────────────────────────────────────
// Replace with colours extracted from the client's logo
const PRIMARY   = "1B3A6B";   // header backgrounds, section banners
const ACCENT    = "2E5FAC";   // sub-banners, highlights
const LIGHT     = "D6E4F7";   // alternate table rows, info cells
const WHITE     = "FFFFFF";
const GRAY      = "F2F4F7";   // default table cell fill
const TEXT      = "2D2D2D";   // body text
const SUBTEXT   = "555555";   // footer, secondary text

// ── Logo ─────────────────────────────────────────────────────────────────────
const LOGO_B64 = require('./logo_b64');
const LOGO_BUF = Buffer.from(LOGO_B64, 'base64');
const LOGO_W   = 162;  // pixel width — adjust to fit header
const LOGO_H   = 59;   // pixel height — maintain aspect ratio

// ── Borders ──────────────────────────────────────────────────────────────────
const border    = { style: BorderStyle.SINGLE, size: 1, color: "CCCCCC" };
const borders   = { top: border, bottom: border, left: border, right: border };
const noBorder  = { style: BorderStyle.NONE, size: 0, color: WHITE };
const noBorders = { top: noBorder, bottom: noBorder, left: noBorder, right: noBorder };

// ── Header ───────────────────────────────────────────────────────────────────
// Three-column layout: logo | company subtitle | doc code
function makeHeader(title, docCode) {
  return new Header({
    children: [
      new Table({
        width: { size: 9360, type: WidthType.DXA },
        columnWidths: [2400, 4560, 2400],
        borders: {
          top: noBorder, left: noBorder, right: noBorder,
          insideH: noBorder, insideV: noBorder,
          bottom: { style: BorderStyle.SINGLE, size: 12, color: ACCENT }
        },
        rows: [new TableRow({ children: [
          // Logo
          new TableCell({
            borders: noBorders,
            width: { size: 2400, type: WidthType.DXA },
            margins: { top: 40, bottom: 40, left: 0, right: 120 },
            children: [new Paragraph({ children: [new ImageRun({
              data: LOGO_BUF,
              transformation: { width: LOGO_W, height: LOGO_H },
              type: "png"
            })] })]
          }),
          // Centre — subtitle
          new TableCell({
            borders: noBorders,
            width: { size: 4560, type: WidthType.DXA },
            verticalAlign: VerticalAlign.CENTER,
            margins: { top: 40, bottom: 40, left: 120, right: 120 },
            children: [new Paragraph({
              alignment: AlignmentType.CENTER,
              children: [new TextRun({ text: "WH&S Management System", size: 20, color: SUBTEXT, font: "Arial", italics: true })]
            })]
          }),
          // Doc code
          new TableCell({
            borders: noBorders,
            width: { size: 2400, type: WidthType.DXA },
            verticalAlign: VerticalAlign.CENTER,
            margins: { top: 40, bottom: 40, left: 120, right: 0 },
            children: [new Paragraph({
              alignment: AlignmentType.RIGHT,
              children: [new TextRun({ text: docCode, size: 18, color: ACCENT, font: "Arial", bold: true })]
            })]
          })
        ]})]
      }),
      // Title banner
      new Table({
        width: { size: 9360, type: WidthType.DXA }, columnWidths: [9360],
        rows: [new TableRow({ children: [new TableCell({
          width: { size: 9360, type: WidthType.DXA },
          shading: { fill: PRIMARY, type: ShadingType.CLEAR },
          margins: { top: 100, bottom: 100, left: 180, right: 180 },
          borders: noBorders,
          children: [new Paragraph({ children: [new TextRun({ text: title, bold: true, size: 26, color: WHITE, font: "Arial" })] })]
        })] })]
      })
    ]
  });
}

// ── Footer ────────────────────────────────────────────────────────────────────
// CRITICAL: Use SimpleField for page numbers — NOT PageNumberElement
// PageNumberElement generates invalid <w:pgNum> that Word rejects
function makeFooter(version) {
  return new Footer({
    children: [new Paragraph({
      border: { top: { style: BorderStyle.SINGLE, size: 6, color: ACCENT, space: 4 } },
      children: [
        new TextRun({ text: "[Company Name]  |  Version: " + version + "  |  Review: Annual  |  Page ", size: 17, color: SUBTEXT, font: "Arial" }),
        new SimpleField({ instruction: "PAGE", cachedValue: "1" }),
        new TextRun({ text: " of ", size: 17, color: SUBTEXT, font: "Arial" }),
        new SimpleField({ instruction: "NUMPAGES", cachedValue: "1" })
      ]
    })]
  });
}

// ── Section Banners ───────────────────────────────────────────────────────────
function sectionBanner(text) {
  return new Table({
    width: { size: 9360, type: WidthType.DXA }, columnWidths: [9360],
    rows: [new TableRow({ children: [new TableCell({
      width: { size: 9360, type: WidthType.DXA },
      shading: { fill: PRIMARY, type: ShadingType.CLEAR },
      margins: { top: 100, bottom: 100, left: 160, right: 160 },
      borders: noBorders,
      children: [new Paragraph({ children: [new TextRun({ text, bold: true, size: 22, color: WHITE, font: "Arial" })] })]
    })] })]
  });
}

function subBanner(text) {
  return new Table({
    width: { size: 9360, type: WidthType.DXA }, columnWidths: [9360],
    rows: [new TableRow({ children: [new TableCell({
      width: { size: 9360, type: WidthType.DXA },
      shading: { fill: ACCENT, type: ShadingType.CLEAR },
      margins: { top: 80, bottom: 80, left: 160, right: 160 },
      borders: noBorders,
      children: [new Paragraph({ children: [new TextRun({ text, bold: true, size: 20, color: WHITE, font: "Arial" })] })]
    })] })]
  });
}

function warningBanner(text, fill) {
  return new Table({
    width: { size: 9360, type: WidthType.DXA }, columnWidths: [9360],
    rows: [new TableRow({ children: [new TableCell({
      width: { size: 9360, type: WidthType.DXA },
      shading: { fill, type: ShadingType.CLEAR },  // fill must be 6-digit hex — no opacity suffix
      margins: { top: 100, bottom: 100, left: 180, right: 180 },
      borders: noBorders,
      children: [new Paragraph({ alignment: AlignmentType.CENTER, children: [new TextRun({ text, bold: true, size: 21, color: WHITE, font: "Arial" })] })]
    })] })]
  });
}

// ── Typography ────────────────────────────────────────────────────────────────
function heading2(text) {
  return new Paragraph({
    spacing: { before: 200, after: 80 },
    children: [new TextRun({ text, bold: true, size: 22, color: PRIMARY, font: "Arial" })]
  });
}

function para(text, opts = {}) {
  return new Paragraph({
    spacing: { before: 60, after: 80 },
    children: [new TextRun({ text, size: 21, font: "Arial", color: TEXT, ...opts })]
  });
}

function bullet(text) {
  return new Paragraph({
    numbering: { reference: "bullets", level: 0 },
    spacing: { before: 40, after: 40 },
    children: [new TextRun({ text, size: 21, font: "Arial", color: TEXT })]
  });
}

function numbered(text) {
  return new Paragraph({
    numbering: { reference: "numbers", level: 0 },
    spacing: { before: 40, after: 40 },
    children: [new TextRun({ text, size: 21, font: "Arial", color: TEXT })]
  });
}

function emptyRow(height = 200) {
  return new Paragraph({ spacing: { before: height, after: 0 }, children: [] });
}

// ── Table Helpers ─────────────────────────────────────────────────────────────
// Label-value row: dark background label | light fill value
function blueBox(label, value) {
  return new Table({
    width: { size: 9360, type: WidthType.DXA }, columnWidths: [2400, 6960],
    rows: [new TableRow({ children: [
      new TableCell({
        width: { size: 2400, type: WidthType.DXA },
        shading: { fill: PRIMARY, type: ShadingType.CLEAR },
        margins: { top: 80, bottom: 80, left: 120, right: 120 }, borders,
        children: [new Paragraph({ children: [new TextRun({ text: label, bold: true, size: 20, color: WHITE, font: "Arial" })] })]
      }),
      new TableCell({
        width: { size: 6960, type: WidthType.DXA },
        shading: { fill: GRAY, type: ShadingType.CLEAR },
        margins: { top: 80, bottom: 80, left: 120, right: 120 }, borders,
        children: [new Paragraph({ children: [new TextRun({ text: value, size: 20, font: "Arial", color: TEXT })] })]
      })
    ]})]
  });
}

// Standard table header row
function tableHeader(cols) {
  return new TableRow({
    children: cols.map(([text, width]) => new TableCell({
      width: { size: width, type: WidthType.DXA },
      shading: { fill: PRIMARY, type: ShadingType.CLEAR },
      margins: { top: 80, bottom: 80, left: 100, right: 100 }, borders,
      children: [new Paragraph({ children: [new TextRun({ text, bold: true, size: 19, color: WHITE, font: "Arial" })] })]
    }))
  });
}

// Standard data cell
function tableCell(text, width, opts = {}) {
  return new TableCell({
    width: { size: width, type: WidthType.DXA },
    margins: { top: 100, bottom: 100, left: 100, right: 100 }, borders,
    shading: opts.shade ? { fill: LIGHT, type: ShadingType.CLEAR } : undefined,
    children: [new Paragraph({
      alignment: opts.center ? AlignmentType.CENTER : AlignmentType.LEFT,
      children: [new TextRun({ text: String(text), size: 19, font: "Arial", color: TEXT, bold: opts.bold || false })]
    })]
  });
}

// Checkbox cell (for checklist tables)
function checkCell(width) {
  return new TableCell({
    width: { size: width, type: WidthType.DXA },
    shading: { fill: GRAY, type: ShadingType.CLEAR },
    margins: { top: 80, bottom: 80, left: 100, right: 100 }, borders,
    children: [new Paragraph({ alignment: AlignmentType.CENTER, children: [new TextRun({ text: "\u2610", size: 22, font: "Arial" })] })]
  });
}

// Empty cell for fill-in rows
function emptyCell(width) {
  return new TableCell({
    width: { size: width, type: WidthType.DXA },
    margins: { top: 140, bottom: 140, left: 100, right: 100 }, borders,
    children: [new Paragraph({ children: [new TextRun({ text: "", size: 20 })] })]
  });
}

// Standard 3-column sign-off table
function signoffTable(rows) {
  return new Table({
    width: { size: 9360, type: WidthType.DXA }, columnWidths: [3120, 3120, 3120],
    rows: [
      new TableRow({ children: ["Name", "Signature", "Date"].map(h => new TableCell({
        width: { size: 3120, type: WidthType.DXA },
        shading: { fill: PRIMARY, type: ShadingType.CLEAR },
        margins: { top: 80, bottom: 80, left: 120, right: 120 }, borders,
        children: [new Paragraph({ children: [new TextRun({ text: h, bold: true, size: 20, color: WHITE, font: "Arial" })] })]
      })) }),
      ...rows.map(([name]) => new TableRow({ children: [
        new TableCell({ width:{size:3120,type:WidthType.DXA}, margins:{top:80,bottom:80,left:120,right:120}, borders, children:[new Paragraph({children:[new TextRun({text:name,size:20,font:"Arial",color:TEXT})]})] }),
        new TableCell({ width:{size:3120,type:WidthType.DXA}, margins:{top:180,bottom:180,left:120,right:120}, borders, children:[new Paragraph({children:[new TextRun({text:"",size:20})]})] }),
        new TableCell({ width:{size:3120,type:WidthType.DXA}, margins:{top:180,bottom:180,left:120,right:120}, borders, children:[new Paragraph({children:[new TextRun({text:"",size:20})]})] })
      ]}))
    ]
  });
}

// Document meta table (version, dates, scope, legislation)
function metaTable(scope, leg) {
  return [
    new Table({
      width: { size: 9360, type: WidthType.DXA }, columnWidths: [2400, 6960],
      rows: [
        ["Version","1.0"], ["Issue Date","[Month Year]"], ["Review Date","Annual"],
        ["Applies To", scope], ["Legislative Basis", leg]
      ].map(([l,v]) => new TableRow({ children: [
        new TableCell({ width:{size:2400,type:WidthType.DXA}, shading:{fill:LIGHT,type:ShadingType.CLEAR}, margins:{top:80,bottom:80,left:120,right:120}, borders, children:[new Paragraph({children:[new TextRun({text:l,bold:true,size:20,font:"Arial",color:PRIMARY})]})] }),
        new TableCell({ width:{size:6960,type:WidthType.DXA}, shading:{fill:GRAY,type:ShadingType.CLEAR}, margins:{top:80,bottom:80,left:120,right:120}, borders, children:[new Paragraph({children:[new TextRun({text:v,size:20,font:"Arial",color:TEXT})]})] })
      ]}))
    }),
    emptyRow(120)
  ];
}

// ── Document Config ───────────────────────────────────────────────────────────
const numbering = {
  config: [
    { reference: "bullets", levels: [{ level: 0, format: LevelFormat.BULLET, text: "\u2022",
      alignment: AlignmentType.LEFT,
      style: { paragraph: { indent: { left: 720, hanging: 360 } } } }] },
    { reference: "numbers", levels: [{ level: 0, format: LevelFormat.DECIMAL, text: "%1.",
      alignment: AlignmentType.LEFT,
      style: { paragraph: { indent: { left: 720, hanging: 360 } } } }] }
  ]
};

const styles = {
  default: { document: { run: { font: "Arial", size: 21, color: TEXT } } }
};

// A4 page size with standard margins (adjust margins if client uses different)
const pageProps = {
  page: {
    size: { width: 11906, height: 16838 },
    margin: { top: 1400, right: 1100, bottom: 1100, left: 1100 }
  }
};

const fs = require('fs');

module.exports = {
  Document, Packer, Paragraph, TextRun, Table, TableRow, TableCell,
  Header, Footer, AlignmentType, HeadingLevel, BorderStyle, WidthType,
  ShadingType, VerticalAlign, SimpleField, LevelFormat, PageBreak, ImageRun,
  PRIMARY, ACCENT, LIGHT, WHITE, GRAY, TEXT, SUBTEXT,
  border, borders, noBorder, noBorders,
  makeHeader, makeFooter, sectionBanner, subBanner, warningBanner,
  heading2, para, bullet, numbered, emptyRow,
  blueBox, tableHeader, tableCell, checkCell, emptyCell,
  signoffTable, metaTable,
  numbering, styles, pageProps, fs, LOGO_BUF
};
```

---

## Testing the Common Module

Before building any documents, run a smoke test:

```javascript
// test_common.js
const C = require('./common_client');

const doc = new C.Document({
  numbering: C.numbering, styles: C.styles,
  sections: [{
    properties: C.pageProps,
    headers: { default: C.makeHeader('Test Document', 'CLIENT-TEST-001') },
    footers: { default: C.makeFooter('1.0') },
    children: [
      C.sectionBanner('Section 1 — Test'),
      C.emptyRow(100),
      C.para('Test paragraph body text.'),
      C.bullet('Bullet item one'),
      C.numbered('Numbered item one'),
      C.blueBox('Label', 'Value'),
      C.emptyRow(80),
      C.signoffTable([['Manager'], ['Worker']])
    ]
  }]
});

C.Packer.toBuffer(doc).then(buf => {
  C.fs.writeFileSync('/mnt/user-data/outputs/test_common.docx', buf);
  console.log('Test doc written');
});
```

```bash
node test_common.js
python /mnt/skills/public/docx/scripts/office/validate.py /mnt/user-data/outputs/test_common.docx
```

Expected output: `All validations PASSED!`
