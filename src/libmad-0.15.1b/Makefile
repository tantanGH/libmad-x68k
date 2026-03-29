# elf2x68k インストールパス
ifndef ELF2X68K_DIR
	ELF2X68K_DIR=/opt/homebrew/Cellar/elf2x68k/20260301
endif

# デフォルトサフィックスを削除
.SUFFIXES:

# 各種コマンド短縮名
CC = m68k-xelf-gcc
AS = m68k-xelf-as
AR = m68k-xelf-ar

# HAS to GAS コンバータ
HAS2GAS = python3 has2gas.py

# 出力アーカイブファイル名
TARGET_FILE_000 = ../../lib/libmad.a
TARGET_FILE_030 = ../../lib/libmad030.a
TARGET_FILE_060 = ../../lib/libmad060.a
TARGET_FILE_000HM = ../../lib/libmadhm.a
TARGET_FILE_030HM = ../../lib/libmad030hm.a
TARGET_FILE_060HM = ../../lib/libmad060hm.a

# ヘッダ検索パス
INCLUDE_FLAGS = -I${ELF2X68K_DIR}/lib/gcc/m68k-elf/13.4.0/include \
				-I${ELF2X68K_DIR}/m68k-elf/include \
				-I../libs/libhimem/include \
				-I../../include

# コンパイルフラグ
COMMON_FLAGS_000 = -m68000 -O3 -msoft-float \
					-fomit-frame-pointer \
				    -finline-functions -finline-limit=10000 \
				    -falign-functions=4 -falign-loops=4 \
				    -funroll-loops -fpeel-loops -fno-builtin \
				   $(INCLUDE_FLAGS)

CFLAGS_000 = -DFPM_DEFAULT -DOPT_SPEED -DOPT_SSO \
				-D__OPT_X68K_FAST_LAYER_DECODE__ \
				-D__OPT_X68K_16BIT_PCM_DIRECT__ \
				$(COMMON_FLAGS_000)

CFLAGS_000HM = -D_OPT_X68K_HIMEM $(CFLAGS_000)

COMMON_FLAGS_030 = -m68030 -O3 -msoft-float \
					-fomit-frame-pointer \
				    -finline-functions -finline-limit=10000 \
				    -falign-functions=16 -falign-loops=16 \
				    -fno-unroll-loops -fno-peel-loops -fno-builtin \
				   $(INCLUDE_FLAGS)

CFLAGS_030 = -DFPM_DEFAULT -DOPT_SPEED -DOPT_SSO \
				-D__OPT_X68K_FAST_LAYER_DECODE__ \
				-D__OPT_X68K_16BIT_PCM_DIRECT__ \
				$(COMMON_FLAGS_030)

CFLAGS_030HM = -D_OPT_X68K_HIMEM $(CFLAGS_030)

COMMON_FLAGS_060 = -m68060 -O3 -msoft-float -mstrict-align \
					-fomit-frame-pointer -fstrict-aliasing \
   					-fschedule-insns -fschedule-insns2 \
				    -finline-functions -finline-limit=5000 -fopt-info-inline \
				    -falign-functions=16 -falign-loops=16 \
    				-fgcse -fgcse-after-reload \
				    -fno-unroll-loops -fno-peel-loops -fno-builtin \
				   $(INCLUDE_FLAGS)

CFLAGS_060 = -DFPM_DEFAULT -DOPT_SPEED -DOPT_SSO \
				-D__OPT_X68K_FAST_LAYER_DECODE__ \
				-D__OPT_X68K_16BIT_PCM_DIRECT__ \
				$(COMMON_FLAGS_060)

CFLAGS_060HM = -D_OPT_X68K_HIMEM $(CFLAGS_060)

# *.c ソースファイル
C_SRCS = bit.c decoder.c fixed.c frame.c huffman.c layer3.c layer12.c stream.c synth.c timer.c version.c

# *.s ソースファイル
ASM_SRCS =

# *.h ヘッダーファイル
HEADER_SRCS = ../../include/mad.h bit.h decoder.h fixed.h frame.h huffman.h layer3.h layer12.h stream.h synth.h timer.h version.h

# リンク対象のライブラリファイル
LIBS =

# 中間ファイル生成用ディレクトリ
INTERMEDIATE_DIR_000 = _build000/elf2x68k
INTERMEDIATE_DIR_030 = _build030/elf2x68k
INTERMEDIATE_DIR_060 = _build060/elf2x68k
INTERMEDIATE_DIR_000HM = _build000hm/elf2x68k
INTERMEDIATE_DIR_030HM = _build030hm/elf2x68k
INTERMEDIATE_DIR_060HM = _build060hm/elf2x68k

# オブジェクトファイル
OBJS_000 = $(addprefix $(INTERMEDIATE_DIR_000)/,$(patsubst %.c,%.o,$(C_SRCS))) \
	       $(addprefix $(INTERMEDIATE_DIR_000)/,$(patsubst %.s,%.o,$(ASM_SRCS)))
OBJS_030 = $(addprefix $(INTERMEDIATE_DIR_030)/,$(patsubst %.c,%.o,$(C_SRCS))) \
	       $(addprefix $(INTERMEDIATE_DIR_030)/,$(patsubst %.s,%.o,$(ASM_SRCS)))
