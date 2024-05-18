## Podstawowe informacje o operacjach wejścia/wyjścia w Prologu

1. Termowe operacje wejścia/wyjścia, standardowe predykaty:
      * `write(Term)` - wypisanie termu (na bieżący plik wyjściowy)
      * `nl`  - wypisanie znaku końca wiersza (na bieżący plik wyjściowy)
            (równoważne wywołaniu: `write('\n')`)
      * `format('<opis formatu>', ListaElementów)`
          wypisanie wszystkich elementów, zgodnie z podanym opisem
          znaki sterujące: poprzedzone `~` (np. `~d`, `~n`)
          np. `format('Proces ~d: ~d~n', [Id, Licznik])`, więcej - patrz [dokumentacja](https://www.swi-prolog.org/pldoc/man?predicate=format/2).
2. Wczytywanie
      * `read(Term)` - wczytanie (z bieżącego pliku wejściowego, domyślnie stdin) dowolnego
          (poprawnego składniowo) termu, zakończonego kropką;
          np. `read(program(ListaInstrukcji))`
      * zmiana znaku zachęty ze standardowego `|:` przy użyciu `prompt/2`; np. `prompt(_,' ')`
      * `compile(NazwaPliku)` - wczytanie klauzul z pliku, np `compile(`graph.txt`).
 
3. (Elementarne) operacje na plikach
     - otwarcie pliku (do czytania):    see(NazwaPliku)
     - zamknięcie pliku (do czytania):  seen

4. Kompilacja programu (SWI-Prolog)
   ```
   swipl -g start -c program.pl -o program
   ```
   gdzie w miejsce `start` wstawiamy (zeroargumentowy predykat), od którego ma sie rozpocząć wykonanie

5. Argumenty programu
   ```
   start :- prolog_flag(argv, Args), write(Args), nl.
   ```
6. Kompilacja programu (SICStus)
```
$ cat args.pl
main :- prolog_flag(argv, Args), write(Args), nl.
user:runtime_entry(start) :- main.

$ echo "compile('args.pl'). save_program('args.sav')." | /opt/sicstus/sicstus4.8.0/bin/sicstus
$ spld --static --exechome=/opt/sicstus/sicstus4.8.0/bin args.sav -o args 
$ ./args foo 'bar(1,2)' '2+3'
[foo,bar(1,2),2+3]
```

Przykładowy Makefile:

```
SICSTUSHOME=/opt/sicstus/sicstus4.8.0
SICSTUSBIN= $(SICSTUSHOME)/bin
PL = $(SICSTUSBIN)/sicstus
SPLD = spld
SPLDFLAGS = --static --exechome=$(SICSTUSBIN)

ALL = mojProgram

all: $(ALL)

%: %.sav
	$(SPLD) $(SPLDFLAGS) $< -o $@ 

%.sav: %.pl
	echo "compile('$<'). save_program('$@')." | $(PL)

clean:
	rm -f $(ALL) *.sav
```


## Ćwiczenie

Napisz program, który wczyta plik o nazwie podanej jako argument ,postaci

```
variables(ListaNazwZmiennychProstych).
arrays(ListaNazwZmiennychTablicowych).
program(ListaInstrukcji).
```

i wypisze go na stdout

Przykładowy plik:

```
vars([k]).
arrays([chce]).
program([assign(arr(chce, pid), 1),
	 assign(k, pid),
         condGoto(arr(chce, 1-pid) = 0, 5),
	 condGoto(k = pid, 3),
         sekcja,
	 assign(arr(chce, pid), 0),
	 goto(1)]).
```
