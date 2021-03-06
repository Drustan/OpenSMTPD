#	$OpenBSD$

.PATH:		${.CURDIR}/.. /usr/src/lib/libc/asr

# detect if asr is built in libc
TESTASR!=	nm `ls /usr/lib/libc.so.* | sort | tail -1` | grep "T asr_unpack_rr" | cut -d ' ' -f 3
.if ${TESTASR} != "asr_unpack_rr"
NEED_ASR=	Yes
.endif

PROG=		smtpd

SRCS=		aliases.c bounce.c ca.c compress_backend.c config.c		\
		control.c crypto.c csprng.c delivery.c dict.c dns.c envelope.c	\
		expand.c forward.c iobuf.c ioev.c limit.c lka.c	lka_session.c	\
		log.c mda.c mfa.c mfa_session.c mproc.c				\
		mta.c mta_session.c parse.y queue.c queue_backend.c		\
		ruleset.c runq.c scheduler.c scheduler_backend.c		\
		smtp.c smtp_session.c smtpd.c ssl.c ssl_privsep.c		\
		ssl_smtpd.c stat_backend.c table.c to.c tree.c util.c		\
		waitq.c

# backends
SRCS+=		compress_gzip.c
SRCS+=		delivery_filename.c
SRCS+=		delivery_maildir.c
SRCS+=		delivery_mbox.c
SRCS+=		delivery_mda.c
SRCS+=		delivery_lmtp.c
SRCS+=		table_db.c
SRCS+=		table_getpwnam.c
SRCS+=		table_proc.c
SRCS+=		table_static.c
SRCS+=		queue_fs.c
SRCS+=		queue_null.c
SRCS+=		queue_proc.c
SRCS+=		queue_ram.c
SRCS+=		scheduler_ramqueue.c
SRCS+=		scheduler_null.c
SRCS+=		scheduler_proc.c
SRCS+=		stat_ramstat.c

.ifdef NEED_ASR
SRCS+=		asr.c asr_debug.c asr_utils.c gethostnamadr_async.c	\
		res_send_async.c getaddrinfo_async.c getnameinfo_async.c \
		res_search_async.c
.endif

MAN=		smtpd.8 smtpd.conf.5 table.5
BINDIR=		/usr/sbin

LDADD+=		-levent -lutil -lssl -lcrypto -lm -lz
DPADD+=		${LIBEVENT} ${LIBUTIL} ${LIBSSL} ${LIBCRYPTO} ${LIBM} ${LIBZ}
CFLAGS+=	-g3 -ggdb -I${.CURDIR}/.. -I/usr/src/lib/libc/asr
CFLAGS+=	-Wall -Wstrict-prototypes -Wmissing-prototypes
CFLAGS+=	-Wmissing-declarations
CFLAGS+=	-Wshadow -Wpointer-arith -Wcast-qual
CFLAGS+=	-Wsign-compare -Wbounded
#CFLAGS+=	-Werror # during development phase (breaks some archs)
CFLAGS+=	-DIO_SSL
CFLAGS+=	-DQUEUE_PROFILING
.ifdef NEED_ASR
CFLAGS+=	-DASR_OPT_THREADSAFE=0
.endif
YFLAGS=

.include <bsd.prog.mk>
