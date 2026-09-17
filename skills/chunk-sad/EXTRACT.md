# Extract

Load this for `.docx`, `.xlsx`, or `.xls`. For `.md` and `.txt`, Read the file.

Done when every office source is readable text, or the extract failure is reported to the user.

Run Python against the source path. Stdlib only: `zipfile` and `xml.etree.ElementTree`.

## .docx

Unzip `word/document.xml`. Join `w:t` text in document order, paragraph-break on `w:p`.

```python
import zipfile
from xml.etree import ElementTree as ET
from pathlib import Path

W = "{http://schemas.openxmlformats.org/wordprocessingml/2006/main}"
path = Path("SOURCE.docx")
with zipfile.ZipFile(path) as z:
    root = ET.fromstring(z.read("word/document.xml"))
paras = []
for p in root.iter(W + "p"):
    paras.append("".join(t.text or "" for t in p.iter(W + "t")))
text = "\n".join(paras)
print(text)
```

## .xlsx

Unzip `xl/sharedStrings.xml` and each `xl/worksheets/sheet*.xml`. Resolve shared-string indices. Emit one TSV block per sheet.

```python
import zipfile
from xml.etree import ElementTree as ET
from pathlib import Path

NS = {"m": "http://schemas.openxmlformats.org/spreadsheetml/2006/main"}
T = "{http://schemas.openxmlformats.org/spreadsheetml/2006/main}t"
path = Path("SOURCE.xlsx")
with zipfile.ZipFile(path) as z:
    ss = []
    names = z.namelist()
    if "xl/sharedStrings.xml" in names:
        root = ET.fromstring(z.read("xl/sharedStrings.xml"))
        for si in root.findall("m:si", NS):
            ss.append("".join(t.text or "" for t in si.iter(T)))
    blocks = []
    for name in names:
        if not (name.startswith("xl/worksheets/sheet") and name.endswith(".xml")):
            continue
        root = ET.fromstring(z.read(name))
        rows = []
        for row in root.findall(".//m:row", NS):
            cells = []
            for c in row.findall("m:c", NS):
                t = c.get("t")
                if t == "inlineStr":
                    val = "".join(x.text or "" for x in c.iter(T))
                else:
                    v = c.find("m:v", NS)
                    val = v.text if v is not None else ""
                    if t == "s" and val.isdigit():
                        val = ss[int(val)]
                cells.append(val)
            rows.append("\t".join(cells))
        blocks.append(f"# {name}\n" + "\n".join(rows))
text = "\n\n".join(blocks)
print(text)
```

## .xls

Convert to `.xlsx` with whatever is on PATH (`soffice --headless --convert-to xlsx`, `ssconvert`), then use the `.xlsx` path above. If nothing converts, ask the user for an `.xlsx` export.
