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
    - [Restrizioni di Confine](#boundaries-restrictions)
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
If one queries the affordance system, the system
should enlist, among other things, an action similar to:

```
...
(k)       AddArgument advance age
(k+1)     AddArgument advance steps
...
```

where k represents an arbitrary index. As one can see, the affordance
system is telling the programmer that two of the possible actions that
can be done are to add another argument to the advance function, and
the global definitions *age* and *steps* are among the options to act
as arguments.

It is noteworthy to mention that affordances should always be
enumerated, and their order should be constant over several calls to
the affordance system. The reason behind this is that the programmer
should be able to indicate the system what affordance is to be applied
after examining the query's result.

### Type Restrictions

The common behaviour in programming languages is to have a typing system
which restricts the programmer from sending arguments of unexpected
types to function calls. Even in weakly typed programming languages,
an operation such as `true / "hello world"` should raise an error
(unless in the case of an
[esoteric language](https://en.wikipedia.org/wiki/Esoteric_programming_language),
of course). CX follows a very
[strict typing system](#strict-typing-system), and arguments that are
not exactly of the expected type should not be considered as
candidates for affordances' actions (although a workaround is to wrap
these arguments in cast functions before being shown as affordances).

Type restrictions must also be considered when assigning a new value
to an already existant variable. In CX, a variable declared of a
certain type must remain of that type during all of its lifetime
(unless it is removed using meta-programming commands/functions and
created anew). Thus, a variable declared to hold a 32 bit integer should not
be considered as a candidate to receive a 64 bit float output
argument, for example.

### Existential Restrictions

This type of restriction can seem trivial at first sight: if an
element does not exist, an affordance that involves it should not
exist either. Nevertheless, this restriction becomes a challenge once
we consider a situation where a function was renamed, and it has
already been used as an operator in expressions throughout a
program. If the program is in its source code form, this problem is
reduced to a simple "search & replace" process, but if it's during
runtime, the affordance system becomes very useful: an affordance
to change the identifier bound to this operator.

Even if an element has not be renamed, determining if an element
exists or not is not trivial. The elements to be used in affordances
must be searched in the call stack's current scope, in the global
scope, and in other modules' global scopes.

### Identifier Restrictions

Adding new named elements are commonly candidate actions for affordances. A
restriction that arises when trying to apply such type of affordance
is to ensure a unique identifier for the new element to avoid
redefinitions. The affordance system can either generate a unique
identifier in the element's scope, or can ask the programmer to
provide a suitable identifier.

### Boundaries Restrictions

CX provides native functions for accessing and modifying elements from
arrays. Examples of an array reader and an array writer are:


```
readI32([]i32{0, 10, 20, 30}, 3)
writeF32([]f32{0.0, 10.10, 20.20}, 1, 5.5)
```

In the first expression, an array of four 32 bit integers is accessed
at index 3, which returns the last element of the array. In the second
expression, the second element of an array of three 32 bit floats is
changed to 5.5. If any of these arrays was accessed using either a
negative index or an index which exceeds the length of the array, an
"out of boundaries" error is raised.

By obeying only the type restrictions, the affordance system will tell
the programmer that any 32 bit integer argument can be used as an
index to access any array. Although these programs would compile, out
of boundaries errors are very probable to occur if the programmer does not
pay extra attention to what is being chosen to be applied.

The affordance system needs to filter the affordances according to the
following criteria: discard any negative 32 bit integer, and discard
any 32 bit integer which exceeds the length of the array being sent to
the array reader or writer.

### User-defined Restrictions
*Note: The user-defined restrictions system is still in its
 experimental stages.*

The basic restrictions described above should at least guarantee that
the program does not encounter any runtime errors. These restrictions
should be enough to build some interesting systems, such as CX's
native
[evolutionary algorithm](#integrated-evolutionary-algorithm). Nevertheless,
in some situations a more robust system is required. For this purpose,
clauses, queries and objects are used to describe a module's
environment. These elements are defined by using an integrated Prolog
interpreter, and the CX native functions *setClauses*, *setQuery*, and
*addObject*.

The most general description of this restriction system is that the
programmer defines a series of Prolog clauses (facts and rules), that
will be queried using the defined Prolog query, for each of the
objects added. This will hardly make any sense to anyone reading this
for the first time. An example should clarify the concepts and the
process a bit more:

```
setClauses("move(robot, north, X, R) :- X = northWall, R = false.")

setQuery("move(robot, %s, %s, R).")
```

In this example, only one rule is defined. The rule can roughly be
interpreted as "if the robot wants to move north, ask what is X. If X
is northWall, then it can't move." The query is just a format string
that will serve as a query for the *move* action, and for the *robot*
element that will receive two more arguments: a direction, and an
object.

Objects can be defined by using the *addObject* function:

```
addObject("southWall")
addObject("northWall")
```

The restriction system will query the system for each of the objects
present in the module. In this example, the system will first perform
the query "move(robot, north, southWall)," and the system will respond
"nil," which means that it does not have any rule defined to handle
such situation, and the default action is to not discard the
affordance. The second query will be "move(robot, north, northWall),"
and the system will respond "false." In this case, the affordance did
not pass the test, and is discarded.

The example above illustrates how these rules can negate an
affordance using a condition. But rules can also be used to accept
affordances, even after being negated by previous rules.

```
setClauses("move(robot, north, X, R) :- X = northWall, R = false.
    move(robot, north, X, R) :- X = northWormhole, R = true.")

setQuery("move(robot, %s, %s, R).")
```

The added rule in the code above tells the system to accept the robot
movement towards north if a wormhole is present. If the object array
is left as it was defined before, the movement affordance will still
be discarded, but if `addObject("northWormhole")` is evaluated, the
"northWormhole" will be added and the robot will be able to pass
through the wall using the wormhole.

# Strict Typing System

As mentioned in the introduction, there is no implicit casting in
CX. Because of this, multiple versions for each of the primitive types
are defined in the core module. For example, four native functions for
addition exist: addI32, addI64, addF32, and addF64.

The parser attaches a default type to data it finds
in the source code: if an integer is read, its default type is *i32*
or 32 bit integer; and if a float is read, its default type is *f32* or 32
bit float. There is no ambiguity with other data read by the parser:
*true* and *false* are always booleans; a series of characters
enclosed between double quotes are always strings; and array needs to
indicate its type before the list of its elements, e.g., `[]i64{1, 2,
3}`.

For the cases where the programmer needs to explicitly cast a value of
one type to another, the core module provides a number of cast
functions to work with primitive types. For example, `byteAToStr`
casts a byte array to a string, and `i32ToF32` casts a 32 bit integer
to a 32 bit float.

# Compiled and Interpreted

The CX specification enforces a CX dialect to provide the developer
with both an interpreter and a compiler. An interpreted program is far
slower than its compiled counterpart, as is expected, but will allow a
more flexible program. This flexibility comes from meta-programming
functions, and affordances, which can modify a program's structure
during runtime.

A compiled program needs a more rigid structure than an interpreted
program, as many of the optimizations leverage this rigidity. As a
consequence, the affordance system and any function that operates over
the program's structure will be limited in functionality in a compiled
program.

The compiler should be used when performance is the biggest concern,
while a program should remain being interpreted when the programmer
requires all the flexibility provided by the CX features. In the
following subsections, some of these features are presented, without
the aim of serving as a tutorial, but rather as a mere introduction.

### Read-Eval-Print Loop

The read-eval-print loop (REPL) is an interactive tool where a
programmer can input new program elements and evaluate them. Starting
a new REPL session will print the following messages to the console:

```
CX REPL
More information about CX is available at https://github.com/skycoin/cx

*
```

The "*" tells the programmer that the REPL is ready to receive a new
line of code. The REPL will keep reading input from the user until a
semicolon and a new line character are encountered.

If no program was initially loaded into the REPL, CX will start with
an empty program. This can be seen if the `:dProgram true;`
meta-programming command is given as input:


```
* :dProgram true;
Program

*
```

The REPL is only printing the word "Program" followed by an empty
line. As a first step, a new module and function can be declared:

As the first steps, a new *main* module and a new *main* function
should be declared:

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

As can be seen, the program structure is being printed every time a
new element is added to the program.

### Meta-programming Commands

`:dProgram` was used in the subsection above. Any statement that
starts with a colon (:) is part of a category of instructions known as
"meta-programming commands."

Declaring elements in the REPL instructs CX to add them to the
program's structure. But, as in many other programming languages,
these declarations are limited to only be added, and at most be
redefined.

But, as in many other programming languages that provide a REPL, the
programmer is limited to adding new elements to a program and, at
most, redefining elements. Meta-programming commands allow the
programmer to be in more control on how the program's structure is
being modified.

`:dProgram`, `:dState`, and `:dStack` are used for
debugging purposes only, by printing the program's structure, the
current call's state, and the full call stack to the user,
respectively. `:step` instructs the interpreter to go forward or
backward in its execution. `:package`, `:func`, and `:struct`, known
as *selectors*, are used to change the program's scope. `:rem` gives
the programmer access to *removers*, which can be used to selectively
remove elements from a program's structure. `:aff` is used to access
CX's affordance system; this meta-programming command is used to both
query and apply affordances for the different elements of a
program. Lastly, `:clauses` is used to set a module's clauses to be used by the
[user-defined restrictions system](#user-defined-restrictions);
`:object` and `:objects` are used for adding and printing objects,
respectively; and the last two meta-programming commands: `:query`,
which is used for setting the module's query, and `:dQuery` which is a
helper for debugging the user-defined restrictions.

### Stepping

A program started in REPL mode can be initialized with a program
structure defined in a source file. For example:
current directory

```
$ ./cx --load examples/looping.cx

```

loads `looping.cx` from the examples directory (the full list of
examples can be found in the
[project's repository](https://github.com/skycoin/cx)). Even though a
program has been loaded, it has not yet been executed. In the REPL, in
order to execute a program one has to use the meta-programming command
`:step`. To run a program until the end, `:step 0;` must be used. But
`:step` is interesting because it can take other integers as its
argument (even negative integers). For example:

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

The *examples/looping.cx* program is being run 5 steps at a time. We
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

