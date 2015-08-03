## Historie

Distribuerte systemer er ikke noe nytt. Allerede på 1970-tallet var det aktivitet i feltet.
Det som stod på agendaen den gangen var blant annet å løse problemer som hvordan avgjøre
rekkefølge av hendelser som oppstår mellom maskiner i ett nettverk. Dette blir presentert i
den vitenskapelige artikkelen "Time, Clocks, and the Ordering of Events in a Distributed System"
1]. Utfordringen er at klokker ikke er synkroniserte på tvers av maskiner i ett nettverk. For å
avgjøre en total rekkefølge over hendelser ser man på bruken av vektor-klokker og synkronisering
av klokker.
En annen viktig artikkel er "Paxos" av Lamport [2]. Paxos er en algoritme for å oppnå konsensus i
ett netverk av maskiner, som er mye i bruk i reelle systemer, på tross av at den er tilnærmelig
umulig å forstå. Det har siden blitt introdusert flere konsensusalgoritmer -
vi kommer tilbake til de senere

Fagfeltet var lenge usynlig for industrien, men da Google ga ut sin første åpne forskningsartikkel
om MapReduce i 2004 [3] fikk man plutselig øynene opp for hva distribuerte systemer kan bidra med
på industriell skala. Det var på det tidspunket det veldig ofte misbrukte
og misforståtte begrepet "Big Data" ble utbredt.

* [1] [Time, Clocks, and the Ordering of Events in a Distributed System](http://web.stanford.edu/class/cs240/readings/lamport.pdf)
