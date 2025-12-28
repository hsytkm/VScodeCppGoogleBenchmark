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

#### test.exe の作成（`Ctrl+B`）

    ```sh
    $ g++ -fdiagnostics-color=always .\src\*.cc -o ./test.exe -std=c++11 -DBENCHMARK_STATIC_DEFINE -LC:\\dev\\mingw64\\x86_64-w64-mingw32\\lib -lbenchmark -lpthread -lshlwapi
    ```

#### test.exe の実行（`F5`）

1. シンプル実行

    ```sh
    $ ./test.exe

    2025-12-28T17:14:19+09:00
    Running D:\dev\mine\VScodeCppGoogleBenchmark\test.exe
    Run on (16 X 3094 MHz CPU s)
    CPU Caches:
    L1 Data 32 KiB (x8)
    L1 Instruction 32 KiB (x8)
    L2 Unified 512 KiB (x8)
    L3 Unified 4096 KiB (x2)
    ------------------------------------------------------------
    Benchmark                  Time             CPU   Iterations
    ------------------------------------------------------------
    BM_StringCreation       18.6 ns         18.8 ns     37333333
    BM_StringCopy           32.7 ns         33.0 ns     21333333
    ```

2. 標準シンプル実行

    ```sh
    $ ./test.exe --benchmark_repetitions=10 --benchmark_report_aggregates_only=true

    2025-12-28T17:16:54+09:00
    Running D:\dev\mine\VScodeCppGoogleBenchmark\test.exe
    Run on (16 X 3094 MHz CPU s)
    CPU Caches:
    L1 Data 32 KiB (x8)
    L1 Instruction 32 KiB (x8)
    L2 Unified 512 KiB (x8)
    L3 Unified 4096 KiB (x2)
    -------------------------------------------------------------------
    Benchmark                         Time             CPU   Iterations
    -------------------------------------------------------------------
    BM_StringCreation_mean         18.7 ns         18.8 ns           10
    BM_StringCreation_median       18.7 ns         18.8 ns           10
    BM_StringCreation_stddev      0.173 ns        0.265 ns           10
    BM_StringCreation_cv           0.92 %          1.41 %            10
    BM_StringCopy_mean             33.4 ns         33.3 ns           10
    BM_StringCopy_median           33.4 ns         33.0 ns           10
    BM_StringCopy_stddev          0.473 ns        0.518 ns           10
    BM_StringCopy_cv               1.42 %          1.55 %            10
    ```

    cv は Coefficient of Variation で 変動係数 のこと。
    cv = mean / stddev​ * 100 で求まり、以下の意味合い。

    | CV   | 評価           |
    | ---- | -------        |
    | < 1% | 非常に安定     |
    | 1–3% | 実用上問題なし |
    | 5%+  | ノイズ多め     |
    | 10%+ | 信頼性低い     |

