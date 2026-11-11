# One Earth: optional GeoSPARQL module

This package lets you compare your existing ontology with the same ontology plus bioregion boundary geometry. It contains three Turtle ontologies and a small local import catalog.

| File | Purpose | Approximate size |
| --- | --- | ---: |
| `one_earth_bioregions.ttl` | Your current GitHub ontology, copied byte for byte | 1.62 MB |
| `one_earth_geosparql.ttl` | GeoSPARQL schema alignment and all 185 bioregion geometries | 20.90 MB |
| `one_earth_bioregions_with_geosparql.ttl` | Small wrapper importing both ontologies | 1.3 KB |

The wrapper has its own ontology IRI. It contains no duplicate copy of the base or geometry data. The base file has no imports, so it can still be opened independently.

## Compare in Protégé

1. Extract the ZIP to a new folder. Keep all three Turtle files and `catalog-v001.xml` together.
2. Open `one_earth_bioregions.ttl`. Run Pellet and observe loading, classification, and browsing behavior.
3. Close that Protégé session, then open `one_earth_bioregions_with_geosparql.ttl` in a fresh session. Confirm that the imports panel shows both modules successfully loaded, and run the same Pellet tasks.
4. Use the same reasoner settings for both comparisons. Fresh sessions help avoid comparing a cold load with already cached data.

`catalog-v001.xml` maps the canonical ontology IRIs to files in this folder. This is Protégé configuration, not a fourth ontology. There is no need to publish the new ontology IRIs before trying the package locally. Do not remove the catalog or rename its target files independently. If Protégé prompts for an import location, select the corresponding local Turtle file.

Imported axioms normally appear as imported/read-only content while the wrapper is active. Select the base or extension as the active ontology when you intend to edit that module. Opening the base alone never loads the extension.

If you make further changes to the base in your main working folder, you can replace this package's copy with that newer file, keeping the filename and ontology IRI. Existing bioregion IRIs are the links between the files.

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

## AllegroGraph

For a later AllegroGraph test, load `one_earth_bioregions.ttl` and `one_earth_geosparql.ttl` into the same repository/query dataset. Loading the small wrapper alone should not be assumed to load OWL imports automatically. Protégé's local catalog is not an AllegroGraph configuration file.

The geometry data use standard GeoSPARQL vocabulary. No AllegroGraph-specific index triples or spatial relationship results are precomputed in these files.

## Sources and ontology identities

Base source: [mdebellis/One_Earth](https://github.com/mdebellis/One_Earth/blob/196cfe58775f5b6475886f8a1b28ec62ad33858a/ontologies/one_earth_bioregions.ttl), commit `196cfe58775f5b6475886f8a1b28ec62ad33858a`.

Base SHA-256: `09dafaa4a4c4d864544bcd3e56eca31d6b495931653c339d6aba5152dc7d2993`.

Boundaries: [One Earth downloads](https://www.oneearth.org/downloads/), One Earth Bioregions 2023 GeoJSON. Geometry-source SHA-256: `f9c360f0797ef211b207f95720189219ce6ae43d645b87a134ab386b25a4150d`.

Schema reference: [OGC GeoSPARQL 1.1](https://docs.ogc.org/is/22-047r1/22-047r1.html). Import behavior: [Protégé ontology imports](https://protegewiki.stanford.edu/wiki/Importing_Ontologies_in_P41).

| Ontology | IRI |
| --- | --- |
| Base | `https://www.in2use.com/one_earth_bioregions` |
| Optional extension | `https://www.in2use.com/one_earth_geosparql` |
| Combined wrapper | `https://www.in2use.com/one_earth_bioregions_with_geosparql` |

The new One Earth-derived module follows the project's CC BY-NC 4.0 license. Existing base and reused vocabulary license/provenance notices retain their scope. Creation date: November 10, 2026, America/Los_Angeles.
