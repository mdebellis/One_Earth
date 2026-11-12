# One Earth Bioregions — three active modules

This directory maintains one active copy of each module:

| File | Contents |
| --- | --- |
| `one_earth_bioregions.ttl` | Base geography and Big_Bro foundation. |
| `one_earth_geosparql.ttl` | GeoSPARQL alignment and 185 geometry records. |
| `one_earth_what3words_examples.ttl` | Four published examples, with the original 49 example/property triples preserved and an ontology header added. |

The sibling Solutions ontology directly imports these three files using their raw GitHub URLs. Open Solutions in Protégé to load everything. The what3words module also imports the base file, using exactly the same URL; both references resolve to the same ontology.

The existing `one_earth_bioregions_integrated.ttl` and `one_earth_bioregions_with_geosparql.ttl` belong in Archive. No replacement wrapper is needed. Preserve their paths beneath Archive rather than leaving active duplicates.

Keep this directory's catalog and the sibling Solutions catalog. They map the four active ontology identities and their raw URLs to the actual local files. They contain no recursive Archive discovery or wrapper mappings. Base and geometry contents are unchanged.

## AllegroGraph

Explicitly load the base, GeoSPARQL, what3words, and Solutions files into the same repository. Loading the dependencies first is a useful convention, but RDF references do not require their definitions to have been loaded earlier. Wait until all required data and axioms are present before running the Materializer and testing the combined model. The catalog is for local ontology loading in Protégé, not AllegroGraph.

## Original geometry description and validation

The following describes the existing geometry content and its earlier validation. Current direct-import checks are recorded in the sibling Solutions `validation.json`.

## What the extension adds

- `oeb:Geographical_Region rdfs:subClassOf geo:Feature`, preserving its existing superclass.
- 185 named geometry individuals in the existing `https://www.in2use.com/one_earth_bioregions/` namespace, with names such as `Pacific_Northwest_Coastal_Forests_Geometry`.
- Explicit `geo:hasGeometry` and `geo:hasDefaultGeometry` links to the same source geometry for each bioregion.
- WKT typed literals with explicit CRS84 coordinates: longitude, then latitude.
- Geometry source links, labels, and identifiers.
- A small subset of standard GeoSPARQL class/property declarations and axioms needed by this data. The full OGC ontology is not imported. GeoSPARQL query functions are implemented by the query engine and do not require declarations here.

The module preserves all source coordinate values and the Polygon/MultiPolygon structure, including holes. It performs no simplification, rounding, or geometry repair. Biome types remain classification concepts. Ecoregion, subrealm, realm, and political-region geometries are not included in this release.

`geo:Feature` and Big_Bro's `sulo:Feature` have different meanings; they have not been equated. Standard GeoSPARQL names and IRIs are reused, including the standard `#` namespace delimiter.

## Validation and limits

- The packaged base is byte-identical to the GitHub source below, including the updated biome page links.
- All three files parse as Turtle.
- All 185 bioregion codes match existing base individuals; only source-code leading zeros were normalized for matching, such as `NA05` to `NA5`.
- All 185 geometries passed GEOS/Shapely planar geometry-validity checks and exact coordinate round-trip comparisons between source GeoJSON and WKT.
- Both the base and the combined graph passed local Pellet consistency checking, classification, realization, and unsatisfiable-class checks. No unsatisfiable named classes were found.
- Those reasoner checks used Owlready2's bundled `pellet-2.3.1.jar` with the OWLAPIv3 loader and Java 21. They are not tests of your exact Protégé/Pellet installation and are not GUI performance benchmarks.
- Pellet accepts the WKT datatype in this test but does not validate polygon syntax or compute spatial relationships. Geometry validation was performed separately.
- AllegroGraph spatial execution, indexing, and global edge-case behavior have not been tested against your server. Local geometry validity does not by itself establish boundary accuracy or that every spatial engine interprets global shapes identically.

## Sources and ontology identities

Base source: [mdebellis/One_Earth](https://github.com/mdebellis/One_Earth/blob/196cfe58775f5b6475886f8a1b28ec62ad33858a/ontologies/one_earth_bioregions.ttl), commit `196cfe58775f5b6475886f8a1b28ec62ad33858a`.

Base SHA-256: `09dafaa4a4c4d864544bcd3e56eca31d6b495931653c339d6aba5152dc7d2993`.

Boundaries: [One Earth downloads](https://www.oneearth.org/downloads/), One Earth Bioregions 2023 GeoJSON. Geometry-source SHA-256: `f9c360f0797ef211b207f95720189219ce6ae43d645b87a134ab386b25a4150d`.

Schema reference: [OGC GeoSPARQL 1.1](https://docs.ogc.org/is/22-047r1/22-047r1.html). Import behavior: [Protégé ontology imports](https://protegewiki.stanford.edu/wiki/Importing_Ontologies_in_P41).

| Ontology | IRI |
| --- | --- |
| Base | `https://www.in2use.com/one_earth_bioregions` |
| Optional extension | `https://www.in2use.com/one_earth_geosparql` |
| what3words examples | `https://www.in2use.com/one_earth_what3words_examples` |

The new One Earth-derived module follows the project's CC BY-NC 4.0 license. Existing base and reused vocabulary license/provenance notices retain their scope. Creation date: November 10, 2026, America/Los_Angeles.
