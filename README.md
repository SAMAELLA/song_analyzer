<img width="1529" height="323" alt="スクリーンショット 2026-10-04 165612" src="https://github.com/user-attachments/assets/45d7ecb2-12f9-4a17-9b83-56821a7f1e49" />


# Song Analyzer

YouTube Music の曲情報を取得し、音声をダウンロードして分離・分析するための実験プロジェクトです。
このリポジトリは、Jupyter Notebook を中心に構成されており、音楽解析に必要な処理を順番に実行できます。

## 概要

このプロジェクトでは、次の流れで楽曲を分析します。

1. YouTube Music の URL から曲情報を取得
2. 曲の音声をダウンロード
3. Demucs でボーカル・ベース・ドラム・その他のトラックに分離
4. Librosa を用いて tempo / key / time signature / melody などを推定
5. SQLite に解析結果を保存

主な目的は、音声データを可視化し、楽曲の構造や特徴を自動で把握することです。

## 代表的な機能

- YouTube Music URL からタイトル・アーティスト・アルバム・リリース日などを取得
- MP3 音声のダウンロード
- Demucs による楽器別音源分離
- スペクトログラムの描画
- BPM / 拍子 / 推定キーの算出
- SQLite への保存

## 対応技術

- Python
- yt-dlp
- FFmpeg
- Demucs
- Librosa
- NumPy
- Matplotlib
- SQLite

## ディレクトリ構成

```text
song_analyzer/
├── content/
│   ├── my_audio.mp3
│   └── separated/
│       └── htdemucs/
│           └── my_audio/
│               ├── vocals.wav
│               ├── bass.wav
│               ├── drums.wav
│               ├── other.wav
│               └── other_bass_combined.wav
├── song_analysis.db
├── songanalyze.ipynb
├── .gitignore
└── README.md
```

- `songanalyze.ipynb`: 実際の処理を実行するノートブック
- `content/`: ダウンロードした音声や分離済み音源の出力先
- `song_analysis.db`: 解析結果を保存する SQLite データベース

## 必要環境

- Python 3.9 以上を推奨
- FFmpeg がインストールされていること
- GPU があると Demucs の処理が高速になりますが、CPU でも実行可能です

Windows の場合は、ノートブック内で次のような設定を使用しています。

```python
'ffmpeg_location': r"C:\ffmpeg\bin"
```

そのため、ローカル環境に FFmpeg が `C:\ffmpeg\bin` にある前提です。

## セットアップ

1. リポジトリをクローン

```bash
git clone https://github.com/SAMAELLA/song_analyzer.git
cd song_analyzer
```

2. 仮想環境を作成（推奨）

```bash
python -m venv .venv
.venv\Scripts\activate
```

3. 依存ライブラリをインストール

```bash
pip install yt-dlp demucs librosa matplotlib soundfile numpy scipy
```

4. FFmpeg をインストールして PATH を通す

- Windows: https://www.ffmpeg.org/download.html
- macOS: `brew install ffmpeg`
- Ubuntu/Debian: `sudo apt install ffmpeg`

## 使い方

1. `songanalyze.ipynb` を開く
2. `target_url` に対象の YouTube Music URL を設定

```python
target_url = "https://music.youtube.com/watch?v=..."
```

3. セルを順番に実行する
4. 解析結果が `content/` と `song_analysis.db` に出力される

Notebook の流れは以下の通りです。

- YouTube Music からメタデータ取得
- 音声ダウンロード
- 音源分離
- スペクトログラムと特徴量解析
- SQLite への保存

## 生成される出力

### 音声ファイル

```text
content/
├── my_audio.mp3
└── separated/
    └── htdemucs/
        └── my_audio/
            ├── vocals.wav
            ├── bass.wav
            ├── drums.wav
            ├── other.wav
            ├── other_bass_combined.wav
```

### データベース

`song_analysis.db` にはアーティスト別のテーブルが作られ、解析結果が保存されます。
保存対象には、次のような項目が含まれます。

- id
- url
- title
- artist
- album
- release_date
- duration
- tempo_bpm
- time_signature
- estimated_key
- estimated_mode
- chord_progression
- saved_at

## 注意事項

- YouTube Music の取得には、利用規約と著作権に注意してください。
- YouTube / Music のアクセス制限や JavaScript runtime の問題により、メタデータ取得が失敗する場合があります。
- Demucs は重い処理になりやすいため、十分なメモリと CPU を確保した環境で実行するのがおすすめです。

## ライセンス

このプロジェクトのライセンスは未定義です。必要に応じて独自のライセンスを追加してください。

## 今後の改善案

- CLI 化してコマンド一発で分析できるようにする
- 分析結果を JSON / CSV として出力する
- Web UI で可視化する
- 複数楽曲を一括分析できるようにする
- 曲ごとの特徴量比較機能を追加する

## 参考

- yt-dlp: https://github.com/yt-dlp/yt-dlp
- Demucs: https://github.com/facebookresearch/demucs
- Librosa: https://librosa.org/
- FFmpeg: https://www.ffmpeg.org/
