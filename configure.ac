AC_INIT([scxvid], [1], [https://github.com/dubhater/vapoursynth-scxvid/issues], [scxvid], [https://github.com/dubhater/vapoursynth-scxvid/])

: ${CXXFLAGS=""}

AM_INIT_AUTOMAKE([foreign no-dist-gzip dist-xz subdir-objects no-define])
AM_SILENT_RULES([yes])

LT_INIT([disable-static win32-dll])

AC_PROG_CXX

AC_CANONICAL_HOST


dnl Workaround for a bug in libtool
dnl The windows libtool uses a file magic checking method that only accepts
dnl dynamic libraries. Change it for libtool's alternative checking method.

dnl Workaround found in configure.ac from ffms2.

dnl Reproducing the error message below, for search engines and people
dnl looking for a solution...

dnl *** Warning: linker path does not have real file for library -lfftw3f.
dnl *** I have the capability to make that library automatically link in when
dnl *** you link to this library.  But I can only do this if you have a
dnl *** shared version of the library, which you do not appear to have
dnl *** because I did check the linker path looking for a file starting
dnl *** with libfftw3f and none of the candidates passed a file format test
dnl *** using a file magic. Last file checked: /home/asdf/mingw-w64/i686/mingw/lib/libfftw3f.a
dnl *** The inter-library dependencies that have been dropped here will be
dnl *** automatically added whenever a program is linked with this library
dnl *** or is declared to -dlopen it.
dnl *
dnl *** Since this library must not contain undefined symbols,
dnl *** because either the platform does not support them or
dnl *** it was explicitly requested with -no-undefined,
dnl *** libtool will only create a static version of it.

if test "$lt_cv_file_magic_cmd" = "func_win32_libid" ; then
    deplibs_check_method='file_magic file format pei*-(i386|x86-64)|(.*architecture: i386)?'
    file_magic_cmd='$OBJDUMP -f'
fi


AS_CASE(
  [$host_cpu],
  [i?86],   [BITS="32"],
  [x86_64], [BITS="64"],
  []
)

AS_CASE(
   [$host_os],
   [cygwin*|mingw*],
   [
      AS_IF(
         [test "x$BITS" = "x32"],
         [
          AC_SUBST([PLUGINLDFLAGS], ["-Wl,--kill-at"])
         ]
      )
   ]
)


AC_ARG_VAR([XVID_CFLAGS], [CFLAGS for xvid])
AC_ARG_VAR([XVID_LIBS], [LIBS for xvid])

AS_IF(
      [test -z "$XVID_LIBS"],
      [XVID_LIBS="-lxvidcore" AC_SUBST([XVID_LIBS])]
)

AC_LANG_PUSH([C++])
saved_cppflags="$CPPFLAGS"
saved_libs="$LIBS"
CPPFLAGS="$XVID_CFLAGS"
LIBS="$XVID_LIBS"
AC_LINK_IFELSE(
               [AC_LANG_PROGRAM(
                                [[
#include <xvid.h>
                                ]],
                                [[
xvid_gbl_info_t xvid_info;
(void) xvid_global(0, XVID_GBL_INFO, &xvid_info, 0);
                                ]]
               )],
               [],
               [AC_MSG_ERROR([failed to link libxvidcore. See config.log for details.])]
)

CPPFLAGS="$saved_cppflags"
LIBS="$saved_libs"
AC_LANG_POP([C++])


PKG_CHECK_MODULES([VapourSynth], [vapoursynth])

AC_CONFIG_FILES([Makefile])
AC_OUTPUT
