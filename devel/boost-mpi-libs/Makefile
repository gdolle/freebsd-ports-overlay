# Created by: Alexander Churanov <churanov.port.maintainer@gmail.com>
# $FreeBSD: head/devel/boost-mpi-libs/Makefile 371280 2014-10-20 16:04:12Z mva $

PORTNAME=	boost-mpi-libs
COMMENT=	Framework for interfacing Python and C++

OPTIONS_DEFINE=	VERBOSE_BUILD DEBUG OPTIMIZED_CFLAGS
VERBOSE_BUILD_DESC=	Show compiler messages

.include "${.CURDIR}/../boost-all/common.mk"

.include "${.CURDIR}/../boost-all/compiled.mk"

.if ${ARCH} == "sparc64"
CXXFLAGS=	-O0
.endif

LIB_DEPENDS+=	libboost_serialization.so:${PORTSDIR}/devel/boost-libs

BUILD_DEPENDS+=	${BJAM}:${PORTSDIR}/devel/boost-jam

do-build:
	@${ECHO_CMD} "using mpi : " ${MPI_VER} " : " ${MPI_CMD} " : " ${MPI_INCLUDEDIR} ";" >> ${WRKSRC}/tools/build/v2/site-config.jam
# build the library
	@cd ${WRKSRC} && \
		${BJAM} ${BJAM_ARGS} --with-mpi stage
# do not break compat yet
	@cd ${WRKSRC}/stage/lib && \
		( for l in lib*.so.*; do ${LN} -sf $${l} $${l%.${PORTVERSION}}.5; done )

do-install:
	@cd ${WRKSRC}/stage/lib && ${PAX} -rw -p p libboost* ${STAGEDIR}${PREFIX}/lib
# For some reasons BJAM forget about the links
	@cd ${WRKSRC}/stage/ && ${FIND} lib -type l | ${PAX} -rw -p p ${STAGEDIR}${PREFIX}

post-install:
# display pkg-message
	@${TOUCH} ${PKGMESSAGE}
	@${CAT} ${PKG_MESSAGE_FILE_THREADS} >> ${PKGMESSAGE}
	@${ECHO_CMD} >> ${PKGMESSAGE}

.include <bsd.port.post.mk>
