# One Earth Solutions — draft 0.2.2

This ontology contains the Solutions process model and directly imports the three files in the sibling Bioregions directory. It is the entry point for the complete model in Protégé. There are no active integration-wrapper ontologies.

## Imports and local use

The three direct imports use raw GitHub URLs for:

- `../one_earth_bioregions/one_earth_bioregions.ttl`
- `../one_earth_bioregions/one_earth_geosparql.ttl`
- `../one_earth_bioregions/one_earth_what3words_examples.ttl`

Keep both directories under `ontologies`, retain their local `catalog-v001.xml` files, and open `one_earth_solutions.ttl`. The catalogs resolve the raw import URLs to those same local files. They contain no Archive or wrapper mappings. There is no Bioregions copy in this Solutions directory and no root-level catalog is required for this layout.

The Solutions ontology IRI remains `https://www.in2use.com/one_earth_solutions`; all existing entity IRIs remain unchanged. Version 0.2.2 changes the imports and their documentation; the process class model is unchanged from 0.2.0. Geometry and what3words are now included when opening Solutions through its imports.

## AllegroGraph loading convention

For ordinary RDF loading, explicitly load each required file into the same repository. The `owl:imports` triples record dependencies; they do not themselves fetch the imported files in the normal loading workflow.

Use this order for clarity: base Bioregions, GeoSPARQL, what3words, then Solutions. This is an organizational convention, not an RDF requirement: referenced IRIs can be stored before their definitions are loaded. Run the Materializer after all required files are loaded. If you add further axioms or data later, materialize again as appropriate. GeoSPARQL indexing is a separate operation.

The raw import document URLs and the ontology/entity IRIs serve different purposes. Matching entity IRIs allow the loaded triples to connect; AllegroGraph does not require an ontology header to exist before accepting those references. Protégé's catalog does not configure AllegroGraph.

Supporting files `demo_queries.rq`, `source_class_mapping.csv`, and `LICENSE` are unchanged. `validation.json` records the model and direct-import checks.

## What changed from 0.1

Version 0.1 represented the framework as standalone category individuals. **Replace that draft; do not merge the two versions.** Some category IRIs now identify classes, and noun headings have action-oriented replacements. Loading both versions would retain obsolete categories and unintended class/individual punning.

Version 0.2 contains:

- Three pillar classes, 12 subpillar classes, and 76 solution-pathway classes under `Climate_Solution`, a subclass of `prov:Activity` and therefore SULO `Process`.
- Seven intersectional themes and seven levers of change as controlled individuals. These describe concerns and enabling mechanisms, rather than occurrences of activities.
- Three controlled framework-level individuals, attached to process classes through the `framework_level` annotation. Source levels remain queryable without treating the process classes as individuals.
- Two supporting activity classes for the examples: `Climate_Fundraising` and `Native_Vegetation_Planting`. These are CA additions, outside the 76 published pathways.
- Simple `Species` and `Ecosystem` classes. A Species individual represents a species-level taxon, not a particular organism or population.

New IRIs use `/`, Title_Case for classes and individuals, and lower_case for properties. English labels match the IRI suffix with underscores replaced by spaces. `dcterms:title` preserves original One Earth headings. Imported IRIs are unchanged.

## Modeling choices

| Relationship | Meaning |
| --- | --- |
| `rdfs:subClassOf` | A kind of activity: Wetland Restoration is a kind of Ecosystem Restoration. |
| `has_subprocess` / `is_subprocess_of` | Actual component activities. Transitive and aligned with SULO part relations; no order or dependency is implied. |
| `has_target` / `is_target_of` | Intended focus, such as a particular region, ecosystem, or species taxon. Does not assert successful benefit, location, or physical participation. |
| `has_location` / `is_location_of` | Where an activity, entity, or agent is situated. Reuses `prov:atLocation` with the existing Geographical Region range. |
| `has_responsible_agent` / `is_responsible_agent_of` | An agent responsible for an activity. Reuses `prov:wasAssociatedWith`. |
| `sulo:hasParticipant` / `sulo:isParticipantIn` | Existing Big_Bro relations for actual participation. No new participant vocabulary is needed. |
| `has_intersectional_theme` / `is_intersectional_theme_of` | A cross-cutting concern explicitly associated with an activity. |
| `has_lever_of_change` / `is_lever_of_change_of` | An enabling mechanism or strategy used by an activity. |

The three broad pillar classes mean activities contributing to the respective area through one or more pathways. They do not mean a single global transition event or require every activity to use all pathways. The classes are not disjoint: an activity may address several solution areas.

