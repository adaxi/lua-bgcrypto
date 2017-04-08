--- /dev/null
+++ b/Makefile
@@ -0,0 +1,41 @@
+#!/usr/bin/make
+
+OMIT_FRAME_POINTER = -fomit-frame-pointer
+
+LUAPKG_CMD = $(shell pkg-config --list-all | grep Lua | awk 'FNR == 1 {print $$1}')
+CFLAGS = -fPIC -O3 $(shell pkg-config "$(LUAPKG_CMD)" --cflags)
+LFLAGS = -shared $(OMIT_FRAME_POINTER)
+INSTALL_PATH = $(shell pkg-config "$(LUAPKG_CMD)" --variable=INSTALL_CMOD)
+LUA_LIBDIR = /usr/lib
+
+
+SOURCES = src/l52util.c src/sha/sha1.c src/sha/sha2.c src/sha/hmac.c
+OBJECTS = ${SOURCES:.c=.o}
+LIBNAMES = sha1 sha224 sha256 sha384 sha512 sha512_128 sha512_192 sha512_224 sha512_256
+CC = gcc
+
+INCS = -Isrc/sha
+DEFS = -DRETURN_VALUES -DVOID_RETURN=void -DINT_RETURN=int
+
+all: dirs ${OBJECTS} ${LIBNAMES}
+
+dirs:
+	mkdir -p bgcrypto
+
+$(OBJECTS):
+	$(CC) $(CFLAGS) -c ${@:.o=.c} -o $@  ${DEFS} ${INCS}
+
+$(LIBNAMES):
+	$(CC) $(CFLAGS) -c src/l$@.c -o src/$@.o ${DEFS} ${INCS}
+	$(CC) $(LFLAGS) -o bgcrypto/$@.so -L${LUA_LIBDIR} ${OBJECTS} src/$@.o
+
+install:
+	mkdir -p $(DESTDIR)$(INSTALL_PATH)/ $(DESTDIR)/usr/share/lua/5.1/bgcrypto/
+	cp -dr --no-preserve=ownership bgcrypto/ $(DESTDIR)$(INSTALL_PATH)/
+	cp -dr --no-preserve=ownership src/lua/ $(DESTDIR)/usr/share/lua/5.1/bgcrypto/
+
+clean:
+	rm bgcrypto -rf
+	find -name '*.so' -o -name '*.o' -exec rm -f {} \;
+
+.PHONY: all build install clean
