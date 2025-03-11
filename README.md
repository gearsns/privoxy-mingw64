# privoxy-mingw64
privoxyをmingw64でビルド(+openssl対応)

Windows版のprivoxy何となくopenssl対応してなさそうな雰囲気なのでソースからビルドしてみる

## ビルド

1. MSYS2-64bitをインストール

2. mingw64起動

3. privoxy 4.0.0をダウンロード

	```
	git clone https://www.privoxy.org/git/privoxy.git
	```

4. privoxy.patchをダウンロード

	```
	https://github.com/gearsns/privoxy-mingw64/privoxy.patch
	```
5. patchの適用

	```
	patch -p0 < privoxy.patch
	```

	* configure.in : AC_CHECK_HEADERSでエラーが出るので最後の \ を削除
		Error:!!
		```
		./configure: line 6602: syntax error near unexpected token `as_ac_Header=`printf "%s\n" "ac_cv_header_$ac_header" | sed "$as_sed_sh"`'
		./configure: line 6602: `  as_ac_Header=`printf "%s\n" "ac_cv_header_$ac_header" | sed "$as_sed_sh"`'
		```
	* jbsockets.c : 型変換(cast)追加
	* openssl.c : applink.cが無いのでコメント
	* GNUmakefile.in : w32.resをpe-x86-64に変更

6. ビルド

	```
	autoheader && autoconf
	./configure --disable-toggle --disable-editor --disable-force --with-openssl --disable-pthread
	```