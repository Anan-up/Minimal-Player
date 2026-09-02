[English](https://github.com/Anan-up/Minimal-Player/blob/main/README.md) | [简体中文](https://github.com/Anan-up/Minimal-Player/blob/main/README_Simplified_Chinese.md) | [繁体中文](https://github.com/Anan-up/Minimal-Player/blob/main/README_Classical_Chinese.md.md)

此乃**全然自守於一處之媒體播放器**也（以 HTML、CSS、JS 為之），不假外求，無所憑藉於外物。諸般數據，唯於瀏覽器中就地處理，重隱私而尚簡約。

---

### 一、其本與用
- **兼備二式**：能自辨**影像**（MP4、WebM、MOV）與**音聲**（MP3、WAV、FLAC、M4A）。
- **納物之法**：可**點選**以取文件，亦可**曳之**投於播演之所。
- **隱私之守**：假 `URL.createObjectURL` 以立本地一時之鏈，文件不傳於任何伺服之器。

### 二、播演之制
- **常制**：播放、暫停（按鈕或觸影像之面皆可）、移進度（點波形之條以躍）、調聲之大小、與夫靜音。
- **殊制**：**變速而播**（自半倍至三倍）、**全屏之式**（影像音聲皆宜）。
- **狀之報**：其上書文件名與格式之籤，其下隨時更播演之刻。

### 三、音聲之呈象（其技之奇）
於音聲之式，備**二獨立之視象**以為報：

- **下附波形（覽其全而要其躍）**：
  - 假 `AudioContext` 以解音聲之數，算其峰（`computePeaks`）而繪為柱狀之形。
  - **可相與**：點波形任意之所，即躍至其相應之播處（`video.currentTime`）。
  - 播之時，波形隨其進度，以黑標已播之處（灰變為黑）。

- **中庭頻譜（隨時而動）**：
  - 音聲既播，則屏影像之素，而顯 `canvas`。
  - 假 **`AnalyserNode`** 隨時取其頻域之數（FFT），繪為動態柱狀之譜。
  - 音聲暫息或既終，頻譜自斂為靜止微動之飾波，使界面終不空疏。

### 四、其技之要
- **Web Audio API 緩而後啟**：`ensureAudioGraph` 之法，待首次播音聲方立 `AudioContext`，以避瀏覽器自播之禁，且 `MediaElementSource` 唯連一次，免其致誤。
- **高清之繪**：凡 `canvas` 之圖，皆乘 `devicePixelRatio`，使視於 Retina 之屏亦清。
- **以事為驅**：謹聽 `play`、`pause`、`ended`、`timeupdate` 諸事，使界面之態（按鈕之象、頻譜之開闔、波形之進）相應而不忒。

### 五、其觀與感
- **尚簡**：底色淡，邊纖細，角圓大，去繁飾。
- **應觸之報**：遊其上則色變，按之則有動，曳之則邊明，其用也暢然。
![project-screenshot](player.png)
---

### 許可

[MIT](LICENSE)
