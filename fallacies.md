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
