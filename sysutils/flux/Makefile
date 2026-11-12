COMMENT =		GitOps toolkit for Kubernetes

MODGO_MODNAME =		github.com/fluxcd/flux2/v2
MODGO_VERSION =		v2.10.0

DISTNAME =		flux2-${MODGO_VERSION}
PKGNAME =		flux-2.10.0

CATEGORIES =		sysutils

HOMEPAGE =		https://fluxcd.io/

MAINTAINER =		virtualroot <virtualroot@gmail.com>

# Apache 2.0
PERMIT_PACKAGE =	Yes

WANTLIB +=		c pthread

MODULES =		lang/go

ALL_TARGET =		${MODGO_MODNAME}/cmd/flux

# the patch phase cd's into WRKDIST, which defaults to a directory named
# after DISTNAME -- but for MODGO_MODNAME ports the proxy zip's actual
# top-level directory is named ${MODGO_MODNAME}@${MODGO_VERSION} (= WRKSRC).
# Without this, "make patch" fails with "can't cd to .../flux2-v2.10.0".
WRKDIST =		${WRKSRC}

MODGO_LDFLAGS +=	-X main.VERSION=2.10.0

# cmd/flux embeds its Kubernetes manifests from cmd/flux/manifests/*.yaml,
# a directory that upstream gitignores and regenerates at release time
# with kustomize, pulling remote fluxcd/*-controller release URLs -- both
# unavailable in the ports build sandbox. Upstream instead publishes a
# ready-built, flat manifests.tar.gz release asset; fetch it as a second
# distfile and extract it into place ourselves.
DISTFILES.manifests =	flux-manifests-2.10.0{manifests}.tar.gz
SITES.manifests =	https://github.com/fluxcd/flux2/releases/download/v2.10.0/

.include "modules.inc"

post-extract:
	mkdir -p ${WRKSRC}/cmd/flux/manifests
	tar -C ${WRKSRC}/cmd/flux/manifests -xzf ${FULLDISTDIR}/flux-manifests-2.10.0.tar.gz

.include <bsd.port.mk>
