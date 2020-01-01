CC=gcc
CFLAGS=-Wall -g

calcula: calcula.c calc.o
	$(CC) $(CFLAGS) calcula.c calc.o -o calcula.exe

calc.o: calc.c calc.h
	$(CC) $(CFLAGS) -c calc.c -o calc.o

.PHONY: clean
clean:
	-rm -rf *.o
	-rm calcula.exe

.PHONY: cleanall
cleanall: clean
	-rm -rf calcula.tar.gz
	-rm -rf ../dist;

.PHONY: dist
dist: clean calcula
	-rm -rf ../dist;
	-mkdir -p ../dist/usr/bin/calc
	cp calcula.exe ../dist/usr/bin/calc

.PHONY: targz
targz: clean
	mkdir -p source
	cp *.c *.h Makefile source
#	tar -cvf calcula.tar source
#	gzip calcula.tar
	tar -czvf calcula.tar.gz source
	rm -rf calcula.tar
	rm -rf source

.PHONY: install
install: dist
	sudo cp -r ../dist/* /
