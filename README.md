# SPARQL Anything: open data from the Tate Gallery

This project includes examples using open data from the Tate Gallery collection.

In what follows, `fx` is `java -jar sparql-anything-<version>.jar`.


## Extract artists
```
fx -q queries/artists.sparql -f TTL -o artists.ttl
```
Output excerpt from `artists.ttl`:
```
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
```
fx -q queries/arts-and-subjects.sparql -f TTL -o arts-and-subjects.ttl
```

## Build a SKOS taxonomy of subjects

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
```
fx -q queries/subjects-list.sparql -f CSV -o subjects.csv
```

## Generate a CSV list of the subjects hierarchy
```
fx -q queries/subjects-hierarchy.sparql -f CSV -o hierarchy.csv
```

## Generate a CSV of artworks + related subjects
```
fx -q queries/arts-and-subjects-list.sparql -f CSV -o arts-and-subjects-list.csv
```

## Subjects artworks count
Generate a table associating subjectId and artworkId
```
fx -q queries/subjects-artworks-id.sparql -o subjects-artworks-id.csv -f CSV -l arts-and-subjects.ttl
```
Reading the table and counting the number of artworks for each subject
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