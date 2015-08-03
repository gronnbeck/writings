# En kort historie om distribuerte systemer
Det er ikke lenge siden det holdt med én maskin for å kjøre all slags software:
de fleste problemer kunne løses ved å kaste raskere maskinvare på maskinen du brukte.
Hvis rask nok maskinvare ikke var tilgjengelig, eller du ikke hadde råd til nytt ustyr,
kunne du la Moore's Lov gjøre jobben sin og vente, ja, faktisk vente, på at maskinvaren enten ble rask nok eller billig
nok til å løse akkurat dine problemer.

> "Moore's law" is the observation that, over the history of computing hardware,
> the number of transistors in a dense integrated circuit doubles
> approximately every two years.

Tiden har endret seg. Vi kan ikke lengre vente på at Moore's Lov gjør jobben sin.
En stor del av det er fordi vi produserer mer data enn vi noen gang har gjort.
På grunn veksten av data kan vi ikke lengre lagre alt på en og samme maskin.
Og vi kan heller ikke behandle all informasjonen vi trenger med én prosessor på én maskin hvis vi ønsker at jobben
ikke skal bruke mer enn et år på å fullføre. Vi tar derfor i bruk teknikker fra fagfeltet for
distribuerte systemer som distribuert lagring og prosessering for å løse disse problemene.

Hva er ett distribuert system?

> A distributed system is a software system in which components located on networked
> computers communicate and coordinate their actions by passing messages. The components
> interact with each other in order to achieve a common goal. [1]

## Hvorfor?
I dag blir distribuerte systemer hovedsaklig forbundet med NoSQL-databaser, gjerne i forbindelse med 
mikrotjeneste-arkitektur [2]. Men vi tror ikke alle har et forhold
til hvorfor man trenger distribuerte systemer. Mange mener nok at det kun trengs når du vil oppnå
bedre ytelse, som koker ned til spørsmålet om hvor mye data du kan kverne per tidsenhet.
Men feltet er større enn som så.
Distribuerte systemer handler i tillegg til ytelse om isolasjon og tilgjengelighet.

Med isolasjon mener vi at vi ofte ikke har lyst til å kjøre alle
applikasjoner på samme server. Hva hvis en applikasjon med et sikkerhetshull blir
utsatt for et angrep av en ondsinnet person kan alle andre tjenester på samme server være utsatt.
Isolasjon lar oss hindre at en feil i en applikasjon påvirker andre applikasjoner,
eller andre instanser av samme applikasjon.

Tilgjengelighet handler om hva som skjer hvis man mister en server eller om en
database-server tar fyr. Skal en tjeneste slutte å svare fordi en node i et cluster
faller ned? Skal en brann på serverrommet føre til at vi mister forretningskritiske data?

Og til slutt selvfølgelig ytelse. Hvordan kan vi bruke flere ressurser til å løse problemet vårt raskere. Men vi kan også tenke på ytelse i form av skalerbarhet: hvordan kan vi prosessere datasett som er for store til å behande på én maskin?

Disse konseptene eksluderer ikke hverandre, men byr på komprimisser der vi må vrake noe for å få
noe annet. Hvordan henger dette sammen? Det er her distribuert systemteori kommer inn.

## Veien videre
Denne bloggposten er den første i en serie om distribuerte systemer.
I de neste bloggpostene kommer vi til å dykke dypere ned i distribuert teori og implementasjon.
I skrivende stund har vi planer om å skrive om konsensus, arkitektur, prosessering og databaser i kontekst av distribuerte systemer.
Agendaen er ikke spikret. Det er mye spennende å ta tak i!
Kom gjerne med innspill hvis du har ønsker om tema innenfor distribuerte systemer vi skal skrive om!

## Sources
* [1] [Wikipedia: Distributed programming](http://en.wikipedia.org/wiki/Distributed_computing)
* [2] [Martin Fowler: Microservices](http://martinfowler.com/articles/microservices.html)
