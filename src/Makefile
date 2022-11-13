# -- compiler --
C	=gcc
CC	=g++
# ---

# -- dirs --
ODIR   =obj
BDIR   =build
IDIRS  =-I./
IDIRS :=-Ilib
BIN    =spritepack
# ---

# -- flags --
CFLAGS =-O3 -lm -ldl -std=c++11 -I. $(IDIRS)
CFLAGS+=-g -pedantic -Wall -Wshadow -Werror -Wno-unused -Wno-switch -Wno-int-in-bool-context -fdiagnostics-color=always
# ---

# -- deps --
SRCDIR =*.c */*.c */*/*.c *.cpp */*.cpp */*/*.cpp
HDIR   =*.h */*.h */*/*.h *.hpp */*.hpp */*/*.hpp
DEPS   =$(filter-out $(wildcard lib/*.h) $(wildcard lib/*/*.h),$(wildcard $(HDIR)))
LDEPS  =$(wildcard lib/*.c lib/*/*.c lib/*.cpp lib/*/*.cpp)

_OBJ   =$(patsubst %.c,%.o,$(wildcard $(SRCDIR)))
_OBJ  :=$(patsubst %.cpp,%.o,$(_OBJ))
OBJ    =$(addprefix $(ODIR)/, $(notdir $(_OBJ)))
# ---

# -- windows --
ifeq ($(OS),Windows_NT)
C     =x86_64-w64-mingw32-gcc
CC    =x86_64-w64-mingw32-g++
LD    =x86_64-w64-mingw32-ld
FLAGS   =-Wl,-Bstatic -g -lm -static -static-libgcc -static-libstdc++ -lstdc++ \
-lmingw32 -Wl,-Bdynamic -static-libstdc++ -static -I. $(IDIRS)

CFLAGS  =$(FLAGS)
CFLAGS +=-std=c++11 -DAL_LIBTYPE_STATIC
BNAME   =spritepack.exe
endif
# ---

# -- macos --
UNAME  = $(shell uname -s)
ifeq ($(UNAME),Darwin)
CFLAGS =-O3 -g -lm -framework Foundation -framework IOKit -I. $(IDIRS) -Wno-unused-command-line-argument
CFLAGS+=-std=c99 -Wall -Wno-unused
endif
# ---

# -- rules --
# my code
$(ODIR)/%.o: %.c $(DEPS)
	$(CC) -c -o $@ $< $(CFLAGS)

$(ODIR)/%.o: %.cpp $(DEPS)
	$(CC) -c -o $@ $< $(CFLAGS)

$(ODIR)/%.o: lib/%.c $(DEPS)
	$(CC) -c -o $@ $< $(CFLAGS)

$(ODIR)/%.o: lib/%.cpp $(DEPS)
	$(CC) -c -o $@ $< $(CFLAGS)
# ---

# -- main --
all: files spritepack

spritepack: $(OBJ)
	$(CC) -o $(BDIR)/$(BIN) $^ $(CFLAGS) $(LIBS)
# ---

# -- files --
files:
	mkdir -p $(ODIR)
	mkdir -p $(BDIR)
# ---

# -- util --
.PHONY: clean

clean:
	rm -rf $(ODIR)
# ---
