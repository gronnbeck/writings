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

To eksempler, som jeg har sakset og oversatt fra Wikipedia, 
på hva som kan skje ved å gå i fallgruven til en eller flere av
disse feilantagelsene er

1. Legger stort press på nettverket ved å legge unødvendig mye data på nettverket.
2. Nettverket består ofte av flere subnets, med flere én nettverksadministratorer
som beskytter sine interesser og har sine regler som en sender (et system) må
forholde seg til

Og det finnes en hel del andre feil og fallgruven man kan ufrivillig snuble over om man
ikke tar hensyn til feilantagelsene jeg presentere overfor. Det er derfor viktig å gjøre seg
kjent med disse feilantagelsene og fagfeltet før  
man skal bygge et eller vurdere et distribuert system.


## CAP Teoremet
CAP Teormet, også kalt Brewers teorem, som jeg nevnte tidligere
går som følgende, et distribuert system kan ikke garantere alle følgende
egenskapene samtidig:

* Konsitens (Consitency)
* Tilgjengelighet (Availability)
* Partisjonstoleranse (Partition tolerance)

Før jeg begynner vil jeg poengtere at CAP har mange forskjellige
tolkninger, og en ekstrem tolkning av dette er at man kun kan lage systemer som 
tilfredstiller to av tre av egeneskapene jeg presenterte overfor. 
Setter man sammen to av de egenskapene kan man få systemer tre typer systemer:

 * CP, konsistens og partisjonstoleranse,
 * AP, tilgjengelighet og partisjonstoleranse, og
 * CA, konsistens og tilgjengelighet,

men hvilke tradeoffs gjør man typisk for få de egenskapene man ønsker i et distribuert system.
Jeg kommer til å bruke denne ekstreme tolkning for å illsutrere de forskjellige
tilpassningene man kan gjøre på konsistens, tilgjengelighet og partisjonstoleranse.
Men vær oppmerksom på at man ikke nødvendigvis trenger å bytte ut én egenskap for
å oppnå en annen [5].

La oss begynne med konsistens. Grovt sett har man to hovedkategorier av konsistens, de er "strongly consistent"
og "eventually consistent". Den første nevnte kan garantere at dataene du leser
fra én node vil være den samme for alle andre nodene i et kluster. Mens eventually consistency
ikke kan garanterer at dataene du leser fra én node være det samme for en annen node i et kluster.

Tilgjengelighet handler om å garantere at en forespørsel skal få et svar om operasjonen
var vellykket eller har feilet. Ikke-teknisk betyr det at løsningen alltid kan gi deg et svar.
Tilgjengelighet kan måles i form av oppetid. Et system som bytter bort oppetid
for eksempel konsistens (CP) kan ikke garantere at man får respons før systemet kan
bekrefte at alle nodene er konsistente.
Og man risikere da at systemet ikke er tilgjengelig hvis en node i en kluster forsvinner.

Partisjonstoleranse er en type feiltoleranse som
sikter til hvor godt et distribuert system tolerer feil i deler av systemet (f.eks
  at en node faller ned) eller vilkårelig tap av meldinger. Å byte bort partisjonstoleranse
betyr egentlig at man bytter ut muligheten for å kjøre systemet ditt på flere maskiner. 
Så et rent CA system finnes ikke i distribuerte systemer. Men det finnes systemer som ofrer
noe partisjonstoleranse, men databasesystemer som Spinnaker [8] ofrer partisjonstoleranse mellom datasenter for å oppnå
de egenskapene de ønsker innenfor et datasenter.

## Konsensus
Konsensus har vært et løst problem innenfor distribuerte systemer ganske lenge. Enkelt sett
er konsensus i distribuerte systemer oppnåd via algoritme som får flere noder i et kluster
til å bli enig om noe. Noe kan være å bli enig om hvilken node som er sjefen i klusteret eller
å bli enige om hvilke versjon av samme data som er den nyeste. Konsensus er sterkt
knyttet til konseptet om konsistens som fra CAP teoremet. Og man trenger en
konsensusalgoritme med riktige tradeoffs for å få den konsistensmekansimen man ønsker.

Den mest kjente protkollen for konsensus må være Paxos. Paxos, av Leslie Lamport,
er en konsensusprotokoll som er "strongly consistent". 
Kort fortalt sørger Paxos for at noder i et nettverk
får konsensus i ett nettverk med ustabile og upålitelige noder.
Paxos er kjent for å være notorisk vanskelig å forstå og enda vanskeligere å implementere.
Jeg vik derfor ikke diskutere protokollen videre, 
men er absolutt verdt å prøve å sette seg inn i.
For den interesserte anbefaler jeg å først lese
Wikipedia artikkelen om Paxos før du prøver deg på Lamports paper "Paxos Made Simple" fra 2001.
Og med det ønsker jeg deg lykke til!

I ettertid har det dukket opp flere konsensusprotokoller. Der de mest kjente
er Zab [6] som brukes av Zookeeper og Raft [7] implementert i Consul, etcd og en hel del
andre distribuerte systemer. For Zab anbefaler jeg å lese paperet om Zab og Zookeeper
for å få en forståelse av hva og hvorfor. Når det gjelder Raft er github
sidene om "Raft Consensus" veldig bra lesing.