One Earth's grouping is sometimes by objective rather than literal process subtype. `Rangeland_Sustainability_Activity` therefore includes efforts to reduce livestock demand as well as grazing management. Classes for urban gardening and bioregional sourcing denote those activities when intended to contribute to food-waste reduction; Sustainable Forestry denotes management intended to support ecosystem recovery in this hierarchy. Scope notes make these contextual meanings explicit. This does not claim that all gardening reduces waste or all forestry restores an ecosystem. These are reviewable CA modeling choices; the published grouping alone does not establish those implications.

An activity's location and target are independent. A conservation fundraiser can occur in Britain while targeting the Pacific Northwest. A target link to a bioregion does not assert that every place in that bioregion is being restored or that the activity benefits every species there. Locations and targets do not automatically propagate between subprocesses; assert or query the intended relationships explicitly.

The draft adds no workflow sequencing, scheduling, inputs/outputs, mandatory target restrictions, or new property chains. These can be introduced if CA later needs them.

## Demonstration data

Six individuals beginning `Example_` are explicitly marked `is_illustrative_example true`, with comments identifying them as fictional:

- `Example_Wetland_Restoration` occurs in the existing Pacific Northwest Coastal Forests bioregion and targets an example wetland, Coho Salmon, and that bioregion as its geographical focus.
- `Example_Habitat_Work` is a component of the restoration; `Example_Native_Planting` is a component of that habitat work.
- `Example_Conservation_Fundraising` occurs in the existing Great Britain Ireland And Faroe Islands bioregion and targets Pacific Northwest Coastal Forests.
- `Example_Conservation_Organization` is responsible for the restoration and fundraising.
- `Example_Wetland` is a fictional ecosystem located in Pacific Northwest Coastal Forests; it has no asserted geometry.

Coho Salmon is a real species-level taxon, with scientific name `Oncorhynchus kisutch` and a [NOAA source link](https://www.fisheries.noaa.gov/species/coho-salmon). Its association with the fictional restoration effort is demonstration data. Imported regions are real framework entries. Absence of the illustrative flag is not a general guarantee of data verification.

## AllegroGraph and GraphTalker

For a new repository, load the base Bioregions, GeoSPARQL, what3words, and Solutions files into the same repository. A convenient convention is to load them in that order. Loading an `owl:imports` triple alone should not be assumed to fetch its target in AllegroGraph. If the repository already contains the same base, geometry, and what3words data, load only the revised Solutions file. Remove the old Solutions 0.1 draft before loading its replacement; do not delete unrelated geography or geometry data.

Materialize the required OWL class and property inferences as in your existing workflow. GeoSPARQL index creation is a separate operation and is not needed for these examples. The SPARQL examples in `demo_queries.rq` use paths where needed so they also work on asserted data.

Suggested GraphTalker questions:

1. “What are the three main areas of climate action, and how is wetland restoration classified within them? Use the ontology's class hierarchy.”
2. “For the example wetland restoration effort, show its bioregion, subrealm, and realm, and the species it aims to help. Clearly identify the effort as fictional.”
3. “Which example activity takes place in Britain but aims to support conservation in the Pacific Northwest? Show its location and target separately.”
4. “What activities are part of the example wetland restoration effort, including activities nested inside another component?”
5. “Who is responsible for the example restoration effort, and which themes and enabling strategies are associated with it?”

The current geography and taxonomy support these questions. They do not assert which of all 76 solution types is suitable for every region, nor do they record real projects or verified outcomes.

## Source notes

Membership and hierarchy follow the [main framework](https://www.oneearth.org/solutions/) reviewed 2026-11-11, with individual links checked against the [Solutions Hub](https://www.oneearth.org/categories/). Short definitions are editorial descriptions, not copied official definitions. `source_class_mapping.csv` makes the action-oriented renaming reviewable.

The Renewable Power category page additionally lists Advanced Nuclear, absent from the main framework. This draft retains the main framework's 76 pathways pending source reconciliation. One Earth's Climate Refugia heading is interpreted in its carbon-storage and climate-stabilization sense. Shared geothermal, hydrogen, and biomass source URLs do not imply equivalence between their power, heat, and fuel uses.

Active base: [one_earth_bioregions.ttl on GitHub](https://github.com/mdebellis/One_Earth/blob/main/ontologies/one_earth_bioregions/one_earth_bioregions.ttl).

## Validation

Validation uses the union of Solutions, base Bioregions, GeoSPARQL, and what3words. Validation resolves the raw import URL to the intended local file; the distributed Solutions file retains the raw GitHub import declaration.

Checks cover Turtle parsing; framework counts; names and English labels; inverse declarations; imported-IRI reuse; absence of class/individual punning; Pellet consistency and classification; and expected inferences for process types, subprocesses, responsible agents, participants, and the geographical hierarchy. Negative checks confirm that a target is not inferred to be a location or participant, and that the example ecosystem does not become a process. See `validation.json` for the actual results.

This is a first process-model draft for review. Passing the reasoner establishes logical consistency under the current axioms; it does not verify the suitability or effectiveness of any climate intervention.
