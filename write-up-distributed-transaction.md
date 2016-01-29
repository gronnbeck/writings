# Distributed transaction. Don't.

Disclaimer: Dette er manus for en lyntale om distrbuerte transaksjoner for
BEKK fagdag 04.09.15.

## Om meg selv

**Keywords**:

 * Ken
 * Jobber som systemutvikler i Forvaltning
 * Har interesse for distribuerte systemer
 * Faggruppen for distribuerte systemer


Hei! For de som ikke kjenner meg heter jeg Ken. Til daglig jobber jeg som
systemutvikler i Forvaltning. Og lever derfor naturlig nok i .NET-verdenen.

Selv om jeg ikke jobber mye med distribuerte systemer til daglig har jeg
en stor intersse for det og bruke, litt til mye tid, til å lære mer om det.
Og har ganske mange meninger rundt det. Altså lite praktisk erfaring, men
mange meninger. Det kan vel aldri så feil...

## Introduksjon til distribuerte systemer

**Keyword:**

 * For bredt
 * For mange essensielle ting.
 * Kanskje jeg kommer meg hit en dag

I dag skulle jeg egentlig gi en kort intro til distribuerte systemer.
Men jeg sleit med å finne en godt nok scope som faktisk ville gi verdi.
Jeg vurdere mange ting, men følte at det ville bli veldig overfladisk.

## Introduksjon til Mikrotjenester

**Keywords:***
 * Også for bredt
 * 10 minutter
 * Vanskelig å scope

Jeg dykket derfor litt lenger ned i distsys verden, og fant ut at jeg kunne
snakke litt mer om miktrotjenester. Da ikke en introduksjon, men litt prat
rundt det. Men! Etter å ha satt meg ned å lagd alle slidene. Synes jeg det
var vanskelig å få til en fin rød tråd på 10 minutter. Så jeg scopet det ned
igjen.

Her hadde jeg tenkt til å prøve å sammenligne klassisk SOA vs nymoderne SOA
aka Microtjenester. Men sparer det til en annen gang

(Snart burde jeg lage en lyntale om hvordan snakket om noe helt
  annet enn det du sa. Og gjøre det sånn passe OK.)

## Slutt med distribuerte distribuerte

Og et av takeawayene til den forrige lyntalen jeg egentlig skulle lage var

> Slutt med distribuerte transaksjoner

den andre var

> En database per applikasjon

en ting jeg også sikkert kunne stått her å rantet om.

Så da har jeg brukt litt tid på å si hva jeg egentlig skal snakke om.
Kan jeg vel bare begynne


### Hva er en distribuert transaksjon?

Så hva er en distribuert transaksjon?

Først det åpenbare: En distribuert transaksjon er en transaksjon som foregår
over et distribuert system. Altså en transaksjon som går over flere noder
i nettverket ditt.

Eksempel vis er det å oppdatere to tabeller i to sql databaser og sørge for
at oppdatering en konsistent. Hvis en oppdatering feiler så skal man "trygt"
rulle tilbake.

Og de er notorisk vanskelig å implementere. Alt mulig kan gå galt. En av nodene
du prøver å kjøre transaksjonen over kan feile. Og systemet som koordinerer
transaksjonen kan feile.

Rett og slett ikke lett å få til i det hele tatt.

I en miktrotjeneste verdenen har man akseptert at det er lettere å lage
systemer som en transaksjonsfrie. Og ved å gå for en slik løsning er man
nødt til å akseptere at man mister konsistens. Og kan få ikonsistente systemers
som blir konsistente over tid. Altså det man kaller for "eventual consistancy".

### Men jeg trenger konsistens!

Så er det sikkert mange som tenker at de trenger konsistens. Men jeg vil påstå
(etter å ha stjålet alle mine argumenter fra en annen) at man ikke nødvendigvis
trenger det bare forid man er vant til det. Mange foretningspraksiserer og
"real-world" eksempler er faktisk ikke konsitente.

### Så hvordan ser en distrbuert transaksjon ut i det virkelige liv?

Så hvordan ser en distribuert transaksjon ut i det virkelige liv?
Er det noen som husker hvordan Vinmonopolet fungerte før?
Der du måtte trekke en lapp, vente på din tur, gå til kassen,
si til personen i kassa hva du ville ha, vente på at varene
ble levert, betale og gå.  

