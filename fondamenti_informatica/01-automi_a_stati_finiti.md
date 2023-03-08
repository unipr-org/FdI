# Automi a stati finiti
```toc
```
--- 
## Alfabeti e linguaggi
Alcune definizioni.
-   _Simbolo_: entità primitiva astratta che non è formalmente definita
-   _Stringa_ (o _parola_): una sequenza finita di simboli giustapposti (uno dietro l’altro)    
-   _Lunghezza_ di una stringa $w$: si denota con $|w|$ ed è il numero di _occorrenze_ di simboli che compongono una stringa. La stringa vuota, costituita da zero simboli, si denota con $\epsilon$ e $|\epsilon| = 0$.

![prefix](./images/prefix.jpg)

La _concatenazione_ di due stringhe $v, w$ è la stringa $vw$ che si ottiene facendo seguire alla prima la seconda; è un’operazione associativa.

Un **alfabeto** $\mathbf{\Sigma}$ è un insieme finito di simboli. 
Un _linguaggio formale_ è un insieme di stringhe costruite a partire dai simboli di un alfabeto $\Sigma$. 
$\mathbf{\Sigma^*}$ indica l’insieme di tutte le stringhe generabili a partire da un fissato alfabeto $\Sigma$ (l’asterisco si chiama _stella di Kleene_).

