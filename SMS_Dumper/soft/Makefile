
#OS = LINUX
#OS = MACOSX
OS = WINDOWS

PROG = SMS_dumper_v0

# To set up Ubuntu Linux to cross compile for Windows:
#
#    apt-get install i686-w64-mingw32-gcc
#
# Just edit the variable above for WINDOWS, then use "make" to build rawhid.exe

ifeq ($(OS), LINUX)
TARGET = $(PROG)
CC = gcc
STRIP = strip
CFLAGS = -Wall -O2 -DOS_$(OS)
LIBS = -lusb
else ifeq ($(OS), MACOSX)
TARGET = $(PROG).dmg
SDK = /Users/fabrice/Developer/SDKs/MacOSX10.9.sdk
ARCH = -mmacosx-version-min=10.9 -arch i386
CC = gcc
STRIP = strip
CFLAGS = -Wall -O2 -DOS_$(OS) -isysroot $(SDK) $(ARCH)
LIBS = $(ARCH) -Wl,-syslibroot,$(SDK) -framework IOKit -framework CoreFoundation
else ifeq ($(OS), WINDOWS)
TARGET = $(PROG).exe
CC=i686-w64-mingw32-gcc
STRIP=i686-w64-mingw32-strip
CFLAGS = -Wall -O2 -DOS_$(OS)
LIBS = -lhid -lsetupapi
endif

OBJS = $(PROG).o hid.o


all: $(TARGET)

$(PROG): $(OBJS)
	$(CC) -o $(PROG) $(OBJS) $(LIBS)
	$(STRIP) $(PROG)

$(PROG).exe: $(PROG)
	cp $(PROG) $(PROG).exe

$(PROG).dmg: $(PROG)
#	mkdir tmp
#	cp $(PROG) tmp
#	hdiutil create -ov -volname "Md Dumper" -srcfolder tmp $(PROG).dmg

hid.o: hid_$(OS).c hid.h
	$(CC) $(CFLAGS) -c -o $@ $<

clean:
	rm -f *.o $(PROG) $(PROG).exe $(PROG).dmg
	rm -rf tmp
