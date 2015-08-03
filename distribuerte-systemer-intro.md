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
En stor del av det er fordi vi produsere mer data enn vi noen gang har gjort.
På grunn av denne vektsten av data kan vi ikke lengre lagre all dataen på en og samme maskin.
Og vi kan heller ikke prosessere dataene med én prosessor på én maskin hvis vi ønsker at jobben
ikke skal bruke mer enn et år på å fullføre. Vi tar derfor i bruk teknikker fra fagfeltet for
distribuerte systemer som distribuert lagring og prosessering for å løse disse problemene.
Hva er ett distribuert system?

> A distributed system is a software system in which components located on networked
> computers communicate and coordinate their actions by passing messages. The components
> interact with each other in order to achieve a common goal. [1]

## Hvorfor?
I dag blir distribuerte systemer hovedsaklig forbundet med nosql databaser, og mer og mer med
mikrotjeneste arkitektur [2]. Men vi tror ikke alle har et forhold
til hvorfor man trenger distribuerte systemer. Mange mener nok at det er kun for å oppnå
bedre ytelse, som koker ned til spørsmålet om hvor mye data du kan kverne per tidsenhet.
Men feltet er større enn som så.
Distribuerte systemer handler i tillegg til ytelse om isolasjon og tilgjengelighet.

Med isolasjon mener vi at vi nødvendigvis ikke har lyst til å kjøre alle
applikasjoner på samme server. Hva hvis en applikasjon med et sikkerhetshull blir
utsatt for et angrep av en ondsinnet person kan alle andre tjenester på samme server være utsatt.
Isolasjon koker ned til om en feil i en applikasjon skal påvirke andre applikasjoner,
eller andre instanser av samme applikasjon.

Tilgjengelighet går ut på hva som skjer hvis man mister en server eller om en
database-server tar fyr. Skal en tjeneste slutte å svare fordi en node i et cluster
faller ned? Eller om en brann på serverrommet skal føre til at vi mister all foretningskritiske data?

Og til slutt selvfølgelig ytelse. Som er spørsmålet rundt hvordan
vi kan bruke flere resursser til å løse problemet raskere. Men vi kan også tenke på ytelse
i form av skalerbarhet. Der man ønsker å proessesere data som er for store til å kunne
proesseres på én maskin.

Disse konseptene eksluderer ikke hverandre, men mer komprimisser der man må vrake noe for å få
noe annet. Så du lurer sikker på hvordan. Det er her distribuert system teori kommer inn.

## Veien videre
Denne bloggposten er den første i en serie om distribuerte systemer.
I de neste bloggpostene kommer vi til å dykke dypere ned i distribuert teori og implementasjon.
I skrivende stund har vi planer om å skrive om konsensus, arkitektur, prosessering og databaser i kontekst av distribuerte systemer.
Agendaen er ikke satt. Det er mye spennende å ta tak i!
Kom gjerne med ønsker ellers skriver vi om det vi har lyst til.

## Sources
* [1] [Wikipedia: Distributed programming](http://en.wikipedia.org/wiki/Distributed_computing)
* [2] [Martin Fowler: Microservices](http://martinfowler.com/articles/microservices.html)
