dnl Process this file with autoconf to produce a configure script.

AC_PREREQ(2.61)

AC_INIT(nemo-fileroller, 1.0.1, [http://bugzilla.gnome.org/enter_bug.cgi?product=file-roller])
AM_INIT_AUTOMAKE([1.11 foreign no-dist-gzip dist-xz tar-ustar])
m4_ifdef([AM_SILENT_RULES], [AM_SILENT_RULES([yes])])

AC_CONFIG_SRCDIR([config.h.in])
AC_CONFIG_HEADER([config.h])
AC_CONFIG_MACRO_DIR([m4])

GNOME_COMMON_INIT
AC_PROG_CC
AC_PROG_LIBTOOL
PKG_PROG_PKG_CONFIG
GLIB_GSETTINGS

AC_PATH_PROG(GLIB_GENMARSHAL, glib-genmarshal)
AC_PATH_PROG(GLIB_MKENUMS, glib-mkenums)

dnl ==========================================================================
dnl
dnl If you add a version number here, you *must* add an AC_SUBST line for
dnl it too, or it will never make it into the spec file!
dnl
dnl ==========================================================================

GLIB_REQUIRED=2.29.14
GIO_REQUIRED=2.25.5
GTK_REQUIRED=3.0.2
NEMO_REQUIRED=1.0.0

AC_SUBST(GLIB_REQUIRED)
AC_SUBST(GIO_REQUIRED)
AC_SUBST(GTK_REQUIRED)
AC_SUBST(NEMO_REQUIRED)

dnl ===========================================================================

PKG_CHECK_MODULES(GTK, [gtk+-3.0 >= $GTK_REQUIRED])
AC_SUBST([GTK_CFLAGS])
AC_SUBST([GTK_LIBS])

dnl ===========================================================================

AC_ARG_ENABLE(debug,
	      AS_HELP_STRING([--enable-debug], [enable compilation of debugging messages]),
	      [case "${enableval}" in
	      yes) ENABLE_DEBUG=yes ;;
	      no) ENABLE_DEBUG=no ;;
	      *) AC_MSG_ERROR([bad value ${enableval} for --enable-debug]) ;;
	      esac],
	      [ENABLE_DEBUG=no])
if test x$ENABLE_DEBUG = xyes; then
	AC_DEFINE(DEBUG, 1, [enable compilation of debugging messages])
	CFLAGS="$CFLAGS -g -O0 -DDEBUG"
fi

dnl ===========================================================================

AC_ARG_ENABLE([run_in_place],
	      AS_HELP_STRING([--enable-run-in-place],
			     [load ui data and extensions from source tree]),,
	      [enable_run_in_place=no])

AM_CONDITIONAL(RUN_IN_PLACE, test x"$enable_run_in_place" != x"no")

dnl ===========================================================================

PKG_CHECK_MODULES(FR,			\
	glib-2.0 >= $GLIB_REQUIRED	\
	gthread-2.0			\
	gio-unix-2.0			\
	gtk+-3.0 >= $GTK_REQUIRED)
AC_SUBST(FR_CFLAGS)
AC_SUBST(FR_LIBS)

dnl ******************************

AC_ARG_ENABLE([nemo_actions],
	      [AS_HELP_STRING([--disable-nemo-actions],
			      [do not build the nemo context menu actions])],,
	      [enable_nemo_actions=yes])

if test x"$enable_nemo_actions" != x"no" ; then
	PKG_CHECK_MODULES(NEMO,					\
			  libnemo-extension >= $NEMO_REQUIRED	\
			  glib-2.0					\
			  gio-2.0)
	AC_SUBST(NEMO_CFLAGS)
	AC_SUBST(NEMO_LIBS)
fi

AM_CONDITIONAL(ENABLE_NEMO_ACTIONS, test x"$enable_nemo_actions" != x"no")


NEMO_EXTENSION_DIR=`$PKG_CONFIG --variable=extensiondir libnemo-extension`
AC_SUBST(NEMO_EXTENSION_DIR)

