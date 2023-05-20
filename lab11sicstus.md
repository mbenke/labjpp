Program wykonywalny dla SICStus Prolog

Umieść w swoim programie dyrektywę `user:runtime_entry(start)`, np

```
user:runtime_entry(start) :- write('Boo!'), nl, halt.
```

Kompilacja:

```
$ echo "compile('mojProgram.pl'). save_program('mojProgram.sav')." | sicstus

SICStus 4.6.0 (x86_64-linux-glibc2.17): Mon Apr  6 09:23:37 PDT 2020
Licensed to acSP4.6mimuw.edu.pl
% compiling /home/staff/iinf/ben/Zajecia/Jpp/Prolog/mojProgram.pl...
% compiled /home/staff/iinf/ben/Zajecia/Jpp/Prolog/mojProgram.pl in module user, 71 msec 592368 bytes
yes
% /home/staff/iinf/ben/Zajecia/Jpp/Prolog/mojProgram.sav created in 7 msec
yes
```

Linkowanie:


```
$ spld --static mojProgram.sav -o mojProgram
Created "mojProgram"
```

Wykonanie:

```
$ ./mojProgram 
Boo!
```

Przykładowy Makefile:

```
SICSTUSHOME=/opt/sicstus
SICSTUSBIN= $(SICSTUSHOME)/bin
PL = $(SICSTUSBIN)/sicstus
SPLD = spld
SPLDFLAGS = --static 

BINFILES = mojProgram

all: $(BINFILES)

%: %.sav
	$(SPLD) $(SPLDFLAGS) $< -o $@ 

%.sav: %.pl
	echo "compile('$<'). save_program('$@')." | $(PL)

clean:
	rm -f $(ALL) *.sav
```
