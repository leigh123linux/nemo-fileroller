INCLUDES =						\
	-DFR_DATADIR=\"$(datadir)\"			\
	-DGNOMELOCALEDIR=\""$(datadir)/locale"\" 	\
	-I$(top_srcdir)					\
	-I$(top_builddir)				\
	$(DISABLE_DEPRECATED)				\
	$(NEMO_CFLAGS)				\
	$(FR_CFLAGS)

nemo_extensiondir=$(NEMO_EXTENSION_DIR)

nemo_extension_LTLIBRARIES=libnemo-fileroller.la

libnemo_fileroller_la_SOURCES =     \
	nemo-fileroller.c           \
	nemo-fileroller.h           \
	fileroller-module.c

libnemo_fileroller_la_LDFLAGS = -module -avoid-version -no-undefined
libnemo_fileroller_la_LIBADD  = $(NEMO_LIBS)

-include $(top_srcdir)/git.mk
