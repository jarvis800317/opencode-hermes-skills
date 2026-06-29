---
name: media-processing
description: 影音處理工具技能。當使用者說「下載 YouTube」、「擷取字幕」、「處理影片」時，執行影音處理指令。
---

# 影音處理工具

處理 YouTube 影片、音頻下載、字幕擷取等任務。

## 觸發時機

- 「下載 YouTube」
- 「擷取字幕」
- 「處理影片」
- 「下載音頻」
- 「下載影片」

## YouTube 影片下載

### yt-dlp（推薦）

```bash
# 安裝
pip3 install yt-dlp

# 基本下載
yt-dlp "https://www.youtube.com/watch?v=..."

# 下載最高畫質
yt-dlp -f "bestvideo[ext=mp4]+bestaudio[ext=m4a]/best[ext=mp4]/best" "URL"

# 只下載音頻
yt-dlp -x --audio-format mp3 "URL"

# 下載字幕
yt-dlp --write-subs --write-auto-subs --sub-lang zh-Hant "URL"

# 下載並轉換為 MP3
yt-dlp -x --audio-format mp3 --audio-quality 0 "URL"
```

### 常見參數

| 參數 | 用途 |
|------|------|
| `-f FORMAT` | 指定格式 |
| `-x` | 只下載音頻 |
| `--audio-format mp3` | 轉換為 MP3 |
| `--write-subs` | 下載字幕 |
| `--write-auto-subs` | 下載自動生成的字幕 |
| `-o TEMPLATE` | 輸出檔名模板 |
| `--proxy PROXY` | 使用代理 |

### 輸出檔名模板

```bash
# 使用標題和品質
yt-dlp -o "%(title)s-%(quality)s.%(ext)s" "URL"

# 按頻道分類
yt-dlp -o "%(uploader)s/%(title)s.%(ext)s" "URL"
```

## YouTube 字幕擷取

### youtube-transcript-api

```bash
pip3 install youtube-transcript-api
```

```python
from youtube_transcript_api import YouTubeTranscriptApi

# 從 URL 提取影片 ID
url = "https://www.youtube.com/watch?v=VIDEO_ID"
video_id = url.split("v=")[1].split("&")[0]

# 取得字幕
transcript = YouTubeTranscriptApi.get_transcript(video_id)

# 輸出文字
for line in transcript:
    print(f"{line['start']:.2f}: {line['text']}")
```

### 直接下載字幕檔案

```python
from youtube_transcript_api import YouTubeTranscriptApi

video_id = "VIDEO_ID"

# 下載為 SRT 格式
transcript = YouTubeTranscriptApi.get_transcript(video_id)

with open(f"{video_id}.srt", "w", encoding="utf-8") as f:
    for i, line in enumerate(transcript, 1):
        start = line['start']
        duration = line['duration']
        text = line['text']

        # SRT 格式
        f.write(f"{i}\n")
        f.write(f"{int(start//3600):02d}:{int((start%3600)//60):02d}:{int(start%60):02d},{int((start%1)*1000):03d} --> ")
        f.write(f"{int((start+duration)//3600):02d}:{int(((start+duration)%3600)//60):02d}:{int((start+duration)%60):02d},{int(((start+duration)%1)*1000):03d}\n")
        f.write(f"{text}\n\n")
```

## 本地影片處理

### 使用 ffmpeg（需另外安裝）

```bash
# macOS
brew install ffmpeg

# Windows: 下載 ffmpeg 並加入 PATH
```

```bash
# 轉換格式
ffmpeg -i input.mp4 output.avi

# 擷取音頻
ffmpeg -i input.mp4 -vn -acodec libmp3lame output.mp3

# 裁剪影片
ffmpeg -i input.mp4 -ss 00:01:00 -to 00:02:00 -c copy output.mp4

# 調整解析度
ffmpeg -i input.mp4 -vf "scale=1280:720" output.mp4

# 壓縮影片
ffmpeg -i input.mp4 -crf 23 -preset medium output.mp4
```

## 踩坑筆記

| 狀況 | 解法 |
|------|------|
| YouTube 下載失敗 | 可能是地區限制，嘗試 `--geo-bypass` |
| 字幕取得失敗 | 影片可能沒有字幕或需要登入 |
| ffmpeg 找不到 | 確認已安裝並加入系統 PATH |
| 下載速度慢 | 使用 `-r` 限制速率或 `--proxy` 使用代理 |

## 安全規則

- ❌ 不下載版權受保護的內容
- ❌ 不散播下載的盜版內容
- ✅ 只用於備份自己創作的內容或授權素材