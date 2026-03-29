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

# ヘッダ検索パス
INCLUDE_FLAGS = -I${ELF2X68K_DIR}/lib/gcc/m68k-elf/13.4.0/include \
				-I${ELF2X68K_DIR}/m68k-elf/include \
				-I../../include

# コンパイルフラグ
COMMON_FLAGS_000 = -m68000 -O3 -msoft-float \
					-fomit-frame-pointer \
				    -finline-functions -finline-limit=10000 \
				    -falign-functions=4 -falign-loops=4 \
				    -funroll-loops -fpeel-loops -fno-builtin \
				   $(INCLUDE_FLAGS)

CFLAGS_000 = -DFPM_DEFAULT -DOPT_SPEED -DOPT_SSO \
				-D__OPT_X68K_FAST_FRAME_DECODE__ \
				-D__OPT_X68K_FAST_LAYER_DECODE__ \
				-D__OPT_X68K_16BIT_PCM_DIRECT__ \
				$(COMMON_FLAGS_000)

COMMON_FLAGS_030 = -m68030 -O3 -msoft-float \
					-fomit-frame-pointer \
				    -finline-functions -finline-limit=10000 \
				    -falign-functions=16 -falign-loops=16 \
				    -fno-unroll-loops -fno-peel-loops -fno-builtin \
				   $(INCLUDE_FLAGS)

CFLAGS_030 = -DFPM_DEFAULT -DOPT_SPEED -DOPT_SSO \
				-D__OPT_X68K_FAST_FRAME_DECODE__ \
				-D__OPT_X68K_FAST_LAYER_DECODE__ \
				-D__OPT_X68K_16BIT_PCM_DIRECT__ \
				$(COMMON_FLAGS_030)

COMMON_FLAGS_060 = -m68060 -O3 -msoft-float -mstrict-align \
					-fomit-frame-pointer -fstrict-aliasing \
   					-fschedule-insns -fschedule-insns2 \
				    -finline-functions -finline-limit=5000 -fopt-info-inline \
				    -falign-functions=16 -falign-loops=16 \
    				-fgcse -fgcse-after-reload \
				    -fno-unroll-loops -fno-peel-loops -fno-builtin \
				   $(INCLUDE_FLAGS)

CFLAGS_060 = -DFPM_DEFAULT -DOPT_SPEED -DOPT_SSO \
				-D__OPT_X68K_FAST_FRAME_DECODE__ \
				-D__OPT_X68K_FAST_LAYER_DECODE__ \
				-D__OPT_X68K_16BIT_PCM_DIRECT__ \
				$(COMMON_FLAGS_060)

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

# オブジェクトファイル
OBJS_000 = $(addprefix $(INTERMEDIATE_DIR_000)/,$(patsubst %.c,%.o,$(C_SRCS))) \
	       $(addprefix $(INTERMEDIATE_DIR_000)/,$(patsubst %.s,%.o,$(ASM_SRCS)))
OBJS_030 = $(addprefix $(INTERMEDIATE_DIR_030)/,$(patsubst %.c,%.o,$(C_SRCS))) \
	       $(addprefix $(INTERMEDIATE_DIR_030)/,$(patsubst %.s,%.o,$(ASM_SRCS)))
OBJS_060 = $(addprefix $(INTERMEDIATE_DIR_060)/,$(patsubst %.c,%.o,$(C_SRCS))) \
	       $(addprefix $(INTERMEDIATE_DIR_060)/,$(patsubst %.s,%.o,$(ASM_SRCS)))		   

# デフォルトのターゲット
all : 000 030 060
000 : $(TARGET_FILE_000)
030 : $(TARGET_FILE_030)
060 : $(TARGET_FILE_060)

# 中間生成物の削除
clean : 
	rm -f $(TARGET_FILE_000)
	rm -f $(TARGET_FILE_030)
	rm -f $(TARGET_FILE_060)
	rm -rf $(INTERMEDIATE_DIR_000)
	rm -rf $(INTERMEDIATE_DIR_030)
	rm -rf $(INTERMEDIATE_DIR_060)

# アーカイブファイルの生成
$(TARGET_FILE_000) : $(OBJS_000)
	$(AR) -rs $(TARGET_FILE_000) $(OBJS_000)
$(TARGET_FILE_030) : $(OBJS_030)
	$(AR) -rs $(TARGET_FILE_030) $(OBJS_030)
$(TARGET_FILE_060) : $(OBJS_060)
	$(AR) -rs $(TARGET_FILE_060) $(OBJS_060)

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

# *.s ソースのアセンブル
$(INTERMEDIATE_DIR_000)/%.o : %.s Makefile $(HEADER_SRCS)
	mkdir -p $(INTERMEDIATE_DIR_000)
	$(HAS2GAS) < $*.s > $(INTERMEDIATE_DIR_000)/$*.gas
	$(AS) -c $(INTERMEDIATE_DIR_000)/$*.gas -o $(INTERMEDIATE_DIR_000)/$*.o
$(INTERMEDIATE_DIR_030)/%.o : %.s Makefile $(HEADER_SRCS)
	mkdir -p $(INTERMEDIATE_DIR_030)
	$(HAS2GAS) < $*.s > $(INTERMEDIATE_DIR_030)/$*.gas
	$(AS) -c $(INTERMEDIATE_DIR_030)/$*.gas -o $(INTERMEDIATE_DIR_030)/$*.o
$(INTERMEDIATE_DIR_060)/%.o : %.s Makefile $(HEADER_SRCS)
	mkdir -p $(INTERMEDIATE_DIR_060)
	$(HAS2GAS) < $*.s > $(INTERMEDIATE_DIR_060)/$*.gas
	$(AS) -c $(INTERMEDIATE_DIR_060)/$*.gas -o $(INTERMEDIATE_DIR_060)/$*.o	