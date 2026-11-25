# Inventaria Thesaurus v 0.1

A SKOS thesaurus of the objects, materials, rooms, animals, roles and properties recorded in historical inventories of Tyrolean castles and residences (14th–17th century). It was developed in the **Inventaria** project at the University of Innsbruck.

The thesaurus normalises the terminology of the inventories. Each concept has a standard German term with an English translation, the historical spellings found in the sources, definitions from the project glossary, and links to the [DARIAH Backbone Thesaurus (BBT)](https://vocabs.dariah.eu/bbt/), the [Getty Art & Architecture Thesaurus (AAT)](https://www.getty.edu/research/tools/vocabularies/aat/), Wikidata and the Library of Congress.

## Files

| File | Content |
|---|---|
| `Inventaria-Thesaurus.nq` | The thesaurus in SKOS as N-Quads (named graph `https://w3id.org/uibk/inventaria/thesaurus`). **This is the primary file.** |
| `Inventaria-Thesaurus.xlsx` / `.ods` | Concept list as a spreadsheet: one row per concept with ID, labels, alternative labels, definition, broader concepts and links. |
| `Inventaria-Thesaurus.txt` | Plain-text version of the thesaurus - tab delimited. |
| `Inventaria-Thesaurus_de.mm` / `_en.mm` | Concept hierarchy as a mind map (FreeMind format), with German and English labels. Opens in FreeMind, Freeplane and MindManager. |
| `Inventaria_translations_en.xlsx` | Review sheet for the English labels: German term, English term, broader concept and definition per concept. |
| `SKOS_Thesaurus_Workshop_EN.html` | Browser tool for viewing and editing the thesaurus (see below). |

## Content

- **2,382 concepts**, including:
  - 1,703 movable objects
  - 177 rooms and buildings
  - 129 materials
  - 91 properties and conditions
  - 53 terms of document description
  - 51 processing techniques
  - 41 personal roles
  - 19 livestock
  - colours, actions, quantities and measures
- **Labels**:
  - `skos:prefLabel` in German (`de`) and English (`en`).
  - 9,302 historical spellings from the inventories as `skos:altLabel` with the language tag `de-x-hist`.
  - Further German alternative labels.
- **Definitions**: 947 concepts have a `skos:definition` from the Inventaria glossary.
- **Hierarchy**:
  - Polyhierarchical (`skos:broader`). A concept may have several broader concepts.
  - The top level consists of the BBT top concepts *activities*, *conceptual objects*, *materials*, *material things* and *people*, plus *livestock*.
- **External concepts**: BBT and AAT concepts used as broader concepts keep their original URIs and properties. No copies with local URIs were created.
- **Links**: `skos:exactMatch` to Wikidata, the Library of Congress and the Getty AAT.

## Identifiers

Concept URIs follow the pattern

```
https://w3id.org/uibk/inventaria/E55_type/{prefix}:{number}
```

Each prefix stands for one subject area:

| Prefix | Subject area | Prefix | Subject area |
|---|---|---|---|
| `inv_o` | objects | `inv_quality` | properties and conditions |
| `inv_r` | rooms and buildings | `inv_processing` | processing techniques |
| `inv_m` | materials | `inv_action` | actions |
| `inv_a` | livestock | `inv_color` | colours |
| `inv_role` | personal roles | `inv_size` | quantities and measures |
| `inv_terminus` | document description | `inv` | groupings within the hierarchy |

The concept scheme is `https://w3id.org/uibk/inventaria/thesaurus`. The concepts are used as `E55 Type` in the project's CIDOC-CRM data.

## Using the thesaurus

**In Python (rdflib):**

```python
from rdflib import Dataset, URIRef
from rdflib.namespace import SKOS

ds = Dataset()
ds.parse("Inventaria-Thesaurus.nq", format="nquads")
g = ds.graph(URIRef("https://w3id.org/uibk/inventaria/thesaurus"))

for c, label in g.subject_objects(SKOS.prefLabel):
    if label.language == "en":
        print(c, label)
```

**SPARQL**, e.g. all historical spellings of a concept:

```sparql
PREFIX skos: <http://www.w3.org/2004/02/skos/core#>
SELECT ?concept ?pref ?spelling WHERE {
  ?concept skos:prefLabel ?pref ; skos:altLabel ?spelling .
  FILTER(lang(?pref) = "de" && lang(?spelling) = "de-x-hist")
}
```

The `.nq` file can be loaded into any triple store that supports N-Quads, such as GraphDB, Apache Jena Fuseki or Blazegraph.

## SKOS Thesaurus Workshop

`SKOS_Thesaurus_Workshop_EN.html` is a self-contained web page for viewing and editing SKOS thesauri. It needs no installation: open it in a current browser.

- Open `Inventaria-Thesaurus.nq` with *Load → Open SKOS thesaurus as working thesaurus*, or drag the file onto the page.
- Switch the label language of the tree with the language selector. Missing labels fall back to an available language and are marked with their language tag.
- Edit labels, definitions and links. Move concepts by drag and drop; hold Ctrl/⌥ to add an additional broader concept.
- Load the BBT or other thesauri into the library and take concepts over. Import AAT concepts by ID from the Getty data dump.
- Import concepts from Excel or mind maps.
- Export as Turtle, N-Quads, Excel and mind maps (MindManager `.mmap`, FreeMind `.mm`).

Files are processed locally in the browser; nothing is uploaded. The page loads N3.js, SheetJS and JSZip from a CDN, so it needs an internet connection.

## Status

The English labels are new and still being reviewed; corrections are welcome (see `Inventaria_translations_en.xlsx`). A small number of historical terms whose meaning is unclear have no English label yet.

## License

This repository contains both data and software, which are licensed separately.

### Data – CC BY 4.0

The thesaurus in all formats (`.nq`, `.xlsx`, `.ods`, `.txt`, `.mm`) and the documentation are licensed under the [Creative Commons Attribution 4.0 International License (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/). See [`LICENSE-DATA`](LICENSE-DATA).

You may share and adapt the data for any purpose, including commercially, provided you give appropriate credit.

### Software – Apache License 2.0

The SKOS Thesaurus Workshop (`*.html`) is licensed under the [Apache License 2.0](https://www.apache.org/licenses/LICENSE-2.0). See [`LICENSE`](LICENSE).

### Third-party content

- **Getty Art & Architecture Thesaurus (AAT)**: concepts reused with their original URIs and properties. © J. Paul Getty Trust, licensed under the [Open Data Commons Attribution License (ODC-By) 1.0](https://opendatacommons.org/licenses/by/1-0/).
- **DARIAH Backbone Thesaurus (BBT)**: top-level concepts reused with their original URIs. © DARIAH-EU, [license – please check].
- The SKOS Thesaurus Workshop loads these libraries from a CDN:
  - [N3.js](https://github.com/rdfjs/N3.js) (MIT)
  - [SheetJS Community Edition](https://sheetjs.com) (Apache 2.0)
  - [JSZip](https://stuk.github.io/jszip/) (MIT or GPLv3)

## Citation

> Christina Antenhofer, Elisabeth Gruber-Tokić, Gerald Hiebel, Ingrid Matschinegg, Milena Peralta Friedburg, Claudia Posch and Gerhard Rampl, *Inventaria Thesaurus*, University of Innsbruck, 2026. [DOI or URL]

## Contact

Gerald Hiebel, University of Innsbruck – ORCID [0000-0002-3799-8391](https://orcid.org/0000-0002-3799-8391)
