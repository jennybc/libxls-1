AC_INIT([libxls],[1.5.0],[emmiller@gmail.com], [libxls], [https://github.com/libxls/libxls])
AC_CONFIG_SRCDIR([test/test.c])

LIBXLS_MAJOR_VERSION=1
LIBXLS_MINOR_VERSION=5
LIBXLS_MICRO_VERSION=0

VERSION=$LIBXLS_MAJOR_VERSION.$LIBXLS_MINOR_VERSION.$LIBXLS_MICRO_VERSION
VERSION_INFO=`expr $LIBXLS_MAJOR_VERSION + $LIBXLS_MINOR_VERSION`:$LIBXLS_MICRO_VERSION:$LIBXLS_MINOR_VERSION

AC_SUBST(VERSION_INFO)
AC_SUBST(LIBXLS_MAJOR_VERSION)
AC_SUBST(LIBXLS_MINOR_VERSION)
AC_SUBST(LIBXLS_MICRO_VERSION)

# Define those values into C code
AC_DEFINE_UNQUOTED([LIBXLS_MAJOR_VERSION],$LIBXLS_MAJOR_VERSION,[Major version])
AC_DEFINE_UNQUOTED([LIBXLS_MINOR_VERSION],$LIBXLS_MINOR_VERSION,[Minor version])
AC_DEFINE_UNQUOTED([LIBXLS_MICRO_VERSION],$LIBXLS_MICRO_VERSION,[Micro version])

AM_INIT_AUTOMAKE([foreign subdir-objects])
AM_SILENT_RULES([yes])

LT_INIT([win32-dll disable-static])

AC_CANONICAL_BUILD
AC_CANONICAL_HOST

AC_CONFIG_HEADERS([config.h])
AC_PREFIX_DEFAULT(/usr/local)

AC_PROG_INSTALL
AC_PROG_CC
AC_PROG_CC_C99
AC_PROG_CXX

AC_PATH_PROG(PKG_CONFIG, pkg-config, no)

AC_CHECK_FUNCS([strdup])
AC_CHECK_HEADERS([wchar.h])
AC_C_CONST
AC_FUNC_MALLOC
AC_FUNC_REALLOC
AC_TYPE_SIZE_T

AC_ARG_ENABLE([fuzz-testing], AS_HELP_STRING([--enable-fuzz-testing], ["Enable fuzz testing (requires Clang 6 or later)"]), [
   AC_MSG_CHECKING([whether $CC accepts -fsanitize=fuzzer])
   tmp_saved_flags=$[]_AC_LANG_PREFIX[]FLAGS
   _AC_LANG_PREFIX[]FLAGS="$[]_AC_LANG_PREFIX[]FLAGS -fsanitize=fuzzer"
   AC_LINK_IFELSE([AC_LANG_PROGRAM()],
       [
	    AC_MSG_RESULT(yes)
        fuzzer=yes],
	    AC_MSG_RESULT(no)
       AC_MSG_FAILURE([-fsanitize=fuzzer not supported (Required with --enable-fuzz-testing)]))
   _AC_LANG_PREFIX[]FLAGS=$tmp_saved_flags
], [fuzzer=no])
AM_CONDITIONAL([FUZZER_ENABLED], test "x$fuzzer" = "xyes")

AC_ARG_VAR([LIB_FUZZING_ENGINE], [Location of prebuilt fuzzing engine library])
AC_SUBST([LIB_FUZZING_ENGINE])

AS_CASE([$host],
        [*mingw*|*cygwin*], [EXTRA_LDFLAGS="-no-undefined"], [EXTRA_LDFLAGS=""])
AC_SUBST([EXTRA_LDFLAGS])

AM_ICONV

LIBXLS_LIBS="$LTLIBICONV $LIBS"

AC_SUBST(LIBXLS_LIBS)

AC_CONFIG_FILES([Makefile])
AC_OUTPUT

AC_MSG_NOTICE([---])
AC_MSG_NOTICE([catdoc users: avoid xls2csv conflicts, use ./configure --program-prefix=lib to get libxls2csv])
AC_MSG_NOTICE([---])

AC_MSG_RESULT([
Configuration:

C compiler: $CC
C++ compiler: $CXX
CFLAGS: $CFLAGS
CXXFLAGS: $CXXFLAGS
LIBS: $LIBXLS_LIBS

Host: $host
Extra ld flags: $EXTRA_LDFLAGS])
