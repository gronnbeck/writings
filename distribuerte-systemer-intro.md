# En kort historie om distribuerte systemer (DRAFT 1)

## Tema
* Historie
  - I dag
* Hvorfor?
* Teori
  - Konsensus
    * Byzantine General Problem
  - Fallacies of distributed systems
* Microsystems

## Historie
Det er ikke lenge siden det holdt med en maskin for å kjøre all slags applikasjoner.
De fleste problemer kunne løses med å kaste raskere hardware på maskinen din og problemet var løst.
Og hvis rask nok hardware ikke fantes eller du ikke hadde råd til den hardwaren du ønsket deg.
Kunne du la Moore's Lov gjøre jobben sin og vente på at hardwaren enten ble rask eller billig
nok til å løse dine problemer.

Tidene har endret seg og antallet av transistorer doubles ikke for hvert annet år lenger.
I det minste ikke for en kjerne. I dag observerer vi Moore's Lov i form av flere kjener
og ikke mer kraft. Dette har ført til at vi som utviklere måtte finne andre løsninger
for å utnytte oss av tankene rundt flere kjener. Historisk sett er ikke dette helt sant.
Skal jeg være helt ærlig så har fagfeltet for distribuete systemer eksistert helt siden
slutten av 1970-tallet. Man kan argumenteret at tankene og ideene bak distribuerte systemer
allerede startet med introduksjonen av ARPANET, som vi i dag kjenner som internett,
på 1960-tallet.

I starten var det mye fokus på rekkefølge av hendelser og konsensus mellom maskiner.
Der vitenskaplige artikler som
"Time, Clocks, and the Ordering of Events in a Distributed System" og "Paxos" av
Lamport fortsatt er kjent og mye brukt den dag i dag. Det skal være sagt at
Paxos den dag i dag fortsatt er tilnærmert umulig å forstå. Og siden har
flere konsensuslagoritmer blitt introdusert, jeg kommer tilbake til dem senere.

Lenge var fagfeltet unsynelig for industrien. Og industrien brydde seg fint lite
om hva som skjedde innen for fagfeltet. Men da Google gav ut sitt første whitepaper
på MapReduce i 2004 begynte industrien virkelig å jobbe med distribuerte systemer.
Og det forfærdelige, misforståtte og uklare buzzwordet BigData ble født.

MapReduce starter det jeg vil kalle for data-prosessering bølgen av distribuerte
systemer. MapReduce er et rammerverk med en enkel data-prosesserings model, som
skulle gjøre data-prosessering enkelt, skalerbart og raskt på terrabytes av data.
Ikke at slike løsninger ikke fantes fra før, men dette rammeverket var ikke
avhengig av en superdatamaskin. MapReduce kunne kjøre på helt vanlige maskiner!

At de kunne kjøre rammeverket på helt vanlige maskiner er en effekt av at Google
introduserte et konsept om feiltoleranse. Maskiner kunne kræsje, med noen unntak,
uten at prosesseringsjobben måtte starte på nytt. Feiltoleranse er et begrep
som blir mye brukt om distribuerte systemer i dag. Og spiller en sentral rolle
i CAP teoremet. Kort fortalt forklarer CAP teoremet at et distribuert system må
velge mellom å ha to av tre følgende egenskaper, konsistens (Consistency),
tilgjengelighet (Availability) og partisjons tolerance (Parition tolerance).
CAP blir ofte brukt til å klassifisere hvilke krav et et distribuert systemet,
ofte distribuerte databaser, tilfredstiller. Jeg kommer tilbake til CAP senere i denne posten.

Med Dynamo (2007) definerte Amazon hvordan NoSQL databaser skulle fungere.
Dynamo er en nosql database optimalisert for mange writes og ble designed for å løse
Amazons problem med å lagre data, f.eks. handlekurver, under perioder med mye last.
For eksempel Black Friday, juletider og andre perioder der Amazon opplevde unormal
mengde med trafikk. For å oppnå dette måte Amazon lage en database som kunne skalere.
I whitepaperet om Dynamo presenterte disse ideene og teknikkene. Blant annet ideer
for klient-side konflikthåndtering, distribuering av data
”basert på consisting hashing, replikering av data og feilhåndtering.
Ideer du ser i moderne databaser.

Siden har distribuert teori og rammeverk florert i industrien. Mer forståelige
konsensusalgoritmer som zab og raft har blitt forsket frem og introdusert.
Mengder nosql databaser har blitt introdusert. Cassandra, MongoDB, Voldemort
og CouchDB er noen få eksempler på hva som finnes i dag. Og vi har nok bare sett
toppen av isfjellet for distribuerte (nosql) databaser.

