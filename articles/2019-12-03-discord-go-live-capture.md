---
title: キャプチャした映像をDiscordのGo Liveで配信
---

キャプチャボードで取り込んだSwitchやPS4なんかの音声と映像を、DiscordのGo Live機能で配信する方法について。

## 映像と音声を入力

[OBS][1]を使う。OBSに以下のソースをそれぞれ追加して、デバイスとしてキャプチャボードを指定する。これで、OBSが映像と音声を参照できるようになる。

- 音声入力キャプチャ
- 映像キャプチャデバイス

## 音声を再生

次に、音声ミキサーのオーディオの詳細プロパティから、音声入力プロパティの音声モニタリングの項目を「モニターと出力」に変更する。これで、OBSのアプリケーションから、音声入力キャプチャで取り込んだ音が再生されるようになる。

## Discordを設定

次に、OBSの「全画面プロジェクター (プレビュー)」で全画面に映し出し、Discordのゲームアクティビティの設定画面から、OBSをゲームとして追加する。

## おわり

これで設定は完了で、DiscordでGo Liveの配信をできるようになる。

自分は以下の環境で動作を確認した。

- OBS Studio 24.0.2
- VT-C878 PLUS
- Windows 10 Home

ちなみに、Macでは2019年12月3日時点ではDiscord Go Liveは利用できない。また、OBSのプレビュー画面ではプレイヤーに支障がある程度の遅延が発生するので、キャプチャボードのパススルー機能を利用して、プレイヤーは別画面で映像を見るようにした方が良い。

[1]: https://obsproject.com/ja