# En kort historie om distribuerte systemer

Det er ikke lenge siden det holdt med én maskin for å kjøre all slags software:
de fleste problemer kunne løses ved å kaste raskere maskinvare på maskinen du brukte.
Hvis rask nok maskinvare ikke var tilgjengelig, eller du ikke hadde råd,
kunne du la Moore's Lov gjøre jobben sin og vente på at maskinvaren enten ble rask eller billig
nok til å løse akkurat dine problemer.

> "Moore's law" is the observation that, over the history of computing hardware,
> the number of transistors in a dense integrated circuit doubles
> approximately every two years.

Tidene har endret seg. Selv om antall transistorer på en prosessor fortsatt dobles annenhvert år, dobler ikke hastigheten seg i samme hastighet. I dag observerer vi Moore's Lov i form av flere kjerner -
altså i henhold til prosesseringskraft fremfor hastighet. Dette har ført til at utviklere må finne andre løsninger
for å utnytte den nye kjernebaserte tankegangen.

// Hva er sammenhengen her?

Men hva har dette med distribuerte systemer å gjøre? Det viser seg at teknikker for å
optimalisere for hastighet ved hjelp av flerkjernede prosessorer også kan benyttes i forbindelse
med distribuerte systemer. (saus?) (hvorfor vil skaleringen være bedre i distribuerte systemer?)

Selv om det fremdeles er i en tidlig fase har fagfeltet for distribuerte systemer historisk sett
eksistert siden slutten av 1970-tallet. Man kan argumentere for at tankene og ideene bak distribuerte systemer
startet allerede med introduksjonen av ARPANET, som vi i dag kjenner som internett,
på 1960-tallet.

## Historie

I starten var det mye fokus på rekkefølge av hendelser og konsensus mellom maskiner.
Vitenskapelige artikler som "Time, Clocks, and the Ordering of Events in a Distributed System"
og "Paxos" av Lamport (saus) understøtter denne tankegangen, og de er fortsatt er kjent den dag
i dag. Paxos er mye i bruk i reelle systemer, på tross av at den er tilnærmelig umulig å forstå.
Det har siden blitt introdusert flere konsensusalgoritmer - jeg kommer tilbake til de senere.

Fagfeltet var lenge usynlig for industrien, men da Google ga ut sin første åpne forskningsartikkel
om MapReduce i 2004 (saus) fikk man plutselig øynene opp for hva distribuerte systemer kan bidra med
på industriell skala. Det var her det veldig ofte misbrukte og misforståtte ordet "BigData" oppsto.

Det startet en bølge av dataprosessering (gjerne over internett), og satte distribuerte systemer på kartet,
men hva er egentlig MapReduce? MapReduce er et distribuert rammerverk med en veldig simpel
dataprosesseringsmodell, som hadde ambisjoner om å gjøre dataprosessering enkelt, skalerbart og raskt
på terrabytes av data. Det fantes allerede løsninger som kunne operere på datamengder av denne størrelsen, men
disse led ofte av komplekse datamodeller og måtte kjøres på dyr harware kalt "superdatamaskiner". Det som virkelig
gjorde MapReduce unik var at den åpnet for prosessering av store mengder data på helt vanlige datamaskiner.

På dette tidspunktet fantes det løsninger som raskt prosessere terrabytes med data.
Men disse hadde ofte en komplex datamodel og var avhengig av å bli kjørt på superdatamaskin.
Det som gjør MapReduce unik var at det kunne prosessere store mengder data på helt vanlige maskiner!

At Google kunne prosessere terrabytes av data på helt vanlige maskiner er en effekt av at Google
introduserte et konsept om feiltoleranse. Maskiner kunne kræsje, med noen unntak,
uten at prosesseringsjobben måtte starte på nytt. Feiltoleranse er et begrep
som blir mye brukt om distribuerte systemer i dag. Og spiller en sentral rolle
i CAP teoremet. Kort fortalt forklarer CAP teoremet at et distribuert system må
velge ikke kan oppfylle alle følgende egenskaper, konsistens (Consistency),
tilgjengelighet (Availability) og partisjons tolerance (Parition tolerance).
CAP blir ofte brukt til å klassifisere hvilke krav et et distribuert systemet,
ofte distribuerte databaser, tilfredstiller. Jeg kommer tilbake til CAP senere i denne posten.