dnl ******************************

dnl Checks for mkdtemp function

mkdtemp_missing=false
AC_CHECK_FUNC(mkdtemp,
    [AC_DEFINE([HAVE_MKDTEMP], 1, [Have GlibC function to make temp dirs])],
    mkdtemp_missing=true)
AM_CONDITIONAL(MKDTEMP_MISSING, test x$mkdtemp_missing = xtrue)

AC_CHECK_LIB(m, floor)

dnl ==========================================================================

AC_ARG_ENABLE(packagekit,
	      [AC_HELP_STRING([--disable-packagekit],[build without PackageKit support])],,
	      [enable_packagekit=yes])
if test "x$enable_packagekit" != "xno"; then
	AC_DEFINE(ENABLE_PACKAGEKIT, 1, [define to enable PackageKit installer])
fi

dnl ******************************

dnl ******************************

AC_ARG_ENABLE([magic],
              AS_HELP_STRING([--enable-magic], [use libmagic to detect file type]),,
              [enable_magic=no])

if test x"$enable_magic" = x"yes" ; then
	save_LIBS="$LIBS"
	LIBS="$LIBS -lmagic"
	AC_MSG_CHECKING([whether libmagic works])
	AC_LINK_IFELSE([AC_LANG_SOURCE([
#include <magic.h>
int main () { magic_t m = magic_open(MAGIC_NONE); }
])],
		       [AC_MSG_RESULT([yes])],
		       [AC_MSG_RESULT([no])
		        AC_MSG_ERROR([libmagic is needed for magic])])
	LIBS="$save_LIBS"

	MAGIC_CFLAGS=
	MAGIC_LIBS=-lmagic

	AC_SUBST(MAGIC_CFLAGS)
	AC_SUBST(MAGIC_LIBS)

	AC_DEFINE(ENABLE_MAGIC, 1, [define to enable magic])
fi

AM_CONDITIONAL(ENABLE_MAGIC, test x"$enable_magic" != x"no")

dnl ******************************

IT_PROG_INTLTOOL([0.40.0])

GETTEXT_PACKAGE=nemo-fileroller
AC_SUBST(GETTEXT_PACKAGE)
AC_DEFINE_UNQUOTED(GETTEXT_PACKAGE, "$GETTEXT_PACKAGE", [GetText Package])

if test "x${prefix}" = "xNONE"; then
  AC_DEFINE_UNQUOTED(LOCALEDIR, "${ac_default_prefix}/${DATADIRNAME}/locale", [Locale dir])
else
  AC_DEFINE_UNQUOTED(LOCALEDIR, "${prefix}/${DATADIRNAME}/locale", [Locale dir])
fi

dnl ******************************

GNOME_DOC_INIT

dnl ******************************

AC_ARG_ENABLE(deprecated,
              AS_HELP_STRING([--disable-deprecated],[warn about deprecated usages]),
              [disable_deprecated=yes],
              [disable_deprecated=no])
if test "x$disable_deprecated" = "xyes"; then
        DISABLE_DEPRECATED="-DGSEAL_ENABLE -DG_DISABLE_DEPRECATED -DGDK_DISABLE_DEPRECATED -DGDK_PIXBUF_DISABLE_DEPRECATED -DGTK_DISABLE_DEPRECATED"
fi
AC_SUBST(DISABLE_DEPRECATED)

dnl ******************************

AC_CONFIG_FILES([Makefile

		 nemo/Makefile])
AC_OUTPUT

echo "
Configuration:

	Source code location    : ${srcdir}
	Prefix                  : ${prefix}
	Compiler                : ${CC}
	CFLAGS                  : ${CFLAGS}
	Enable debug code       : ${ENABLE_DEBUG}
	Disable deprecated code : ${disable_deprecated}
	Run in place            : ${enable_run_in_place}
	Internal mkdtemp        : ${mkdtemp_missing}
	Nemo support        : ${enable_nemo_actions}
	PackageKit support      : ${enable_packagekit}
	Use libmagic            : ${enable_magic}
"
