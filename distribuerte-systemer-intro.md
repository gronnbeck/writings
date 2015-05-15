# En kort historie om distribuerte systemer

Det er ikke lenge siden det holdt med én maskin for å kjøre all slags software:
de fleste problemer kunne løses ved å kaste raskere maskinvare på maskinen du brukte.
Hvis rask nok maskinvare ikke var tilgjengelig, eller du ikke hadde råd,
kunne du la Moore's Lov gjøre jobben sin og vente på at maskinvaren enten ble rask eller billig
nok til å løse akkurat dine problemer.

> "Moore's law" is the observation that, over the history of computing hardware,
> the number of transistors in a dense integrated circuit doubles
> approximately every two years.

Tidene har endret seg. Vi kan ikke lenger vente på at Moore's Lov gjør jobben sin.
En stor del av det er fordi vi produsere mer data enn vi noen gang har gjort.
På grunn av denne vektsten av data kan vi ikke lenger lagre all dataen på en og samme maskin.
Og vi kan heller ikke prosessere datene med én prosessor på én maskin hvis vi ønsker at jobben 
ikke skal bruke mer enn et år på å fullføre. Vi tar derfor i bruk teknikker fra fagfeltet for
distribuerte systemer som distribuert lagring og prosessering for å løse disse problemene.
Men før vi kaster oss over de mest sentrale emnene innenfor distribuerte systemer i dag, 
er viktig at vi tar en titt på hvordan fagfeltet har utviklet seg fra starten
av 1970-tallet til i dag.  

## Historie

I starten av 1970-tallet var det mye fokus på å løse problemer som rekkefølge av hendelser og konsensus mellom maskiner.
Vitenskapelige artikler som "Time, Clocks, and the Ordering of Events in a Distributed System" [1]
og "Paxos" av Lamport [2] var sentrale, og de er fortsatt er kjent den dag i dag. 
Paxos, en algoritme for å oppnå konsensus i ett netverk av maskiner, er mye i bruk i reelle systemer, 
på tross av at den er tilnærmelig umulig å forstå.
Det har siden blitt introdusert flere konsensusalgoritmer - jeg kommer tilbake til de senere.

Fagfeltet var lenge usynlig for industrien, men da Google ga ut sin første åpne forskningsartikkel
om MapReduce i 2004 [3] fikk man plutselig øynene opp for hva distribuerte systemer kan bidra med
på industriell skala. Det var på det tidspunket det veldig ofte misbrukte og misforståtte ordet "Big Data" oppsto.

Det startet bølgen for distribuert dataprosessering på kartet,
men hva er egentlig MapReduce? MapReduce er et rammerverk for distribuert prosessering av data, med en veldig enkel
prosesseringsmodell, som hadde som mål å gjøre dataprosessering enkelt, skalerbart og raskt
på terrabytes av data. Det fantes allerede løsninger som kunne operere på datamengder av denne størrelsen, men
disse led ofte av komplekse datamodeller og måtte kjøres på dyr hardware kalt "superdatamaskiner". Det som virkelig
gjorde MapReduce unik var at den åpnet for prosessering av store mengder data på vanlige datamaskiner.

At Google kunne prosessere terrabytes av data på helt vanlige maskiner er en effekt av at Google
introduserte et konsept om feiltoleranse. Maskiner kunne kræsje, med noen unntak,
uten at prosesseringsjobben måtte starte på nytt. Feiltoleranse er et begrep
som blir mye brukt om distribuerte systemer i dag, og spiller en sentral rolle
i CAP teoremet. Kort fortalt forklarer CAP teoremet at et distribuert system
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
Jeg synes fagfeltet for distribuert dataprosessering er svært interessant og kommer nok til å 
diskutere dette mer i dybden i en senere post.

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

For omtrent et år siden ble paperet om Raft gitt ut, og kort tid etter begynte
mange nye distribuert løsninger som Consul og etcd å ta i bruk algoritmen. 
Raft skal være en enklere og mer foreståelig konsensusalgoritme enn Paxos.
Hvis vi tar i betrakning det antallet av nye implementasjoner og rammeverk som tar i bruk algoritmen,
som alle har dukket opp på litt over ett år, ser det ut til å være sant [7]. 

## Hvorfor?
I dag blir distribuerte systemer hovedsaklig forbundet med nosql databaser, og mer og mer på
mikrotjeneste arkitekturen. Men jeg tror svært få har et forhold
til hvorfor man trenger distribuerte systemer. Mange mener nok at det er for ytelsensskyld.
Som koker ned til spørsmålet om hvor mye data du kan kverne per time.
Men jeg mener feltet er større enn som så.
Distribuerte systemer er mer enn å få en mengde med maskiner til å fullføre en
jobb så fort som mulig. Distribuerte systemer handler i tillegg til ytelse om
isolasjon og tilgjengelighet.

Med isolasjon mener vi at vi nødvendigvis ikke har lyst til å kjøre alle
applikasjoner på samme server. Hva hvis en applikasjon med et sikkerhetshull blir
utsatt for et angrep av en ondsinnet person kan alle andre tjenester på samme server være utsatt.
Isolasjon koker ned til om en feil i en applikasjon skal påvirke andre applikasjoner?

Tilgjengelighet går ut på hva som skjer hvjs man mister en server eller om en
database-server tar fyr. Skal en tjeneste slutte å svare fordi en node i et cluster
faller ned? Eller om en brann på serverrommet skal føre til at vi mister all foretningskritiske data?

Og til slutt selvfølgelig ytelse. Som  er spørsmålet rundt hvordan
kan vi bruke flere resursser til å løse samme problemet raskere. Men vi kan også tenke på ytelse
i form av skalerbarhet. Der man ønsker å data som er for store til å kunne lagres på kun én maskin. 

Disse prinsippene er eksluderer ikke hverandre, men mer komprimisser der man må vrake noe for å få
noe annet. Så du lurer sikker på hvordan. Det er her distribuert system teori
kommer inn.

Men før vi kaster oss ut i fagfeltet så må vi snakke om
de mest vanlige feilantagelsene programmerere, med lite erfaring med distribuerte,
systemer gjør. Disse feilantagelsene blir i fagfeltet kalt for
"The Fallacies of Distribited Computing"