Med Dynamo i 2007 definerte Amazon industristandarden nosql databaser skulle fungere.
Dynamo er en nosql database optimalisert for mange writes og ble designed for å løse
Amazons problem med å lagre data, f.eks. handlekurver, under perioder med mye last.
For eksempel Black Friday, juletider og andre perioder der Amazon opplevde unormal
mengde med trafikk. For å oppnå dette måtte Amazon designe og implementere en database
som kunne skalere for å løse deres konkrete problem.
I artikkelen om Dynamo blir disse ideene og teknikkene presentert i god detalj. Blant annet ideer
for klient-side konflikthåndtering, distribuering av data
basert på consisting hashing, replikering av data og feilhåndtering.
Ideer vi fortsatt ser i moderne databaser i dag.

Siden har distribuert teori og rammeverk florert i industrien. Mer forståelige
konsensusalgoritmer som zab og raft har blitt forsket frem og introdusert.
Og vanvittige mange nosql databaser har blitt introdusert. Cassandra, MongoDB, Voldemort
og CouchDB er noen få eksempler på hva som finnes i dag. Og vi har nok bare sett
toppen av isfjellet for distribuerte (nosql) databaser.

MapReduce-modellen var basert på det som kalles en batch-orientert dataprosessering.
Siden MapReduce først ble introdusert har dataprosessering beveget seg vekk
fra batch-orientering til strøm-orientert dataprosessering.
Det var Nathan Marz sammen med BackType, som senere ble kjøpt opp av twitter,
som introduserte Storm et distribuert strøm-orienterte dataprosessering rammeverk.
Strøm-orientert data-prosessering forenklet modellen for å gjøre sanntidsberegninger på store
mengder data. Noe som ikke er lett å få til i MapReduce sin beregningsmodell. MapReduce-modellen
er såpass godt forstått, integrert og raskt at det fortsatt i stor grad brukes i dag.


## Hvorfor?
I dag blir distribuerte systemer hovedsaklig forbundet med nosql databaser, og mer og mer på
mikrotjeneste arkitekturen. Men jeg tror svært få har et forhold
til hvorfor man trenger distribuerte systemer. Mange mener nok at det er for ytelsensskyld.
Og da ytelse som i rå kraft.
Hvor mye data du kan kverne per time. Men jeg mener feltet er større enn som så.
Distribuerte systemer er mer enn å få en mengde med maskiner til å fullføre en
jobb så fort som mulig. Distribuerte systemer handler i tillegg til ytelse om
isolasjon og tilgjengelighet.

Med isolasjon mener vi at vi nødvendigvis ikke har lyst til å kjøre alle
applikasjoner på samme server. Hva hvis en applikasjon med et sikkerhetshull blir
utsatt for et angrep av en ondsinnet person kan alle andre tjenester på samme server være utsatt.
Isolasjon koker ned til om en feil i en applikasjon skal påvirke andre applikasjoner?

Tilgjengelighet går med ut på hva som skjer hvis man mister en server eller en
database-server tar fyr. Skal en tjeneste slutte å svare fordi en node i et cluster
faller ned? Eller om vi skal mista all eller deler av foretningskritiske dataene
våre fordi en server tar fyr?

Og til slutt selvfølgelig ytelse. Som helt enkelt er spørsmålet rundt hvordan
kan vi bruke flere resursser til å løse samme problemet raskere. Disse prinsippene
er eksluderer ikke hverandre, men mer komprimisser der man må vrake noe for å få
noe annet. Så du lurer sikker på hvordan. Det er her distribuert system teori
kommer inn. Skjønnheten og den uendelige kompleksiteten bak konsensus, og
algoritmer for tilgjengelighet og feilhåndtering, osv.

Først la meg introdusere
de største feilantagelsene programmerere med lite erfaring med distribuerte
systemer gjør. Disse feilantagelsene er i fagliteraturen ofte kalt for
"The Fallacies of Distribited Computing"

## The Fallacies of Distributed Computing
"The Fallacies of Distributed Computing" har vært kjent lenge, helt siden 1994,
men jeg mener disse punktene forstatt er gyldig. Nedenfor oppsumerer jeg disse
feilantagelsene:

