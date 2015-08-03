### Konsensus, Paxos og Raft
Tidligere i bloggposten nevnte jeg konsensusalgoritmen Paxos. Forklare hva Paxos er og hvordan den fungerer er en bloggpost i seg selv. Men som en introduksjon til distribuerte systemer er det viktig å være klar over hva konsensusalgoritmer er og hvorfor det er viktig.

I et distribuert system trenger man konsensusalgoritme for helt åpenbart oppnå konsensus mellom maskiner i systemet.
På grunn av distribuerte systemers natur er ikke dette rett frem.
Det er mye man trenger å ta hensyn til for å lage en robust konsensusalgoritme.
Blant annet feil i nettverk, maskiner og progamvare.

Paxos er den mest kjente konsensualgoritmen, og har fra 70-tallet og er fortsatt i dag brukt for å
oppnå konsensus i et distribuert system. Et stort problem med Paxos er
at algoritmen vanskelig å forstå og og enda vanskeligere å implementere,
På grunn av dette introduserte Yahoo Zookeeper [6] i 2010.
Zookeeper er et distrbuert konfigurasjonssystem som
blant annet kan brukes til konsensus, locks eller sentralisert konfigurering.
Zookeeper blir fortsatt brukt i mange open source systemer blant annet i
Apache Kafka, Apache Hadoop og Apache Storm.

Men for omtrent et år siden dukket
Raft opp som en utfordrer til Paxos. Og på kort tid har løsningen blitt
veldig populær og blir brukt i mange moderne distribuerte systemer blant annet
systemer som Consul og etcd. Raft skal være en enklere og mer foreståelig
konsensusalgoritme enn Paxos. Og hvis man tar i betraktning antall nye
distribuerte systemer som tar i bruk Raft [7] ser påstanden å holde vann.

* [2] [Paxos made Simple](http://research.microsoft.com/en-us/um/people/lamport/pubs/paxos-simple.pdf)
* [6] [Zookeeper: Wait-free coordination for Internet-scale systems](http://labs.yahoo.com/publication/zookeeper-wait-free-coordination-for-internet-scale-systems/)
* [7] [Raft Consensus Homepage](https://raftconsensus.github.io/)
