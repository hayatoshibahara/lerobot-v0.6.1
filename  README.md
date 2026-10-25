# LeRobot v0.6.1

## 概要

LeRobot v0.6.1 で SO-101 を動かし、ACT モデルを学習させるワークショップ資料。上から順に進める。

1. [LeRobot のインストール](./01_install.md)
2. [SO-101 のセットアップ](./02_setup.md)
3. [データセットの収録](./03_dataset.md)
4. [ACT の学習と評価](./04_act.md)

### 補足資料

SO-101 を自分で作る場合に読む。

- 10. [3D プリント](./10_3dprint.md)
- 11. [組み立て](./11_assemble.md)

### 事前に用意するアカウント

| サービス | 使う資料 |
| --- | --- |
| [Hugging Face](https://huggingface.co/join) | 3, 4（データセットとモデルの保存） |
| [Google](https://accounts.google.com/) | 4（Colab での学習） |
| [Weights & Biases](https://wandb.ai/signup) | 4（学習の進み具合の確認） |

## v0.6.0 から v0.6.1 への主な変更点

LeRobot v0.6.1 は、VLA／データセット／実機運用まわりの機能追加と安定化を中心とするリリースです。公式リリースは 2026 年 8 月 3 日です。

### 互換性に影響する変更

- `lerobot.types` モジュールは **`lerobot.lerobot_types`** に改名されました。既存コードで `from lerobot.types import ...` を使っている場合は、import 先を置き換えてください。

  ```python
  # v0.6.0 まで
  from lerobot.types import FeatureType

  # v0.6.1 から
  from lerobot.lerobot_types import FeatureType
  ```

### 新機能・改善

- **データ注釈**: `lerobot-annotate` の VLM によるサブタスク注釈を改善。読みやすいコンタクトシート、シードを用いた再ラベル付け、セルフホスト型 vLLM の利用例が追加されました。また `--job.target` により Hugging Face Jobs 上でも実行できます。
- **学習・RL**: DataLoader と学習パイプラインに multiprocessing の設定を追加し、開始方式の既定値を `spawn` に変更。拡散ポリシーにはメモリ使用量を抑える gradient checkpointing が追加されました。
- **データセット**: `LeRobotDataset.__getitem__` がスライスを受け付けるようになり、プライベート Hugging Face Hub データセット用のトークン指定にも対応しました。
- **ロボット・カメラ**: LeKiwi を `lerobot-rollout`／`lerobot-eval` と `lerobot-replay` でサポート。RealSense カメラでは露出・ゲイン・ホワイトバランスを手動設定できます。記録時は `--dataset.no_stamp` で dataset repo ID へのタイムスタンプ付加を無効化できます。
- **モーター**: Dynamixel の XH540-W150、XC330-T288、XC330-T181 を対応テーブルに追加し、SO follower の P ゲインを設定可能にしました。
- **ポリシー基盤**: Pi0、Pi0.5、Pi0-Fast、EO-1、SmolVLA などの共通 VLA コンポーネントを整理し、Safetensors を対象デバイスへ直接ロードする改善を含みます。

### 主な修正

- エピソードをリセットした際の動画フレーム一時領域の解放、空のローカルデータセットからの再開、データセット統計計算時のメモリ使用量を改善。
- 評価後にポリシーの train モードを復元し、複数環境評価時の seed 未指定によるクラッシュを防止。MetaWorld のオブジェクト位置も seed により再現可能になりました。
- Teleoperator をロボットより先に接続して watchdog による急な動作を避けるようにし、カメラ接続失敗時のデバイスハンドル解放、Feetech 通信の一時的エラー時の再試行を追加。
- ローカル processor 状態がない場合の不要な Hub フォールバック、PEFT チェックポイントの revision 無視、Linux の TTS 不応答などを修正。

### 参照

- [LeRobot v0.6.1 公式リリースノート](https://github.com/huggingface/lerobot/releases/tag/v0.6.1)
- [v0.6.0 から v0.6.1 の全変更差分](https://github.com/huggingface/lerobot/compare/v0.6.0...v0.6.1)
