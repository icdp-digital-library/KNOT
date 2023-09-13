# Motivating Scenario

Retrieve information about the entities that a research project used and/or produced such as … .

Given a research project X documented using the KNOT Data Model with a minimum available amount of information about it, we should be able to retrieve the following statements:

- Project X involved... 

# Competency Questions

Prefixes

```
PREFIX : <http://purl.org/knot/ontology/data/>
PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
PREFIX prov: <http://www.w3.org/ns/prov#>
PREFIX dct: <http://purl.org/dc/terms/>
PREFIX crm: <http://cidoc-crm.org/cidoc-crm/7.1.2/>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX dcat: <http://www.w3.org/ns/dcat#>
PREFIX frbroo: <http://iflastandards.info/ns/fr/frbr/frbroo/>

```

## CQ1.1 Data formats

What data formats are the outputs of research project X available in?  

```
SELECT ?output ?data_format ?compression_format
WHERE {
?output a dcat:Dataset ; dcat:distribution ?distribution .
?distribution dct:format ?data_format ; dcat:compressFormat ?compression_format  }

```

*Outcome*

A list of the data formats available for the project's outputs alongside their URI to the EU File Type authority control. 

*Example*

| Output | Data Format | Compression Format |
| --- | --- | --- |
| http://purl.org/knot/ontology/data/alcide_corpus | [TSV](http://publications.europa.eu/resource/authority/file-type/TSV) | [ZIP](http://publications.europa.eu/resource/authority/file-type/ZIP) |
| http://purl.org/knot/ontology/data/alcide_corpus | [XML](http://publications.europa.eu/resource/authority/file-type/XML) | [ZIP](http://publications.europa.eu/resource/authority/file-type/ZIP) |
| http://purl.org/knot/ontology/data/alcide_corpus | [TXT](http://publications.europa.eu/resource/authority/file-type/TXT) | [ZIP](http://publications.europa.eu/resource/authority/file-type/ZIP) |

## CQ1.2 Output types

What types of outputs did research project X produce? 

```
SELECT distinct ?output ?title ?type

WHERE {?project prov:generated ?output .
?output dct:title ?title ; dct:type ?type} }

```

*Outcome*

A list of digital objects that were produced by the research project activity alongside their title and their type based on the KNOT taxonomy of research project outputs.

*Example*

| Result | Type | Title |
| --- | --- | --- |
| http://purl.org/knot/ontology/data/alcide_corpus | http://purl.org/knot/taxonomy#corpus | "De Gasperi's Corpus"@en |
| http://purl.org/knot/ontology/data/alcide_platform | http://purl.org/knot/taxonomy#digital_platform | "ALCIDE platform"@en |

## CQ1.3 Licenses attached to research project 

What licenses are the outputs of research project X available under, if any? 

```
SELECT distinct ?output ?license

WHERE {?project prov:generated ?output .
?output dct:title ?title  .
OPTIONAL {?output dct:license ?license} }

```

```
SELECT distinct ?distribution ?license

WHERE {?output dcat:distribution ?distribution .
?distribution dct:title ?title  .
OPTIONAL {?distribution dct:license ?license} }

```

*Outcome*

A list of available licenses, and their URI to the Licenze controlled vocabulary, attached to the outputs of the research project as well as a list of licenses attached to the downloadable versions of the outputs. 

*Example*

| Output | License |
| --- | --- |
| De Gasperi's Corpus | [CC BY NC SA 4.0](https://w3id.org/italia/controlled-vocabulary/licences/B19_CCBYNCSA40) |
| ALCIDE website | |
| ALCIDE platform | |
| De Gasperi's Corpus - Raw Text Files | [CC BY NC SA 4.0](https://w3id.org/italia/controlled-vocabulary/licences/B19_CCBYNCSA40) |
| De Gasperi's Corpus - TSV Files | [CC BY NC SA 4.0](https://w3id.org/italia/controlled-vocabulary/licences/B19_CCBYNCSA40) |
| De Gasperi's Corpus - XML Files | [CC BY NC SA 4.0](https://w3id.org/italia/controlled-vocabulary/licences/B19_CCBYNCSA40) |


## CQ1.4 Accessing outputs

Who can access the outputs of research project X? 

```
SELECT distinct ?output ?access_rights

WHERE {?project prov:generated ?output .
?output dct:title ?title  .
OPTIONAL {?output dct:accessRights ?access_rights} }

```

*Outcome*

A list of outputs from the research project alongside a statement on their accessibility based on the EU Access Right authority control. 

*Example*

| Output | Access Right |
| --- | --- |
http://purl.org/knot/ontology/data/alcide_corpus	
| De Gasperi's Corpus | [PUBLIC](http://publications.europa.eu/resource/authority/access-right/PUBLIC) |
| ALCIDE website | [PUBLIC](http://publications.europa.eu/resource/authority/access-right/PUBLIC) |
| ALCIDE platform | [PUBLIC](http://publications.europa.eu/resource/authority/access-right/PUBLIC) |

## CQ1.5 Digitized objects 

What physical objects did research project X digitize, if any, and which output used them? 

```
SELECT distinct ?object ?object_title  ?digitized_object ?digitized_object_label ?output 
WHERE { ?object a frbroo:F5_Item ; dct:title ?object_title  . 
?activity crmdig:L1_digitized ?object ; crmdig:L20_has_created ?digitized_object .
?digitized_object rdfs:label ?digitized_object_label .
?output crm:P106_is_composed_of ?digitized_object }

```

*Outcome*

A list of the physical objects a research project digitized, if any, along with its label, its digitized form, along with its label, and the output(s) that used them. 

*Example*

| Object | Label | Digitized Object | Label | Output 
| --- | --- | --- | --- | --- |
http://purl.org/knot/ontology/data/edition_item	| "A. De Gasperi, Scritti e discorsi politici, I-IV"@it	| http://purl.org/knot/ontology/data/edition_digitized	| "Digitized form of the edition"@en	| http://purl.org/knot/ontology/data/alcide_corpus |