1. Nettverket er pålitelig
2. Det er null forsinkelse på nettverket
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


## CAP Teoremet
CAP Teormet, også kalt Brewers teorem, som jeg så vidt nevnte tidligere
går som følgende. Et distribuert system system can ikke garantere alle følgende
egenskapene:

* Konsitens (Consitency)
* Tilgjengelighet (Availability)
* Partisjonstoleranse (Partition tolerance)

Før jeg begynner vil jeg poengtere at CAP har mange forskjellige
tolkninger, og at dette er min tolkning og forståelse av CAP teoremet.

En ekstrem tolkning av dette er at man får systemer som tilfredstiller to av tre av
disse egeneskapene. Man får systemer som f.eks. er

 * CP, konsistens og partisjonstoleranse,
 * AP, tilgjengelighet og partisjonstoleranse, og
 * CA, konsistens og tilgjengelighet,

men hvilke tradeoffs gjør man typisk for få de egenskapene man ønsker i et distribuert system.
Jeg kommer til å bruke denne ekstreme tolkning for å illsutrere de forskjellige
tilpassningene man kan gjøre på konsistens, tilgjengelighet og partisjonstoleranse.
Men vær oppmerksom på at man ikke nødvendigvis trenger å bytte ut én egenskap for
å oppnå en annen.

La oss begynne med konsistens. Grovt sett har man to hovedkategorier av konsistens, de er "strongly consistent"
og "eventually consistent". Den første nevnte kan garantere at dataene du leser
fra én node vil være den samme for alle andre nodene i et kluster. Mens den eventually consistency
ikke kan garanterer at dataene du leser fra én node være det samme for en annen node i et kluster.

Tilgjengelighet handler om å garantere at en forespørsel skal få et svar om operasjonen
var vellykket eller har feilet. Ikke-tekniks betyr det at løsningen svarer.
Tilgjengelighet kan måles i form av oppetid. Et system som bytter bort oppetid
for for eksempel konsistens (CP) kan ikke garantere at man får respons før systemet kan
bekrefte at alle nodene er konsistente.
Og man risikere da at systemet ikke er tilgjengelig hvis en node i en kluster forsvinner.

Partisjonstoleranse er en type feiltoleranse som
sikter til hvor godt et distribuert system tolerer feil i deler av systemet (f.eks
  at en node faller ned) eller vilkårelig tap av meldinger. Det er ikke ofte
man lager systemer der man ofrer partisjonstoleranse (CA), men noen databasesystemer
ofrer, som Spinnaker, ofrer nettop partisjonstoleranse mellom datasenter for å oppnå
de egenskapene de ønsker innenfor et datasenter.

Til slutt vil jeg ta opp at CAP teoremet har mange kritikere blant annet at
CAP formelt sett ikke kan regnes som ett teorem, men heller en pragmatisk regel.
Mange kritiserer at CAP for ofte blir brukt for å bytte bort konsistens når de designer
et distirbuert system. En siste kritikk er at CAP teoremet har så mange
tolkninger, så må man være påpasselig når man diskuterer egenskapene til et
distribuert system ved hjelp av CAP teoremet.

## Konsensus
Konsensus har vært et løst problem innefor distribuerte systemer ganske lenge. Enkelt sett
er konsensus i distribuerte systemer en algoritme for få flere noder i et kluster
til å bli enig om noe. Med noe kan være å bli enig om hvilken node som er master ved
hvilke data som er korrekte. Konsensus er sterkt
knyttet til konseptet om konsistens som fra CAP teoremet. Og man trenger en
konsensusalgoritme med riktige tradeoffs for å få den konsistensmekansimen man ønsker.

Den mest kjente protkollen for konsensus må være Paxos. Paxos, av Leslie Lamport,
er en konsensusprotokoll som er "strongly consistent". Kort fortalt sørger Paxos for at noder i et nettverk
får konsensus, altså at de blir enige, i ett nettverk som kan være ustabilt og
ha upålitelige noder. Paxos er notorisk kjent for å være umulig å forstå, så jeg
vil ikke diskutere protokollen mer enn som så. Men er absolutt verdt å prøve å sette seg inn i.
For den interesserte anbefaler jeg å først lese
Wikipedia artikkelen om Paxos før du prøver deg på Lamports paper "Paxos Made Simple" fra 2001.
Og med det ønsker jeg deg lykke til!

