# Created by: Guillaume Dolle <dolle.guillaume@gmail.com>
# $FreeBSD: head/devel/boost-mpi-libs/Makefile 371280 2014-10-20 16:04:12Z mva $

PORTNAME=	boost-mpi-libs
COMMENT=	Framework for interfacing MPI and C++
PKGNAMEPREFIX= ${MPI_PKGNAMEPREFIX}

#USES=		mpi

OPTIONS_DEFINE=	VERBOSE_BUILD DEBUG OPTIMIZED_CFLAGS
VERBOSE_BUILD_DESC=	Show compiler messages

.include "${.CURDIR}/../boost-all/common.mk"

.include "${.CURDIR}/../boost-all/compiled.mk"

MAKE_ENV+=	MPI_ROOT="${LOCALBASE}"\
		MPI_VERSION="${MPI_VERSION}"\
		MPI_INCLUDES="${MPI_INCLUDEDIR}"\
		MPI_LIB_PATH="${MPI_LIBDIR}"

LIB_DEPENDS+=	libboost_serialization.so:devel/boost-libs
MAKE_ARGS+= --with-mpi
ALL_TARGET= stage

BUILD_DEPENDS+=	bjam:devel/boost-jam

pre-build:
	@${ECHO_CMD} "using mpi : " ${MPI_VER} " : " ${MPI_CMD} " : " ${MPI_INCLUDEDIR} ";" >> ${WRKSRC}/tools/build/src/site-config.jam

do-install:
	@cd ${WRKSRC}/stage/lib && ${PAX} -rw -p p libboost* ${STAGEDIR}${PREFIX}/lib
# For some reasons BJAM forget about the links
	@cd ${WRKSRC}/stage/ && ${FIND} lib -type l | ${PAX} -rw -p p ${STAGEDIR}${PREFIX}

post-install:
# display pkg-message
	@${TOUCH} ${PKGMESSAGE}
	@${CAT} ${PKG_MESSAGE_FILE_THREADS} >> ${PKGMESSAGE}
	@${ECHO_CMD} >> ${PKGMESSAGE}
#	@${CAT} ${PKG_MESSAGE_FILE_MPI} >> ${PKGMESSAGE}
#	@${ECHO_CMD} >> ${PKGMESSAGE}
#	@${REINPLACE_CMD} \
#		-e "s|%%LOCALBASE%%|${LOCALBASE}|g" \
#		-e "s|%%MPI_INCLUDEDIR%%|${MPI_INCLUDEDIR}|g" \
#		-e "s|%%MPI_LIBDIR%%|${MPI_LIBDIR}|g" \
#		-e "s|%%MPI_VERSION%%|${MPI_VERSION}|g" ${PKGMESSAGE}
#	@${STRIP_CMD} ${STAGEDIR}${PREFIX}/lib/libboost_python*.so.${PORTVERSION}

.include <bsd.port.mk>
