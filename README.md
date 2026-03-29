# libmad-x68k

libmad MP3 decode library for X68k

軽量MP3 decoderライブラリのlibmadをX68k向けにelf2x68kでビルドしたものになります。
最終的にまーきゅりーゆにっとで鳴らすことを前提に、24(28)bitPCMデータではなく、16bitステレオインターリーブのデータを直接出力するようになっています。

68000汎用/68030以上用/68060専用があります。
また、それぞれのハイメモリ対応版もあります。


使う時は、サブモジュールとして組み込むのが簡単です。例えばプロジェクト直下にて以下を実行します。

```
git submodule add https://github.com/tantanGH/libmad-x68k.git libs/libmad-x68k
```

以下のようなツリーとなります。

```
my_app/
├── .git/
├── .gitmodules
├── libs/
│   └── libmad-x68k/
│       ├── include/mad.h
│       └── lib/libmad.a
└── src/
    ├── main.c
    └── Makefile
```

ヘッダー検索パスとライブラリ検索パスをMakefile内で
```
-I../libs/libmad-x68k/include
-L../libs/libmad-x68k/lib
```
のように指定し、`-lmad` でリンクできます。
68030以上用、68060専用はそれぞれ`-lmad030`,`-lmad060`としてください。

ハイメモリ対応版は以下の`libhimem`を組み込んだ上で、

https://github.com/tantanGH/libhimem

それぞれ、`-lhimem -lmadhm`,`-lhimem -lmad030hm`,`-lhimem -lmad060hm`としてください。

---

 libmad - MPEG audio decoder library
 Copyright (C) 2000-2004 Underbit Technologies, Inc.

 This program is free software; you can redistribute it and/or modify
 it under the terms of the GNU General Public License as published by
 the Free Software Foundation; either version 2 of the License, or
 (at your option) any later version.

 This program is distributed in the hope that it will be useful,
 but WITHOUT ANY WARRANTY; without even the implied warranty of
 MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
 GNU General Public License for more details.

 You should have received a copy of the GNU General Public License
 along with this program; if not, write to the Free Software
 Foundation, Inc., 59 Temple Place, Suite 330, Boston, MA  02111-1307  USA

 If you would like to negotiate alternate licensing terms, you may do
 so by contacting: Underbit Technologies, Inc. <info@underbit.com>

