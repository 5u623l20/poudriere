AC_INIT([poudriere],[3.4.99],[https://github.com/freebsd/poudriere],[poudriere])
AC_CONFIG_SRCDIR([configure.ac])
AC_CONFIG_MACRO_DIRS([m4])
AC_CONFIG_AUX_DIR([build-aux])
AM_INIT_AUTOMAKE([1.11 foreign -Wall -Wno-portability no-dist-gzip dist-xz])
AM_SILENT_RULES([yes])
AC_CONFIG_HEADER(src/config.h)

AM_MAINTAINER_MODE([disable])
AS_IF([test "$USE_MAINTAINER_MODE" != "no"],
      [ax_is_release=no],[ax_is_release=yes])

AC_PROG_CC([cc clang gcc])
AM_CFLAGS=
AC_SUBST([AM_CFLAGS])

LT_INIT

AC_CHECK_FUNCS([funlinkat strftime_l strchrnul utimensat])

AC_CHECK_HEADERS_ONCE([sys/types.h])
AC_CHECK_HEADERS_ONCE([sys/stat.h])
AC_CHECK_HEADERS_ONCE([sys/param.h])
AC_CHECK_HEADERS_ONCE([sys/procctl.h])
AC_CHECK_HEADERS_ONCE([sys/mman.h])
AC_CHECK_HEADERS_ONCE([stdlib.h])
AC_CHECK_HEADERS_ONCE([stddef.h])
AC_CHECK_HEADERS_ONCE([stdarg.h])
AC_CHECK_HEADERS_ONCE([stdbool.h])
AC_CHECK_HEADERS_ONCE([stdint.h])
AC_CHECK_HEADERS_ONCE([string.h])
AC_CHECK_HEADERS_ONCE([unistd.h])
AC_CHECK_HEADERS_ONCE([ctype.h])
AC_CHECK_HEADERS_ONCE([errno.h])
AC_CHECK_HEADERS_ONCE([limits.h])
AC_CHECK_HEADERS_ONCE([libgen.h])
AC_CHECK_HEADERS_ONCE([stdio.h])
AC_CHECK_HEADERS_ONCE([float.h])
AC_CHECK_HEADERS_ONCE([math.h])
AC_CHECK_HEADERS_ONCE([fcntl.h])

# Look for rm -x support.  Exit status 64 is bad argument, 1 is
# bad file.  So as long as it is not 64 it should be fine.
AC_CACHE_CHECK([whether rm -x works], [ac_cv_rm_x_works],
	       [ac_cv_rm_x_works=no
		rm -x /nonexistent 2>/dev/null
		test $? -eq 1 && ac_cv_rm_x_works=yes])
if test "$ac_cv_rm_x_works" = yes; then
  USE_RM=
else
  # Need bundled
  USE_RM=rm
fi
AC_SUBST([USE_RM])

AC_CHECK_DECLS([F_DUPFD_CLOEXEC], [ac_have_f_dupfd_cloexec=1],
    [ac_have_f_dupfd_cloexec=0], [[#include <fcntl.h>]])

SH=sh
if [[ $ac_have_f_dupfd_cloexec -eq 0 ]]; then
  AC_MSG_WARN([Not using bundled /bin/sh. Some runtime behavior may be wrong.])
  AC_MSG_WARN([It is advised to upgrade your base system to a newer release.])
  SH=
fi
AC_SUBST([SH])

AC_CONFIG_FILES([Makefile])
AC_CONFIG_FILES([test/Makefile])
AC_CONFIG_FILES([src/etc/rc.d/poudriered])

# Preserve timestamps for install. This is needed to preserve hardlinks
# for HTML files.
AC_SUBST([INSTALL], ["${ac_cv_path_install% -c} -p"])

AC_OUTPUT