OBJS_060 = $(addprefix $(INTERMEDIATE_DIR_060)/,$(patsubst %.c,%.o,$(C_SRCS))) \
	       $(addprefix $(INTERMEDIATE_DIR_060)/,$(patsubst %.s,%.o,$(ASM_SRCS)))		   
OBJS_000HM = $(addprefix $(INTERMEDIATE_DIR_000HM)/,$(patsubst %.c,%.o,$(C_SRCS))) \
	         $(addprefix $(INTERMEDIATE_DIR_000HM)/,$(patsubst %.s,%.o,$(ASM_SRCS)))
OBJS_030HM = $(addprefix $(INTERMEDIATE_DIR_030HM)/,$(patsubst %.c,%.o,$(C_SRCS))) \
	         $(addprefix $(INTERMEDIATE_DIR_030HM)/,$(patsubst %.s,%.o,$(ASM_SRCS)))
OBJS_060HM = $(addprefix $(INTERMEDIATE_DIR_060HM)/,$(patsubst %.c,%.o,$(C_SRCS))) \
	         $(addprefix $(INTERMEDIATE_DIR_060HM)/,$(patsubst %.s,%.o,$(ASM_SRCS)))	

# デフォルトのターゲット
all : 000 030 060 000HM 030HM 060HM
000 : $(TARGET_FILE_000)
030 : $(TARGET_FILE_030)
060 : $(TARGET_FILE_060)
000HM : $(TARGET_FILE_000HM)
030HM : $(TARGET_FILE_030HM)
060HM : $(TARGET_FILE_060HM)

# 中間生成物の削除
clean : 
	rm -f $(TARGET_FILE_000)
	rm -f $(TARGET_FILE_030)
	rm -f $(TARGET_FILE_060)
	rm -f $(TARGET_FILE_000HM)
	rm -f $(TARGET_FILE_030HM)
	rm -f $(TARGET_FILE_060HM)
	rm -rf $(INTERMEDIATE_DIR_000)
	rm -rf $(INTERMEDIATE_DIR_030)
	rm -rf $(INTERMEDIATE_DIR_060)
	rm -rf $(INTERMEDIATE_DIR_000HM)
	rm -rf $(INTERMEDIATE_DIR_030HM)
	rm -rf $(INTERMEDIATE_DIR_060HM)

# アーカイブファイルの生成
$(TARGET_FILE_000) : $(OBJS_000)
	$(AR) -rs $(TARGET_FILE_000) $(OBJS_000)
$(TARGET_FILE_030) : $(OBJS_030)
	$(AR) -rs $(TARGET_FILE_030) $(OBJS_030)
$(TARGET_FILE_060) : $(OBJS_060)
	$(AR) -rs $(TARGET_FILE_060) $(OBJS_060)
$(TARGET_FILE_000HM) : $(OBJS_000HM)
	$(AR) -rs $(TARGET_FILE_000HM) $(OBJS_000HM)
$(TARGET_FILE_030HM) : $(OBJS_030HM)
	$(AR) -rs $(TARGET_FILE_030HM) $(OBJS_030HM)
$(TARGET_FILE_060HM) : $(OBJS_060HM)
	$(AR) -rs $(TARGET_FILE_060HM) $(OBJS_060HM)

# *.c ソースのコンパイル
$(INTERMEDIATE_DIR_000)/%.o : %.c Makefile $(HEADER_SRCS)
	mkdir -p $(INTERMEDIATE_DIR_000)
	$(CC) $(CFLAGS_000) -c $*.c -o $(INTERMEDIATE_DIR_000)/$*.o
$(INTERMEDIATE_DIR_030)/%.o : %.c Makefile $(HEADER_SRCS)
	mkdir -p $(INTERMEDIATE_DIR_030)
	$(CC) $(CFLAGS_030) -c $*.c -o $(INTERMEDIATE_DIR_030)/$*.o
$(INTERMEDIATE_DIR_060)/%.o : %.c Makefile $(HEADER_SRCS)
	mkdir -p $(INTERMEDIATE_DIR_060)
	$(CC) $(CFLAGS_060) -c $*.c -o $(INTERMEDIATE_DIR_060)/$*.o
$(INTERMEDIATE_DIR_000HM)/%.o : %.c Makefile $(HEADER_SRCS)
	mkdir -p $(INTERMEDIATE_DIR_000HM)
	$(CC) $(CFLAGS_000HM) -c $*.c -o $(INTERMEDIATE_DIR_000HM)/$*.o
$(INTERMEDIATE_DIR_030HM)/%.o : %.c Makefile $(HEADER_SRCS)
	mkdir -p $(INTERMEDIATE_DIR_030HM)
	$(CC) $(CFLAGS_030HM) -c $*.c -o $(INTERMEDIATE_DIR_030HM)/$*.o
$(INTERMEDIATE_DIR_060HM)/%.o : %.c Makefile $(HEADER_SRCS)
	mkdir -p $(INTERMEDIATE_DIR_060HM)
	$(CC) $(CFLAGS_060HM) -c $*.c -o $(INTERMEDIATE_DIR_060HM)/$*.o