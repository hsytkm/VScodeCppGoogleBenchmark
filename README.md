# VScode Google Benchmark

2024年2月頃に整理した内容を2025年12月にコミット（コミット忘れ）

VSCode で [Google Benchmark](https://github.com/google/benchmark/) を実行します。

## 準備

Google Test を先に導入していたので必要なソフト（CMakeなど）が揃ってる状態想定でまとめます。

Google Test はこちら → [VScodeCppGoogleTestDemo](https://github.com/hsytkm/VScodeCppGoogleTestDemo)

1. 実行環境の導入手順
    公式のコマンドだと NMake でエラーが出たので以下で対応

    ```sh
    $ git clone https://github.com/google/benchmark.git

    $ cd benchmark
    $ mkdir build

    $ cmake -S . -B build -G "MinGW Makefiles" -DBENCHMARK_DOWNLOAD_DEPENDENCIES=ON -DCMAKE_BUILD_TYPE=Release

    $ cd build
    $ mingw32-make
    ```

    上コマンドでもエラーが出たが、目的の .aファイル名は生成できたのでOKとした。

2. ライブラリのコピー
    `benchmark\build\src\` 以下の2つのlibファイルをリネームして `mingw64\x86_64-w64-mingw32\lib` にコピー。

   ```sh
   copy build\src\libbenchmark.a [dest]
   copy build\src\libbenchmark_main.a [dest]
   ```

3. ヘッダーのコピー
    `benchmark\include\benchmark` をフォルダごと `mingw64\x86_64-w64-mingw32\include\` 以下にコピー。


## 実行結果

VScodeから `Ctrl+B` からの `F5` で結果が出るはず。

1. `Ctrl+B` のコマンド

    ```sh
    $ g++ -fdiagnostics-color=always .\src\*.cc -o ./test.exe -std=c++11 -DBENCHMARK_STATIC_DEFINE -LC:\\dev\\mingw64\\x86_64-w64-mingw32\\lib -lbenchmark -lpthread -lshlwapi
    ```

2. `F5` コマンド

    ```sh
    $ ./test.exe
    ```

    ```
    ------------------------------------------------------------
    Benchmark                  Time             CPU   Iterations
    ------------------------------------------------------------
    BM_StringCreation       19.4 ns         19.0 ns     34461538
    BM_StringCopy           34.4 ns         34.5 ns     20363636
    ```

