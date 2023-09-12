# Motivating Scenario 

Retrieve information about a research project such as what activities it involved, what outputs it produced, what objects it may have used, what it’s about, and where, when, and who was involved. 

Given a research project X documented using the KNOT Data Model and with the minimum available amount of information about it, we should be able to retrieve the following statements: 
- Project X involved at least one sub-activity or task, which is classified using a relevant controlled vocabulary such as Tadirah.
- Project X produced at least one output, which is classified using the KNOT Taxonomy.
- Project X may have used some existing object in the course of its activity.
- Project X is about subject Y.
- Project X took place in at least one location, began and ended at specific dates that include at least the year, and involved at least one person.

# Competency Questions 

Prefixes
```
PREFIX : <http://purl.org/knot/ontology/data/> 
PREFIX xsd: <http://www.w3.org/2001/XMLSchema#> 
PREFIX prov: <http://www.w3.org/ns/prov#> 
PREFIX dct: <http://purl.org/dc/terms/> 
PREFIX crm: <http://cidoc-crm.org/cidoc-crm/7.1.2/> 
PREFIX tadirah: <https://vocabs.dariah.eu/tadirah/> 
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#> 
PREFIX foaf: <http://xmlns.com/foaf/0.1/>
```

## CQ1.1 Sub-activities or tasks within the research project 

Given a research project X, what are the sub-activities / tasks that informed the project?  

```
SELECT distinct ?activity ?label ?type

WHERE {?project a prov:Activity .
?project prov:wasInformedBy ?activity .
?activity rdfs:label ?label .
OPTIONAL {?activity crm:P2_has_type ?type}}
```

_Outcome_

A list of the activities that informed the overall research project along with their labels and types, based on the Tadirah vocabulary of digital research activities in the humanities. 

_Example_ 

Activity | Label | Type
--- | --- | ---
<http://purl.org/knot/ontology/data/platform_creation> | Creation of ALCIDE platform | <https://vocabs.dariah.eu/tadirah/creating>
<http://purl.org/knot/ontology/data/corpus_creation> | Creation of de Gasperi corpus	| <https://vocabs.dariah.eu/tadirah/annotating>
<http://purl.org/knot/ontology/data/edition_digitization>	| Digitization of de Gasperi edition	| <https://vocabs.dariah.eu/tadirah/capturing>

## CQ1.2 Research project outputs 

What digital objects did project X produce? 

```
SELECT distinct ?result ?type ?title

WHERE {?project prov:generated ?result .
?result dct:title ?title .
OPTIONAL {?result dct:type ?type} }
```

_Outcome_

A list of digital objects that were produced by the research project activity alongside their title and, if available, their type based on the KNOT taxonomy of research project outputs. 

_Example_ 

Result | Type | Title
--- | --- | ---
http://purl.org/knot/ontology/data/alcide_website | | "ALCIDE website"@en
http://purl.org/knot/ontology/data/alcide_corpus	| http://purl.org/knot/taxonomy#corpus	| "De Gasperi's Corpus"@en
http://purl.org/knot/ontology/data/alcide_platform	| http://purl.org/knot/taxonomy#digital_platform	| "ALCIDE platform"@en

## CQ1.3 Research project inputs 

What objects, if any, did project X use as inputs in the research? 

```
SELECT distinct ?object ?value ?title

WHERE {?project prov:used ?object .
?object dct:title ?title . 

OPTIONAL {?object prov:value ?value} }
```

_Outcome_

A list of entities that a research project used in the course of its activity such as physical or digital objects along with their titles and, if available, a URI. 

_Example_ 

Object | Value | Title
--- | --- | ---
http://purl.org/knot/ontology/data/alcide_edition | https://www.mulino.it/collana/scritti-discorsi-de-gasperi	|  "A. De Gasperi, Scritti e discorsi politici, I-IV"@it

## CQ1.4 Research project location, timespan, and associated agents 

Where did project X take place, when did it begin and end, and who was involved? 

```
SELECT distinct ?location ?agent ?startdate ?enddate
{
{?project prov:atLocation ?location }
UNION {?activity crm:P7_took_place_at ?location }
UNION {?project prov:wasAssociatedWith ?agent }
UNION {?activity crm:P14_carried_out_by ?agent }
UNION {?project prov:startedAtTime ?startdate ; prov:endedAtTime ?enddate }}
```

_Outcome_

A summary of the available information about where the project took place, who was associated with it, and when it began and ended. 

_Example_ 

Trento, Bologna (locations); 2015/01/01 (start date); 2018/12/31 (end date); Maurizio Cau, Matteo Largaiolli, Sara Tonelli, Giovanni Moretti, Rachele Sprugnoli, Matteo Moretti, Il Mulino (agents).

## CQ1.5 Research project subject/topic

What are the primary subjects/topics of project X? 

```
SELECT ?subject ?title ?value
WHERE {
?project dct:subject ?subject .
OPTIONAL {?object prov:value ?value ; dct:title ?title} }
```

_Outcome_

The subject/topic, or subjects/topics, of the research project alongside their label, if available, and a URI of their representation in an authority control. 

_Example_ 

Subject | Label | Value
--- | --- | ---
http://purl.org/knot/ontology/data/alcide_de_gasperi |	"Alcide de Gasperi"@it | https://viaf.org/viaf/32011324
https://vocabs.dariah.eu/tadirah/contentAnalysis	| | 
http://purl.org/knot/thesaurus#data_viz	| | 
