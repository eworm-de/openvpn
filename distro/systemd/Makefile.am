#
#  OpenVPN -- An application to securely tunnel IP networks
#             over a single UDP port, with support for SSL/TLS-based
#             session authentication and key exchange,
#             packet encryption, packet authentication, and
#             packet compression.
#
#  Copyright (C) 2017-2018 OpenVPN Inc <sales@openvpn.net>
#

%.service: %.service.in Makefile
	$(AM_V_GEN)sed -e 's|\@sbindir\@|$(sbindir)|' \
		-e 's|\@SYSTEMD_USER\@|$(SYSTEMD_USER)|' \
		-e 's|\@SYSTEMD_CAPS_OPTION\@|$(SYSTEMD_CAPS_OPTION)|' \
		-e 's|\@SYSTEMD_CAPS_VALUES\@|$(SYSTEMD_CAPS_VALUES)|' \
		$< > $@.tmp && mv $@.tmp $@

%.conf: %.conf.in Makefile
	$(AM_V_GEN)sed -e 's|\@SYSTEMD_USER\@|$(SYSTEMD_USER)|g' \
		$< > $@.tmp && mv $@.tmp $@

EXTRA_DIST = \
	sysusers-openvpn.conf \
	tmpfiles-openvpn.conf.in \
	openvpn-client@.service.in \
	openvpn-server@.service.in

if ENABLE_SYSTEMD
if ENABLE_IPROUTE
SYSTEMD_USER=root
SYSTEMD_CAPS_OPTION=CapabilityBoundingSet
SYSTEMD_CAPS_VALUES=CAP_IPC_LOCK CAP_NET_ADMIN CAP_NET_BIND_SERVICE CAP_NET_RAW CAP_SETGID CAP_SETUID CAP_SYS_CHROOT CAP_DAC_OVERRIDE
else
SYSTEMD_USER=openvpn
SYSTEMD_CAPS_OPTION=AmbientCapabilities
SYSTEMD_CAPS_VALUES=CAP_IPC_LOCK CAP_NET_ADMIN CAP_NET_BIND_SERVICE CAP_NET_RAW CAP_SYS_CHROOT CAP_DAC_OVERRIDE

sysusers_DATA = \
	sysusers-openvpn.conf
endif

systemdunit_DATA = \
	openvpn-client@.service \
	openvpn-server@.service
tmpfiles_DATA = \
	tmpfiles-openvpn.conf
dist_doc_DATA = \
	README.systemd

install-data-hook:
	mv $(DESTDIR)$(tmpfilesdir)/tmpfiles-openvpn.conf $(DESTDIR)$(tmpfilesdir)/openvpn.conf
	mv $(DESTDIR)$(sysusersdir)/sysusers-openvpn.conf $(DESTDIR)$(sysusersdir)/openvpn.conf || true
endif

MAINTAINERCLEANFILES = \
	$(srcdir)/Makefile.in