I kryptografi har de et ordtak "venner lar ikke venner skrive sin egen krypto". 
Det samme bør også gjelde for konsensusalgoritmer, spesielt for Paxos. 
Da mange heller kunne dratt nytte av å bruke Zookeeper for å kordinere 
sitt distribuerte system enn å rette på bugs i sin egen 
implementasjon av Paxos [5]. 

### Replikering og konflikthåndtering
Konsensus og replikering av state har, som konsistens, også mye med hverandre å gjøre.
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
og Two-Phase Commits (CA). I Dynamo bruker de en versjon av Gossip
protokollen, men i Dynamo er det klientene som har ansvar for konflikthåndtering.
Siden jeg synes det er mest interessant å se på hvordan Dynamo gjør konflikthåndering,
vil jeg ikke gå i dybden på hvordan Gossip Protokollen fungerer. Jeg anbefaler
å lese mer om det på Wikipedia og i Amazons paper om Dynamo.

Som sagt er det klientene som har ansvar for konfliktshåndtering i Dynamo.
Helt overordnet så vil DynamoDB sende alle versjoner av et dokument
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
Det er mange feil som kan oppstå i et distribuert system.
På grunn av populæriteten til distribuerte AP databaser 
(Dynamo eller Cassandra [9]) blir ofte feilhåndtering av
synkroniseringsfeil diskutert. Men slike feil er kun 
en av mange feil som kan oppstå. Feil som at noder faller ned,
nettverket mellom noder går ned, forskjellige versjoner 
av systemet kjører samtidig eller at noen av nodene i systemet er ondsinnede,
er eksempler på hva som kan gå galt i et distribuert system. 
Feiltoleransen til et distribuert system
defineres av hvordan systemet er designet for å kunne håndtere slike feil.
I de neste avsnittene vil jeg beskrive problemene jeg listet opp overfor.
Og komme med et eksempel på hvordan man kan håndtere hvert av problemene.

At en node forsvinner kan være lett å håndtere. For AP systemer betyr det at man
på forhånd har tatt høyde for at data skal være replikert mellom flere noder.
Det finnes flere teknikker for å garantere at datene er tilgjengelige. Helt
overordnet betyr det at all data som var lagret på den noden som kræsjet må
finnes hos én eller flere andre noder. Det finnes flere teknikker
for å gjøre dette og et eksempel på hvordan det kan gjøres er godt beskrevet
i artikkelen om Dynamo.

Noder kan forsvinne fra nettverket og deler av nettverket kan falle ned. Men i et
distribuert system er det ofte vanskelig å skille mellom disse. Og man kan i
svært få tilfeller skille mellom dem. Det er derfor viktig å lage løsninger
som er tolerante for begge typer feil. Det som skiller en død node fra
dødt nettverk er at noder på hver sin side av en nettverkspartisjon kan
fortsette å operere uavhengig av hverandre, og man kan for eksempel i et master/slave
database-system kan man ende opp med et "split-brain" problem [10]. Et problem som
ofte er vanskelig å løse hvis man ikke kan leve med inkorrekte data.

Bysantisk feiltoleranse er en teknikk designet for å håndtere ondsinnede noder.
Men kan også brukes til sikre et system for å tåle at en node ikke oppfører seg
som forventet. Det finnes mange forskjellige fremgangsmåter for å oppnå 
bysantisk feiltoleranse. En løsning basert på BitCoins konsept om proof-of-work 
som du kan lese mer om på 
[Wikipedias](WikiByzantinePractice) side om Bysantisk feiltoleranse.

## Veien videre
I denne bloggposten har jeg prøvd på en enkel måte å forklare hvor behovet for
distribuerte systemer kommer fra. Og jeg har introdusert noe grunnteori for
distribuerte systemer. I tillegg håper jeg har belyst hvilke problemer man kan
møte på når man lager eller tar i bruk et distribuert system.

Denne bloggposten er ment som den første av en serie med bloggposter om
distribuerte systemer. 

## Anbefalt lesing
* [1] [Time, Clocks, and the Ordering of Events in a Distributed System](http://web.stanford.edu/class/cs240/readings/lamport.pdf)
* [2] [Paxos made Simple](http://research.microsoft.com/en-us/um/people/lamport/pubs/paxos-simple.pdf)
* [3] [MapReduce](http://static.googleusercontent.com/media/research.google.com/en//archive/mapreduce-osdi04.pdf)a
* [4] [Dynamo: Amazon's Highly Available Key-value Store](http://www.allthingsdistributed.com/files/amazon-dynamo-sosp2007.pdf)
* [5] Building Microsystems, Sam Newman, O'Reilly 2014
* [6] [Zab: High-performance broadcast for primary-backup systems](http://web.stanford.edu/class/cs347/reading/zab.pdf)
* [7] [Raft](https://raftconsensus.github.io)
* [8] [Spinnaker](http://www.vldb.org/pvldb/vol4/p243-rao.pdf)
* [9] [Cassandra](http://cassandra.apache.org/)
* [10] [Split-brain problem](http://en.wikipedia.org/wiki/Split-brain_%28computing%29)


[WikiByzantinePractice]: http://en.wikipedia.org/wiki/Byzantine_fault_tolerance#Byzantine_fault_tolerance_in_practice
