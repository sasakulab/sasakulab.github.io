---
layout: content
title: Windows 環境の整備
permalink: /memo/env-windows
layout: post
toc:
  ordered: false
---

Windows 環境の整備で忘れそうなやつ

## Winget エクスポートデータ アーカイブ

楽になります。**楽になりましょう。** 移行。

### Winget に登録されているアプリケーションの出力

Winget を用いて、JSON ファイルを出力します。

```sh
winget export -o wingetlog_%DATE:/=%.json --include-versions
```

### JSON ファイルに合わせてインストール

JSON ファイルに出力します。

```sh
winget import -i {import するファイル}.json --ignore-versions --accept-package-agreements --ignore-unavailable
```

## Winget を用いたアップデート

Winget でインストールしてなくてもアップデートしてくれるので Good!👍

**事前に管理者権限で Powershell (cmd) に入っておくと UAC を求められない。**

```sh
winget upgrade --all -h
```

- `--all` ですべてのパッケージ
- `-h` で サイレントインストール をもとめる

## pip の保存

Python そろそろ `pyenv` に移動するべき。
そんなのわかってます。

### すべてのライブラリをエクスポート

```sh
pip freeze -l > requirements.txt

```

### `requirements.txt` を用いたインストール

```sh
pip install -r requirements.txt
```

### `pip` で全件アップデート

> 普通やんないほうがいい。普通やらない。
{: .block-warning}

- Windows

```cmd
pip list -o --format freeze | % { $_ -replace "=.*", "" } | % { python -m pip install -U &_ }
```

- Linux

```sh
pip list -o --format freeze | sed 's/=.*//' | xargs python -m pip install -U
```

## よく忘れる `pip` のアップデート

`pip` 自体のアップデート。よく忘れる。

```sh
pip install --upgrade pip
```

## リンク

- [winget の JSON ファイル][env_01]

[env_01]: https://github.com/sasakulab/env/tree/master/winget
