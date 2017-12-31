+++
title = "Skycoin Overview"
tags = [
    "Skycoin",
]
bounty = 15
date = "2017-08-26"
categories = [
    "Overview",
]
+++

<!-- MarkdownTOC autolink="true" bracket="round" depth="1" -->

- [Introduzione a Skycoin](#skycoin-introduction)
- [Innovazioni e Difetti del Bitcoin e gli Attuali Protocolli Blockchain](#innovations-and-flaws-with-bitcoin-and-the-current-blockchain-protocols)
- [Innovazioni Prodotte dal Bitcoin](#innovations-produced-by-bitcoin)
- [Maggiori Difetti del Bitcoin](#major-flaws-of-bitcoin)
- [Proprietá Desiderabili per Sistemi di Consenso Distribuito per Registri Finanziari](#desirable-properties-for-systems-of-distributed-consensus-for-financial-ledgers)
- [La Filosofia di Sicurezza di Skycoin](#skycoin-security-philosophy)
- [Trasparenza e Sicurezza: Obelisk e Canali di Trasmissione Pubblici](#transparency-and-security-obelisk-and-public-broadcast-channels)
- [Obelisk](#obelisk)
- [Semplice Algoritmo di Consenso Binario: Scegliere tra Due Blocchi](#simple-binary-consensus-algorithm-choosing-between-two-blocks)
- [Consenso su Scelte Multiple Concomitanti Diramate](#consensus-on-multiple-concurrent-branch-choices)

<!-- /MarkdownTOC -->

# Introduzione a Skycoin 

Skycoin é basato su una tecnologia 
che introduce una nuova crittografica primitiva
conosciuta come un canale di trasmissione pubblica. Implementa
anche un nuovo algoritmo di consenso, chiamato
Obelisk, il quale mitiga i problemi di impegno 
che si presentano dal Proof-of-Work e dal processo di mining
sottostante al Bitcoin, quindi affrontando una serie di problemi
di sicurezza associati a quest'utlimo. Obelisk non é un
singolo algoritmo, ma un'implementazione che impiega
tecniche multiple per fornire garanzie di sicurezza
specifiche.

# Innovazioni e Difetti del Bitcoin e gli Attuali Protocolli Blockchain

In Bitcoin, le nuove transazioni vengono collocate
in un blocco, aggiunto alla blockchain. Qualsiasi
peer nella rete Bitcoin puó creare nuovi blocchi.
Pertanto ogni blocco ha un solo genitore ma uno o piú
validi successori (bambini). Le chains formano un
albero e il problema principale che Bitcoin risolve é far in
modo che ogni nodo nella rete sia d'accordo su quale delle 
chains prospettate nell'albero é la blockchain
di consenso.

Bitcoin usa una tecnica chiamata Proof‐of‐Work
(PoW) per determinare ch la blockchain sia unica. Un blocco
valido richiede un valore hash, al di sotto di un valore
target. I nodi aggiungono transazioni a un nuovo blocco e
randomicamente provano finché un hash valido per un 
blocco viene scoperto.

Viene usata una funzione per creare un ordine di chains
nell'albero del blocco. Per la chain con la difficoltá
piú elevata é richiesta la maggior quantitá di operazioni
hash per produrre “la chain piú lunga” e   
formare la chain consenso. La nozione di  “block
depth” e “difficulty” crea un ordine su tutte le 
chains lineari nell'albero blocco e solo la catena
ad alta intensitá di risorse é accettata per produrre
la chain di consenso.

I nodi Bitcoin si connettono fra di loro in modo casuale
e ogni nodo trasmette la catena di blocchi che conosce
ai suoi simili. Se un nodo a maggior difficoltá di produrre
la chain allora si un altro peer connesso, il nodo riceverá i
blocchi sequenzialmente. Il nodo valuterá la funzione e 
decide se la chain ricevuta sia piú complicata da 
produrre e quindi potenzialmente scambiare il suo consenso 
alla chain ricevuta. Il nodo, in seguito, la pubblicizzerá
agli altri nodi. In questo modo, il consenso viene 
propagato attraverso la rete e tutti i nodi raggiungono
lo stesso consenso.

Il Bitcoin non presuppone che i nodi abbiano identitá
e non presuppone che i nodi siano onesti.
I nodi possono mandare agli altri nodi ogni dato e ció
non puó influenzare le decisioni di consenso perché la 
difficulty puó essere verificata indipendentemente da
sé.

# Innovazioni prodotte dal Bitcoin

### * La Blockchain

Una singola struttura dati che ogniuno
puó possedere.

### * Registro Pubblico Per Le Transazioni

Memorizzazione delle transazioni finanziarie
sulla blockchain.

### * Uso del PoW e Ricalcolo della Difficultyt per Mantenere una Produzione di Blocchi Costante 

### * Uso di Chiavi Pubbliche come Hashes e Indirizzi 

Le chiavi pubbliche non vengono divulgate
fin quando non usate.

### * Uso di “outputs” per i Saldi

Ignora il tentativo di creare denaro
digitale divisibile: Per pagare $20 da un output di $25,
inviare $20 a una persona e $5 indietro a te stesso.

### * Funzione PoW Difficulty Grandezza Blocco

Il primo utilizzo della funzione é definire 
l'ordine dei blocchi. Il registro pubblico
risolve il problema denominato double spending
delle valute digitali tradizionali.

# Maggiori Difetti del Bitcoin

Questi rappresentano i maggiori problemi che devono
essere affrontati nello sviluppo di una cryptovaluta.
Il Bitcoin dovrebbe essere considerato come una cryptovaluta
embrionica sulla quale sviluppare futuri miglioramenti. La
tecnologia sulla quale Skycoin é basata affronta le maggiori 
deficienze del Bitcoin, riprogettando l'intero sistema di 
consenso distribuito.

### * Le Decisioni di Consenso nel Bitcoin non Sono Definitive e Possono Essere Ripristinate

Una persona o un'organizzazione che possa
affittare o comprare abbastanza hashing power
puó ripristinare le transazioni.

### * Il Bitcoin Raggiunge il Consenso della Rete ma i Singoli Nodi della Rete Sono Altamente Vulnerabili da Parte di Avversari che Controllano i Routers Attraverso i Quali Passano i Pacchetti

Un avversario che controlla il router
ha l'assoluto controllo sulla vista
di un nodo e puó influenzare arbitrariamente
le decisioni di consenso del nodo.

### * Gli Exchanges di Bitcoin Sono Diventati Altamente Vulnerabili ad Attacchi

Gli attaccanti professionisti potrebbero
impiegare l'attacco del 51% e comprare
e vendere alt coins sugli exchanges di Bitcoin
per guidare quest'ultimo verso l'insolvenza.

### * Le Banche e i Siti di Gioco D'Azzardo Sono Diventati Vulnerabili all'Attacco del 51% 

### * Al maturare del Bitcoin, L'Acquisto di Opzioni Contro il Bitcoin e Attacchi Contro la Sua Rete Diventano piú Redditizi

In futuro, attacchi al Bitcoin possono
risultare in centinaia di milioni di dollari
di profitto in opzioni trading.

### * Stati Con Un Forte Controllo Dei Capitali, Cosí Come Societá Concorrenti, Potrebbero Direttamente Attaccare La Rete Bitcoin Per Proteggere I Loro Interessi Finanziari.

Queste entitá potrebbero facilmente
assorbire i costi di un attacco alla rete
e compromettere la sicurezza del Bitcoin. 

### * I Servizi Che Permettono L'hashing In Cloud e L'affitto Di Potenza Hash Di Terze Parti Hanno Sempre Piú Successo 

Molte grandi pools hanno la capacitá
di affittare potenza hash per sferrare
un attacco di maggioranza.

### * Hackers Can Use Numerous Security Holes In Routers And Networking Equipment To Steal Coins From Banks And Exchanges

An attacker can control the peers
connected to a Bitcoin node and ensure
connections to attacker-controlled
nodes. For instance, an attacker may
introduce a deposit transaction to the
side chain of a bank and get the bank
to issue a withdraw transaction which
is then relayed to the main network.

### * Bitcoin Cannot Offer Security At A Low Cost

The Bitcoin network is using immense
and exponentially growing amounts of
electricity. Bitcoin’s security purposely
relies upon creating as much electrical
waste as possible. As security is related
to the cost of achieving majority hash
rate, the cost of running the Bitcoin
network are constantly driven up. In
a well-designed system, $1 in security
costs $1000 to circumvent. In Bitcoin
the ratio is $1 to $1. In addition, this is
environmentally irresponsible.

### * Bitcoin Fundamentally Cannot Decrease Transaction Times Without Compromising Security

Bitcoin transactions take on average
10 minutes to get included in a block,
and more time is required for more
security.

# Desirable Properties For Systems Of Distributed Consensus For Financial Ledgers

The criteria on which Bitcoin can be improved are:

### * No Double Spending

Once a transaction has executed,
it should be impossible to revert
consensus. Consensus should be as
irreversible as possible.

### * Efficiency

The cost to run a perfectly secure
ledger should be extremely low.

### * Speed

The system should allow transactions
to be confirmed within seconds.

### * Transparency

It should be easy to audit and identify
malicious nodes.

### * Router Attack Security

Nodes should be able to detect if their
consensus differs from the network.

Some security properties should remain intact
even if the vast majority of nodes in the network are
malicious and colluding.

On a fundamental level, many of the security
issues associated with the Bitcoin system arise from the
inherent commitment problem of the Proof of Work
and mining processes. Its security issues represent
a real-world Byzantine General Problem. Incentives
exist for participants to manipulate verification
processes, by engaging in bribery and hacking for
instance. Attackers will manipulate system clocks,
compromise routers, use hash collisions, flood the
network with hundreds of thousands of bots and
exploit signature malleability.

A secure system must not only protect against
every known attack, but be robust enough to evolve
and adapt to future attacks. Some issues in Bitcoin
can be fixed, such as signature malleability. Other
issues are fundamental and cannot be addressed
without defining an entirely new framework, such as
the reliance on Proof of Work and miners.

# Skycoin Security Philosophy

Security is a process of continuous identification
and fortification against threats. A good system
achieves “defense in depth”, has multiple redundant
systems and will survive the complete failure of
any individual measure. Good security requires the
differentiation between threats which are existential
and those which are mere annoyances.

While it is obvious that no single system can
eliminate all security threats and simultaneously
achieve all the objectives listed above, Skycoin
represents the next step in cryptocurrency technology
because it takes a modular layered approach to
security and uses different systems to enforce
particular guarantees. Skycoin security is focused on
addressing the existential threats faced by Bitcoin and
protecting users from day-to-day threats, attempting
to give the highest degree of protection against the
class of attacks that would infict the greatest losses
upon its users, stakeholders and institutions. This
requires a complete redesign of Bitcoin at both ends
from wallet generation to blockchain consensus and
fundamental innovation is several other areas.

Most of the losses in Bitcoin derive from
deficiencies in design, a lack of usability, and end user
mistakes rather than fundamental technical attacks in
the software or mathematics. Skycoin must address
both the looming existential mathematical threats
and the security perils that Bitcoin’s incomplete and
poorly thought out user experience has created for
everyday users. The poor usability and design has
forced users to compromise security, with millions
of dollar routinely relying on insecure web wallets.
Despite the frequent and massive thefts reported by
the media on a daily basis, to date more Bitcoin have
been lost due to usability issues than all the efforts of
criminals to steal Bitcoin.

As many as half of all existing Bitcoins have
never been moved from their initial addresses and
never will be because they are lost as unrecoverable
wallet files, lost wallets, or misunderstandings of
what was actually being backed up in a wallet file.
Mt. Gox recently reported “finding” 200,000 Bitcoin
in a wallet they were unaware carried Bitcoins. The
wallet had been previously ignored and could have
easily been deleted by mistake. Wallets are frequently
mistaken as empty because computer software was
unable to load a wallet created by software that is “too
old”. Thus most security issues concerning Bitcoin
occur at the level of usability, end users and exchange
security.

The rest of this section covers some of the new
techniques we have created in cooperation with our
partners to address network level security issues and
render the Skycoin blockchain more secure than
previous networks.

We have proven mathematically that our system
achieves consensus, has the security properties we want
and operates well under normal network conditions.
We have some exciting new data‐structures that have
not been seen in any coin or piece of software before.
At the moment we are prototyping the system for
deployment. The Skycoin development process is
iterative. There will be changes, improvements and
refinements as we work through the details, address
known flaws, test the system and get feedback.

# Transparency And Security: Obelisk And Public Broadcast Channels

To address the commitment problems associated
with the Bitcoin system, the technology underlying
our Skycoin implements the blockchain in the form
of a public broadcast channel. Everyone can read
the chain, but only the owner can mint blocks for it.
To be valid for a personal chain, each block must be
signed by the owners private key. Each node in this
consensus algorithm system (Obelisk) has a personal
blockchain and it is the core primitive in the Obelisk
system.

The public broadcast channel imposes several
constraints:

### * Once A Block Is Published, It Cannot Be Unpublished

Blocks are replicated peer to peer to
all subscribers. Once a block has been
published, it spreads to all subscribers.
You have to destroy all peers who have
received the block to erase it from
internet.

### * A Node Cannot Publish A Different Version Of An Earlier Block Without Detection

Blocks are numbered and it would
be detected if the node signed two
different blocks with the same
sequence number.

### * A Node Cannot Backdate The Timestamp On The Receipt Of A Block, Without Delaying The Publication Of A Block

Timestamps only go up, timestamps
increase monotonously with block
sequence count.

### * A Block In The Middle Of The Chain Cannot Be Changed Without Invalidating Every Block That Comes After It

In a hash chain, each block header contains
a hash of the previous block.

# Obelisk

Each Obelisk node (Skycoin Consensus Node)
has a public key (an identity) and personal blockchain
(a public broadcast channel). Consensus decisions
and communication happen within the personal
blockchains of each Obelisk node. This is a public
record of everything a node does. This allows the
community to audit nodes for cheating and collusion.
It gives the community a way to identify nodes which
are participating in attacks on the network and
it makes public how decisions in the network are
being made and which nodes are influencing those
decisions.

Each node has a list of other nodes that it
subscribes to. Nodes with more subscribers are more
“trusted” and yield more influence in the network. If
the community does not trust the nodes representing
them or feels that power within the network is too
concentrated (or not concentrated enough) the
community is able to collectively shift the balance of
power in the network by collectively changing their
trust relationships in the network.

Node subscription relationships can be
random and/or can be formed through web of trust
(subscribe to nodes of people you know and people
in the community you trust).

When a node receives a new block from a chain
it is subscribed to, it publishes the hash of the block
it publishes. This is a public acknowledgment of the
receipt of the block. Each block is timestamped
and counter-references blocks from other chains.
This creates a dense interlinked chain of block
acknowledgments. These chains establish causal
relationships and can act as a distributed time
stamping system as described in the next section.
This allows the network to prove that data did not
exist or was not published to the network or establish
that particular nodes were active or offline during a
particular time interval.

The current Obelisk consensus algorithm
is based upon Ben‐Or’s randomized consensus
algorithm.

A Sybil attack in a random graph (worst case)
allows the Sybil nodes to control consensus, but the
nodes are unable to revert transactions, removing the
only economic incentive to attack the network. In real
world graphs the Sybil resistance of the network is
actually very high and running a node is moderately
costly in terms of bandwidth, which makes large
botnets prohibitive.

Trust relationships are scarce and can be
rescinded. In the event of an attack, the network
reacts by severing connections to less trustworthy
nodes and contracting to a smaller core of trusted
nodes. The public record left by each node’s personal
blockchain makes it very easy to identify the nodes
participating in an attack. As attacking nodes are
identifed, individuals sever relationships with those
nodes, reducing their influence. Therefore, the major
benefits of the Skycoin network are:

- Skycoin consensus is democratic and nodes are run by the community
- Skycoin node consensus is public
- Every node is accountable to the community and 3rd party audits
- Influence within the skycoin consensus system is democratic and transparent (but unequal)

# Simple Binary Consensus Algorithm: Choosing Between Two Blocks

Each voting decision is a hash pair (A,B). A is
the hash of the parent of the block and B is the hash of
the block. Each node votes on the next block
it believes should be the consensus block. If 40% of
the nodes it is subscribed to have the same candidate
for consensus, the node changes its consensus to that
block. The node flips randomly between candidates
until consensus is reached.

# Consensus On Multiple Concurrent Branch Choices

A more advanced system publishes (A,B,P),
where P is a value from 0 to 1. P values across all
successors to block would sum to 1. This allows for
concurrent consensus decisions on multiple chain
branches.

If the majority of nodes in the network are
honest, they will also converge to the same consensus.

Skycoin also has a limited form of Proof of
Stake. We bias voting in favor of blocks with a larger
transaction fee.

If there are only two possible consensus
choices for a given parent and both blocks execute
your transaction, then the transaction is effectively
executed regardless of which of the two blocks end up
chosen by the network. The probability of reversion
of an early consensus decision declines exponentially
with block depth.
