# ESW

# Java Virtual Machine

**Java Virtual Machine** je virtuální stroj, který vykonává bytecode. Idea za JVM je abstrahovat architekturu zařízení a umožňovat spouštět aplikace ve stejném prostředí.

## Memory Layout

### Stack

- každé vlákno má svůj vlastní privátní prostor (ukládá lokální proměnné, parametry metod, …)
- je tvořen tzv. **stack fame** - pro každou vykonávanou metodu (**interpreted frame**) x pro všechny inline metody (**complied frame**)
    - ukládá data lokálních proměnných dané metody
    - slouží pro výpočty a mezivýpočty
    - ukládá návratovou hodnotu
    - ukládá reference na objekt, na kterém je metoda volána (pokud se nejedná o statickou metodu)

### Heap

- o správu, resp. uvolňování se stará **garbage collector**
- je sdílená mezi všemi vlákny
- ukládá objekty, instance, …

### Stack-oriented Machine

- Nevyužívá registrů ale stacku i pro ukládání mezi výpočtů
- JVM bytecode používá **stack-oriented** přístup pro většinu operací
- **Výhody**: přenositelnost (nezávislost na hardwaru), možnost optimalizace (Just-In-Time)
- **Nevýhody**: výkon

## Ordinary Object Pointer

Vznešený název pro reference. Ordinary je veliký 64-bit, compressed je velký 32-bit.

## JVM bytecode

1 byte = 1 instrukce

Obecně kompilátor překládá *source code *****do *bytecode *****bez jakékoliv optimalizace. Optimalizaci je možné provést externími nástroji jako je ProGuard.

### Just-In-Time Compiler (JIT)

Převádí *bytecode* do *assembly code* při běhu. Využívá adaptivní optimalizace (od verze 7). Mezi optimalizace patří například:

- optimalizace větvení
- vložení inline kódu pro minimalizaci přepínání kontextu
- monitoring *hot spots* a zrychlení jejich vykonávání
- …

JIT funguje asynchronně na více vláknech.

- C1 compiler - rychlý, využívá CPU registry, využívá SIMD (Math, arraycopy, …)
- C2 compiler - možnosti customizace

**On-Stack Replacement (OSR)** je technika, která umožňuje přepnout mezi více metodami během vykonávání kódu.

### Global and Local Safe Points

Safe point je takový stav, kdy je paměť JVM konzistentní. Tedy všechny reference jsou správně definovány.

**Global Safe Point** všechna vlákna jsou v safe pointu. Můžeme: spustit *Garbage Collector*, tisknout informace o vláknech, předefinovánat class,..

**Local Safe Point** jedno vlákno je v safe pointu. Můžeme provádět de-optimalizaci či OSR.

**Time To Safe Point (TTSP)** - Jak dlouho trvá než se dostaneme do *safepoint*.

## Automatic Memory Management

Na JVM je implementovaný Garbage Collector (GB), který se stará o úklid, resp. uvolňování alokované paměti. Výhody využití GB je primárně minimalizace chyb týkajících se alokace paměti.

## Generation Hypothesis

**Weak** - slabí umřou první (malé objekty, často využívané).

**Strong** - mladší zemřou dříve (větší objekty, používané ne tak často).

Celkově se tedy tato hypotéza dá shrnout do toho, že čím je objekt starší, tím je větší pravděpodobnost, že přežije delší dobu.

## Garbage Collector

**Myšlenka:** Hledá nedosažitelné objekty (tedy ty, které nejsou již využívány kódem) a maže je.

Existuje *sériová* nebo *paralelní* verze GB. Funguje většinou jednoduše. Jsou známé například tyto algoritmy:

- čítač referencí (u každého objektu, pokud je roven 0 nebo mimo rozsah, je uvolněn z paměti) - nevýhoda nemožnost detekce cyklů - **nepoužívá se**
- sledovací algoritmy (probíhají v safe pointu)
    - mark and sweep - nastaví status a zkusí projít, ty ke kterým se nedostane, smaže
    - kopírovací algoritmus
    - generační algoritmy

## Profiling

Monitorování, kolik CPU stráví času u jednotlivých metod, kolik paměti využije, …

**Sampling** znamená periodické vzorkování. Nemá zas tak veliký dopad na snížení výkonu.

**Tracing** naopak má větší dopad, protože je třeba přidat orchestraci bytecode.

### Warm-Up

