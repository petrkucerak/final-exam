# PAL

<link rel="stylesheet" href="style.css">

## Asymptotická notace

Mějme fci $f(n)$ a funkci $g(n)$. Říkáme, že:

- $f(n) \in \Omega(g(n))$, pokud $(\exist c > 0)(\exist n_0) (\forall n > n_0) : c \cdot g(n) \leq f(n)$
- $f(n) \in \Omicron(g(n))$, pokud $(\exist c > 0)(\exist n_0) (\forall n > n_0) : f(n) \leq c \cdot g(n)$
- $f(n) \in \Theta(g(n))$, pokud $(\exist c_1 > 0, \exist c_2 > 0)(\exist n_0) (\forall n > n_0) : c_1 \cdot g(n) \leq f(n) \leq c_2 \cdot g(n)$

## Grafy

**Node degree** (neboli *stupeň uzlů*) je počet hran, které do nebo z daného vrcholu vedou. Pokud se jedná o orientovaný graf rozlišujeme tzv *indegree* a *outdegree*, kde *indegree* je množina všech vstupních hran a *outdegree* je množinou všech hran výstupních.

Graph **path** (neboli *cesta*), je sekvence hran a vrcholů, kde se žádné 2 vrcholy neopakují, tedy ani hrany.

- **tah** je posloupnost, kde se mohou opakovat vrcholy ale ne hrany
- **sled** je posloupnost, kde se mohou opakovat jak hrany, tak vrcholy

Graph **circuit** je cesta, která je uzvařená, tedy $v_t = v_0$, kde $v_t$ je koncový vrchol cesty a $v_0$ je počátečná vrchol cesty mohou se v ní opakovat vrcholy.

Graph **cycle** je cesta, která je uzavřená stejně jako *circuit*. Oproti němu se ale nesmí opakovat ani hrana ani vrchol.

### Reprezentace grafů

<div class="col-2">
<div>

#### Adjacency Matrix

Nechť $G=(V,E)$ je graf s $n$ vrcholy. Značme si je $v_1, ..., v_n$. Adjacency matrix of graph $G$ je čtevrcová matice $A_G=(a_{i,j})^n_{i,j=1}$ definovavné, jako $a_{i,j} == 1 \text{ for }\{v_i,v_j\} \in E \text{ otherwise } 0$.

Jednoduše, pokud jsou hrany spojeny, v daném směru, je tam jednička, pokud spojny nejsou, je tam nula.

</div>
<div>

![alt text](assets/PAL_00.png)

</div>
</div>

#### Laplacian Matrix

Spojení adjecency matrix s informaci o node degree.

Jsou-li hrany spojené, zapiš `-1`, je-li v matici hrana sama se sebou, zapiše její `degree` (tedy počet hran), pokud není s daným vrcholme spojená, zapiš `0`.

![alt text](assets/PAL_01.png)

<div class="col-2">
<div>

#### Incidence Matrix

Matice je tvořena $n\times m$, kde $|V|=n$ a $|E| = m$ a hodnotami $\{-1, 0, 1\}$. Hodnota v poli je rovna:

- $-1$ pokud z  daného vrcholu daná hrana odvádí edge
- $0$ pokud v daném vrcholu hrana neexistuje
- $1$ pokud do daného vrcholu daná hrana vede edge

</div>
<div>

![alt text](assets/PAL_02.png)

</div>
</div>

<div class="col-2">
<div>

#### Adjacency List

Jinak se této grafové reprezetnaci také říká *list of neighborurs* neboli listy sousedů. Funguje tak, že pro každý vrchol drží list pointerů na sousední vrcholy.

</div>
<div>

![alt text](assets/PAL_03.png)

</div>
</div>

### Srovnání jednotlivých reprezentací

|               | Adjecency Matrix | Laplacian Matrix | Incidence Matrix      | Adjacency List  |
| ------------- | ---------------- | ---------------- | --------------------- | --------------- |
| Storage       | $\Omicron(V^2)$  | $\Omicron(V^2)$  | $\Omicron(V \cdot E)$ | $\Omicron(V+E)$ |
| Add vertex    | $\Omicron(V^2)$  | $\Omicron(V^2)$  | $\Omicron(V \cdot E)$ | $\Omicron(V)$   |
| Add edge      | $\Omicron(1)$    | $\Omicron(1)$    | $\Omicron(V \cdot E)$ | $\Omicron(1)$   |
| Remove vertex | $\Omicron(V^2)$  | $\Omicron(V^2)$  | $\Omicron(V \cdot E)$ | $\Omicron(E)$   |
| Remove edge   | $\Omicron(1)$    | $\Omicron(1)$    | $\Omicron(V \cdot E)$ | $\Omicron(V)$   |


