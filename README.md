# VScode Google Benchmark

VSCode で Google Benchmark を実行します。

Google Test はこちら → [VScodeCppGoogleTestDemo](https://github.com/hsytkm/VScodeCppGoogleTestDemo)



1.8.3



### 準備

Google Test を先に導入していたので必要なソフト（CMakeなど）が揃ってる状態から始まっています。



1. `benchmark-1.8.3\include\benchmark` フォルダを `mingw64\x86_64-w64-mingw32\include` に丸ごとコピー

2. `benchmark-1.8.3\build\src\Release` 以下の2つのlibファイルをリネームして `mingw64\x86_64-w64-mingw32\lib` にコピーしてみた。

   ```
   benchmark.lib -> libbenchmark.a
   benchmark_main.lib -> libbenchmark_main.a
   ```

   





```shell
$ g++ mybenchmark.cc -std=c++11 -isystem benchmark/include \
  -Lbenchmark/build/src -lbenchmark -lpthread -o mybenchmark
```



























# ごみ

[Google Test を使ってみる（その１：準備編） | 豆蔵デベロッパーサイト](https://developer.mamezou-tech.com/blogs/2022/11/04/google-test-01/)

[Google Test を使ってみる（その４：VSCode拡張機能編） | 豆蔵デベロッパーサイト](https://developer.mamezou-tech.com/blogs/2022/11/20/google-test-04/)

1. MinGW-w64 の入手

   [Releases · niXman/mingw-builds-binaries](https://github.com/niXman/mingw-builds-binaries/releases)

   `x86_64-13.2.0-release-posix-seh-msvcrt-rt_v11-rev1.7z`

2. CMake の入手

   [Download CMake](https://cmake.org/download/)

   `cmake-3.29.0-rc4-windows-x86_64.zip`

3. GoogleTest のビルド

   [Release v1.14.0 · google/googletest](https://github.com/google/googletest/releases/tag/v1.14.0)

4. GoogleTest の生成物をコピー

   lib / header ともに、ウェブページの指示通り `C:\dev\mingw64\` 以下でなく `C:\dev\mingw64\x86_64-w64-mingw32\` 以下にコピーします。（多分大事なはず）

5. VSCode の拡張機能 `GoogleTest Adapter` をインストールします。



### テストの実行

```sh
$ g++ add.c add_test.cpp -o test -g -pthread -lgtest_main -lgtest
$ ./test.exe
```

- `-g`: デバッグ情報を含んだ実行ファイルを生成するオプションです。デバッグ情報を含むことで、デバッグ時に変数名や行番号などの情報が使えるようになります。
- `-pthread` : マルチスレッドプログラミングに使用される pthread ライブラリをリンクします
- `-lgtest_main` : Google Test のメイン関数を提供するライブラリをリンクします。
- `-lgtest` : Google Test の本体のライブラリをリンクします。



### 参考元

[google/googletest: GoogleTest - Google Testing and Mocking Framework](https://github.com/google/googletest?tab=readme-ov-file)  GitHub Page

[GoogleTest User’s Guide | GoogleTest](https://google.github.io/googletest/)  GitHub.io

[Google Test — Google Test ドキュメント日本語訳](http://opencv.jp/googletestdocs/index.html)  日本語の解説ページ  v1.6.0(2011年)ベース



