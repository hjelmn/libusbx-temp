all: libusb-1.0.la libusb-1.0.dll

lib_LTLIBRARIES = libusb-1.0.la

LINUX_USBFS_SRC = os/linux_usbfs.c
DARWIN_USB_SRC = os/darwin_usb.c
OPENBSD_USB_SRC = os/openbsd_usb.c
WINDOWS_USB_SRC = os/poll_windows.c os/windows_usb.c libusb-1.0.rc libusb-1.0.def
WINCE_USB_SRC = os/wince_usb.c os/wince_usb.h

EXTRA_DIST = $(LINUX_USBFS_SRC) $(DARWIN_USB_SRC) $(OPENBSD_USB_SRC) \
	$(WINDOWS_USB_SRC) $(WINCE_USB_SRC) \
	os/threads_posix.c os/threads_windows.c \
	os/linux_udev.c os/linux_netlink.c

if OS_LINUX

if USE_UDEV
OS_SRC = $(LINUX_USBFS_SRC) os/linux_udev.c
else
OS_SRC = $(LINUX_USBFS_SRC) os/linux_netlink.c
endif

endif

if OS_DARWIN
OS_SRC = $(DARWIN_USB_SRC)
AM_CFLAGS_EXT = -no-cpp-precomp
endif

if OS_OPENBSD
OS_SRC = $(OPENBSD_USB_SRC)
endif

if OS_WINDOWS
OS_SRC = $(WINDOWS_USB_SRC)

.rc.lo:
	$(AM_V_GEN)$(LIBTOOL) $(AM_V_lt) $(AM_LIBTOOLFLAGS) $(LIBTOOLFLAGS) --tag=RC --mode=compile $(RC) $(RCFLAGS) -i $< -o $@

libusb-1.0.rc: version.h version_nano.h
endif

libusb-1.0.dll: libusb-1.0.def
if CREATE_IMPORT_LIB
# Rebuild the import lib from the .def so that MS and MinGW DLLs can be interchanged
	$(AM_V_GEN)$(DLLTOOL) $(DLLTOOLFLAGS) --kill-at --input-def $(srcdir)/libusb-1.0.def --dllname $@ --output-lib .libs/$@.a
endif

if THREADS_POSIX
THREADS_SRC = os/threads_posix.h os/threads_posix.c
else
THREADS_SRC = os/threads_windows.h os/threads_windows.c
endif

libusb_1_0_la_CFLAGS = $(AM_CFLAGS)
libusb_1_0_la_LDFLAGS = $(LTLDFLAGS)
libusb_1_0_la_SOURCES = libusbi.h core.c descriptor.c io.c sync.c $(OS_SRC) \
	os/linux_usbfs.h os/darwin_usb.h os/windows_usb.h os/windows_common.h \
	hotplug.h hotplug.c $(THREADS_SRC) os/poll_posix.h os/poll_windows.h

hdrdir = $(includedir)/libusb-1.0
hdr_HEADERS = libusb.h