Jedná se o čas, který je potřeby k provádění JIT. Tedy spuštění a JVM sbírá informace o běhu programu. S každým dalším je běh optimálnější.

# Data Races and other parallel problematic

## Data races

Více vláken přistupuje ke stejným prostředkům, například ke sdílené proměnné. K zabránění je třeba využít orchestrace.

## Superscalar and Pipelining Architecture

**Superscalar** - více instrukcí zároveň

**Pipelining -** v každém stádiu může být jedna instrukce

## Memory Barrier

Všechny operace musí být dokončené než daná operace začne.

## Volatile Variable

Říká kompilátoru, že nad danou proměnnou nemá provádět žádné optimalizace. Používá se pro proměnné, které se mohou změnit z jiného zdroje (mapovaná periferie, jiné vláknou, přerušení jako externí událost).

Kompilátor zajistí, že každý přístup k proměnné je zajištěn přesně podle pořadí v kódu. Nezaručuje atomické operaci.

## Synchronization

### Thin

Obyčejný lock / mutex bez historie a dalších informací

### Fat

Používá historii - tedy kdo zamknul, na jak dlouho, …

### Lazy Lock

Není uvolnění po opuštění kritické sekce, ale ostatní vlákna musejí potvrdit, že je možné lock uvolnit.

### Biased locking

Lock nadržuje (upřednostňuje) vlákno, které požádalo o lock jako první. Následné přístupy daného vlákna k tomu zámku jsou mnohem rychlejší.

### Reentrant Locks

Zámek, který umožňuje vláknu, které je zamknuté znovuvstoupit i když nebyl zdroj uvolněn. To inkrementuje counter a pří výstupu se deiknkremnetuje. V C++ se využívá `std::recursive_mutex`.

## Atomic Operations

Atomická operace je taková, která zajisti konzistenci při vykonávání dané instrukce, tj. že daná instrukce nemůže být něčím přerušena (ani interruptem).

### Compare-and-set

Provede operaci porovnání, zdali se hodnota v paměti shoduje s očekávanou a vrátí `true` nebo `false`. Používá se v situacích, kdy si potřebujeme ověřit, že se předchozí operace povedla.

## Non-blocking algorithm

Algoritmy, které nevyužívají lock operace ale atomických instrukcí jako je CAS (Compare-and-Swap).

### Lock-free Stack (Treiber Stack)

Datová struktura, která funguje bez zámků a implementuje stack, který lze použít mezi více vlákny. Je založen na atomických operacích, konkrétně CAS (Compare-and-Swap) aby zajistil safe `push` a `pop`.

# CAS

Myšlenka **Compare-and-Swap** je taková, že se načte hodnota, kterou chceme změnit. Provede se s ní operace a uloží se do nové proměnné. Následně se zavolá operace CAS. Pokud hodnota odpovídá originální hodnotě, víme, že jiné vlákno danou proměnnou nezměnilo. Pokud se hodnota změní, musím celý proces opakovat.

```c
uint32_t g_counter = 0;

void update_counter(void){

	while(true){
		
		// read origin value
		uint32_t origin = g_counter;
		// increment value
		uint32_t new = origin++;
	
		// process compare and swap operation
		if(CAS(origin, new)) break;
	}
}
```

# Memory Analysis

Typy analýzy paměti:

- **Static Memory Analysis** - analýza stavu paměti v daný okamžik
- **Dynamic Memory Analysis** - analýza paměti v průběhu času

Na analyzovanou paměť se lze dívat 2 způsoby:

- **Shallow size** - paměť, která je potřebná pro uložení pouze tohoto objektu bez jakýkoliv referencí
- **Retained size** - paměť, kterou by bylo třeba vyklidit při uklízení GC, tedy včetně všech referencí, na které objekt odkazuje

**Memory leak** je taková paměť, která již není dále využívaná, ale stále na ni existuje reference.

## Data structure

### Java primitives

- `boolean` 1B
- `byte` 1B
- `char` 2B - využívá 16-bit kód
- `int` 4B
- `long` 8B
- `float` 4B
- `double` 8B

### Objects

Jsou vždy alokované na heap.

### Auto-boxing a unboxing

Jedná se o mechanismy, které převádějí primitivní datové fce (`int`, `double`, …) na jejich objektové ekvivalenty (`Integer`, `Double`, …).

### Memory efficiency of complex data structures

Kolik paměti je zabíráno režií vs. kolik paměti zabírají pouze data.

