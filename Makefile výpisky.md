# Makefile
- **Make** spouštíme příkazem:

  ```
  make [options] target1 target2 ...
  ```

  *Soubory* - make hledá soubory s názvem `makefile` a `Makefile` v tomto pořadí.
  Pokud chceme předat jiný soubor, zapíšeme to `make -f soubor target`.
  Pokud nepředáme makeu target, tak se spustí první target v souboru.

  *Fungování* - make pracuje na základě změn v zdrojových souborech. Překompilují
  se jen ty soubory, u kterých je to nutné. Při kompilaci se porovná čas poslední
  změny objektového souboru a jeho závislostí. Pokud je některá závislost novější,
  tak se objektový soubor překompiluje.

- **Makefile** se skládá z těchto 5 částí:
  - Explicitní pravidla
  - Implicitní pravidla
  - Proměnné
  - Direktivy
  - Komentáře - komentář začíná znakem `#`

Kompletní návod k make [zde][make návod]

[make návod]: https://www.gnu.org/software/make/manual/ "GNU Make"

* * *

## Explicitní pravidla
**Pravidlo** - makefile se skládá z pravidel, které říkají, jak vytvořit určitý
cíl. Pravidla mají formát:

```
target: dependencies
    system command(s)
```

- <u>Target</u> - jméno cíle, který se má vytvořit/provést (v základu bráno jako
  jméno souboru)
    - Jedno pravidlo může být použito pro více cílů (oddělují se mezerou:
      `target1 target2: dependencies`)
- <u>Dependencies</u> - seznam souborů, na kterých je cíl závislý (nemusí být
  žádná závislost)
- <u>System command(s)</u> - příkaz(y), které se mají provést pro
  vytvoření/provedení cíle. Musí být odsazeny jedním tabem.

Příklad makefile:
``` Makefile
hello: factorial.o hello.o main.o
	g++ -o hello factorial.o hello.o main.o

factorial.o: factorial.cpp functions.h
	g++ -c factorial.cpp

hello.o: hello.cpp functions.h
	g++ -c hello.cpp

main.o: main.cpp functions.h
	g++ -c main.cpp
```

**Samostatné shelly** - každý řádek se provádí v samostatném shellu - př. změna
pracovní složky se neprojeví na dalším řádku. Lze řešit:

``` Makefile
cd folder && rm *.cpp
```

**Ignorování chyb** - make skončí, pokud některý z příkazů vrátí chybovou
návratovou hodnotu. Pokud chceme, aby make chybu ignoroval, napíšeme před
příkaz `-`.

``` Makefile
clean:
  -rm *.o core
```

**Vypnutí výpisu** - make vypisuje příkazy, které provádí. Lze vypnout zapsáním
před příkaz znak `@`.

``` Makefile
install:
  @echo You must be root to install
```

**Standardní cíle** - u makefile se očekává, že budou existovat tyto cíle:

- `all` - vše zkompiluje. Programy lze otestovat, než budou nainstalovány.
  Tento cíl se má spustit, když se spustí make bez parametrů.
- `clean` - odstranění všeho, co se vytvořilo při kompilaci: spustitelné
  soubory, dočasné soubory, objektové soubory ...
- `install` - nainstaluje program(y) na správná místa v systému. Můžou být
  potřeba administrátorská práva.
- `uninstall` - odinstalování programu ze systému

**Falešné cíle** - standardně je jméno cíle bráno jako jméno souboru, který
se má vytvořit, když se pravidlo provede. Pokud daný soubor existuje (a není
potřeba ho aktualizovat), tak make dané pravidlo nespouští.

Často máme ale cíle, které nevytvářejí soubory, ale jsou receptem na určitou
činnost. Tyto cíle chceme provést vždy, i když existuje soubor se stejným
názvem. Definují se jako závislosti speciálního cíle `.PHONY`, který se může v
makefile souboru objevovat opakovaně.

``` Makefile
.PHONY: all clean install uninstall
```

* * *

## Proměnné
Proměnné jsou podobné unixovým proměnným. Definují se jako pár s rovnítkem:

``` Makefile
PROMĚNNÁ = obsah proměnné
CFLAGS = -O3 -std=c++11
LIBS = -lncurses -lm -lsdl
MŮŽU_NADEFINOVAT_COKOLI = ":-)"
```

