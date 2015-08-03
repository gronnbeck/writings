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
* [5] [Wikipedia: Distributed programming](http://en.wikipedia.org/wiki/Distributed_computing)
