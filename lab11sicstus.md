## Program wykonywalny dla SICStus Prolog

Umieść w swoim programie dyrektywę `user:runtime_entry(start)`, np

```
user:runtime_entry(start) :- write('Boo!'), nl, halt.
```

Kompilacja:

```
$ echo "compile('mojProgram.pl'). save_program('mojProgram.sav')." | sicstus
...
% compiling /home/staff/iinf/ben/Zajecia/Jpp/Prolog/mojProgram.pl...
% compiled /home/staff/iinf/ben/Zajecia/Jpp/Prolog/mojProgram.pl in module user, 71 msec 592368 bytes
yes
% /home/staff/iinf/ben/Zajecia/Jpp/Prolog/mojProgram.sav created in 7 msec
yes
```

Linkowanie:


```
$ spld --static mojProgram.sav --exechome=/opt/sicstus/bin -o mojProgram mojProgram.sav
Created "mojProgram"
```

Wykonanie:

```
$ ./mojProgram 
Boo!
```
## Argumenty programu
Do odczytania arumentów można użyć `prolog_flag(argv, Args)`

```
main :- prolog_flag(argv, Args), write(Args), nl.

user:runtime_entry(start) :- main.
```

W trybie interaktywnym argumenty można przekazać przy poomocy opcji `-a`, np:

```
$ sicstus -l args.pl -a ala ma kota
...
| ?- main.
[ala,ma,kota]
yes
```

## Przykładowy program główny w Prologu

```
user:runtime_entry(start):-
    (current_prolog_flag(argv, [File]) ->
        set_prolog_flag(fileerrors, off),
        (compile(File) -> true
         ;
	 format('Error opening file ~p.\n', [File])
        ),
        prompt(_Old, ''),         % pusty prompt
	przetwarzaj
     ;
	write('Incorrect usage, use: program <file>\n')
    ).


% właściwe przetwarzanie

przetwarzaj :-
    write('Podaj miejsce startu: '),
    read(Start),
    write('Podaj koniec: '),
    read(Meta),
    (
      trasa(_Id, Start, Meta, _Rodzaj, _Kierunek, Km) ->
      format('Istnieje trasa dlugosci ~d.~n', [Km])
    ;
      format('Brak trasy z ~p do ~p.~n', [Start, Meta])
    ).
```

## Przykładowy Makefile

```
SICSTUSHOME=/opt/sicstus
SICSTUSBIN= $(SICSTUSHOME)/bin
PL = $(SICSTUSBIN)/sicstus
SPLD = spld
SPLDFLAGS = --static --exechome=$(SICSTUSBIN)

BINFILES = mojProgram

all: $(BINFILES)

%: %.sav
	$(SPLD) $(SPLDFLAGS) $< -o $@ 

%.sav: %.pl
	echo "compile('$<'). save_program('$@')." | $(PL)

clean:
	rm -f $(ALL) *.sav
```
