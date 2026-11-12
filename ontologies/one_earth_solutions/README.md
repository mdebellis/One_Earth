# One Earth Solutions — draft 0.2.0

This draft models solutions as **kinds of activity** and connects actual activity instances to the regions, ecosystems, species, and agents they concern. It is Common Action's process-oriented interpretation of [One Earth's Solutions Framework](https://www.oneearth.org/solutions/), with original source headings and links retained. It is not an official One Earth ontology.

## Open in Protégé

1. Extract the ZIP into a **new folder**. Its files sit directly in that folder; the archive has no extra directory layer.
2. Keep the two Turtle files and `catalog-v001.xml` together.
3. Open `one_earth_solutions.ttl`. The catalog resolves its canonical Bioregions import to the accompanying local file.
4. Start Pellet. Under the imported `prov:Activity` / SULO `Process` hierarchy, inspect `Climate_Solution` and its subclasses. The `Example_...` individuals demonstrate the connections.

The Solutions import declaration is the ontology IRI `https://www.in2use.com/one_earth_bioregions`. It contains no machine-specific path. The catalog supplies the local resolution; the canonical IRI remains in the ontology. Without the catalog, remote loading depends on that IRI resolving to the ontology document.

The included Bioregions file is an unchanged snapshot from the public GitHub repository, retrieved 2026-11-11. It already includes the Big_Bro foundation (SULO and PROV terms), so Solutions does not copy those definitions or import another Big_Bro version. It does not import the full process_model ontology.

If your own Bioregions file contains newer changes, use that version instead of overwriting it with this snapshot. Keep its canonical ontology IRI, and adjust the catalog's relative filename if necessary.

## Files and imports

| File | Purpose |
| --- | --- |
| `one_earth_solutions.ttl` | New process classes, properties, themes, levers, and demonstration data. Imports Bioregions. |
| `one_earth_bioregions.ttl` | Unchanged imported geography and Big_Bro foundation. |
| `catalog-v001.xml` | Maps canonical ontology IRIs to the local Turtle files. |
| `demo_queries.rq` | Five SPARQL SELECT examples; run each separately with the prefixes. |
| `source_class_mapping.csv` | Exact mapping of 91 published headings to their process-class IRIs. |
| `validation.json` | Results of structural and reasoning checks. |
| `LICENSE` | Existing project license: CC BY-NC 4.0. |

**The included base Bioregions snapshot does not contain WKT geometries or what3words examples.** Keep using the existing geometry and what3words modules when needed. Their absence does not prevent the process, target, and geographical hierarchy examples from working. No additional integration ontology is required for this draft.

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

For a new repository, load both Turtle files. Loading an `owl:imports` triple alone should not be assumed to fetch its target in AllegroGraph. If the repository already contains the same Bioregions model, load only the new Solutions file. Remove the old Solutions 0.1 draft before loading its replacement; do not delete unrelated geography or geometry data.

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

Base snapshot: [one_earth_bioregions.ttl on GitHub](https://github.com/mdebellis/One_Earth/blob/main/ontologies/one_earth_bioregions.ttl).

## Validation

Validation uses the union of the new Solutions model and the unchanged Bioregions snapshot. Temporary validation files resolve the import locally; the distributed Solutions file retains its canonical import.

Checks cover Turtle parsing; framework counts; names and English labels; inverse declarations; imported-IRI reuse; absence of class/individual punning; Pellet consistency and classification; and expected inferences for process types, subprocesses, responsible agents, participants, and the geographical hierarchy. Negative checks confirm that a target is not inferred to be a location or participant, and that the example ecosystem does not become a process. See `validation.json` for the actual results.

This is a first process-model draft for review. Passing the reasoner establishes logical consistency under the current axioms; it does not verify the suitability or effectiveness of any climate intervention.