Například array je vysoce efektivní. Linked list není, protože obsahuje pointery (reference) na předchozí a případně i následujícíc pole. Navíc nefuguje efeketivně při načítání do cache, protože mohou být data uložena v jiné části paměti.

## Collection for performance

Patří mezi ně například maps, sets, lists či queues. Odebírají overhead, který je nutný pro *auto-boxing* a *un-boxing*, protože ho implementují sami na úrovni struktury.

Umožňují lepší cachování, typicky implementace zajišťuje uložení na jednom místě v paměti.

### Types

- ArrayList - automaticky se zvětšuje, zmenšení musí být provedeno manuálně
    - TIntArrayList (Trove) - eliminuje *auto-boxing*, zrychluje proces, vhodné pro práci s velkým množstvím primitivních prvků
    - IntArrayList (FastUtil) - optimalizováno pro primitivní datové typy, opět eliminuje *auto-boxing*, vhodné pro výkon
- HasMap - hashovací tabulka
    - TIntDoubleHashMap (Trove) - efektivní pro primitiva
    - Int2DoubleOpenHashMap (FastUtil) - efektivní pro primitiva

## Bloom Filter

Struktura, která umožňuje:

- přidávat nové objekty do množiny
- testovat, zdali je objekt součástí dané množiny
- *neumožňuje odstraňování*

V podstatě funguje tak, že když chce přidat nový prvek, tak pro něho spočítá skrze sekvenci hash funkcí jeho lokalitu.

Jakmile chci prvek nalézt, tak spočítám opět sekvenci hashovacích funkcí a ty mě navedou na lokalitu, kde se muže prvek nacházet.

![image.png](ESW%2020f363fcd29e801b8d1ec7eae7eeb390/image.png)

**Výhody:** rychlost, paměťová efektivita, …

**Nevýhody**:

- falešná pozitivní odpověď, tj. prvek nemusí být v množině
- nelze realizovat odebírání - pokud není rozšířený algoritmus
- čím více prvků, tím vyšší chybovost

**Bloom filter** lze rozšířit například o counter a tak mohu odhadnout, kolik už je prvků. Popř. přidat counter na každý bit.

## Reference v Javě

- strong reference - defaultní typ objektové reference, objekt není uklizen GB, dokud není reference nastavena na `null`
- soft reference - objekt je uklizen pouze při nedostatku paměti, vhodná aplikace je například cache
- weak reference - uklizen pokud má slabé reference a není dost místa v paměti
- final reference - v praxi se nepoužívá
- phantom reference - objekt je před uklizením zařazen do referenční fronty

# JVM Allocation

Pro alokaci se využívá techniky **bump-the-pointer**. Funguje tak, že sleduje konce posledního alokovaného místa (ukazatele na volnou paměť) a paměť pro další objekt alokuje zde.

Výhodou je rychlá alokace díky pouhému posunutí ukazatele. Nevýhodou je nutnost synchronizace mezi vlákny.

## Thread-Local Allocation Buffer (TLABs)

Jedná se o malé paměti vyhrazené pro jednotlivá vlákna, které umožňují rychlou alokaci objektů bez nutnosti synchronizace mezi vlákny.

Alokace probíhá bez zámků.

Pokud dojde paměť, dojde k synchronizaci s JVM a získá se nový TLBA (děje se výjimečně).

## Object Escape Analysis

Optimalizační technika prováděná C2 kompilátorem, která určuje, zdali objekt *uniká* z metody nebo vlákna, ve které byl vytvořen.

Využívá se pro modelování vhodnější optimalizace a optimalizačních technik.

## Data Locality

Pojme označuje princip ukládání dat blízko sebe tak, aby je cache načetla najednou a došlo tak k minimalizaci cache missu a zrychlení celého programu.

## Non-uniform Memory Allocation (NUMA)

Přístup k datům závisí na fyzické vzdálenosti data vzhledem k procesoru. Každý procesor má svoji lokální paměť, která má rychlejší přístup k datům. Přístup k vzdálenějším datům bude tedy pomalejší. Více je diskutováno v PAP.

JVM může optimalizovat alokace a tak zrychlit přístup k datům a to například tak, že změní TLABs.

# Networking

## OSI model

physical layer, data-link layer, network layer, transport, session, presentation, application layer

## C10K problem

Obsloužení velikého množství klientů (10 000) v jeden moment (relevantní k 90 létům).

K vypořádání se s tímto problémem je třeba efektivního plánování. V současnosti je tento problém spíše C1M či C10M.