Som dere kan tenke deg er dette ganske ineffektiv.
Dette er et eksempel på en distrbuert transaksjon.

### Hva skjer når du kommer in på Starbucks?

Hva har dette her emd en kaffesjappe å gjøre tenker du kanskje?

Se for deg hvordan bestillingsprosessen hos Starbucks eller en lignende
kaffesjappe fungerer.

 * Du går til kassen
 * Bestiller det du skal ha
 * Forlater køen slik at noen andre kan bestille
 * Kaffen blir lagret async

Hvis du følger med på hva som skjer så ser du at

 * Kopper blir satt i kø
 * Baristar tar kopper fra køen
  * Lager Kaffen
  * Setter kaffen klar til henting

Så kan du eller den som har bestilt kaffen hente den.

Dette systemet er helt asynkront, distribuert og fungerer helt fint
uten distirbuerte transaksjoner.  

### Starbucks har et korrelasjonsproblem

Og slik som det er beskrevet her. Har Starbucks et korrelasjonsproblem.

Kaffe blir ferdig i forskjellig rekkefølge fra det de kom inn i køen.
Det kan skje på forskjellige måter. Men den mest vanlige er at noen
kopper kaffe tar lenger tid å lage enn andre.

Så hvordan løser Starbucks dette?

### Navnelapper

Ved å bruke, humoristiske, navnelapper. Om de er morsomme emd vilje kan man
bare spekulere i

### Feilhåndtering

Så hva med feilhåndtering? Hva skjer hvis noen ikke kan betale alikevel?
Eller de lager feil ting til deg? Eller hvis noen stjeler den?

Enten så kaster de bare det de har laget, gir det til noen andre eller
bare tar å lager noe nytt til deg. Kostaden er så liten at noen feil
en gang i blant ikke har noe å si.

Slike strategien kan man også bruke i et distribuert system. Man kan dele
opp strategiene i tre forskjellige kategorier.

  1. Write-off: Når en feil skjer. Dropp å gjøre noe med det. En liten feil
 en gang i mellom gjør ingenting. Selvfølgelig må kostnaden være liten.

    1. Et eksempel på dette er f.eks. at hvis for eksempel en internett leverandør
    glemmer å sende ut 1 regning 1 gang har dette lite utslag på pengestrømmen.
    Og kostanden assosiert med å fikse det er sannsynligvis mye større

  1. Retry: Rett og slett er det bare å prøve igjen. Hvis en oppdatering feilet,
  feks at systemet du prøvde å snakke med er nede, så vent litt og prøv igjen.
  Dette gjelder selvfølgelig ikke hvis businessregler er brutt.

  1. Compensating Action: Eller en handling som utføres i ettertid for
  å få systemet konsistent igjen. Denne blir feks brukt i Minibanker.
  Hvis Minibanken ikke har nett vil den la deg ta ut penger selv om du
  ikke har penger på kortet. Du vil derfor kanskje ende opp i minus på
  debitkortet ditt. Det banken gjør da i ettertid er å sørge for at du
  betaler tilbake slik at systemet er konsistent igjen.

### Samtalemønster

Denne historien illustrer hvordan distrbuerte transaksjoner kan unngåes ved
å ha korte og konsise, synkrone, samtaler med asynkrone jobber for å løse
et problem. Noen kaller dette for "Samtale"-mønsteret eller
"Conversation pattern" på engelsk.


### Men det er jo ingen som gjør dette innenfor IT?

Amazon gjør faktisk dette. Dette skjedde i alle fall meg engang. Faktisk min historie.
Jeg bestilte en ebook og den ble levert rett til kindlen.

Noen timer senere fikk jeg beskjed av Amazon om at de ikke hadde klart å trekke
beløpet fra kontoen min. Som var fordi jeg var tom for penger.

Dette er et klassisk eksempel på en kort synkron-aksjon der man bestiller en ebook
og får den tilsendt. Så tar Amazon og senere prøver å trekke credit-kortet.
Hvis det ikke går prøver de bare igjen.

Jeg fylte på penger på kortet. Så jeg er usikker på hva de ville gjort hvis
jeg aldri hadde hatt penger på det igjen. Antagligvis ingenting...


### Takeaway

### Takk for meg  
