### Distribuert prosessering
MapReduce startet bølgen for å sette distribuert dataprosessering på kartet.
Men hva er egentlig MapReduce? MapReduce er et rammerverk for distribuert prosessering av data, med en veldig enkel
prosesseringsmodell, som hadde som mål å gjøre dataprosessering enkelt, skalerbart og raskt
over terrabytes av data. Det fantes allerede løsninger som kunne operere på datamengder av slike størrelser, men
disse led ofte av komplekse datamodeller og måtte kjøres på dyr hardware kalt "superdatamaskiner". Det som virkelig
gjorde MapReduce unik var at den åpnet for prosessering av store mengder data på vanlige datamaskiner.

At Google kunne prosessere terrabytes av data på helt vanlige maskiner er en effekt av at Google
bygde MapReduce rundt faktumet at feil vil oppstå. Maskiner kunne kræsje, med noen unntak,
uten at prosesseringsjobben måtte starte på nytt. Dette gjorde at utviklerne som bruker
MapReduce kunne fokusere på funksjonalitet - uten å bekymre seg om feilhåndtering.
Feiltoleranse er et begrep som blir mye brukt om distribuerte systemer i dag, og spiller en
sentral rolle i CAP teoremet. Kort fortalt forklarer CAP teoremet at et distribuert system
ikke kan oppfylle alle følgende egenskaper: konsistens (Consistency),
tilgjengelighet (Availability) og partisjonstoleranse (Parition tolerance).
CAP blir ofte brukt til å klassifisere hvilke krav et et distribuert system,
ofte distribuerte databaser, tilfredstiller. Jeg går dypere inn i CAP teoremet om litt.

MapReduce-modellen var basert på det som kalles batch-orientert dataprosessering.
Siden MapReduce først ble introdusert har dataprosessering beveget seg vekk
fra batch-orientering til strøm-orientert dataprosessering.
Det var Nathan Marz sammen med BackType, som senere ble kjøpt opp av twitter,
som introduserte Storm, et distribuert strøm-orientert dataprosesseringsrammeverk.
Strøm-orientert dataprosessering forenklet modellen for å gjøre sanntidsberegninger på store
mengder data. Noe som ikke er lett å få til i MapReduce sin beregningsmodell. MapReduce-modellen
er såpass godt forstått, integrert og raskt at det fortsatt i stor grad brukes i dag.
