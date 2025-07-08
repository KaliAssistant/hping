# -*- Autoconf -*-
AC_INIT([hping3], [3.0.0], [antirez@invece.org])
AM_INIT_AUTOMAKE([-Wall -Werror foreign])
AC_CONFIG_HEADERS([config.h])
AC_CONFIG_FILES([Makefile])
AC_CONFIG_MACRO_DIRS([m4])


# Tools
AC_PROG_CC
AC_PROG_INSTALL
AC_PROG_AR
LT_INIT

# Dependencies
AC_CHECK_LIB([pcap], [pcap_open_live], [], [AC_MSG_ERROR([libpcap not found])])

# Optional: Tcl scripting support
AC_ARG_ENABLE([tcl],
  [AS_HELP_STRING([--disable-tcl], [Disable Tcl scripting support])],
  [],
  [enable_tcl=yes])


AS_IF([test "x$enable_tcl" = "xyes"], [
  AC_MSG_NOTICE([Checking for Tcl (8.0–8.6)...])

  have_tcl_lib=no
  have_tcl_hdr=no
  TCL_LIBS=""
  TCL_VERSION_DETECTED=""
  USE_TCL_CPPFLAGS=""

  for ver in 8.6 8.5 8.4 8.3 8.2 8.1 8.0; do
    AC_MSG_CHECKING([for Tcl version $ver])
    AC_CHECK_LIB([tcl$ver], [Tcl_Init], [
      TCL_LIBS="-ltcl$ver -lm -lpthread"
      TCL_VERSION_DETECTED="$ver"
      have_tcl_lib=yes
      AC_MSG_RESULT([yes])
      break
    ], [
      AC_MSG_RESULT([no])
    ])
  done

  AC_CHECK_HEADER([tcl.h], [have_tcl_hdr=yes], [
    AC_MSG_WARN([tcl.h not found; Tcl scripting disabled])
  ])

  AS_IF([test "x$have_tcl_lib" = "xyes" -a "x$have_tcl_hdr" = "xyes"], [
    AC_DEFINE([USE_TCL], [1], [Define if using Tcl scripting])
    USE_TCL_CPPFLAGS="-DUSE_TCL"
  ], [
    AC_MSG_WARN([Tcl scripting support disabled])
    enable_tcl=no
    TCL_LIBS=""
    USE_TCL_CPPFLAGS=""
    TCL_VERSION_DETECTED=""
  ])
])

AC_SUBST([TCL_LIBS])
AC_SUBST([USE_TCL_CPPFLAGS])
AC_SUBST([TCL_VERSION_DETECTED])






# Endianness
# Endianness
AC_C_BIGENDIAN(
  [AC_DEFINE([__BIG_ENDIAN_BITFIELD], [1], [Define if target is big endian])],
  [AC_DEFINE([__LITTLE_ENDIAN_BITFIELD], [1], [Define if target is little endian])]
)

# OS type macros
case "$host_os" in
  linux*)   AC_DEFINE([OSTYPE_LINUX], [1], [Define if OS is Linux]) ;;
  solaris*) AC_DEFINE([OSTYPE_SUNOS], [1], [Define if OS is Solaris])
            SOLARISLIB="-lsocket -lresolv -lnsl"
            AC_DEFINE([STUPID_SOLARIS_CHECKSUM_BUG], [1], [Define if workaround needed for Solaris checksum bug]) ;;
  freebsd*) AC_DEFINE([OSTYPE_FREEBSD], [1], [Define if OS is FreeBSD]) ;;
  openbsd*) AC_DEFINE([OSTYPE_OPENBSD], [1], [Define if OS is OpenBSD]) ;;
  netbsd*)  AC_DEFINE([OSTYPE_NETBSD], [1], [Define if OS is NetBSD]) ;;
  darwin*)  AC_DEFINE([OSTYPE_DARWIN], [1], [Define if OS is Darwin/macOS]) ;;
  cygwin*)  AC_DEFINE([OSTYPE_CYGWIN], [1], [Define if OS is Cygwin]) ;;
  *)        AC_DEFINE([OSTYPE_UNKNOWN], [1], [Define if OS is unknown]) ;;
esac
AC_SUBST([SOLARISLIB])

# Output summary
AC_MSG_NOTICE([])
AC_MSG_NOTICE([------ Configuration Summary ------])
AC_MSG_NOTICE([OS             : ${host_os}])
AC_MSG_NOTICE([libpcap        : yes])
AC_MSG_NOTICE([Tcl support    : ${enable_tcl}])
AC_MSG_NOTICE([Tcl version    : ${TCL_VERSION_DETECTED}])
AC_MSG_NOTICE([Static build   : ${enable_static:-no}])
AC_MSG_NOTICE([Cross-compiler : ${host}])
AC_MSG_NOTICE([------------------------------------])
AC_MSG_NOTICE([Run 'make' to build hping3])
AC_MSG_NOTICE([])
AC_OUTPUT