I ettertid har det dukket opp flere konsensusprotokoller. Der de mest kjente
er Zab som brukes av Zookeeper og Raft implementert i Consul, etcd og en hel del
andre distribuerte systemer. For Zab anbefaler jeg å lese paperet om Zab og Zookeeper
for å få en forståelse av hva og hvorfor. Når det gjelder Raft er github
sidene om "Raft Consensus" veldig bra lesing.

### Replikering og konflikthåndtering
Konsensus og replikering av state er, som konsistens, også sterkt knyttet.
Uten en ordentlig konsensusalgoritme kan ikke noder i et distribuert system bli enige om hvilke
data som er korrekte.

Ved hjelp av replikering av data kan man enklere beskrive hva konsensus faktisk betyr.
For eksempel hvis vi har et distribuert system med tre noder, som granaterer deg eventuell konsistens
vil man potensielt kunne få opp til tre forskjellige versjoner av den samme
dataen. Vi har dermed fått en konflikt i dataene og den må håndteres.
Hvordan skal man finne ut hvilken data som er den riktige? I de fleste systemer
er det for mye jobb for mennekser å håndtere disse konfliktene manuelt. Så man
bør undersøke hvordan det kan gjøres automatisk.

Det er mange algoritmer for å håndtere replikering av data mellom nodene i et
distribuert system. De mest kjente kategoriene er Gossip Protocol (AP), Paxos (CP)
og Two-Phase Commits (CA). I Amazons DynamoDB bruker de en versjon av Gossip
Protokollen, men her er det klientene som har ansvar for konflikthåndtering.
Siden jeg synes det er mest interessant å se på hvordan DynamoDB gjør konflikthåndering,
vil jeg ikke gå i dyben på hvordan Gossip Protokollen fungerer. Jeg anbefaler
å lese mer om det på Wikipedia og i Amazons paper om DynamoDB.

Som sagt er det klientene som har ansvar for konfliktshåndtering i DynamoDB.
Helt overordnet så vil DynamoDB sende alle versjonen av et dokument
når en klient ber om et dokument som er i konflikt. Deretter er det opp til
klienten å velge eller flette sammen dokumentet for å håndtere konflikten.

Amazons handlekurv fungerer som en
god illustrasjon for hvordan man kan utføre konflikthåndtering. Deres strategi
for å håndtere en regersjon mellom versjoner av et dokument er å flette sammen
handlekurver. Det vil si at de tar en union av handlekurvene.

```
Handlekurv 1.0:
* Gjenstand 1
* Gjenstand 2
* Gjenstand 4

Handlekurv 1.1:
* Gjenstand 1
* Gjenstand 5

Ny Handlekurv:
* Gjenstand 1
* Gjenstand 2
* Gjenstand 4
* Gjenstand 5

```

I et system som tar i bruk en konsensusalgoritme som garanterer konsistens, for
eksempel Paxos. Trenger man ikke å håndtere regresjon av data. Men man trenger
da strategier for å mitigere tap av tilgjengelighet.

## Feiltoleranse
Under konsensus var vi innom et spesialtilfelle av feilhåndtering. Nemlig hvordan
man håndterer data synkroniseringsfeil. Men i et distribuert system er dette kun
en av mange feil som kan oppstå. Feil som at noder faller ned,
nettverket mellom noder går ned, forskjellige versjoner av systemet kjører samtidig
eller at noen av nodene i systemet er ondsinnede. Er noen eksempler på hva som
kan gå galt i et distribuert system. Feiltoleransen til et distribuert system
defineres av hvordan systemet er designet for å kunne håndtere slike feil.
I de neste avsnittene vil jeg beskrive problemene jeg listet opp overfor.
Og komme med et eksempel på hvordan man kan håndtere hvert problem.

At en node forsvinner kan være lett å håndtere. For AP systemer betyr det at man
påforhånd har tatt høyde for at data skal være replikert mellom flere noder.
Det finnes flere teknikker for å garantere at datene er tilgjengelige. Helt
overordnet betyr det at all data som var lagret på den noden som kræsjet må
finnes en eksakt kopi av dataen hos én eller flere andre noder. Det finnes flere teknikker
for å gjøre dette og et eksempel på hvordan det kan gjøres er godt beskrevet
i artikkelen om Dynamo.

