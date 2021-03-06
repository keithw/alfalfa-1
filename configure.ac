#                                               -*- Autoconf -*-
# Process this file with autoconf to produce a configure script.

AC_PREREQ([2.69])
AC_INIT([alfalfa], [0.1], [alfalfa@mit.edu])
AM_INIT_AUTOMAKE([foreign])
AC_CONFIG_SRCDIR([src/decoder/prediction.cc])
AC_CONFIG_HEADERS([config.h])

# Checks for programs.
AC_PROG_CXX
AC_PROG_RANLIB
AC_PATH_PROG([PERL], [perl], [])
AS_IF([test x"$PERL" = x],
  [AC_MSG_ERROR([cannot find perl])])

# Add picky CXXFLAGS
CPPFLAGS="-std=c++11"
PICKY_CXXFLAGS="-pedantic -Wall -Wextra -Weffc++ -Werror"
AC_SUBST([PICKY_CXXFLAGS])
AC_LANG_PUSH(C++)

AC_ARG_ENABLE([debug],
  [AS_HELP_STRING([--enable-debug],
    [Enable assertions and bounds-checking @<:@no@:>@])],
  [NODEBUG_CXXFLAGS="-fsanitize=address -fsanitize=undefined"],
  [NODEBUG_CXXFLAGS="-DNDEBUG"])
AC_SUBST(NODEBUG_CXXFLAGS)

# Checks for assemblers
# TODO only check this when arch is x86
AC_CHECK_PROGS([AS], [yasm nasm], [none])
AS_IF([test x$YASM == xnone], [
  AC_MSG_WARN(yasm or nasm required for assembly; ASM functions are disabled.)
  AC_MSG_WARN(Install one of these to use SSE2 in the decoder.)
], [
  ASFLAGS="-f elf64"

  AC_DEFINE([HAVE_SSE2], [1], [CPU supports SSE2])
  AC_DEFINE([ARCH_X86_64], [1], [64bit CPU])
])

AC_SUBST([AS])
AC_SUBST([ASFLAGS])

# Checks for libraries.
PKG_CHECK_MODULES([X11_XCB], [x11-xcb])
PKG_CHECK_MODULES([GLU], [glu])

# Checks for header files.
AC_PATH_X
AC_CHECK_HEADERS([fcntl.h unistd.h], [], [AC_MSG_ERROR([Missing required header file])])

# Checks for typedefs, structures, and compiler characteristics.
AC_CHECK_HEADER_STDBOOL
AC_C_INLINE
AC_TYPE_INT16_T
AC_TYPE_INT32_T
AC_TYPE_INT8_T
AC_TYPE_SIZE_T
AC_TYPE_UINT16_T
AC_TYPE_UINT32_T
AC_TYPE_UINT64_T
AC_TYPE_UINT8_T

# Checks for library functions.
AC_FUNC_MMAP
AC_CHECK_FUNCS([munmap strerror], [], [AC_MSG_ERROR([Missing required library function])])

AC_LANG_POP(C++)

AC_CONFIG_FILES([Makefile
                 src/Makefile
		 src/util/Makefile
                 src/decoder/Makefile
                 src/display/Makefile
                 src/frontend/Makefile
		 src/tests/Makefile
		 src/encoder/Makefile])

AC_OUTPUT
