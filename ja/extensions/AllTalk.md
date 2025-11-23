---
order: tts-alltalk
route: /extensions/alltalk/
---

# AllTalk TTS V2

AllTalkは、Coqui XTTS、F5-TTS、VITS、Piperなどのテキスト音声合成エンジンに基づいた音声クローニングシステムです。高品質の音声再現（ゼロショット音声クローニングまたは組み込み音声）を生成するように設計されています。AllTalk V2では、機能性と使いやすさを向上させる大幅な更新が行われており、複数のTTSエンジンサポート、拡張カスタマイズ、パフォーマンス最適化が含まれています。完全な機能リストについては、[AllTalk Wiki](https://github.com/erew123/alltalk_tts/wiki)を参照してください。

---

## 🟩 AllTalk V2の主な機能

- **マルチエンジンサポート**：Coqui XTTS、VITS、Piper、Parler、F5、カスタムエンジン間で簡単に切り替え。
- **音声変換(RVC)**：強化された検索ベースの音声クローニングパイプライン。
- **カスタマイズ可能な設定**：エンジンごとの設定を調整し、スタートアップ構成を保存。
- **ナレーター機能**：ナレーションとキャラクター用に異なる音声を指定。
- **スタンドアロンおよび統合使用**：SillyTavernとのシームレス統合。
- **DeepSpeedおよび低VRAMモード**：リソース制限環境向けのパフォーマンス最適化。
- **スクリーンショット**：AllTalk V2インターフェース[こちら](https://github.com/erew123/alltalk_tts/discussions/237)。

---

## 🟨 セットアップとインストール オプション

AllTalkは、スタンドアロンと統合インストール方法の両方を提供します。最速のセットアップは、提供されるクイックインストールオプションの1つを使用することです。スクリプトはほとんどのプロセスを自動化します。

- **スタンドアロン インストール**：ほとんどのユーザー推奨([スタンドアロン ガイド](https://github.com/erew123/alltalk_tts/wiki/Install-%E2%80%90-Standalone-Installation))
- **Text-generation-webui統合**：Text-generation-webuiへの統合向け([TGWUIインストール ガイド](https://github.com/erew123/alltalk_tts/wiki/Install-%E2%80%90-Text%E2%80%90generation%E2%80%90webui-Installation))

#### 🟩 自動化インストール
**このメソッドはWindows ユーザーのみ対象です。**
クイックセットアップを希望する新規ユーザー向けに、自動インストールはSillyTavern-Launcherを使用します。
注意：これはSillyTavern-Launcherがすでにインストールされていることを前提としています。インストールされていない場合は、 https://github.com/SillyTavern/SillyTavern-Launcher にアクセスしてreadme.mdファイルの指示に従ってください。
SillyTavern-Launcherがインストールされたら：
1. Launcher.batを実行
2. 次に移動：`Home > Toolbox > App Installer > Voice Generation`
3. **Install AllTalk V2**というラベルの付いたオプションを選択

#### 🟩 手動インストール
詳細な制御が必要な上級ユーザーは、[マニュアルインストール ガイド](https://github.com/erew123/alltalk_tts/wiki/Install-%E2%80%90-Manual-Installation-Guide)に従ってWindows、Linux、またはMac（未テスト）でステップバイステップセットアップを行ってください。

#### 🟩 Google Colabインストール
[Google Colabインストール](https://github.com/erew123/alltalk_tts/wiki/Google-COLAB)でクラウド環境でAllTalkを実行します。ローカルインストールしたくないユーザー向け。

---

## 🟨 SillyTavern内でAllTalkを使用

AllTalkが読み込まれたら、SillyTavernのTTSページで選択し、設定で正しいAllTalkサーバーバージョンを選択してください。

- **設定管理**：AllTalkは選択した構成に基づいて特定の設定を有効または無効にします。
- **読み込み シーケンス**：SillyTavernがAllTalkの前に読み込まれた場合、TTS拡張ページをリロードしてください。
- **パフォーマンス最適化**：システムリソースに基づいてパフォーマンスを向上させるためにDeepSpeedおよび低VRAMモードを選択的に有効にします。
- **ナレーター機能**：ナレーター機能の詳細は、[AllTalk Wiki](https://github.com/erew123/alltalk_tts/wiki/Narrator-Function)にあります。

SillyTavern AllTalk拡張の完全な詳細は、[AllTalk Wiki SillyTavernページ](https://github.com/erew123/alltalk_tts/wiki/SillyTavern-Extension)で更新されます

AllTalk拡張をTGWUIで使用するユーザーは、TGWUIチャットインターフェイスで`Enable TGWUI TTS`を無効にする必要があります。そうしないと、重複したTTSオーディオが生成されます。

---

## 🟨 トラブルシューティング

SillyTavern内のAllTalkに固有と思われる問題が発生した場合は、最新情報について[AllTalk Wiki SillyTavernページ](https://github.com/erew123/alltalk_tts/wiki/SillyTavern-Extension)を参照してください。

---

### 🟪 サポート、支援、機能リクエスト

さらにサポートが必要な場合：
- [Wiki](https://github.com/erew123/alltalk_tts/wiki)および組み込みドキュメントを参照してください。
- [ディスカッション ボード](https://github.com/erew123/alltalk_tts/discussions/245)でディスカッションに参加してください。
- [Issue Tracker](https://github.com/erew123/alltalk_tts/issues)を通じてバグまたは機能リクエストを送信してください。

---