Noder kan forsvinne fra nettverket og deler av nettverket kan falle ned. Men i et
distribuert system er det ofte vanskelig å skille mellom disse. Og man kan i
svært få tilfeller skille mellom dem. Det er derfor viktig å lage løsninger
som er tolerante for begge typer feil. Det som skiller en død node fra
dødt nettverk er at noder på hver sin side av en nettverkspartisjon kan
fortsette å operere uavhengig av hverandre, og man kan for eksempel i et master/slave
database-system kan man ende opp med et "split-brain" problem. Et problem som
ofte er vanskelig å løse hvis man ikke kan leve med inkorrekte data.

Bysantisk feiltoleranse er en teknikk designet for å håndtere ondsinnede noder.
Men kan også brukes til sikre et system for å tåle at en node ikke oppfører seg
som forventet. Personlig synes jeg bysantisk feiltoleranse er interessant fordi
problemet har mange forskjellige løsninger. En løsning basert på kryptografi er
proof-of-work. "Proof-of-work" kjeder, som blant annet er brukt i BitCoin, kan brukes
til å unngå Bysantiske feil.

## Veien videre
I denne bloggposten har jeg prøvd på en enkel måte å forklare hvor behovet for
distribuerte systemer kommer fra. Og jeg har introdusert noe grunnteori for
distribuerte systemer. I tillegg håper jeg har belyst hvilke problemer man kan
møte på når man lager eller tar i bruk et distribuert system.

Denne bloggposten er ment som den første av en serie med bloggposter om
distribuerte systemer. I senere poster ønsker jeg å gå dypere ned i teori, konsepter
og rammeverk.


## Anbefalt lesing
### Time, Clocks, and the Ordering of Events in a Distributed System
Lamport gir en god forklaring på hvorfor man ikke kan bruke timestamps til
å kunne bestemme den globale rekkefølgen på eventer i et distribuert system.

[PDF](http://web.stanford.edu/class/cs240/readings/lamport.pdf)


### Dynamo: Amazon's Highly Available Key-value Store
Amazon har gjort en fantastisk jobb med å forklare hvordan man kan lage
skalerbare databasetjenester. Paperet gir en veldig god forklaring av de
teorien og de underliggende mekanismene implementert i Dynamo. Dette paperet
vil jeg anbefale alle som er interessert i databaser eller distribuerte systemer.

[PDF](http://www.allthingsdistributed.com/files/amazon-dynamo-sosp2007.pdf)

### Paxos made Simple
Lamport prøver å forklare Paxos på en enklere måte enn det han gjorde i sitt
første paper om algoritmen. Paxos er kjent for å være veldig kompleks og vanskelig
å forstå. Derfor er dette paperet er for de mest interesserte og tålmodige.

[PDF](http://research.microsoft.com/en-us/um/people/lamport/pubs/paxos-simple.pdf)

### Zab: High-performance broadcast for primary-backup systems
Det er lenge siden jeg har lest dette paperet. Men gir en god beskrivelse av en
alternativ konsensusalgortime. For de som synes Paxos paperet var interessant
anbefaler jeg å lese dette paperet.

[PDF](http://web.stanford.edu/class/cs347/reading/zab.pdf)

### Raft
Uavhengig om du var modig nok til å sette deg inn i Paxos eller ikke, kan det
være interessant å sette seg inn i hvordan Raft løser konsensusproblemet.
[Web](https://raftconsensus.github.io)
[PDF](https://www.google.no/url?sa=t&rct=j&q=&esrc=s&source=web&cd=2&cad=rja&uact=8&ved=0CCUQFjAB&url=https%3A%2F%2Framcloud.stanford.edu%2Fraft.pdf&ei=X23DVNTADca4UeiKgJAO&usg=AFQjCNE8XQb0VEwFmg-Xo5yUdZpYq7BEOg&sig2=rGAgp402q2x3QFAVr7ogMQ)

### Andre
* [Byzantine General Problem av Lamport](http://research.microsoft.com/en-us/um/people/lamport/pubs/byz.pdf)
* [Spinnaker](http://www.vldb.org/pvldb/vol4/p243-rao.pdf)
