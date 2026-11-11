COMMENT =		CLI for managing Talos Linux clusters

MODGO_MODNAME =		github.com/siderolabs/talos
MODGO_VERSION =		v1.14.5

DISTNAME =		talos-${MODGO_VERSION}
PKGNAME =		talosctl-1.14.5

CATEGORIES =		sysutils

HOMEPAGE =		https://www.talos.dev/

MAINTAINER =		virtualroot <virtualroot@gmail.com>

# MPL 2.0
PERMIT_PACKAGE =	Yes

WANTLIB +=		c pthread

MODULES =		lang/go

ALL_TARGET =		${MODGO_MODNAME}/cmd/talosctl

# talos's go.mod unconditionally replaces its own pkg/machinery submodule
# with the local directory "./pkg/machinery" (they are developed in
# lockstep in a go.work workspace upstream). That directory does not exist
# in the plain module-proxy zip of the main module, so it must be fetched
# and put in place as a second distfile before the build can see it.
DISTFILES.machinery =	talos-machinery-1.14.5{v1.14.5}.zip
SITES.machinery =	https://proxy.golang.org/github.com/siderolabs/talos/pkg/machinery/@v/

.include "modules.inc"

post-extract:
	cd ${WRKSRC}/pkg && unzip -q ${FULLDISTDIR}/talos-machinery-1.14.5.zip
	mv ${WRKSRC}/pkg/${MODGO_MODNAME}/pkg/machinery@${MODGO_VERSION} \
		${WRKSRC}/pkg/machinery
	rm -rf ${WRKSRC}/pkg/github.com

# go.port.mk's default do-build unconditionally also builds ./cmd/... for
# any module that has a WRKSRC/cmd directory, regardless of ALL_TARGET.
# talos's cmd/ additionally contains cmd/installer (the Talos OS image
# installer), which is deeply Linux-specific (blockdevice partitioning,
# EFI vars, nftables, SELinux) and does not cross-compile for OpenBSD.
# Override do-build/do-install to build and install only cmd/talosctl.
do-build:
	cd ${WRKSRC} && ${MODGO_BUILD_CMD} ${ALL_TARGET}

do-install:
	${INSTALL_PROGRAM_DIR} ${PREFIX}/bin
	${INSTALL_PROGRAM} ${MODGO_WORKSPACE}/bin/talosctl ${PREFIX}/bin/

.include <bsd.port.mk>
