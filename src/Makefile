
CC=gcc
CFLAGS=-levent -lgd -lmemcached -lm -I../deps/hiredis -O3 -g -Wall

CCCOLOR="\033[34m"
LINKCOLOR="\033[34;1m"
SRCCOLOR="\033[33m"
BINCOLOR="\033[37;1m"
MAKECOLOR="\033[32;1m"
ENDCOLOR="\033[0m"

ifndef V
QUIET_CC = @printf '    %b %b\n' $(CCCOLOR)CC$(ENDCOLOR) $(SRCCOLOR)$@$(ENDCOLOR);
QUIET_LINK = @printf '    %b %b\n' $(LINKCOLOR)LINK$(ENDCOLOR) $(BINCOLOR)$@$(ENDCOLOR);
endif


CCOPT= $(CFLAGS) $(ARCH) $(PROF)

PREFIX= /opt/hcaptcha
INSTALL_ETC= $(PREFIX)/etc
INSTALL_BIN= $(PREFIX)/bin
INSTALL_FONT= $(PREFIX)/fonts
INSTALL= cp -p

OBJ=hcaptchad.o config.o ../deps/hiredis/sds.o

PRGNAME=hcaptchad

all: hcaptchad 
	@echo ""
	@echo "hcaptchad build complete ;)"
	@echo ""

# Deps (use make dep to generate this)
hcaptchad.o: hcaptchad.c config.h
config.o: config.c config.h

dependencies:
	@printf '%b %b\n' $(MAKECOLOR)MAKE$(ENDCOLOR) $(BINCOLOR)hiredis$(ENDCOLOR)
	cd ../deps/hiredis && $(MAKE) 

hcaptchad: dependencies $(OBJ)
	$(QUIET_LINK)$(CC) -o $(PRGNAME) $(CCOPT) $(DEBUG) $(OBJ) $(CCLINK)

.c.o:
	$(CC) -c $(CFLAGS) $(OBJARCH) $(DEBUG) $(COMPILE_TIME) $<
	
clean:
	rm -rf $(PRGNAME) *.o *.gcda *.gcno *.gcov

dep:
	$(CC) -MM *.c -I ../deps/hiredis 

install: all
	mkdir -p $(INSTALL_ETC)
	$(INSTALL) ../scripts/hcaptchad.conf $(INSTALL_ETC)
	mkdir -p $(INSTALL_BIN)
	$(INSTALL) $(PRGNAME) $(INSTALL_BIN)
	mkdir -p $(INSTALL_FONT)
	$(INSTALL) ../fonts/* $(INSTALL_FONT)

