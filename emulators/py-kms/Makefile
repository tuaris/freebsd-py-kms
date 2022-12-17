# Created by: Daniel Morante <daniel@morante.net>

PORTNAME=	kms
DISTVERSION=	g20221207
CATEGORIES=	emulators
PKGNAMEPREFIX=	${PYTHON_PKGNAMEPREFIX}

MAINTAINER=	daniel@morante.net
COMMENT=	KMS Server Emulator written in Python
#WWW=	https://github.com/Py-KMS-Organization/py-kms

LICENSE=	UNLICENSE MIT
LICENSE_COMB=	multi
LICENSE_FILE_MIT=		${WRKSRC}/LICENSE.gui.md
LICENSE_FILE_UNLICENSE=	${WRKSRC}/LICENSE

RUN_DEPENDS=	${PYTHON_PKGNAMEPREFIX}argparse>0:devel/py-argparse@${PY_FLAVOR} \
			${PYTHON_PKGNAMEPREFIX}sqlite3>0:databases/py-sqlite3@${PY_FLAVOR} \
			${PYTHON_PKGNAMEPREFIX}tzlocal>=1.5:devel/py-tzlocal@${PY_FLAVOR}

USES=	python:3.8+ shebangfix

USE_GITHUB=	yes
GH_ACCOUNT=	Py-KMS-Organization
GH_PROJECT=	py-${PORTNAME}
GH_TAGNAME=	ee5be9a

USE_PYTHON=	flavors
USE_RC_SUBR=	py-kms

_KMS_DBDIR=		/var/db/${PORTNAME}
_KMS_LOGDIR=	/var/log/${PORTNAME}
_KMS_RUNDIR=	/var/run/${PORTNAME}
_KMS_SUBDIR=	py-${PORTNAME}

SHEBANG_FILES=	${_KMS_SUBDIR}/*.py

NO_ARCH=	yes
NO_BUILD=	yes

SUB_LIST+=	PORTNAME=py-${PORTNAME} RCNAME=py_${PORTNAME} \
		PYTHON_CMD=${PYTHON_CMD} PYTHON_VER=${PYTHON_VER} \
		KMS_RUNDIR=${_KMS_RUNDIR} KMS_LOGDIR=${_KMS_LOGDIR} KMS_DBDIR=${_KMS_DBDIR}

OPTIONS_DEFINE=	X11
OPTIONS_SUB=	yes

X11_RUN_DEPENDS=	${PYTHON_PKGNAMEPREFIX}tkinter>0:x11-toolkits/py-tkinter@${PY_FLAVOR}
X11_USES=	xorg tk
X11_USE=	xorg=x11

.include <bsd.port.options.mk>

.if ${PORT_OPTIONS:MX11}
DESKTOP_ENTRIES=	"Py-KMS GUI" "${COMMENT}" "${PREFIX}/share/pixmaps/py-kms.png" \
			"${PREFIX}/share/${_KMS_SUBDIR}/pykms_Server.py" \
			"Network;"  ${FALSE}	
.endif

pre-install:
	${FIND} ${WRKSRC}/${_KMS_SUBDIR} -type f \( -name \*.bak \) -delete

do-install:
	@${MKDIR} ${STAGEDIR}${_KMS_RUNDIR} ${STAGEDIR}${_KMS_LOGDIR} ${STAGEDIR}${_KMS_DBDIR}
	(cd ${WRKSRC}/${_KMS_SUBDIR} && \
		${PYTHON_CMD} ${PYTHON_LIBDIR}/compileall.py \
			-d ${WRKSRC}/${_KMS_SUBDIR} \
			-f ${WRKSRC}/${_KMS_SUBDIR} && \
		${PYTHON_CMD} -O ${PYTHON_LIBDIR}/compileall.py \
			-d ${WRKSRC}/${_KMS_SUBDIR} \
			-f ${WRKSRC}/${_KMS_SUBDIR})
	@${MKDIR} ${STAGEDIR}${PREFIX}/share/${_KMS_SUBDIR}
	${INSTALL_SCRIPT} ${WRKSRC}/${_KMS_SUBDIR}/pykms_Server.py ${STAGEDIR}${PREFIX}/share/${_KMS_SUBDIR}
	(cd ${WRKSRC}/${_KMS_SUBDIR} && \
		${COPYTREE_SHARE} . ${STAGEDIR}${PREFIX}/share/${_KMS_SUBDIR} '! -name pykms_Server.py')
.if ${PORT_OPTIONS:MX11}
	${INSTALL_DATA}	${FILESDIR}/py-kms.png ${STAGEDIR}${PREFIX}/share/pixmaps/py-kms.png
.endif

.include <bsd.port.mk>
