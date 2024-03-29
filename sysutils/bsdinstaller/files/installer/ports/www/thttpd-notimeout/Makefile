# New ports collection makefile for: 	thttpd
# Date created:		24 May 1999
# Whom:			Anders Nordby <anders@fix.no>
#
# $FreeBSD$
#

PORTNAME=	thttpd
PORTVERSION= 	2.24
PKGNAMESUFFIX=	-notimeout
CATEGORIES=	www ipv6
MASTER_SITES=	http://www.acme.com/software/thttpd/ \
		http://atreides.freenix.no/~anders/
DISTFILES=	${DISTNAME}${EXTRACT_SUFX} notes.html
DIST_SUBDIR=	${PORTNAME}
EXTRACT_ONLY=	${DISTNAME}${EXTRACT_SUFX}

MAINTAINER=	anders@FreeBSD.org
COMMENT=	Tiny/turbo/throttling HTTP server

IGNOREFILES=	notes.html

GNU_CONFIGURE=	yes
USE_REINPLACE=	yes
NO_STAGE=	yes

MAN1=		makeweb.1 htpasswd.1
MAN8=		thttpd.8 redirect.8 ssi.8 syslogtocern.8

do-install:
	@${INSTALL_PROGRAM} ${WRKSRC}/thttpd ${PREFIX}/sbin/
	@${INSTALL_MAN} ${WRKSRC}/thttpd.8 ${PREFIX}/man/man8
	@${INSTALL} -d -m 0755 ${PREFIX}/www
	@${INSTALL} -d -m 0755 ${PREFIX}/www/data
	@${INSTALL} -d -m 0755 ${PREFIX}/www/logs
	@${INSTALL} -d -m 0755 ${PREFIX}/www/cgi-bin
	@${INSTALL_PROGRAM} ${WRKSRC}/cgi-src/redirect ${PREFIX}/www/cgi-bin/
	@${INSTALL_PROGRAM} ${WRKSRC}/cgi-src/ssi ${PREFIX}/www/cgi-bin/
	@${INSTALL_PROGRAM} ${WRKSRC}/cgi-src/phf ${PREFIX}/www/cgi-bin/
	@${INSTALL_MAN} ${WRKSRC}/cgi-src/redirect.8 ${PREFIX}/man/man8
	@${INSTALL_MAN} ${WRKSRC}/cgi-src/ssi.8 ${PREFIX}/man/man8
	@${INSTALL_PROGRAM} ${WRKSRC}/extras/makeweb ${PREFIX}/bin/
	@${INSTALL} -c -m 755 ${WRKSRC}/extras/syslogtocern ${PREFIX}/bin/
	@${INSTALL_PROGRAM} ${WRKSRC}/extras/htpasswd ${PREFIX}/bin/
	@${INSTALL_MAN} ${WRKSRC}/extras/makeweb.1 ${PREFIX}/man/man1
	@${INSTALL_MAN} ${WRKSRC}/extras/htpasswd.1 ${PREFIX}/man/man1
	@${INSTALL_MAN} ${WRKSRC}/extras/syslogtocern.8 ${PREFIX}/man/man8
	@if [ ! -f ${PREFIX}/sbin/thttpd_wrapper ]; then \
		${ECHO} "Installing ${PREFIX}/sbin/thttpd_wrapper file."; \
		${ECHO_CMD} "#!/bin/sh" > ${PREFIX}/sbin/thttpd_wrapper; \
		${ECHO_CMD} "${ECHO_CMD} \$$\$$ > /var/run/thttpd_wrapper.pid" >> ${PREFIX}/sbin/thttpd_wrapper; \
		${ECHO_CMD} "while true ; do" >> ${PREFIX}/sbin/thttpd_wrapper; \
		${ECHO_CMD} "	${PREFIX}/sbin/thttpd -D -C ${PREFIX}/etc/thttpd.conf" >> ${PREFIX}/sbin/thttpd_wrapper; \
		${ECHO_CMD} "	sleep 10" >> ${PREFIX}/sbin/thttpd_wrapper; \
		${ECHO_CMD} "done" >> ${PREFIX}/sbin/thttpd_wrapper; \
		${CHMOD} 751 ${PREFIX}/sbin/thttpd_wrapper; \
	fi
	@${ECHO} "Installing ${PREFIX}/etc/thttpd.conf.sample configuration file."
	@${ECHO_CMD} "dir=${PREFIX}/www/data" > ${PREFIX}/etc/thttpd.conf.sample
	@${ECHO_CMD} "chroot" >> ${PREFIX}/etc/thttpd.conf.sample
	@${ECHO_CMD} "cgipat=*.cgi" >> ${PREFIX}/etc/thttpd.conf.sample
	@${ECHO_CMD} "logfile=${PREFIX}/www/logs/thttpd.log" >> ${PREFIX}/etc/thttpd.conf.sample
	@${ECHO_CMD} "pidfile=/var/run/thttpd.pid" >> ${PREFIX}/etc/thttpd.conf.sample
	@${ECHO} "Installing ${PREFIX}/etc/rc.d/thttpd.sh sample startup file."
	@${INSTALL_SCRIPT} -m 751 ${FILESDIR}/thttpd.sh ${PREFIX}/etc/rc.d/thttpd.sh.sample
	@${REINPLACE_CMD} -i '' -e 's|%%PREFIX%%|${PREFIX}|g' ${PREFIX}/etc/rc.d/thttpd.sh.sample

.if !defined(NOPORTDOCS)
	@${MKDIR} ${PREFIX}/share/doc/thttpd
	@${INSTALL_DATA} ${DISTDIR}/${DIST_SUBDIR}/notes.html ${PREFIX}/share/doc/thttpd
.endif

post-install:
	@${CAT} ${PKGMESSAGE}

post-configure: config-message
	@cd ${WRKSRC} && patch < ${PATCHDIR}/extra-patch-config.h

post-build: config-message

config-message:
	@${ECHO} "====================================================================="
	@${ECHO} ""
	@${ECHO} "Be sure to take a look at the options in config.h before (re)building"
	@${ECHO} "if you want to tweak the server."
	@${ECHO} ""
	@${ECHO} "====================================================================="

.include <bsd.port.mk>