Obecně existuje několik přístupů, jak se s tímto problémem vypořádat:

- **tread-per-request server** (Apache)
    - každý request je obsloužený jedním vláknem, těch je omezený počet
    - connection je typicky **blocking** operations, tedy než se vlákno přijme spojení, není možné nic jiného vykonávat
        - neefektvní přístup, vlákna nejsou využití, čekají například na odpověď od klienta
        - vysoce paměťově náročné, každé vlákno má svůj pamětní prostor
- event-drive I/O servers (Nginx, Node.js)
    - single/multi-threaded event loop
    - používá **non-blocking** (asynchronous) operační mód, kdy jsou síťové operace spravovány skrze event intercpetory (např. `epoll`) a ty hlásí, že jsou data připravena k zápisu, či čtění
        - výhoda je efektivní škálovatelnost a nízká paměťová náročnost
        - nevýhodou je složitější implementace a nutnost vhodně ošetřit race conditions

## Native Buffer

Používá se pro efektivní práci s binárními daty. Není uložený na heap, kterou spravuje GC. Výhodou této struktury je to, že není spravována GC a tudíž je rychlejší a umožňuje i asynchronní využití. Nevyhodou je nutnost manuálně ošetřtit její zprávu, jelikož v ní nefunguje GC. 

## Channels and Selectors

Analogie

- Buffer = Buffer
- Channel = Socket
- Selector = epol

![image.png](ESW%2020f363fcd29e801b8d1ec7eae7eeb390/image%201.png)

# Synchronization Mechanism

Je třeba řešit jenom v paralelních systémech.

## Naivní mechanismus - busy waiting

```c
uint8_t locked = 0;

void func(){
	while (locked) {
		// busy wait
	}
	locked = 1;
	data++;
	locked = 0;
}
```

## Atomic Operations

Využívají *compare-and-swap* nebo *compare-and-set* mechanismus a vyžadují synchronizaci celého systému. Atomicicta je zajištěna na úrovni hardware.

V porovnáním s lockem jsou celkem rychlé, ovšem poskytují pouze malé množství operací.

## Barrier

Všechny instrukce musejí být dokončeny před danou bariérou. Zamezí spekulativnímu vykonávání.

## Spin-lock

Nechceme používat, protože je postavené na principu *busy waiting.* V některých aplikacích se ale využití nevyhneme. Typickým příkladem je aplikace v embedded aplikacích, konkrétně interrupt handler nemůže jít spát, protože není, kdo by ho vzbudil.

## Mutex

Nejvíc basic mechanismu. Využívá atomicity. Může být implementovaný různými způsoby: kernel semaphore, futex.

### Kernel Mutex

Pamlaý, proto se nevyužívá.

### Futex

**Fast Userpsace Mutex** je linuxová implementace mutexu. Atomický counter a čekající v queue v kernelu. 

Jedná se o primitivní mechanismus, který se využívá pro implementaci dalších mechanismů, jako jsou:

- mutexes
- semaphores
- conditional variables
- thread barriers
- read-write locks

Je tricky - protože, když selže proces, který zamknul mutex, nastává deadlock. Proto Linux implementuje tzv. **Robust futex**, který se umí i s tímto vypořádat.

## Read-Write Lock

Jeden z klasických mechanismů. Více vláken může číst zároveň. Update zablokuje všechny čtenáře.

Zde se využívají zámky. Konkrétně čtenářský, ten může mít až $N$ vláken. Zapisovací může mít vždy ale pouze jen jedno vlákno.

![image.png](ESW%2020f363fcd29e801b8d1ec7eae7eeb390/image%202.png)

## Read-Copy-Update (RCU)

Tento mechanismus vylepšuje Read-Write lock. Vhodné, když je veliké množství čtenářů a nízké množství zápisů. Mechanismus umožňuje přistupovat čtenářům k datům bez zámků.

Při přidání dat je si zapisovatel vytvoří kopii, data upraví a ty jsou vložena na původní místo a originální data jsou smazaná až když je nikdo nečte.

# Cache-efficient data structures and algorithms

- **Cache hit** - hledaná data jsme našli v cache paměti
- **Cache miss** - hledaná data jsme v cache paměti nenašli
- **Cache replacement policy** - různé například last recently used, random, …
- **TLB - translation lookaside buffer** - buffer pro překládání virtuálních adres na fyzickou paměť

## Typy cache misses