Innenfor data-prosessering har teknologien beveget seg vekk fra batch-orienterte Hadoop,
MapReduce sin open-source søster, til en strøm-orientert data-prosesseringsmodell,
da Nathan Mars sammen med BackType introduserte Storm. Strøm-orientert
data-prosessering forenklet modellen for å gjøre sanntidsberegninger på store
mengder data. Noe som ikke var like lett i MapReduce-modellen. MapReduce-modellen
og Hadoop er såpass godt forstått og raskt at det fortsatt i stor grad brukes
den dag i dag.


## Hvorfor?
I dag blir distribuerte systemer mest forbundet med nosql databaser, og kanskje
mikrotjeneste arkitekturen om du er hipp nok. Men jeg tror svært få har et forhold
til hvorfor man trenger distribuerte systemer. Mange mener nok, som jeg beskrev
under historie, at det er for ytelsensskyld. Og da ytelse som i rå kraft.
Hvor mye data du kan kverne per time. Men jeg mener feltet er større enn som så.
Distribuerte systemer er mer enn å få en mengde med maskiner til å fullføre en
jobb så fort som mulig. Distribuerte systemer handler i tillegg til ytelse om
isolasjon og tilgjengelighet. Og helt sikkert en hel del ting til.

Med isolasjon mener vi at vi nødvendigvis ikke har lyst til å kjøre alle
applikasjoner på samme server. Hva skjer hvis serveren faller ned eller blir utsatt
for ondsinnende. Isolasjon koker ned til om en feil i en applikasjon skal
påvirke andre applikasjoner?

Tilgjengelighet går med ut på hva som skjer hvis man mister en server eller en
database-server tar fyr. Skal en tjeneste slutte å svare fordi en node i et cluster
faller ned? Eller om vi skal mista all eller deler av foretningskritiske dataene
våre fordi en server tar fyr?

Og til slutt selvfølgelig ytelse. Som helt enkelt er spørsmålet rundt hvordan
kan vi bruke flere resursser til å løse samme problemet raskere. Disse prinsippene
er eksluderer ikke hverandre, men mer komprimisser der man må vrake noe for å få
noe annet. Så du lurer sikker på hvordan. Det er her distribuert system teori
kommer inn. Skjønnheten og den uendelige kompleksiteten bak konsensus, ... ,
algoritmer for tilgjengelighet og feilhåndtering ... Men først la meg introdusere
de største feilantagelsene programmerere med lite erfaring med distribuerte
systemer gjør. Disse feilantagelsene er i fagliteraturen ofte kalt for
"The Fallacies of Distribited Computing"

## The Fallacies of Distributed Computing
"The Fallacies of Distributed Computing" har vært kjent lenge, helt siden 1994,
men jeg mener disse punktene forstatt er gyldig. Nedenfor oppsumerer jeg disse
feilantagelsene:

1. Nettverket er pålitelig
2. Det er null forsinkelse på et nettverk
3. Båndbredde er uendelig
4. Nettverket er sikkert
5. Topologien i nettverket endrer seg ikke
6. The er alltid én nettverksadministrator
7. Transport er gratis
8. Nettverket er homogent

To eksempler på hva som kan skje ved å gå i fallgruven til en eller flere av
disse feilantagelsene er

1. Legger stort press på nettverket ved å legge unødvendig mye data på nettverket.
2. Nettverket består ofte av flere subnets, med flere én nettverksadministratorer
som beskytter sine interesser og har sine regler som en sender (et system) må
forholde seg til

Og det finnes en hel del andre feil og fallgruven man kan introdusere om man
ikke er forsiktig eller bruker kjente standarder. Hvorfor tar jeg opp dette?
Fordi det er viktig å vite om disse antagelsene og være klar over dem når
man skal bygge et eller vurdere et distribuert system. 


## Resursser
* http://en.wikipedia.org/wiki/Distributed_computing#History
* http://en.wikipedia.org/wiki/ARPANET
* http://web.stanford.edu/class/cs240/readings/lamport.pdf
* http://research.microsoft.com/en-us/um/people/lamport/pubs/paxos-simple.pdf
* http://www.allthingsdistributed.com/files/amazon-dynamo-sosp2007.pdf
* http://en.wikipedia.org/wiki/Fallacies_of_distributed_computing
