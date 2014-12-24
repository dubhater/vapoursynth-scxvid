AM_CXXFLAGS = -O2 -std=c++11 -Wall -Wextra -Wno-unused-parameter -Wshadow

AM_CPPFLAGS = $(VapourSynth_CFLAGS) $(XVID_CFLAGS)

lib_LTLIBRARIES = libscxvid.la

libscxvid_la_SOURCES = src/scxvid.cpp

libscxvid_la_LDFLAGS = -no-undefined -avoid-version $(PLUGINLDFLAGS)

libscxvid_la_LIBADD = $(XVID_LIBS)