- **cold miss** - první přístup k danému bloku paměti
- **capacity miss** - nemáme dost místa
- **conflict miss** - dva kousky paměti jsou mapované do stejné cache line
- **true sharing miss** - popsáno níže
- **false sharing miss** - popsáni níže

## Cache policies

- **Write trough** - zápisy jdou do cache a zároveň do write bufferu

![image.png](PAP%20210363fcd29e808899b7ede5a6b9f928/image%2014.png)

- **Write back** - nastavujeme jenom do cache a až pokud potřebujeme data vyměnit, zapíšeme je do write bufferu, zde musíme využít **dirty bit**

![image.png](PAP%20210363fcd29e808899b7ede5a6b9f928/image%2015.png)

## Násobení matic

Naivní algoritmus nefunguje dobře, protože využívá neefektivně $B$ matici.

![image.png](ESW%2020f363fcd29e801b8d1ec7eae7eeb390/image%203.png)

Jako vylepšení může pomoci provést transpozici $B$ matice, tedy $B^T$. Pak se budou data lépe cachovat.

![image.png](ESW%2020f363fcd29e801b8d1ec7eae7eeb390/image%204.png)

ještě lepšího výsledku lze docílit pomocí tzv. Tiled násobení (tedy po blokách), kdy každé *tile* se vejde do cache.

![image.png](ESW%2020f363fcd29e801b8d1ec7eae7eeb390/image%205.png)

## True sharing

Všechny procesory přistupují ke stejným datům - všechny mají stejná data ve své lokální cache paměti. Pokud dojde k změně jedněch dat, je je třeba dostat do cache všem ostatním. To je časově náročné.

**Řešení:** minimalizace sdílení dat.

## False sharing

Více procesorů přistupuje k různým proměnným, které jsou ale uloženy v jedné cache line a alespoň jedno jádro provádí zápis. To vede ke zpomalení. Tento problém nelze poznat ze zdrojového kódu. Pro zjištění je třeba provést analýzu.

**Řešení:** Zarovnání proměnné do paměti

```c
std::atomic_int32_t counter_cpu0 __attribute__((aligned(64)));
std::atomic_int32_t counter_cpu1 __attribute__((aligned(64)));
```

# Profiling

## Hardware performance counter

Hardwarová komponenta uvnitř CPU (Intel, ARM, …), která umožňuje počítat různé události. Například:

- cache misses
- number of interrupts
- branch misprediction

Narozdíl od softwarového profileru má hardwarový menší overhead a dokáže poskytnout informací o harwaru, protože se k nim dostane jednoduše.

Pro tento debugging existuje nástroj `perf`. Umí analyzovat jak HW (performance counter) tak i SW (system calls, trace points, …) události

## Profile-guided Optimization

Na základě profilování můžeme provést optimalizaci, kdy identifikujeme konkrétní části kódu, které nám celou aplikaci zpomalují nejvíce.

## Basics of C/C++ compilers

Debugging flag `-g`

Optimization levels: `-O0`, `-O1`, `-O2`, `-O3`, `-Os` (size), `-Og` (debugging)

Kompilace se skládá z těchto kroků

1. Compiler frontend
    - preprocessor (macar, inline, headers, comments, …)
    - parser - výsledkem je AST (Abstract Syntaxe Tree)
2. Sematic check - program make sense
    - sedí typy
    - proměnné jsou definovány než jsou použity, …
3. Optimization passes
    - high level (závislé na jazyku: inline, loop enrolling, padding, alignment, …)
        - **dead store elimination pass** - při writer-after-write ignoruj první write a proveď jenom druhý
        - unroll loops
        - dead code elimination
        - global variable optimizing
        - simplifying constrol-flow graph
    - low level (závislé na architektuře - register alocation, instruction schediling, branch prediction, profile-based optimalization)
        - peephole optimalization - kompilátor sleduje malé části kódu a snaží se je nahradit alternativami, které jsou lepší (např. místo `push` a `pop` raději nedělat nic)
        - machine instruction combiner
4. Backend - generování assemebly nebo machine code
5. Linker

## Abstract Syntax Tree (AST)

Výstup z parseru - kontroluje správnost sémantiky

## Intermediate Representation (IR)

Převádí se z AST a převádí se na assembler. Kód je podobný assembely ale je nezávislý na architektuře. Většin a optimalizací se provádí právě na této reprezentaci, tedy na IR

## High-level and Low-level optimalization passes

- je jich strašlivě moc, píšu je výše