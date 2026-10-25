# 1. LeRobot のインストール

## 概要

SO-101 を操作する PC に LeRobot v0.6.1 を入れる。所要時間は約 30 分。

> モデルの学習は Google Colab で行うため、この PC に GPU は不要。

## 準備物

- macOS または Windows 10/11 の PC（WSL2 は使わない）
- インターネット接続
- 20 GB 以上の空き容量

## コマンドの読み方

- 灰色の枠内のコマンドは、コピーしてそのまま貼り付け、`Enter` で実行する。
- macOS は「ターミナル」、Windows は「Miniforge Prompt」（手順 1 で入る）で実行する。
- **Windows の場合**、複数行のコマンドは行末の `\` を `^` に置き換えて実行する。

## 手順 1: Miniforge を入れる

Miniforge は Python を管理するソフトである。

**macOS**: ターミナルで次の 2 行を実行する。

```bash
curl -L -O "https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-$(uname)-$(uname -m).sh"
bash Miniforge3-$(uname)-$(uname -m).sh
```

質問には次のように答える。

| 表示 | 入力 |
| --- | --- |
| `press ENTER to continue` | `Enter` |
| `Do you accept the license terms?` | `yes` |
| `Miniforge3 will now be installed into this location` | `Enter`（変更しない） |
| `initialize conda?` | `yes` |

終わったら **ターミナルを閉じて開き直す**。

**Windows**: [Miniforge のダウンロードページ](https://conda-forge.org/download/) から Windows 用インストーラーを入手して実行する。設定は既定のままでよい。完了後、スタートメニューから **Miniforge Prompt** を開く。

**確認**: 次を実行し、`conda 25.x.x` のようにバージョンが出れば OK。

```bash
conda --version
```

> `conda: command not found` と出たら（macOS）: `~/miniforge3/bin/conda init zsh` を実行し、ターミナルを開き直す。

## 手順 2: LeRobot 用の環境を作る

```bash
conda create -y -n lerobot python=3.12
conda activate lerobot
```

**確認**: 行の先頭に `(lerobot)` と表示されれば OK。

## 手順 3: FFmpeg を入れる

動画の保存と読み込みに使う。

```bash
conda install -y -c conda-forge "ffmpeg<9"
```

## 手順 4: LeRobot を入れる

`(lerobot)` が表示されていることを確認してから実行する。数分かかる。

```bash
pip install "lerobot[core_scripts,feetech]==0.6.1"
```

| 指定 | 内容 |
| --- | --- |
| `core_scripts` | キャリブレーション、操作、データ収録などのコマンド |
| `feetech` | SO-101 のモーターと通信する部品 |
| `==0.6.1` | ワークショップと同じバージョンに固定する |

## 手順 5: 動作を確認する

```bash
lerobot-info
```

次のように表示されれば完了。

```text
- LeRobot version: 0.6.1
- Python version: 3.12.x
...
- lerobot scripts: [..., 'lerobot-calibrate', 'lerobot-find-port', 'lerobot-record', ...]
```

`GPU model: N/A` や `Using GPU in script?: <fill in>` はエラーではない。

> `lerobot-info: command not found` と出たら: `conda activate lerobot` を実行してから再度試す。直らなければ手順 4 をやり直す。

## 毎回の始め方

ターミナル（Windows は Miniforge Prompt）を開くたびに、最初にこれを実行する。

```bash
conda activate lerobot
```

## 次へ

[2. SO-101 のセットアップ](./02_setup.md)

## 参考

- [LeRobot 公式: Installation](https://huggingface.co/docs/lerobot/v0.6.1/installation)
- [Miniforge](https://github.com/conda-forge/miniforge)
