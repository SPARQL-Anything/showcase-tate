# SPARQL Anything showcase: open data from the Tate Gallery

This showcase provides examples of using SPARQL Anything to query [open data from the Tate Gallery collection](https://github.com/tategallery/collection).
The repository is included as Git submodule in folder `collection`.

In what follows, `fx` is a placeholder for `java -jar sparql-anything-<version>.jar`.
See the SPARQL Anything [usage documentation](https://sparql-anything.readthedocs.io/en/latest/#usage) for details on Java options such as enabling logging.


## Artists as Schema.org
The query generates a Schema.org description of artists from the CSV file.

|  | Extract artworks and subjects |
|:----------|:-------------|
| __Query__ | [queries/artists.sparql](queries/artists.sparql) |
| __Input__ | [collection/artist_data.csv](collection/artist_data.csv) |
| __Output__ | [artists.ttl](artists.ttl) |
| __Type__ | CONSTRUCT |
| __Options__ | csv.headers=true |
| __Formats__ | CSV
| __Level__ | Novice 


Usage:

```
fx -q queries/artists.sparql -f TTL -o artists.ttl
```

Output excerpt from `artists.ttl`:

```turtle
@prefix schema: <http://schema.org/> .
@prefix fx:    <http://sparql.xyz/facade-x/ns/> .
@prefix dct:   <http://purl.org/dc/terms/> .
@prefix rdf:   <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix xyz:   <http://sparql.xyz/facade-x/data/> .
@prefix tate:  <http://sparql.xyz/example/tate/> .
@prefix tsub:  <http://sparql.xyz/example/tate/subject/> .
@prefix rdfs:  <http://www.w3.org/2000/01/rdf-schema#> .

tate:artist-1218  a        schema:Person ;
        rdfs:label         "Greiffenhagen, Maurice" ;
        schema:birthDate   "1862" ;
        schema:birthPlace  "London, United Kingdom" ;
        schema:deathDate   "1862" ;
        schema:deathPlace  "London, United Kingdom" ;
        schema:gender      "Male" ;
        schema:url         "http://www.tate.org.uk/art/artists/maurice-greiffenhagen-1218" .

tate:artist-10241  a       schema:Person ;
        rdfs:label         "Apóstol, Alexander" ;
        schema:birthDate   "1969" ;
        schema:birthPlace  "Barquisimeto, Venezuela" ;
        schema:deathDate   "1969" ;
        schema:deathPlace  "" ;
        schema:gender      "Male" ;
        schema:url         "http://www.tate.org.uk/art/artists/alexander-apostol-10241" .
...
```

## Extract artworks and subjects
This query combines two `x-sparql-anything` transformation. The first, iterates over the CSV file `artworks_data.csv`. For each one of the artworks, the Tate Gallery open data includes a JSON file with some more details, including a list of annotated subjects. The query finds the related JSON and queries it to retrieve the subjects. The output is projected in a KG of artworks and subjects, via a `CONSTRUCT` query.

| Title | Extract artworks and subjects |
|:----------|:-------------|
| __Query__ | [queries/arts-and-subjects.sparql](queries/arts-and-subjects.sparql)
| __Input__ | [collection/artworks_data.csv](collection/artworks_data.csv), [collection/artworks/](collection/artworks/)
| __Output__ | [arts-and-subjects.ttl](arts-and-subjects.ttl) |
| __Type__ | CONSTRUCT |
| __Options__ | csv.headers=true |
| __Formats__ | CSV, JSON
| __Level__ | Novice

Run the example as follows:

```
fx -q queries/arts-and-subjects.sparql -f TTL -o arts-and-subjects.ttl
```


## Build a SKOS taxonomy of subjects
This example explores all the JSON files of the open data collection and generates a unified SKOS taxonomy of all artwork subject annotations of the Tate Gallery open dataset.


| Title | Build a SKOS taxonomy of subjects |
|:----------|:-------------|
| __Query__ | [queries/subjects-as-skos.sparql](queries/subjects-as-skos.sparql)
| __Input__ | [collection/artworks_data.csv](collection/artworks_data.csv), [collection/artworks/](collection/artworks/)
| __Output__ | [subjects.ttl](subjects.ttl) |
| __Type__ | CONSTRUCT |
| __Options__ | csv.headers=true |
| __Formats__ | CSV, JSON
| __Level__ | Novice

Run the query as follows:

```
fx -q queries/subjects-as-skos.sparql -f TTL -o subjects.ttl
```

Output (excerpt, see `subjects.ttl`):
```
@prefix fx:    <http://sparql.xyz/facade-x/ns/> .
@prefix rdf:   <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix xyz:   <http://sparql.xyz/facade-x/data/> .
@prefix tate:  <http://sparql.xyz/example/tate/> .
@prefix skos:  <http://www.w3.org/2004/02/skos/core#> .
@prefix rdfs:  <http://www.w3.org/2000/01/rdf-schema#> .
@prefix tsub:  <http://sparql.xyz/example/tate/subject/> .

tsub:852  a            skos:Concept ;
        rdfs:label     "condom" ;
        skos:broader   tsub:88 ;
        skos:inScheme  tsub:subjects .

tsub:2166  a           skos:Concept ;
        rdfs:label     "Caiaphas" ;
        skos:broader   tsub:134 ;
        skos:inScheme  tsub:subjects .

tsub:18356  a          skos:Concept ;
        rdfs:label     "New York, Rockefeller Center" ;
        skos:broader   tsub:0 ;
        skos:inScheme  tsub:subjects .

tsub:15852  a          skos:Concept ;
        rdfs:label     "Texel" ;
        skos:broader   tsub:111 ;
        skos:inScheme  tsub:subjects .
		...
```

## Generate a CSV list of subjects
This example extracts all subjects from the JSON files in `collections/artworks/` and return a distinct set of subjects as CSV.

| Title | Generate a CSV list of subjects |
|:----------|:-------------|
| __Query__ | [queries/subjects-list.sparql](queries/subjects-list.sparql)
| __Input__ | [collection/artworks_data.csv](collection/artworks_data.csv), [collection/artworks/](collection/artworks/)
| __Output__ | [subjects.csv](subjects.csv) |
| __Type__ | SELECT |
| __Options__ | csv.headers=true |
| __Formats__ | CSV, JSON
| __Level__ | Novice

Run the query as follows:

```
fx -q queries/subjects-list.sparql -f CSV -o subjects.csv
```

## Generate a CSV list of the subjects hierarchy
This example extracts all subjects from the JSON files in `collections/artworks/` and return the whole hierarchy as CSV table.

| Title | Generate a CSV list of the subjects hierarchy |
|:----------|:-------------|
| __Query__ | [queries/subjects-hierarchy.sparql](queries/subjects-hierarchy.sparql)
| __Input__ | [collection/artworks_data.csv](collection/artworks_data.csv), [collection/artworks/](collection/artworks/)
| __Output__ | [subjects.csv](hierarchy.csv) |
| __Type__ | SELECT |
| __Options__ | csv.headers=true |
| __Formats__ | CSV, JSON
| __Level__ | Novice

Run the query as follows:

```
fx -q queries/subjects-hierarchy.sparql -f CSV -o hierarchy.csv
```

## Generate a CSV of artworks + related subjects
This query combines two `x-sparql-anything` transformation. The first, iterates over the CSV file `artworks_data.csv`. For each one of the artworks, the Tate Gallery open data includes a JSON file with some more details, including a list of annotated subjects. The query finds the related JSON and queries it to retrieve the subjects. The output is projected in a KG of artworks and subjects, via a `CONSTRUCT` query.

| Title | Generate a CSV of artworks + related subjects |
|:----------|:-------------|
| __Query__ | [queries/arts-and-subjects-list.sparql](queries/arts-and-subjects-list.sparql)
| __Input__ | [collection/artworks_data.csv](collection/artworks_data.csv), [collection/artworks/](collection/artworks/)
| __Output__ | [arts-and-subjects-list.csv](arts-and-subjects-list.csv) |
| __Type__ | SELECT |
| __Options__ | csv.headers=true |
| __Formats__ | CSV, JSON
| __Level__ | Novice

Run the example as follows:

```
fx -q queries/arts-and-subjects-list.sparql -f CSV -o arts-and-subjects-list.csv
```

## Count artworks for each subjects
This example is a process divided in two steps.

**Step 1:** Generate a table associating subjectId and artworkId. The table is produced by querying an RDF file of artworks and subjects. I this example, the input is an RDF and the output is CSV!

| Title | Count artworks for each subjects (Part 1) |
|:----------|:-------------|
| __Query__ | [queries/subjects-artworks-id.sparql](queries/subjects-artworks-id.sparql)
| __Input__ | [arts-and-subjects.ttl](arts-and-subjects.ttl)
| __Output__ | [subjects-artworks-id.csv](subjects-artworks-id.csv) |
| __Type__ | SELECT |
| __Options__ |  |
| __Formats__ | - |
| __Level__ | Novice

Run the example as follows:

```
fx -q queries/subjects-artworks-id.sparql -o subjects-artworks-id.csv -f CSV -l arts-and-subjects.ttl
```

**Step 2:** Reading the CSV table and counting the number of artworks for each subject

| Title | Count artworks for each subjects (Part 2) |
|:----------|:-------------|
| __Query__ | [queries/subjects-artworks-count.sparql](queries/subjects-artworks-count.sparql)
| __Input__ | [subjects-artworks-id.csv](subjects-artworks-id.csv)
| __Output__ | [subjects-artworks-count.csv](subjects-artworks-count.csv) |
| __Type__ | SELECT |
| __Options__ |  |
| __Formats__ | - |
| __Level__ | Novice

Run the example as follows:

```
fx -q queries/subjects-artworks-count.sparql -o subjects-artworks-count.csv -f CSV
```

## Generate CSV of artworks + subjects + link to images
```
fx -q queries/subjects-artworks-images.sparql -o subjects-artworks-images.csv -f CSV -l arts-and-subjects.ttl 
```

## Generate CSV of id + materials
```
fx -q queries/materials.sparql -o materials.csv -f CSV

## Generate CSV of id + time
```
fx -q queries/time.sparql -o time.csv -f CSV
```