**Typy přiřazení**:

- *Lazy set* - standardní deklarace proměnné, hodnoty uvnitř proměnné jsou
  rekurzivně nahrazovány, když je proměnná použita, ne když je deklarována.

  ``` Makefile
  VARIABLE = value
  ```

- *Immediate Set* - hodnoty jsou nahrazovány při deklaraci, jednoduchá expanze

  ``` Makefile
  VARIABLE := value
  ```

- *Set If Absent* - nastaví proměnnou jen pokud nemá hodnotu

  ``` Makefile
  VARIABLE ?= value
  ```

- *Append* - připojí hodnotu na konec již existující. Před vkládaný text je
  vložena mezera. Pokud proměnná neexistuje, je vytvořena.

  ``` Makefile
  VARIABLE += value
  ```

**Použití proměnných** - v místě, kde chceme nahradit proměnnou její hodnotou
napíšeme `$(PROMĚNNÁ)` nebo `${PROMĚNNÁ}`.

  - Pokud chceme v příkazu použít znak `$`, musíme ho zvojit, jinak bude brán
    jako označení proměnné. Příkladně, když chceme použít shellovou proměnnou:

    ``` Makefile
    all:
	   for d in $(SUBDIR); do $(MAKE) -C $$d; done
    ```

**Automatické proměnné** - jsou automaticky vytvořeny
(seznam všech [zde][automatic variables]):

- `$@` - název cíle
- `$^` - závislosti cíle

``` Makefile
OBJ = factorial.o hello.o main.o
CXXFLAGS = -O3

hello: $(OBJ)
	$(CXX) $(LDFLAGS) -o $@ $^
```

[automatic variables]: https://ftp.gnu.org/old-gnu/Manuals/make-3.79.1/html_chapter/make_10.html#SEC101 "Automatic variables"

Proměnné jsou case sensitive.

### Předdefinované proměnné
Předdefinované proměnné se dělí na *proměnné programů* a
*proměnné argumentů programů*.
Rozsáhlý seznam [zde][variables]. Úplný seznam lze zjistit příkazem `make -p`.
(Jsou používány v implicitních pravidlech.)

[variables]: https://ftp.gnu.org/old-gnu/Manuals/make-3.79.1/html_chapter/make_10.html#SEC96 "Variables used by Implicit Rules"

#### Proměnné programů
Proměnná | Program
---------|--------
AR | program pro archivaci, default je `ar`
CC | program pro komplilaci programů v C, default `cc`
CXX | program pro kompilaci C++ programů, default `g++`
MAKE | make

#### Proměnné argumentů programů
Defaultní hodnota je prázdný řetězec, pokud není řečeno jinak.

Proměnná | Význam
---------|-------
ARFLAGS | argumenty pro archivační program, defaultní obsah je `rv`
CFLAGS | argumenty pro C compiler
CXXFLAGS | argumenty pro C++ compiler
LDFLAGS | argumenty pro kompilery, když mají vyvolat linker, default `ld`

* * *

## Implicitní pravidla
**Implicitní pravidla** - specifikují, jak zpracovávat soubory s podobným jménem,
aniž by bylo potřeba pro každý z nich specifikovat samostatné pravidlo.

**Použití** - implicitní pravidlo je použito, pokud nespecifikujeme příkazy.

- Buď napíšeme pravidlo bez příkazů (což umožní kontrolovat, zda se změnily
  header soubory):

  ``` Makefile
  factorial.o: factorial.cpp factorial.h
  ```

- Nebo žádné pravidlo nespecifikujeme:

  ``` Makefile
  # toto je celý makefile
  hello: factorial.o hello.o main.o
	 $(CXX) $(LDFLAGS) -o $@ $^
  ```

Make má zabudovaná implicitní pravidla pro tvorbu `.o` souborů z `.c` a `.cpp`
souborů. (seznam všech zabudovaných implicitních pravidel [zde][impl rules]).

[impl rules]: https://www.gnu.org/software/make/manual/html_node/Catalogue-of-Rules.html "Implicitní pravidla"

### Tvorba implicitních pravidel
Tvoří se jako normální pravidla, ale obsahují v targetu jeden znak `%`.
`%` je nahrazen jakýmkoliv neprázdným řetězcem.
Závislosti používají `%` pro použití stejného řetězce:

