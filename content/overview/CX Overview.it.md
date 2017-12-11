+++
title = "CX Overview"
tags = [
    "CX",
]
bounty = 20
date = "2017-09-06"
categories = [
    "Overview",
]
+++

<!-- MarkdownTOC autolink="true" bracket="round" depth="2" -->

- [Introduzione di CX](#cx-introduction)
- [Archivio del Progetto](#projects-repository)
- [Sintassi](#syntax)
- [Affordances](#affordances)
    - [Restrizioni Arity](#arity-restrictions)
    - [Restrizioni sui Tipi](#type-restrictions)
    - [Restrizioni Esistenziali](#existential-restrictions)
    - [Restrizioni di Identificazione](#identifier-restrictions)
    - [Restrizioni dei Confini](#boundaries-restrictions)
    - [Restrizioni Definite dall'Utente](#user-defined-restrictions)
- [Sistema di Battitura Rigoroso](#strict-typing-system)
- [Compilato e Interpretato](#compiled-and-interpreted)
    - [Read-Eval-Print Loop](#read-eval-print-loop)
    - [Comandi di Meta-programmazione](#meta-programming-commands)
    - [Stepping](#stepping)
    - [Debugging Interattivo](#interactive-debugging)
- [Algoritmi Integrati Evolutivi](#integrated-evolutionary-algorithm)
- [Serializzazione](#serialization)

<!-- /MarkdownTOC -->

# Introduzione di CX

CX è sia un linguaggio di programmazione che una specifica designato per 
abbracciare un nuovo paradigma di programmazione basato sul concetto di 
"affordance". L'Affordances permette a un programma di conoscere quali azioni
possono o non possono essere fatte dallo stesso. Per esempio, possiamo chiedere
al programma quali elementi possono essere inviati a una funzione, e il programma
ci restituirà un elenco di possibili azioni. Dopo aver deciso quale azione della
lista è appropriata, possiamo scegliere una delle opzioni e il programma
applicherà l'azione scelta. Come conseguenza del sistema di "affordance" di CX, 
un algoritmo di programmazione genetica è costruito e fornito come funzione
nativa, la quale può essere usata per ottimizzare la struttura del programma
durante la fase di esecuzione.

La specifica CX afferma che sia un compilatore che un interprete devono
essere accessibili al programmatore. L'interprete può essere accessibile
attraverso un read-eval-print loop, dove il programmatore può interattivamente
aggiungere e rimuovere elementi in un programma. Una volta che il programma
viene completato,può essere compilato per aumentarne le prestazioni.

Il sistema di battitura in CX è molto rigoroso. L'unico "casting implicito"
si verifica quando il parser determina cos'è un integer , un float, un valore
booleano, una stringa o un array. Per esempio, se una funzione richiede un 
integer da 64 bit, occorre usare una funzione cast per convertirlo nel tipo
richiesto.

Infine, un programma CX può essere completamente serializzato su arrays di byte,
mantenendo il suo stato di esecuzione e la sua struttura. Questa versione 
serializzata di un programma può essere deserializzata in seguito, e continuare
la sua esecuzione in qualsiasi dispositivo che abbia un compilatore/interprete CX.

Nele sezioni seguenti, le funzionalità di CX, illustrate nei precedenti paragrafi,
sono descritte in maggior dettaglio.

# Archivio del Progetto

Il codice sorgente del progetto può essere scaricato dal suo archivio Github :
[https://github.com/skycoin/cx](https://github.com/skycoin/cx). L'archivio include
i file di specifica, la documentazione, gli esempi e il codice sorgente stesso.

# Sintassi

Come è stato menzionato nell'introduzione, CX è sia un linguaggio di programmazione
sia una specifica. Le specifiche di CX non impongono una sintassi,
ma piuttosto strutture e processi che un dialetto CX deve implementare
per essere considerato un CX. Come conseguenza, si potrebbero implementare
due dialetti CX, uno con una sintassi simile a quella del Lisp e un'altra come
la sintassi del C. Questo linguaggio sottostante è chiamato CX Base,
o "il linguaggio di base". In questo documento, un'implementazione è usata
per mostrare le capacità della specifica, sebbene il suo scopo non è solo
quello di strumento accademico,ma quello di diventare un linguaggio robusto e
completo che possa essere utilizzato per scopi generali.

Il CX utilizzato in questo documento ha l'obiettivo generale di avere una sintassi
il più simile possibile alla sintassi del linguaggio GO.

# Affordances

Un programmatore deve prendere una serie di decisioni durante la costruzione
di un programma, per esempio, quanti paramentri una funzione deve ricevere, 
quanti parametri deve restituire, quali dichiarazioni sono necessarie per
ottenere la funzionalità desiderata e, inoltre, quali argomenti devono essere
inviati come parametri per le funzioni di asserzione. Il sistema di affordance
in CX può essere interrogato per ottenere una lista delle possibili azioni
che possono essere applicate a un elemento. In questo contesto, esempi di 
elementi sono funzioni, strutture, moduli ed espressioni. 

Senza avere un insieme di regole e fatti che stabiliscano quale debba essere la
logica e lo scopo di un programma, si possono determinare alcune restrizioni di
base, che almeno, garantirebbero la corretta semantica di un programma. Il sistema
di affordance fornisce tali restrizioni come primo strato filtrante, e sono spiegate
di seguito.

### Restrizioni Arity

Le espressioni in CX possono restituire differenti valori. Ciò crea una sfida
per il sistema affordance, siccome il numero di variabili che ricevono gli argomenti
di output di un'espressione devono corrispondere al numero di outputs definiti dall'
operatore dell'espressione.

```
out1, out2, ..., outN := op(inp1, inp2, ..., inpM)
```

Se l'esempio sopra è corretto, allora *op* deve produrre *N* argomenti.
Questo problema può diventare ancora più difficile se consideriamo
che la definizione di *op* può essere cambiata dal sistema affordance stesso
o dall'utente in futuro: Non appena la definizione di *op* cambia, possono 
essere applicate nuove affordances a qualsiasi espressione che usi *op* come suo
operatore, perchè il numero di variabili ricevute come argomento output di *op*,
sono adesso non in corrispondenza.

La logica precedente implica, inoltre, che se il numero delle variabili in ricezione
corrisponde al numero di parametri in output dell'espressione dell'operatore, l'azione
di aggiungere nuove variabili in ricezione non può essere eseguita.

Le restrizioni Arity si applicano anche agli argomenti di input nelle espressioni, 
ad esempio,se la chiamata a una funzione ha già tutti gli argomenti in input 
definiti, allora il sistema di affordance non dovrebbe aggiungere un altro 
argomento come possibile azione. Similmente, se un'espressione sta cercando di 
chiamare un operatore con meno argomenti del necessario, il sitema di affordance,
quando richiesto, dovrebbe dire al programmatore che è possibile aggiungere un 
nuovo argomento alla funzione.

**Esempio:**

*Nota: La concatenazione di stringhe non è ancora stata implementata. Inoltre,
la funzione print allega sempre una nuova riga alla fine della stringa stampata.
Una versione futura del'implementazione CX presentata in questo documento affronterà
questi problemi.*

```
var age i32 = 18
var steps i32 = 23

func advance (direction str, numberSteps i32) () {
    printStr("Advancing:")
    printStr(direction)
    printStr("Number of steps:")
    printI32(numberSteps)
}

func main () () {
    advance("North")
}
```

Nell'esempio precedente, manca un argomento  nella chiamata a *advance* nella funzione *main*.
Se si interroga il sistema affordance, il sistema dovrebbe adottare, tra le altre cose, un'azione simile a:

```
...
(k)       AddArgument advance age
(k+1)     AddArgument advance steps
...
```

dove k rappresenta un indice arbitrario. Come si può vedere, il sistema affordance
sta dicendo al programmatore che due delle possibili azioni possono aggiungere un
altro argomento alla funzione avanzata, e la definizione globale *age* e *steps* 
sono fra le opzioni che agiscono come argomenti.

E' interessante mezionare che le affordances dovebbero essere sempre enumerate, 
e il loro ordine dovrebbe essere costante su diverse chiamate al sistema affordance.
La ragione di ciò è che il programmatore dovrebbe essere in grado di indicare al sistema
quale affordance deve essere applicata dopo aver esaminato il risultato della query.

### Restrizioni sui Tipi

Il comportamento comune nei linguaggi di programmazione è avere un sistema di 
battitura che limitia il programmatore dall'inviare argomenti di tipi inaspettati
alle funzioni chiamate.Anche nei linguaggi di programmazione debolemente tipizzati,
un'operazione come `true / "hello world"` dovrebbe generare un errore
(tranne nel caso dei
[linguaggi esoterici](https://en.wikipedia.org/wiki/Esoteric_programming_language),
certamente). CX segue un vero
[sistema di battitura rigoroso](#strict-typing-system), e argomenti che non sono
esattamente il tipo atteso non dovrebbero essere considerati come candidati
per le azioni affordances (sebbene una soluzione temporanea sia quella di 
avvolgere questi argomenti in funzioni cast prima di essere mostrate come affordances).

Le restrizioni sui tipi devono essere considerate quando si sta assegnando
un nuovo valore a una variabile già esistente. In CX, una variabile dichiarata
a un certo tipo deve rimanere di quel tipo durante tutta la sua durata. Quindi, 
una varibile dichiarata di 32 bit interi non dovrebbe essere considerata come
candidata per ricevere come output un argomento float da 64 bit, per esempio.

### Restrizioni Esitenziali

Questo tipo di restrizione può sembrare irrilevante a prima vista: se un elemento
non esiste, un affordance che lo coinvolge non dovrebbe nemmeno esistere. 
ciò nonostante, questa restrizione diventa una sfida una volta che consideriamo
una situazione dove una funzione è stata rinominata, ed è già stato usato come
operatore in un'espressione durante un programma. Se il programma è nel suo modulo codice sorgente,
questo problema viene ridotto a un semplice processo "cerca & sostituisci", 
ma se è durante il processo di esecuzione, il sistema di affordance diventa davvero utile: 
un affordance per cambiare l'identificatore legato a questo operatore.

Anche se un elemento non è stato rinominato, non è facile determinare se un elemento 
esiste o no. Gli elementi da utilizzare nelle affordances devono essere ricercati nella
chiamata allo stack's current scope , nel global scope, e in altri moduli global scope.

### Restrizioni di Identificazione

L'aggiunta di nuovi elementi nominati sono azioni comunemente candidate per le affordances.
Una restrizione che si presenta quando si sta provando ad applicare questo tipo di affordance
è quello di garantire un unico identificatore per il nuovo elemento per evitare ridefinizioni.
Il sistema affordance può generare un unico identificatore nell'element scopo, o può chiedere
al programmatore di fornire un identifier adatto.

### Restrizione dei Confini

CX fornisce funzioni native per l'accesso e la modifica di elementi dagli arrays.
Esempi di un lettore di array e di uno scrittore array sono:


```
readI32([]i32{0, 10, 20, 30}, 3)
writeF32([]f32{0.0, 10.10, 20.20}, 1, 5.5)
```

nella prima espressione, si accede a un array di quattro integers da 32 bit
all'indice 3, il quale restituisce l'ultimo elemento dell'array. Nella seconda
espressione, il secondo elemento di un array di tre floats da 32 bit è cambiato a 5.5. 
se viene effettuato l'accesso a uno di questi arrays usando o un indice negativo o un 
indice che eccede la lungezza dell'array, viene generato un errore "fuori dai limiti" .

Obbedendo solo alla restrizione sui tipi, il sistema affordance dirà
al programmatore che qualsiasi integer a 32 bit come argomento può essere usato
come un indice per accedere a qualsiasi array. Sebbene questi programmi possano essere
compilati, errori di confine sono molto probabili se il programmatore non presta
attenzione extra su ciò che viene scelto per essere applicato.

Il sistema affordance ha bisogno di filtrare le affordances secondo 
i seguenti criteri: scartare qualsiasi integer negativo a 32 bit, e scartare
qualsiasi integer a 32 bit che superi la lunghezza dell'array inviato al
lettore o scrittore array.

### Restrizioni Definite dall'Utente
*Nota: Il sistema di restrizioni definite dall'utente è ancora
nella sua fase sperimentale.*

Le restrizioni di base sopra descritte dovrebbero almeno garantire
che il programma non incontri nessun errore di runtime. Queste restrizioni
dovrebbero bastare per costruire alcuni sistemi interessanti, come
[l'algoritmo evoluzionistico](#integrated-evolutionary-algorithm) nativo di CX. 
Tuttavia, in alcune situzioni è richiesto un sistema più robusto. Per questo scopo,
clausole, queries e oggetti sono usati per descrivere l'ambiente del modulo. 
Questi elementi sono definiti usando un interprete Prolog integrato, 
e le funzioni native di CX *setClauses*, *setQuery*, e *addObject*.

La descrizionepiù generale di questo sistema di restrizione è che
il programmatore definisce una serie di clausole Prolog (fatti e regole), 
che saranno interrogate usando una query prolog definita, per ogniuno 
degli oggetti aggiunti. Ciò non avrà alcun senso per chiunque lo legga per la 
prima volta. Un esempio dovrebbe chiarire i concetti e il processo un po di più:

```
setClauses("move(robot, north, X, R) :- X = northWall, R = false.")

setQuery("move(robot, %s, %s, R).")
```

In questo esempio, è definita solamente una regola. a regola può essere
grossolanamente interpretata come "se il robot vuole muoversi verso nord, 
chiedi cos'è X. se X è northWall, quindi non può muoversi". La query è solo
una stringa di formato che fungerà come query per l'azione *move*, e per 
l'elemento *robot* che riceverà altri due argomenti: una direzione e un oggetto.

Gli oggetti possono essere definiti usando la funzione *addObject*:

```
addObject("southWall")
addObject("northWall")
```

Il sistema di restrizione interrogherà il sitema per ogni oggetto presente 
nel modulo. In questo esempio, il sistema performerà prima la query "move(robot, north, southWall)," 
e il sistema risponderà "nil," il quale significa che non vi è alcuna regola definita
per gestire tale situazione, e che l'azione predefinita non è quella di scartare 
l'affordance. La seconda query sarà "move(robot, north, northWall),"
e il sistema risponderà "false." In questo caso, l'affordance non ha superato il test 
ed è quindi scartato.

L'esempio sopra illustra come queste regole possono negare un affordance usando
una condizione. Ma le regole possono essere usate per accettare affordances, anche se 
negate da regole precedenti.

```
setClauses("move(robot, north, X, R) :- X = northWall, R = false.
    move(robot, north, X, R) :- X = northWormhole, R = true.")

setQuery("move(robot, %s, %s, R).")
```

La regola aggiunta nel codice sopra dice al sistema di accettare
il movimento del robot verso nord se è presente un wormhole. Se l'oggetto
array è lasciato come definito precedentemente, il movimento affordance
verrà ancora scartato, ma se `addObject("northWormhole")` viene valutato, il
"northWormhole" verrà aggiunto e il robot sarà in grado di passare
attraverso il muro usando il wormhole.

# Sistema di Battitura Rigoroso

Come menzionato nell'introduzione, non esiste un cast implicito in
CX. A causa di ciò, sono definite più versioni di ogni tipo primitivo
nel modulo principale. Per esempio, esistono quattro funzioni 
native aggiunte: addI32, addI64, addF32, e addF64.

Il parser allega un tipo predefinito ai dati che trova all'interno
del codice sorgente: se un integer è letto, il suo tipo di default è *i32*
o un integer da 32 bit; e se un float è letto, il suo tipo di default è *f32*
o un float da 32 bit. Non c'è alcuna ambiguità con gli altri dati letti 
dal parser: *true* e *false* sono sempre booleani; una serie di caratteri 
racchiusi tra virgolette sono sempre stringhe; e l'array ha bisogno di 
indicare il suo tipo prima dell'elenco dei suoi elementi, esempio, `[]i64{1, 2,
3}`.

Per i casi in cui il programmatore ha bisogno di esprimere esplicitamente 
di cast un valore di un tipo a un'altro, il modulo principale fornisce un 
numero di funzioni cast che lavorano con tipi primitivi. Per esempio, `byteAToStr`
casts un byte array a una stringa, e `i32ToF32` casts un integer a 32 bit
a un float a 32 bit.

# Compilato e Interpretato

La specifica CX rafforza un dialetto CX per fornire allo sviluppatore
sia un interprete sia un compilatore. Un programma interpretato è molto 
più lento della sua controparte compilata, come previsto, ma permetterà
un programma maggiormente flessibile. Questa flessibilità deriva dalle
funzioni di meta-programmazione e affordances, capaci di modificare la 
struttura di un programma durante il suo tempo di esecuzione.

Un programma compilato ha bisogno di una struttura più rigida di un 
programma interpretato, poichè molte delle ottimizzazioni sfruttano
questa rigidità. Come conseguenza, il sistema affordance e qualsiasi
funzione che operi sulla struttura di un programma sarà limitata in 
quanto a funzionalità in un programma compilato.

Il compiler dovrebbe essere usato quando la performance rappresenta il 
la maggiore preoccupazione, mentre un programma dovrebbe rimanere interpretato
quando il programmatore richiede tutta la flessibilità fornita dalle 
funzionalità di CX. Nella subsezione seguente, vengono presentate alcune di queste
funzionalità, senza lo scopo di servire come tutorial ma piuttosto come semplice
introduzione.

### Read-Eval-Print Loop

Il read-eval-print loop (REPL) è uno strumento interattivo dove un 
programmatore può inserire nuovi elementi nel programma e valutarli. 
Iniziando una nuova sessione REPL sarà stampato il seguente messaggio 
sulla console:

```
CX REPL
More information about CX is available at https://github.com/skycoin/cx

*
```

Il "*" dice al programmatore che il REPL è pronto per ricevere una nuova
linea di codice. Il REPL continuerà la lettura degli input dell'utente
fino a che non viene rilevato un punto e virgola e un carattere di nuova
riga.

Se inizialmente nessun programma viene caricato in REPL, CX comincierà
con unprogramma vuoto. ciò può essere visto se `:dProgram true;`
il comando di meta-programmazione è dato come input:


```
* :dProgram true;
Program

*
```

Il REPL sta solo stampando la parola "Program" seguita da una linea
vuota. come primo step, può essere dichiarato un nuovo modulo e una
nuova funzione:

Come primo step, andrebbe dichiarato un nuovo modulo *main* 
e una nuova funzione *main*:

```
* package main;
Program
0.- Module: main

* func main () () {};
Program
0.- Module: main
	Functions
		0.- Function: main () ()

*
```

Come può essere visto, la struttura del programma viene stampata ogni volta
che un nuovo elemento viene aggiunto al programma.

### Comandi di Meta-Programmazione

`:dProgram` è stato utilizzato nella sottosezione precedente. Ogni affermazione
che comincia con i due punti(:) fa parte di una categoria di istruzioni conosciuta come
"comandi di meta-programmazione."

Dichiarare elementi in REPL instruisce CX ad aggiungerli alla struttura 
del programma. Ma, come in molti altri linguaggi di programmazione,
queste dichiarazioni sono limitate all'essere solo aggiunte, e al massimo 
essere ridefinite.

Ma, come in molti altri linguaggu di programmazione che forniscono REPL, il
programmatore è limitato ad aggiungere nuovi elementi al programma e, al 
massimo, ridefinire elementi. I comandi di meta-programmazione consentono al 
programmatore di aver maggior controllo su come la struttura del programma 
viene modificata.

`:dProgram`, `:dState`, e `:dStack` sono usati solo per
scopi di debugging, stampando la struttura del programma, rispettivamente
lo stato correntemente chiamato e lo stack completo chiamato dall'utente. 
`:step` istruisce l'interprete ad andare avanti o indietreggiare
nella sua esecuzione. `:package`, `:func`, e `:struct`, conosciuto
come *selectors*, viene usato per cambiare lo scope del programma. `:rem`
da al programmatore l'accesso a *removers*, che può essere usato per 
rimuovere selettivamente elementi dalla struttura di un programma.
`:aff` è usato per accedere al sistema affordance di CX;
questo comando di meta-programmazione è usato sia come query sia
per applicare affordances per differenti elementi di un programma. 
Infine, `:clauses` è usato per impostare le clausole di un modulo che
devono essere usate dal
[sistema di restrizioni definito dall'utente](#user-defined-restrictions);
`:object` e `:objects` sono usati, rispettivamente, per aggiungere e stampare oggetti; 
e gli ultimi due comandi di meta-programmazione: `:query`,
che è usato per impostare la query del modulo, e `:dQuery` che aiuta
per il debbuging delle restrizioni definite dall'utente.

### Stepping

Un programma cominciato in modalità REPL può essere inizializzato
con una struttura di programma definita in un file sorgente. Per esempio:
directory corrente

```
$ ./cx --load examples/looping.cx

```

carica `looping.cx` dalla directory esempi (la lista completa di 
esempi può essere trovata nell'
[Archivio del progetto](https://github.com/skycoin/cx)). Nonostante un 
programma sia stato caricato, non è stato ancora eseguito. nella REPL, per
poter eseguire un programma si deve usare il comando di meta-programmazione
`:step`. Per eseguire un programma fino alla fine, `:step 0;`deve essere usato.
Ma `:step` è interessante perchè puo prendere altri integers come sui argomenti
(anche integers negativi). Per esempio:

```
CX REPL
More information about CX is available at https://github.com/skycoin/cx

* :dStack false;

* :step 5;
0

* :step 5;
1

* :step 5;
2

*
```

Il programma *examples/looping.cx* is being run 5 steps at a time. We
can see that 5 steps are required in order for the program to
re-evaluate the *while* condition, print the counter, and add 1 to the
counter.

Likewise, we should "go back in time" if the REPL is instructed to
`:step -5`.

```
...

* :step 5;
2

* :step -5;

* :step 5;
2

*
```

After instructing CX to advance 5 steps again, the 2 is printed again
to the console. It must be noted that the counter is not just being
assigned with a different value. What is happening is that the call
stack is being reverted to a previous state.

### Interactive Debugging

A CX program will enter the REPL mode once an error has been
found. This behaviour gives the programmer the opportunity to debug
the program before attempting to resume its execution.

In the example below, a division by 0 error is raised, the REPL alerts
the programmer about the error, the last call in the call stack is
dumped, and the REPL continues its execution.


```
CX REPL
More information about CX is available at https://github.com/skycoin/cx

* package main;

* func main () () {};

* :func main;
main
:func main {...
	* foo := divI32(5, 3);
main
:func main {...
	* bar := divI32(10, 0);
main
:func main {...
	* :step 0;
fn:main ln:0, 	locals:
>> 1
fn:main ln:1, 	locals: foo: 1

Call's State:
foo:		1

divI32() Arguments:
0: 10
1: 0

0: divI32: Division by 0
main
:func main {...
	*
```

Likewise, if a program is given as input to the CX interpreter,
without calling the REPL, but an error is raised, the REPL will be
called for the programmer or system administrator to debug the
program:

```
$ ./cx examples/program-halt.cx
1

Call's State:
nonAssign_0:		1
nonAssign_1:		1

divI32() Arguments:
0: 5
1: 0

5: divI32: Division by 0
CX REPL
More information about CX is available at https://github.com/skycoin/cx

*
```

# Integrated Evolutionary Algorithm

The affordance system and meta-programming functions in CX allow the
flexibility of changing the program's structure in a supervised
manner. However, affordances can still be automated by having a
function that selects the index of the affordance to be applied.

`evolve` is a native function that constructs user-defined functions
by using random affordances. An iterative process is used to test

`evolve` follows the principles of evolutionary computation. In
particular, evolve performs a technique called genetic
programming. Genetic programming tries to find a combination of
operators and arguments that will solve a problem. For example, you
could instruct `evolve` to find a combination of operators that, when
sent 10 as an argument, returns 20. This might sound trivial, but
genetic programming and other evolutionary algorithms can solve very
complicated problems.

In the *examples* directory from the repository, one can find an
example (*examples/evolving-a-function.cx*) that describes the process for
evolving a
[curve-fitting](https://en.wikipedia.org/wiki/Curve_fitting) function.

# Serialization

A program in CX can be partially or fully serialized to a byte
array. This serialization capability allows a program to create a
program image (similar to
[system images](#https://en.wikipedia.org/wiki/System_image)), where
the exact state at which the program was serialized is
maintained. This means that a serialized program can be deserialized,
and resume its execution later on. Serialiation can also be used to
create backups.

A CX program can leverage its integrated features to create some
interesting scenarios. For example, a program can be serialized to
create a backup of itself, and start an
[evolutionary algorithm](#integrated-evolutionary-algorithm) on one of
its functions. If the evolutionary algorithm finds a function that
performs better than the previous definition, one can keep this new
version of the program. However, if the evolutionary algorithm
performed badly, the program can be restored to the saved backup. All
of these tasks can be automated.