[_Torna all'indice_](#automi%20a%20stati%20finiti)

---

## Automi
Un **automa a stati finiti** è un modello matematico di un sistema avente *input*,
ed eventualmente *output*, a valori discreti. Il sistema può essere in uno stato tra un
insieme finito di stati possibili. L’essere in uno stato gli permette di tener traccia
della storia precedente.

Il comportamento dell’automa si definisce in maniera univoca mediante una tabella, detta **matrice di transizione**, come ad esempio:
![[matrice_transizione.jpeg]]

Con la rappresentazione tramite grafo:
![[automa01.jpeg | 350]]

[_Torna all'indice_](#automi%20a%20stati%20finiti)

---

### DFA - Automi deterministici
Un <mark style="background: #FFB86CA6;">automa a stati finiti deterministico (DFA)</mark> è una quintupla $\langle Q,\,\Sigma,\,\delta,\,q_0,\,F \rangle$ dove:
- $Q$ è un insieme finito di *stati*;
- $\Sigma$ è un alfabeto (di input)
- $\delta : Q \times \Sigma \longrightarrow Q$ è la *funzione di transizione* 
- $q_0$ lo stato iniziale
- $F \subseteq Q$ è l'*insieme degli stati finali*.

Tramite la funzione di transizione $\delta$ è possibile definire l'analoga $\hat{\delta}$ che opera direttamente su una stringa e restituisce lo stato finale dell'elaborazione:
$$
\left\{\begin{array}{ll}
	\hat{\delta}\left({q,\,\epsilon}\right) &= q \\
	\hat{\delta}\left({q,\,wa}\right) &= \delta\left({\hat{\delta}\left({q,w}\right),\,a}\right)
\end{array}\right.
$$
o analogamente
$$
\left\{\begin{array}{ll}
	\hat{\delta}\left({q,\,\epsilon}\right) &= q \\
	\hat{\delta}\left({q,\,aw}\right) &= \hat{\delta}\left( {\delta\left({q,a}\right),\,w} \right)
\end{array}\right.
$$
Il <mark style="background: #ABF7F7A6;">linguaggio accettato</mark> da un DFA $\mathcal{M}$ è l'insieme delle stringhe accettate e si denota con $L(\mathcal{M})$.
Un <mark style="background: #ABF7F7A6;">linguaggio</mark> è detto <mark style="background: #ABF7F7A6;">regolare</mark> se è accettato da qualche DFA, ovvero se esiste $\mathcal{M}$ tale che $L = L(\mathcal{M})$. 

> $\varnothing$ e $\Sigma^*$ sono linguaggi regolari.  

#### In parole più semplici
Un DFA è costituito da cinque elementi:
1.  <mark style="background: #BBFABBA6;">Un insieme finito di stati</mark>, solitamente rappresentati da cerchi o quadrati. Ogni stato rappresenta una condizione in cui l'automa può trovarsi.
2.  <mark style="background: #BBFABBA6;">Un alfabeto finito di simboli di input</mark>, solitamente rappresentati da lettere o numeri. Ogni simbolo rappresenta un tipo di input che l'automa può ricevere.
3.  <mark style="background: #BBFABBA6;">Una funzione di transizione</mark>, che descrive come l'automa si sposta da uno stato all'altro in base all'input ricevuto. La funzione di transizione viene solitamente rappresentata da frecce o linee tra i cerchi o quadrati degli stati, e indica il simbolo di input che deve essere letto per passare da uno stato all'altro.
4.  <mark style="background: #BBFABBA6;">Uno stato iniziale</mark>, che rappresenta lo stato in cui si trova l'automa all'inizio dell'elaborazione dell'input. Solitamente viene rappresentato da una freccia che punta verso lo stato iniziale.
5.  <mark style="background: #BBFABBA6;">Uno o più stati finali</mark>, o di accettazione, che rappresentano gli stati in cui l'automa deve trovarsi al termine dell'elaborazione dell'input per accettare la stringa. Solitamente questi stati sono rappresentati da cerchi o quadrati doppi o con una doppia circonferenza.

Un DFA può essere utilizzato per accettare o rifiutare stringhe di un linguaggio formale. L'automa riceve come input una stringa, carattere per carattere, e si sposta da uno stato all'altro in base alla funzione di transizione. 
Se, alla fine dell'elaborazione dell'input, l'automa si trova in uno stato di accettazione, allora la stringa è accettata dal DFA, altrimenti viene rifiutata.

[_Torna all'indice_](#automi%20a%20stati%20finiti)

---

### NFA - Automi non deterministici
Un **automa a stati finiti non deterministici** (NFA) presenta la stessa struttura vista per i DFA, eccetto per la *funzione di transizione*:
$$ 
	\delta : Q \times \Sigma \longrightarrow \mathcal{P}{(Q)}
$$
Infatti, ora il codominio di $\delta$ è *l'insieme delle parti di Q*; pertanto essa calcola, a partire da una coppia (*stato*, *simbolo*), un insieme di stati risultante. Si noti che ora è possibile $\delta({q,\,a}) = \varnothing$ per qualche $q \in Q$ ed $a \in \Sigma$ (in questo caso la computazione si arresta).

Anche in questo caso si può definire
$$
\left\{\begin{array}{ll}
	\hat{\delta}\left({q,\,\epsilon}\right) &= \{q\} \\
	\hat{\delta}\left({q,\,wa}\right) &= 
	\bigcup_{p \in \hat{\delta}({q,\,w})}{\delta{(p,\,a)}}
\end{array}\right.
$$
Una stringa $x$ è accettata da un NFA $\mathcal{M}$ se $\hat{\delta}{(q_0,\,x)} \cap F \neq \varnothing$, ovvero se nell'insieme risultante dalla computazione c'è almeno uno stato accettante.
Il <mark style="background: #ABF7F7A6;">linguaggio accettato</mark> da $\mathcal{M}$ è l'insieme delle stringhe accettate, ovvero che godono della precedente proprietà.

La rappresentazione mediante tabella degli NFA è profondamente diversa (per ogni simbolo letto va indicato un insieme di stati), mentre la rappresentazione a grafo rimane quasi immutata. L'unica differenza è che da un nodo possono uscire più archi (o nessuno) etichettati dallo stesso simbolo.

#### Un esempio
![[NFA_esempio2.jpeg]]

[_Torna all'indice_](#automi%20a%20stati%20finiti)

---

#### In parole più semplici
Un NFA è un tipo di automa a stati finiti che ha la capacità di eseguire una o più transizioni per uno stesso simbolo in input. In altre parole, quando si legge un determinato simbolo, l'automa può scegliere di passare da uno stato all'altro in modo non deterministico, ovvero senza che vi sia un'unica opzione possibile.

<mark style="background: #BBFABBA6;">Un NFA è composto</mark> da un insieme finito di stati, un alfabeto di simboli di input, una funzione di transizione, uno stato iniziale e uno o più stati finali. La funzione di transizione definisce come l'automa si muove tra i suoi stati in base ai simboli di input letti, restituendo **non** un singolo stato ma un **insieme** di stati.

<mark style="background: #FFB86CA6;">Un NFA può accettare</mark> una stringa di input se esiste almeno una possibile sequenza di transizioni che porti l'automa da uno stato iniziale ad uno stato finale accettante. In altre parole, se almeno un risultato tra tutte le computazioni sviluppate è accettante, la stringa viene accettata dall'automa.

Esiste un procedimento per convertire un NFA in un DFA equivalente, chiamato <mark style="background: #FFF3A3A6;">"costruzione di subset"</mark>. Questo processo consiste nel creare un DFA che ha come stati tutti i possibili sottoinsiemi degli stati dell'NFA ($\mathcal{P}(Q)$) e come funzione di transizione una combinazione delle transizioni non deterministe dell'NFA. In altre parole, il DFA "simula" l'elaborazione dell'input in modo deterministico, esplorando tutte le possibili scelte di transizione dell'NFA.

<mark style="background: #ABF7F7A6;">La conversione da NFA a DFA</mark> può essere utile quando si vuole ottenere una rappresentazione più efficiente dell'automa, ad esempio per accelerare l'elaborazione di stringhe di input. Tuttavia, in alcuni casi l'NFA può essere più efficiente o comodo da usare, ad esempio quando si definiscono espressioni regolari o si costruiscono parser per il riconoscimento di linguaggi formali

[_Torna all'indice_](#automi%20a%20stati%20finiti)

---

### ε-NFA: Automi con ε-transizioni
Un $\text{NFA}$ con $\varepsilon$-transizioni è un automa analogo agli NFA ma da cui differisce per la *funzione di transizione* $\delta$ così definita:
$$
	\delta : Q \times \left({ \Sigma \cup \{\varepsilon\} }\right) \longrightarrow \mathcal{P}(Q)
$$
ovvero ammette come input non solo simboli ma anche la stringa vuota, associata al "simbolo speciale" $\varepsilon$.
Questa definizione permette all'automa di passare ad un altro stato anche senza "leggere" caratteri di input.

La costruzione della funzione $\hat\delta$ necessita del supporto di una nuova funzione, chiamata $\varepsilon \text{-closure}$; applicata ad uno stato, essa restituisce l'insieme degli stati raggiungibili da esso (compreso se stesso) mediante $\varepsilon$-transizioni.
<!-- 
$$ 
\varepsilon\text{-closure}(q) : 
\left\{\begin{array}{l}
	Q \longrightarrow \mathcal{P}(Q) \\
	q \mapsto \{  \} 
\end{array}\right.
$$
-->
Si estende il concetto di $\varepsilon \text{-closure}$ ad un insieme di stati:
$$
	\varepsilon \text{-closure}(\text{P}) = \bigcup_{p \in \text{P}}{\varepsilon \text{-closure}(p)} 
$$
Ora definiamo la funzione $\hat\delta$ :
$$
\left\{\begin{array}{cl}
	\hat{\delta}({q,\,\varepsilon}) &= 
		\varepsilon \text{-closure}(q) \\
	\hat{\delta}({q,\,wa}) &= 
		\bigcup_{p \in \hat{\delta}({q,\,w})}{\varepsilon \text{-closure}(\delta(p,\,a))}
\end{array}\right.
$$
> In questo caso $\hat{\delta}({q,\,a})$ può essere diverso da $\delta({q,\,a})$.

A questo punto, il *linguaggio accettato* da un $\varepsilon \text{-NFA}$ $\mathcal{M}$ è
$$
	L(\mathcal{M}) = \{x \in \Sigma^* :
		\hat{\delta}({q_0,\,x}) \cap F \neq \varnothing\}
$$
[_Torna all'indice_](#automi%20a%20stati%20finiti)

---

#### In parole più semplici
Un $\varepsilon \text{-NFA}$ (NFA con transizioni "epsilon" o "nulle") è un tipo di automa a stati finiti non deterministico in cui le transizioni sono etichettate non solo con simboli di input, ma anche con il simbolo $\varepsilon$ *epsilon*, che rappresenta una transizione "nullo" o "vuota".

<mark style="background: #FFB86CA6;">Le transizioni epsilon permettono all'automa di passare da uno stato all'altro senza consumare alcun simbolo di input.</mark> Ad esempio, se un $\varepsilon$-NFA ha una transizione ε dallo stato $A$ allo stato $B$, l'automa può passare direttamente dallo stato A allo stato B senza consumare alcun simbolo di input. 
Il resto della computazione è analogo a quello degli automi a stati finiti non deterministici (NFA).

<mark style="background: #FF5582A6;">DA VERIFICARE</mark>
Un $\varepsilon$-NFA può rappresentare [espressioni regolari](02-espressioni_regolari.md) che contengono l'operatore "or" (|) o linguaggi formali che includono stringhe vuote, in quanto le transizioni epsilon consentono di passare da uno stato all'altro senza consumare alcun simbolo di input e di rappresentare quindi la possibilità di avere una scelta tra diverse opzioni o di avere una stringa vuota. Inoltre, ogni $\varepsilon$-NFA può essere convertito in un NFA equivalente mediante l'eliminazione delle transizioni epsilon e successivamente in un DFA equivalente mediante la costruzione di subset.
<mark style="background: #FF5582A6;">	</mark>

<mark style="background: #FFF3A3A6;">In sintesi</mark>, un $\varepsilon$-NFA è un automa a stati finiti non deterministico in cui le transizioni sono etichettate non solo con simboli di input, ma anche con il simbolo epsilon, che rappresenta una transizione "nullo" o "vuota". Le transizioni epsilon permettono all'automa di passare da uno stato all'altro senza consumare alcun simbolo di input e di eseguire più transizioni per uno stesso simbolo di input. L'$\varepsilon$-NFA può rappresentare espressioni regolari che contengono l'operatore "or" e può essere convertito in un NFA e successivamente in un DFA equivalente.

[_Torna all'indice_](#automi%20a%20stati%20finiti)

---

## Equivalenza tra DFA e NFA
Poichè un DFA si può vedere come un NFA in cui $\delta({q,\,a})$ restituisce sempre insiemi costruiti da un solo stato, si ha che ogni linguaggio regolare è un linguaggio accettato da un qualunque NFA.

Dimostriamo che i linguaggi accettati dai DFA e dagli NFA coincidono:
- DFA $\mathbb\Rightarrow$ NFA
	Questo lato dell'implicazione è banale; infatti un DFA si può vedere come un NFA in cui per ogni simbolo $a \in \Sigma$ si ha 
$$
	\delta({q,\,a}) = \{a\}
$$
	ovvero la funzione di transizione per NFA restituisce sempre insiemi singoletti.

- NFA $\mathbb\Leftarrow$ DFA
	L'altra implicazione è dimostrata dal **Teorema di Robin-Scott**: 
	Sia $M = \langle Q,\,\Sigma,\,\delta,\,q_0,\,F \rangle$ un NFA. Allora esiste un DFA $M'$ tale che $L(M) = L(M')$.

#### In parole più semplici
Esiste un importante teorema (*Robin-Scott*) nella teoria dei linguaggi formali che afferma che ogni NFA può essere convertito in un DFA equivalente, ovvero che accetta lo stesso linguaggio di stringhe.

In altre parole, per ogni NFA esiste un DFA equivalente che riconosce lo stesso linguaggio formale. Questo teorema è noto anche come *"teorema di equivalenza DFA-NFA"* ed è fondamentale nella teoria dei linguaggi formali, in quanto permette di utilizzare i DFA (che sono più facili da implementare e analizzare) al posto degli NFA, senza perdere l'espressività del modello.

Il processo di conversione di un NFA in un DFA equivalente si basa sulla costruzione di un DFA che "simula" l'elaborazione dell'input in modo deterministico, esplorando tutte le possibili scelte di transizione dell'NFA. Questo processo è noto come "costruzione di subset" e consiste nel creare un DFA che ha come stati tutti i possibili sottoinsiemi degli stati dell'NFA e come funzione di transizione una combinazione delle transizioni non deterministe dell'NFA.

Un NFA è più comodo di un DFA: permette di eseguire più ipotesi.

[_Torna all'indice_](#automi%20a%20stati%20finiti)

---

## Equivalenza tra ε-NFA e NFA
Ogni NFA è, per definizione, un caso particolare di un $\varepsilon$-NFA.
Sia $M = \langle Q,\,\Sigma,\,\delta,\,q_0,\,F \rangle$ un $\varepsilon$-NFA. Allora esiste un NFA $M'$ tale che $L(M) = L(M')$.

### In parole più semplici
Un $\varepsilon$-NFA e un NFA sono due modelli di automi a stati finiti che possono riconoscere lo stesso insieme di linguaggi formali. In altre parole, per ogni linguaggio formale, esiste un $\varepsilon$-NFA equivalente a un NFA, e viceversa.

In generale, ogni epsilon-NFA può essere convertito in un NFA equivalente mediante la rimozione delle transizioni epsilon, e ogni NFA può essere convertito in un $\varepsilon$-NFA equivalente aggiungendo le transizioni epsilon appropriate. Pertanto, $\varepsilon$-NFA e NFA possono essere considerati equivalenti in quanto entrambi i modelli di automi possono essere utilizzati per riconoscere lo stesso insieme di linguaggi formali.

Tuttavia, <mark style="background: #FFB86CA6;">ci sono alcune differenze tra epsilon-NFA e NFA in termini di comportamento.</mark> In particolare, gli $\varepsilon$-NFA possono effettuare transizioni "nulle" o "vuote" che consentono di passare da uno stato all'altro senza consumare alcun simbolo di input, mentre gli NFA possono effettuare solo transizioni etichettate con simboli di input.

Inoltre, poiché gli epsilon-NFA possono eseguire più transizioni per uno stesso simbolo di input, scegliendo uno qualsiasi dei possibili percorsi di transizioni, sono spesso più compatti degli NFA e possono rappresentare espressioni regolari più in modo più efficiente. Tuttavia, la conversione di un epsilon-NFA in un DFA può richiedere più stati rispetto alla conversione di un NFA equivalente.

<mark style="background: #FFF3A3A6;">In sintesi</mark>, epsilon-NFA e NFA sono equivalenti in termini di capacità di riconoscere lo stesso insieme di linguaggi formali, ma ci sono alcune differenze nel loro comportamento e nella loro rappresentazione. La scelta tra l'utilizzo di un epsilon-NFA o di un NFA dipende dalle esigenze specifiche dell'applicazione.

[_Torna all'indice_](#automi%20a%20stati%20finiti)