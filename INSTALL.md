# Direct imports update — replaces the earlier wrapper-based update

The complete model is opened through `one_earth_solutions.ttl`. It contains its own process model and directly imports base Bioregions, GeoSPARQL, and what3words. There are no active wrapper ontologies.

1. Close Protégé. Preserve the existing `one_earth_bioregions_integrated.ttl` and `one_earth_bioregions_with_geosparql.ttl` under `ontologies/one_earth_archive/one_earth_bioregions/`, then remove their active copies. Preserve their current contents before overwriting or deleting anything. If you already saved the old flattened integrated export, keep it too.
2. Finish the previously reviewed duplicate cleanup: the three root-level Turtle files and the Bioregions copy inside Solutions already have byte-identical copies at the corresponding Archive paths in the reviewed GitHub commit. Remove those active duplicates. The root-level catalog also has an archived copy and is no longer needed. If present, move the previous root-level import-review document outside the active ontology directory.
3. Merge this ZIP's `ontologies` folder into the repository's existing `ontologies` folder. Replace the corresponding files; do not create an extra `ontologies/ontologies` level. Keep this installation note outside `ontologies`.
4. Keep the authoritative base and GeoSPARQL files already in the Bioregions directory. They are absent from this update ZIP because they have not changed. Keep the existing Solutions query file, source mapping CSV, and license.
5. Open `ontologies/one_earth_solutions/one_earth_solutions.ttl` in Protégé. Its three direct imports should resolve locally through the supplied catalogs. Run Pellet as usual.
6. Review, commit, and push through GitHub Desktop. The raw URLs serve the revised files after the push. No GitHub changes have been made by this preparation.

The two active directories will contain four ontology files in total: three in Bioregions and one in Solutions. The existing ontology and entity IRIs remain unchanged. The what3words file gains its own ontology header and base import; its original 49 triples are unchanged.

For AllegroGraph, use the import declarations as a checklist and explicitly load all four files. A convenient order is base Bioregions, GeoSPARQL, what3words, then Solutions. This order is not technically required for ordinary RDF loading. Run the Materializer after all required files are present; do not reload modules already present unless you intend to update them.

Reviewed against GitHub commit a854e283bf6b3aace7fada40538a41067e6a14b2. The Solutions process model is unchanged; version 0.2.2 records the direct imports.
