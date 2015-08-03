# En kort historie om distribuerte systemer

Det er ikke lenge siden det holdt med én maskin for å kjøre all slags software:
de fleste problemer kunne løses ved å kaste raskere maskinvare på maskinen du brukte.
Hvis rask nok maskinvare ikke var tilgjengelig, eller du ikke hadde råd til nytt ustyr,
kunne du la Moore's Lov gjøre jobben sin og vente på at maskinvaren enten ble rask nok eller billig
nok til å løse akkurat dine problemer.

> "Moore's law" is the observation that, over the history of computing hardware,
> the number of transistors in a dense integrated circuit doubles
> approximately every two years.

Tiden har endret seg. Vi kan ikke lenger vente på at Moore's Lov gjør jobben sin.
En stor del av det er fordi vi produsere mer data enn vi noen gang har gjort.
På grunn av denne vektsten av data kan vi ikke lenger lagre all dataen på en og samme maskin.
Og vi kan heller ikke prosessere datene med én prosessor på én maskin hvis vi ønsker at jobben
ikke skal bruke mer enn et år på å fullføre. Vi tar derfor i bruk teknikker fra fagfeltet for
distribuerte systemer som distribuert lagring og prosessering for å løse disse problemene.
Hva er ett distribuert system?

> A distributed system is a software system in which components located on networked
> computers communicate and coordinate their actions by passing messages. The components
> interact with each other in order to achieve a common goal.

[5]  

Men før vi kaster oss over de mest sentrale emnene innenfor distribuerte systemer i dag,
er viktig at vi tar en titt på hvordan fagfeltet har utviklet seg fra starten
av 1970-tallet til i dag.  

## Historie

Distribuerte systemer er ikke noe nytt. Allerede på 1970-tallet var det aktivitet i feltet.
Det som stod på agendaen den gangen var blant annet å løse problemer som hvordan avgjøre 
rekkefølge av hendelser som oppstår mellom maskiner i ett nettverk. Dette blir presentert i
den vitenskapelige artikkelen "Time, Clocks, and the Ordering of Events in a Distributed System" 
[1]. Utfordringen er at klokker ikke er synkroniserte på tvers av maskiner i ett nettverk. For å 
avgjøre en total rekkefølge over hendelser ser man på bruken av vektor-klokker og synkronisering
av klokker.
En annen viktig artikkel er "Paxos" av Lamport [2]. Paxos er en algoritme for å oppnå konsensus i 
ett netverk av maskiner, som er mye i bruk i reelle systemer, på tross av at den er tilnærmelig 
umulig å forstå. Det har siden blitt introdusert flere konsensusalgoritmer - 
vi kommer tilbake til de senere.

Fagfeltet var lenge usynlig for industrien, men da Google ga ut sin første åpne forskningsartikkel
om MapReduce i 2004 [3] fikk man plutselig øynene opp for hva distribuerte systemer kan bidra med
på industriell skala. Det var på det tidspunket det veldig ofte misbrukte 
og misforståtte begrepet "Big Data" ble utbredt.

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

### Paxos og konsensus
Tidligere i bloggposten nevnte jeg konsensusalgoritmen Paxos. Forklare hva Paxos er og hvordan den fungerer er en bloggpost i seg selv. Men som en introduksjon til distribuerte systemer er det viktig å være klar over hva konsensusalgoritmer er og hvorfor det er viktig.

I et distribuert system trenger man konsensusalgoritme for helt åpenbart oppnå konsensus mellom maskiner i systemet. På grunn av distribuerte systemers natur er ikke dette rett frem. Det er mye man trenger å ta hensyn til for å lage en robust konsensusalgoritme. Blant annet feil i nettverk, maskiner og progamvare.