``` Makefile
hello: factorial.cpp.o hello.cpp.o main.cpp.o
	$(CXX) $(LDFLAGS) -o $@ $^

f%.cpp.o: f%.cpp  # příklad s předponou
  $(CXX) $(CXXFLAGS) -c -o $@ $^

%.cpp.o: %.cpp # příklad používající jen suffixy
  $(CXX) $(CXXFLAGS) -c -o $@ $^
```

**Automatické proměnné implicitních pravidel**:

- `$<` - jméno souboru, který spustil akci (př. `factorial.cpp`)
- `$*` - obsah zástupného symbolu `%` (př. `actorial` - příklad s předponou)

``` Makefile
f%.cpp.o: f%.cpp
	$(CXX) $(CXXFLAGS) -c $< -o $@
#nebo
f%.cpp.o: f%.cpp
	$(CXX) $(CXXFLAGS) -c f$*.cpp -o $@
```

* * *

## Direktivy

### Podmíněné provedení

- `ifeq` - provede se, pokud se oba řetězce shodují
- `ifneq` - provede se, pokud se oba řetězce neshodují
- `ifdef` - provede se, pokud je argument pravdivý
- `ifndef` - provede se, pokud je argument nepravdivý
- `else` - else větev
- `endif` - ukončení podmínky

Možnosti zápisu podmínky:

``` Makefile
ifeq (arg1, arg2)
ifeq 'arg1' 'arg2'
ifeq "arg1" "arg2"
```

Příklad:

``` Makefile
libs_for_gcc = -lgnu
libs_for_other =

hello: $(objects)
ifeq "$(CC)" "gcc"
  $(CC) -o $@ $(objects) $(libs_for_gcc)
else
  $(CC) -o $@ $(objects) $(libs_for_other)
endif
```

### Include
Použití direktivy `include` pozastaví čtení aktuálního souboru a před
pokračováním přečte specifikované makefile soubory. Zápis:

```
include soubor(y)
```

Příklad:
``` Makefile
#soubor Makefile.inc
CXXFLAGS = -O3

#jiný makefile soubor
include Makefile.inc
hello: main.cpp
  $(CXX) -o $@ $^ $(CXXFLAGS)
```

### Override
Proměnné předané na příkazové řádce přepisují obsah proměnných zapsaných v
makefile souboru. Pokud chceme, aby se ignorovala deklarace z příkazové řádky
a použila se deklarace v makefile, tak použijeme direktivu `override`:

``` Makefile
override proměnná = hodnota
```

* * *

## Ostatní

**Rekurzivní použití make** - v makefile můžeme volat `make` a provádět tak
další makefile soubory (př. určené pro jednotlivé podčásti systému).

``` Makefile
podsystém:
  cd podsložka && $(MAKE)
# nebo
podsystém:
  $(MAKE) -C podsložka
```

**Exportování proměnných do submakeu**:

``` Makefile
export proměnná ...
unexport proměnná ...
```

**Vkládání header souborů z jiné složky** - pokud jsou header soubory v jiné
složce, tak je potřeba k nim poskytnout cestu. To se dělá pomocí parametru `-I`.

``` Makefile
INCLUDES = -I "../header"
OBJ = main.o factorial.o hello.o

hello: $(OBJ)
  $(CC) $(LDFLAGS) $(INCLUDES) -o $@
```

**VPATH** - obsah proměnné `VPATH` je seznam dalších umístění, kde má make
hledat závislosti. Jednotlivé cesty jsou oddělené dvojtečkou nebo mezerou.
Více [zde][vpath].

``` Makefile
VPATH = src:../headers
# specifikuje 2 složky src a ../headers
```

[vpath]: https://www.gnu.org/software/make/manual/html_node/General-Search.html "VPATH"

**Zalamování řádků** - pomocí zpětného lomítka

``` Makefile
OBJ = main.o factorial.o \
      hello.o
```

**Funkce** - [make funkce][functions] mají tvar `$(funkce argumenty)`.
Př. získání všech `.o` souborů složky:

``` Makefile
OBJ := $(patsubst %.cpp, %.o, $(wildcard *.cpp))
```

[functions]: https://www.gnu.org/software/make/manual/html_node/Functions.html "Functions for Transforming Text"
