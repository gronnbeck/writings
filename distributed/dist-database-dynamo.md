### Dynamo og moderne distribuerte databaser
I 2007 definerte Amazon industristandarden nosql-databaser skulle fungere.
Dynamo [4] er en nosql-database optimalisert for ett høyt antall writes og ble designet for å løse
Amazons problem med å lagre data under perioder med mye last;
for eksempel Black Friday, juletider og andre perioder der Amazon opplevde unormal
mengde med trafikk. For å oppnå dette trengte Amazon en database som kunne skalere.
I artikkelen om Dynamo blir disse ideene og teknikkene for å løse deres problemer presentert i detalj.
Blant annet ideer for klient-side konflikthåndtering, distribuering av data
basert på consisting hashing, replikering av data og feilhåndtering.
Ideer vi fortsatt ser i moderne databaser i dag.

I de siste årene har distribuert teori, systemer og rammeverk florert.
Et vanvittig antall med "nosql" databaser har blitt introdusert. Cassandra, MongoDB, Voldemort
og CouchDB er noen få eksempler på hva som finnes i dag. Og jeg tror vi kun har sett
toppen av isfjellet for distribuerte (nosql) databaser.
En viktig milepel for distribuert teori og for fremgangen innen for utvikling av nye
distribuerte systemer er konensusalgoritmen Raft.