Paxos er den mest kjente konsensualgoritmen, og har fra 70-tallet og er fortsatt i dag brukt for å
oppnå konsensus i et distribuert system. Men det
er noen problemer med den. Det ene er at Paxos
er vanskelig å forstå og implementere, og det andre
er at Paxos ikke skalerer veldig bra [påstand].
På grunn av dette introduserte
Yahoo Zookeeper [source] i 2010. Zookeeper er et distrbuert konfigurasjonssystem som
blant annet kan brukes til konsensus. Og er brukt
i mange open source systemer blant annet i
Apache Hadoop og Apache Storm. Men for omtrent et år siden dukket en løsning som valgte å utfordre Paxos. Da ble paperet om en ny konsensusalgoritme Raft gitt ut. Og på kort tid har den nye algoritmen blitt
veldig populær og blir brukt i mange moderne
distribuerte systemer blant annet databaser.

#### Raft
Da Raft ble lansert for omtrent et år siden, tok det ikke veldig lang tid før nye distribuert løsninger som Consul og etcd dukket opp som tok i bruk den nye algoritmen.

Raft skal være en enklere og mer foreståelig konsensusalgoritme enn Paxos. Og hvis man tar i betraktning antall nye distribuerte systemer som tar i bruk Raft, mp man neste tro at det er sant [7].

## Hvorfor?
I dag blir distribuerte systemer hovedsaklig forbundet med nosql databaser, og mer og mer på
mikrotjeneste arkitekturen. Men jeg tror svært få har et forhold
til hvorfor man trenger distribuerte systemer. Mange mener nok at det er for  å oppnå 
bedre ytelse, som koker ned til spørsmålet om hvor mye data du kan kverne per tidsenhet.
Men feltet er større enn som så.
Distribuerte systemer er mer enn å få en mengde med maskiner til å fullføre en
jobb så fort som mulig. Distribuerte systemer handler i tillegg til ytelse om
isolasjon og tilgjengelighet.

Med isolasjon mener vi at vi nødvendigvis ikke har lyst til å kjøre alle
applikasjoner på samme server. Hva hvis en applikasjon med et sikkerhetshull blir
utsatt for et angrep av en ondsinnet person kan alle andre tjenester på samme server være utsatt.
Isolasjon koker ned til om en feil i en applikasjon skal påvirke andre applikasjoner?

Tilgjengelighet går ut på hva som skjer hvis man mister en server eller om en
database-server tar fyr. Skal en tjeneste slutte å svare fordi en node i et cluster
faller ned? Eller om en brann på serverrommet skal føre til at vi mister all foretningskritiske data?

Og til slutt selvfølgelig ytelse. Som er spørsmålet rundt hvordan
kan vi bruke flere resursser til å løse samme problemet raskere. Men vi kan også tenke på ytelse
i form av skalerbarhet. Der man ønsker å data som er for store til å kunne lagres på kun én maskin.

Disse prinsippene er eksluderer ikke hverandre, men mer komprimisser der man må vrake noe for å få
noe annet. Så du lurer sikker på hvordan. Det er her distribuert system teori
kommer inn.

Men før vi kaster oss ut i fagfeltet så må vi snakke om
de mest vanlige feilantagelsene programmerere, med lite erfaring med distribuerte,
systemer gjør. Disse feilantagelsene blir i fagfeltet kalt for
"The Fallacies of Distribited Computing"

## Veien videre
Denne bloggposten er ment som den første av en serie med bloggposter om
distribuerte systemer. Og i denne bloggposten har jeg prøvde
å forklare hvor behovet for distribuerte systemer kommer fra.

I den neste posten av denne bloggserien kommer vi til å dykke dypere i
kjente problemstilliner man møter på når man skal lage et distribuert system.

## Sources
* [1] [Time, Clocks, and the Ordering of Events in a Distributed System](http://web.stanford.edu/class/cs240/readings/lamport.pdf)
* [2] [Paxos made Simple](http://research.microsoft.com/en-us/um/people/lamport/pubs/paxos-simple.pdf)
* [3] [MapReduce](http://static.googleusercontent.com/media/research.google.com/en//archive/mapreduce-osdi04.pdf)a
* [4] [Dynamo: Amazon's Highly Available Key-value Store](http://www.allthingsdistributed.com/files/amazon-dynamo-sosp2007.pdf)
* [5] [Wikipedia: Distributed programming](http://en.wikipedia.org/wiki/Distributed_computing)
