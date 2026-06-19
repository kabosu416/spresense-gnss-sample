# Spresense GNSS Sample

Example for GNSS positioning notification.

Repeat Hot Start and Stop of positioning five times automatically.
In the first positioning, positioning is continued for 200 seconds
after FIX position, accumulating ephemeris. For the second to fifth
iterations, it will perform positioning for 10 seconds.
During positioning, notification is made from gnssfw every period.
Reception of positioning notification can be chosen by poll or by signal.
In both reception methods, position information is read from
the GNSS device by read processing.

The following configuration options can be selected:

* `CONFIG_EXAMPLES_GNSS` -- GNSS positioning example. Default: n
* `CONFIG_EXAMPLES_GNSS_PROGNAME` -- You can choice other name for this program if change this field. Default: "gnss"
* `CONFIG_EXAMPLES_GNSS_PRIORITY` -- Specified this example's task priority. Default: 100
* `CONFIG_EXAMPLES_GNSS_STACKSIZE` -- Specified this example's stack size. Default: 2048
* `CONFIGE_EXAMPLES_GNSS_USE_POLL` -- Choice poll method event notification. If choose this, gnss_poll_main.c is used. Default: CONFIGE_EXAMPLES_GNSS_USE_POLL
* `CONFIGE_EXAMPLES_GNSS_USE_SIGNAL` -- Choice signal method event notification. If choose this, gnss_main.c is used.

In addition to the above, the following definitions are required:
* `CONFIG_CXD56_GNSS`
* `CONFIG_EXAMPLES_GNSS`

---

## How to Build and Run (実行手順)

このプログラムをビルドして Spresense 実機で実行するための基本的な手順です。

### 1. ビルド（コンパイル）
DockerのSDK環境を利用してビルドを行います。（SDKのディレクトリで実行してください）
```bash
cd ~/spresense/sdk
docker run --rm -u $(id -u):$(id -g) -v ~/spresense:/spresense -w /spresense/sdk devworldsony/spresense-sdk-env bash -c "make distclean && ./tools/config.py examples/gnss && make"
```
ビルドが成功すると、同じディレクトリにファームウェア (`nuttx.spk`) が作成されます。

### 2. Spresense に書き込む
作成されたファームウェアをUSB経由で Spresense に書き込みます。
```bash
source ~/spresenseenv/venv/bin/activate
./tools/flash.sh -c /dev/cu.usbserial-130 nuttx.spk
```
※ `/dev/cu.usbserial-130` の部分は、お使いの環境に合わせて接続されているポート名に変更してください。

### 3. シリアルモニタを開く
書き込み完了後、以下のコマンドでシリアル通信を開きます。
```bash
python -m serial.tools.miniterm /dev/cu.usbserial-130 115200
```

### 4. GNSSサンプルの実行
シリアルモニタ上で `nsh>` というプロンプトが表示されたら、以下を入力して実行します。
```bash
nsh> gnss
```
プログラムが起動し、位置情報（緯度・経度）が出力され始めます。
（※シリアルモニタを終了するには `Ctrl + ]` を押します）

動作確認用のものなのでご自由にどうぞ
