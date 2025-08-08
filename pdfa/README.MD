## Problem, PDF sucks

Online Print Service https://www.repro-online.de/de/plakatdruck does not accept our PDF documents.

- BI-BHF-SMC_GR_zusammengefügt.pdf
- Holz.xlsx

We use PDF24 to split them per page, and then convert them to PDF/A.

- https://tools.pdf24.org/split-pdf
- https://tools.pdf24.org/pdf-to-pdfa

## `PDF/A` to the rescue

then we check those ./pdfa/* files for actual PDF/A compliance. to see that there is no more PDF nonsense in them. we do this with Open Source https://verapdf.org/software/

```
verapdf --flavour 2b --format text ./pdfa/Architekt.pdf
verapdf --flavour 2b --format text ./pdfa/split/1.pdf
verapdf --flavour 2b --format text ./pdfa/split/2.pdf
verapdf --flavour 2b --format text ./pdfa/split/3.pdf
verapdf --flavour 2b --format text ./pdfa/split/4.pdf
verapdf --flavour 2b --format text ./pdfa/split/5.pdf
verapdf --flavour 2b --format text ./pdfa/split/6.pdf
verapdf --flavour 2b --format text ./pdfa/split/7.pdf
verapdf --flavour 2b --format text ./pdfa/split/8.pdf
```

| PDF/A | year  | Base     | Limits                                                                                            | Conformance |
| ----- | ----- | -------- | ------------------------------------------------------------------------------------------------- | ----------- |
| A-1   | 2005  | PDF 1.4  | Strict “classic”; **no** transparency/layers/JPEG2000/attachments                                 | 1a, 1b      |
| A-2   | 2011  | PDF 1.7  | Allows transparency, layers (OCG), JPEG2000, better signatures                                    | 2a, 2b, 2u  |
| A-3   | 2012  | PDF 1.7  | Like A-2 **plus** arbitrary file attachments (bundles)                                            | 3a, 3b, 3u  |
| A-4   | 2020  | PDF 2.0  | Modernised; no a/b/u split; profiles: **4** (base), **4e** (engineering/3D), **4f** (attachments) | 4, 4e, 4f   |

- `a` adds tagged structure & language info for accessibility
- `b` basic visual reproducibility
- `u` guarantees every glyph maps to Unicode

## Dream PDF function

PDF is an messy format. like a "mini web". PDF/A is also not perfect, but this is how an ideal or "dream" PDF function would look like

- Embedd all resources (fonts, colors) - PDF/A
- OCR all text
- Collapse "optional content groups" into single layer - PDF/A-2 allowed OCG again
- Delete all interactive objects
  - Forms (text fields, dropdowns)
  - Buttons and JavaScript
  - Video and Audito
