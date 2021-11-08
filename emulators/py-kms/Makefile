# Created by: Daniel Morante <daniel@morante.net>
# $FreeBSD$

PORTNAME=	kms
DISTVERSION=	g20200803
PORTREVISION=	1
CATEGORIES=	emulators
PKGNAMEPREFIX=	${PYTHON_PKGNAMEPREFIX}

MAINTAINER=	daniel@morante.net
COMMENT=	KMS Server Emulator written in Python

LICENSE=	UNLICENSE MIT
LICENSE_COMB=	multi
LICENSE_FILE_UNLICENSE=	${WRKSRC}/LICENSE
LICENSE_FILE_MIT=		${WRKSRC}/LICENSE.gui.md

RUN_DEPENDS=	${PYTHON_PKGNAMEPREFIX}argparse>0:devel/py-argparse@${PY_FLAVOR} \
			${PYTHON_PKGNAMEPREFIX}sqlite3>0:databases/py-sqlite3@${PY_FLAVOR} \
			${PYTHON_PKGNAMEPREFIX}tzlocal>=1.5:devel/py-tzlocal@${PY_FLAVOR}

USES=	python:3.8+ shebangfix

USE_GITHUB=	yes
GH_TAGNAME=	5b328d2
GH_ACCOUNT=	SystemRage
GH_PROJECT=	py-${PORTNAME}

OPTIONS_DEFINE=	X11
OPTIONS_SUB=	yes

X11_USES=	xorg tk
X11_USE=	xorg=x11
X11_RUN_DEPENDS=	${PYTHON_PKGNAMEPREFIX}tkinter>0:x11-toolkits/py-tkinter@${PY_FLAVOR}

PYKMS_SUBDIR=	py-${PORTNAME}
KMS_RUNDIR=	/var/run/${PORTNAME}
KMS_LOGDIR=	/var/log/${PORTNAME}
KMS_DBDIR=	/var/db/${PORTNAME}

SHEBANG_FILES=	${PYKMS_SUBDIR}/*.py

USE_RC_SUBR=	py-kms

USE_PYTHON=	flavors allflavors

SUB_LIST+=	PORTNAME=py-${PORTNAME} RCNAME=py_${PORTNAME} \
		PYTHON_CMD=${PYTHON_CMD} PYTHON_VER=${PYTHON_VER} \
		KMS_RUNDIR=${KMS_RUNDIR} KMS_LOGDIR=${KMS_LOGDIR} KMS_DBDIR=${KMS_DBDIR}

NO_ARCH=	yes
NO_BUILD=	yes

.include <bsd.port.options.mk>

.if ${PORT_OPTIONS:MX11}
DESKTOP_ENTRIES=	"Py-KMS GUI" "${COMMENT}" "${PREFIX}/share/pixmaps/py-kms.png" \
			"${PREFIX}/share/${PYKMS_SUBDIR}/pykms_Server.py" \
			"Network;"  ${FALSE}
.endif

post-patch:
	@${REINPLACE_CMD} -e "s,dbName = 'clients.db',dbName = '${KMS_DBDIR}/clients.db',g" \
		${WRKSRC}/${PYKMS_SUBDIR}/pykms_Sql.py
	@${REINPLACE_CMD} -e "s,os.path.abspath(__file__),'${KMS_RUNDIR}/',g" \
		${WRKSRC}/${PYKMS_SUBDIR}/pykms_Server.py

pre-install:
	${FIND} ${WRKSRC}/${PYKMS_SUBDIR} -type f \( -name \*.bak \) -delete

do-install:
	@${MKDIR} ${STAGEDIR}${KMS_RUNDIR} ${STAGEDIR}${KMS_LOGDIR} ${STAGEDIR}${KMS_DBDIR}
	(cd ${WRKSRC}/${PYKMS_SUBDIR} && \
		${PYTHON_CMD} ${PYTHON_LIBDIR}/compileall.py \
			-d ${WRKSRC}/${PYKMS_SUBDIR} \
			-f ${WRKSRC}/${PYKMS_SUBDIR} && \
		${PYTHON_CMD} -O ${PYTHON_LIBDIR}/compileall.py \
			-d ${WRKSRC}/${PYKMS_SUBDIR} \
			-f ${WRKSRC}/${PYKMS_SUBDIR})
	@${MKDIR} ${STAGEDIR}${PREFIX}/share/${PYKMS_SUBDIR}
	${INSTALL_SCRIPT} ${WRKSRC}/${PYKMS_SUBDIR}/pykms_Server.py ${STAGEDIR}${PREFIX}/share/${PYKMS_SUBDIR}
	(cd ${WRKSRC}/${PYKMS_SUBDIR} && \
		${COPYTREE_SHARE} . ${STAGEDIR}${PREFIX}/share/${PYKMS_SUBDIR} '! -name pykms_Server.py')
.if ${PORT_OPTIONS:MX11}
	${INSTALL_DATA}	${FILESDIR}/py-kms.png ${STAGEDIR}${PREFIX}/share/pixmaps/py-kms.png
.endif

.include <bsd.port.mk>
