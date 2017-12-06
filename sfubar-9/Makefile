# unix and cygwin: make test

# uncomment for debugging
CFLAGS=		-O0 -g

# uncomment for cygwin
#CFLAGS+=	-mno-cygwin -mwindows



# .CURDIR is already set in bmake, use a gmake extension to set it in gmake
.CURDIR?=	$(shell pwd)

PREFIX?=	$(.CURDIR)
SRCDIR?=	$(.CURDIR)
PT=		parsetxt-4
RR=		riffraff-7
SF=		sfubar-9
OBJS=		trap.o sfutil.o aifutil.o wavutil.o
LIB = libfreepatssfubar.so.0

CC=		$(TOOLDIR)gcc
AR=		$(TOOLDIR)ar
RANLIB=		$(TOOLDIR)ranlib
STRIP=		$(TOOLDIR)strip

CFLAGS+=	-Wall -I$(PREFIX)/include
LDLIBS+=	-lm

all:	$(LIB) freepats-sfubar freepats-gensine

$(PREFIX)/lib/lib$(SF).a: install-includes $(PREFIX)/lib/lib$(RR).a $(OBJS)
	mkdir tmp ; \
	cp ${PREFIX}/lib/lib${PT}.a ${PREFIX}/lib/lib${RR}.a ${OBJS} tmp/ ; \
	cd tmp ; \
	${AR} -x lib${PT}.a ; \
	${AR} -x lib${RR}.a ; \
	${AR} -r ${PREFIX}/lib/lib${SF}.a *.o ; \
	${RANLIB} ${PREFIX}/lib/lib${SF}.a ; \
	rm *.[ao]

install-includes:
	cp aifutil.h sfubar.h sfutil.h trap.h wavutil.h ${PREFIX}/include/
	cp trap.h ${SRCDIR}/lib${PT}/include
	cp trap.h ${SRCDIR}/lib${RR}/include
	cp trap.h ${PREFIX}/include

$(LIB):	$(PREFIX)/lib/lib$(SF).a
	mkdir tmp ; \
	cp ${PREFIX}/lib/lib${PT}.a ${PREFIX}/lib/lib${RR}.a ${PREFIX}/lib/lib${SF}.a tmp/ ; \
	cd tmp ; \
	${AR} -x lib${SF}.a ; \
	${AR} -x lib${PT}.a ; \
	${AR} -x lib${RR}.a ; \
	${CC} -shared ${CFLAGS} -o $@ *.o -s ; \
	rm *.[ao] ; \
	mv $@ ..

freepats-sfubar:	main.o $(LIB)
	${CC} ${CFLAGS} -o $@ $(LIB) main.o ${LDLIBS} -s


freepats-gensine:	gensine.o trap.o $(LIB)
	${CC} ${CFLAGS} -o $@ $^ ${LDLIBS} -s

$(PREFIX)/lib/lib$(RR).a:
	cd ${SRCDIR}/lib${RR} && \
	PREFIX=${PREFIX} CFLAGS="${CFLAGS}" TOOLDIR=${TOOLDIR} ${MAKE} install

clean:
	rm -f *.o sfubar gensine *.core bin/ieee80-test tmp/* lib/*
	cd ${SRCDIR}/lib${RR} && \
	PREFIX=${PREFIX} TOOLDIR=${TOOLDIR} ${MAKE} clean

test:	all
	chmod u+x ./test.sh
	./test.sh

sfutil.o:	sfutil.c
main.o:		main.c
aifutil.o:	aifutil.c
wavutil.o:	wavutil.c
gensine.o:	gensine.c
trap.o:		trap.c
