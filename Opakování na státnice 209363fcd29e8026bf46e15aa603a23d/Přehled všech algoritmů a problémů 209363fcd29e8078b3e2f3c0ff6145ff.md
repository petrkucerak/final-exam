# Přehled všech algoritmů a problémů

# Přehled problémů a algoritmy, které je řeší

### Flow

- Max Flow Problem, Minimal Flow Cut Problem
    - Ford-Fulkerson (find feasible and try `+` or `-`)
- Maximum Cardinality Matching
    - specific flow

### Graph

- TSP (obchodní cestující)
    - Double-tree ($2$ heuristics)
    - Cristfides ($\frac{2}{3}$ heuristics)
- MST (minimální kostra)
    - Jarník-Prim (podobně jako dijkstra - `visited` a prioritní seznam)
    - Borůvka (spojuje komponenty souvislosti)
    - Kruskal (seřadí hrany a přidá pokud nevznikne cyklický graf)
- Tree Search
    - ALV (rotace)
    - RB-tree (ne přesný balanc ale rychlé)
    - B+ (*m-array*, data jsou spojena a pouze v listech)
    - Splay (probublává nahoru)
    - K-D tree (dimense na hyperspace)
    - Skip List (list, který může předbíhat)
- Nearest Neighbor Searching
    - vhodná aplikace je K-D tree
- Shortest Path Problem
    - Dijkstra (no-negative edges)
    - Bellman-Ford (negative edges, can detect negative cycle, dynamic)
    - Floyd Algorithm $\Omicron(n^3)$
- Find Connected Components Parallel
    - funguje nad *adjency matrix* a to tak, že rozdělí matici na 2 části, v nich vytvoří MST a následně přidává hranu, které nejsou v dané komponentě do druhé
- Find Strongly Connected Components
    - Kosarju-Shariri (provede DFS, pak otočí orientaci a začne zavírat skrze DFS)
    - Tarjan’s Algorithm (používá jedno DFS)
- Graph Isomorphism Problem
    - mapování
    - vhodná heuristika (počet vrcholů, hran, součet hran u vrcholu, …)
    - pro tree lze využít certifikát

### Patter

- Pattern Matching Problems
    - Hamming distance - počet změny znaků
    - Levstain distance - počet změny znaků včetně přidávání a odebírání

### Generators

- Generování prvočísel
    - Erastotenovo síto - vyškrtám násobky a stačí končit na $\sqrt n$, kde $n$ je námi hledané číslo
- Linear Congruent Generating Problem
    - dle obecného polynomu $x_{n+1}=(Ax_n + C) \text{ mod }M,\ n \geq 0$, kde existují 3 pravidla pro maximalizaci délky periody
        1. $C$ a $M$ jsou nesoudělná
        2. $A-1$ je dělitelné každým prvočíslem, kterým je dělitelné $M$.
        3. Pokud $4$ dělí $M$, pak také $4$ musí dělit $A-1$.

### Calculation

- Matrix x Vector Multiplication
    - 1D - $n$ procesorů, při velikosti matice $n \times n$ a velikost vektoru $n$
    - 2D - $n^2$ procesorů, při velikosti matice $n \times n$ a velikosti vektoru $n$
- Matrix x Matrix Multiplication
    - Canon algoritmus - provádí posuny (1x shift podle řádku/sloupce, následně shift všech sloupců/řádků)
    - DNS algoritmus - mapuje násobení do 3D struktury

### Sorting

- Network Sorting Algorithm
    - Bitonic Sort (vytvořím bitonic sekvenci, následně provedu bitnoci sort)
- Enumeration Sort Algorithm
    - při využití $n^2$ procesorů umí fungovat $\Theta(1)$ - počítá index podle toho, kolik čísel je menší než daný prvek

### Sets

- Find Maximal Independent Set
    - Lubův algotimus (dej náhodně pořadí, pokud není soused menší, proškretej okolí a node obarvi *Pítr učil v PAG posluchárně*)
- Union Find Problem
    - `find(x)` - zjisti, do které množiny patří prvek `x`
    - `union(x,y)` - spoj množiny, ve kterých jsou prvky `x` a `y`

Dynamic

- Knapsack (batoh)
    - 2-aproximation: lepší ze 2 možností (seřazené dle finanční hustoty)
    - dynamické programování

Other

- Scheduling
    - Bratley’s (B&B) - 1x $p$
    - Horn’s (preepmce, multiprocesor, early deadline first)
    - List Scheduling (preempce, listy del heuristiky)
    - dynamic
- CSP
    - AC3 algortihm (domain, constrains, variables)
- Out-of-Order Execution
    - Tomasulo Algorithm
- Coherency Problem
    - MESI, MOESI