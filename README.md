# pomodoro-timer
<!DOCTYPE html>
<html lang="ja">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>ポモドーロタイマー</title>
<style>
  :root {
    --bg-color: #007BFF;
    --text-color: white;
  }
  body {
    background-color: var(--bg-color);
    color: var(--text-color);
    font-family: sans-serif;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    height: 100vh;
    margin: 0;
    transition: background-color 0.3s, color 0.3s;
    position: relative;
  }
  .main-content, #settings {
    border: 2px solid white;
    border-radius: 12px;
    padding: 20px;
    background-color: rgba(255, 255, 255, 0.1);
    margin: 10px;
    text-align: center;
    max-width: 656px;
    box-sizing: border-box;
    transition: background-color 0.3s, border-color 0.3s, color 0.3s;
  }
  .hidden {
    display: none !important;
  }
  h1 {
    margin-bottom: 20px;
  }
  button {
    padding: 10px 20px;
    margin: 10px;
    font-size: 1rem;
    font-weight: bold;
    border: 2px solid #007BFF;
    border-radius: 8px;
    cursor: pointer;
    background-color: white;
    color: #007BFF;
    transition: background-color 0.2s, color 0.2s, border-color 0.2s;
  }
  button:hover {
    background-color: #e0e0e0;
  }
  #settingsButton {
    position: absolute;
    top: 15px;
    right: 15px;
    width: 40px;
    height: 40px;
    border-radius: 50%;
    font-size: 18px;
    font-weight: bold;
    background-color: white;
    color: #007BFF;
    border: 2px solid #007BFF;
    display: flex;
    align-items: center;
    justify-content: center;
    cursor: pointer;
    transition: background-color 0.3s, color 0.3s, border-color 0.3s;
  }
  .info {
    margin: 5px 0;
  }
  .setting-row {
    display: flex;
    align-items: center;
    margin: 8px 0;
  }
  .setting-row label {
    width: 150px;
    text-align: right;
    margin-right: 10px;
    white-space: nowrap;
  }
  .setting-row label::after {
    content: "：";
    margin-left: 2px;
  }
  .setting-row input[type="number"],
  .setting-row input[type="time"],
  .setting-row input[type="color"],
  .setting-row select,
  .setting-row button,
  .setting-row input[type="range"] {
    flex: 1;
    padding: 4px 8px;
    font-size: 1rem;
  }
  input[type="checkbox"] {
    width: auto;
    transform: scale(1.2);
    vertical-align: middle;
    margin-left: 5px;
  }
  /* ファイル入力のラベル部分を隠してinput幅いっぱいに */
  .setting-row input[type="file"] {
    flex: 1;
    padding: 4px 8px;
    font-size: 1rem;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
    box-sizing: border-box;
  }
</style>
</head>
<body>
  <button id="settingsButton" onclick="toggleSettings()">⚙️</button>

  <div class="main-content" id="mainContent">
    <h1>ポモドーロタイマー</h1>
    <div>
      <button id="startPauseButton" onclick="toggleStartPause()">開始</button>
      <button onclick="resetTimer()">リセット</button>
    </div>
    <div class="info">現在のモード: <span id="mode">待機中</span></div>
    <div class="info">残り時間: <span id="timeDisplay">00:00</span></div>
    <div class="info">終了時刻: <span id="endDisplay">--:--</span></div>
    <div class="info">全体の残り時間: <span id="totalTimeDisplay">--:--:--</span></div>
    <div class="info">作業時間: <span id="workDisplay">25分</span> ／ 休憩時間: <span id="breakDisplay">5分</span></div>
  </div>

  <div id="settings" class="main-content hidden">
    <div class="setting-row">
      <label for="workInput">作業時間</label>
      <input type="number" id="workInput" value="25" min="1" />
    </div>
    <div class="setting-row">
      <label for="breakInput">休憩時間</label>
      <input type="number" id="breakInput" value="5" min="1" />
    </div>
    <div class="setting-row">
      <label for="useLongBreak">長休憩を使う</label>
      <input type="checkbox" id="useLongBreak" />
    </div>
    <div class="setting-row">
      <label for="longBreakInput">長休憩時間</label>
      <input type="number" id="longBreakInput" value="10" min="1" disabled />
    </div>
    <div class="setting-row">
      <label for="longBreakInterval">長休憩の間隔</label>
      <input type="number" id="longBreakInterval" value="4" min="1" disabled />
    </div>
    <div class="setting-row">
      <label for="endTimeInput">終了時刻</label>
      <input type="time" id="endTimeInput" />
    </div>
    <div class="setting-row">
      <label for="soundSelect">アラーム音</label>
      <select id="soundSelect">
        <option value="https://actions.google.com/sounds/v1/alarms/beep_short.ogg">ビープ</option>
        <option value="https://actions.google.com/sounds/v1/alarms/alarm_clock.ogg">アラーム</option>
      </select>
    </div>
    <div class="setting-row">
      <label for="volumeSlider">音量</label>
      <input type="range" id="volumeSlider" min="0" max="1" step="0.01" value="1.0" />
    </div>
    <div class="setting-row">
      <label for="soundToggle">音を鳴らす</label>
      <input type="checkbox" id="soundToggle" checked />
    </div>
    <div class="setting-row">
      <label for="bgColorPicker">背景色</label>
      <input type="color" id="bgColorPicker" value="#007BFF" />
    </div>
    <div class="setting-row">
      <label for="darkModeToggle">ダークモード</label>
      <input type="checkbox" id="darkModeToggle" />
    </div>
    <div class="setting-row">
      <label for="musicFile">音楽ファイル</label>
      <input type="file" id="musicFile" accept="audio/*" />
    </div>
    <div class="setting-row">
      <label>音楽操作</label>
      <button id="musicToggleButton" onclick="toggleMusic()">音楽 再生</button>
    </div>
    <div class="setting-row">
      <label>全画面表示</label>
      <button onclick="toggleFullScreen()">全画面切替</button>
    </div>
  </div>

  <audio id="beep"></audio>
  <audio id="bgm" loop></audio>

  <script>
    let interval = null;
    let isRunning = false;
    let isWork = true;
    let remaining = 0;
    let endTime = null;
    let sessionCount = 0;

    const beep = document.getElementById("beep");
    const bgm = document.getElementById("bgm");
    const musicToggleButton = document.getElementById("musicToggleButton");

    function toggleSettings() {
      const settings = document.getElementById("settings");
      const main = document.getElementById("mainContent");
      settings.classList.toggle("hidden");
      main.classList.toggle("hidden");
      updateSettings();
    }

    function updateSettings() {
      document.getElementById("workDisplay").textContent = `${document.getElementById("workInput").value}分`;
      document.getElementById("breakDisplay").textContent = `${document.getElementById("breakInput").value}分`;
    }

    function toggleStartPause() {
      if (!isRunning) {
        startSession();
        isRunning = true;
        document.getElementById("startPauseButton").textContent = "一時停止";
      } else {
        clearInterval(interval);
        interval = null;
        isRunning = false;
        document.getElementById("startPauseButton").textContent = "開始";
      }
    }

    function startSession() {
      const workMinutes = parseInt(document.getElementById("workInput").value);
      const breakMinutes = parseInt(document.getElementById("breakInput").value);
      const longBreakMinutes = parseInt(document.getElementById("longBreakInput").value);
      const longBreakInterval = parseInt(document.getElementById("longBreakInterval").value);
      const useLongBreak = document.getElementById("useLongBreak").checked;

      beep.src = document.getElementById("soundSelect").value;
      beep.volume = parseFloat(document.getElementById("volumeSlider").value);

      if (!endTime) {
        const input = document.getElementById("endTimeInput").value;
        if (input) {
          const [h, m] = input.split(":");
          endTime = new Date();
          endTime.setHours(parseInt(h));
          endTime.setMinutes(parseInt(m));
          endTime.setSeconds(0);
        }
      }

      const duration = isWork
        ? workMinutes * 60
        : (useLongBreak && sessionCount % longBreakInterval === 0 && sessionCount !== 0)
          ? longBreakMinutes * 60
          : breakMinutes * 60;

      document.getElementById("mode").textContent = isWork ? "作業中" : "休憩中";
      remaining = duration;
      updateTimerDisplay();

      interval = setInterval(() => {
        if (remaining <= 0) {
          if (document.getElementById("soundToggle").checked) {
            beep.play();
            if (Notification.permission === "granted") {
              new Notification(isWork ? "休憩時間です。" : "作業時間です。");
            }
          }
          isWork = !isWork;
          if (isWork) sessionCount++;
          startSession();
        } else {
          remaining--;
          updateTimerDisplay();
          updateTotalTime();
        }
      }, 1000);
    }

    function updateTimerDisplay() {
      document.getElementById("timeDisplay").textContent = formatTime(remaining);
    }

    function updateTotalTime() {
      if (!endTime) {
        document.getElementById("totalTimeDisplay").textContent = "--:--:--";
        document.getElementById("endDisplay").textContent = "--:--";
        return;
      }
      const now = new Date();
      const diff = Math.floor((endTime - now) / 1000);
      document.getElementById("totalTimeDisplay").textContent = diff > 0 ? formatFullTime(diff) : "00:00:00";
      document.getElementById("endDisplay").textContent =
        `${endTime.getHours().toString().padStart(2, '0')}:${endTime.getMinutes().toString().padStart(2, '0')}`;
    }

    function resetTimer() {
      clearInterval(interval);
      interval = null;
      isRunning = false;
      isWork = true;
      remaining = 0;
      sessionCount = 0;
      endTime = null;
      document.getElementById("mode").textContent = "待機中";
      document.getElementById("timeDisplay").textContent = "00:00";
      document.getElementById("totalTimeDisplay").textContent = "--:--:--";
      document.getElementById("endDisplay").textContent = "--:--";
      document.getElementById("startPauseButton").textContent = "開始";
    }

    function formatTime(sec) {
      const m = Math.floor(sec / 60);
      const s = sec % 60;
      return `${m.toString().padStart(2, '0')}:${s.toString().padStart(2, '0')}`;
    }

    function formatFullTime(sec) {
      const h = Math.floor(sec / 3600);
      const m = Math.floor((sec % 3600) / 60);
      const s = sec % 60;
      return `${h.toString().padStart(2, '0')}:${m.toString().padStart(2, '0')}:${s.toString().padStart(2, '0')}`;
    }

    function toggleFullScreen() {
      if (!document.fullscreenElement) {
        document.documentElement.requestFullscreen();
      } else {
        document.exitFullscreen();
      }
    }

    // 輝度計算（カラーコードから）
    function getLuminance(hex) {
      const rgb = hex.match(/[\da-f]{2}/gi).map(x => parseInt(x, 16) / 255);
      return 0.2126 * rgb[0] + 0.7152 * rgb[1] + 0.0722 * rgb[2];
    }

    // UI全体の色を背景色に合わせて調整する関数
    function updateUIColor(bgColor) {
      const luminance = getLuminance(bgColor);

      // 文字色は背景明るさに応じて白 or 黒
      const textColor = luminance < 0.5 ? 'white' : 'black';
      document.body.style.color = textColor;

      // main-contentやsettingsの背景色は明るさによって調整（薄い白 or 薄い黒半透明）
      const mainBgColor = luminance < 0.5 ? 'rgba(255, 255, 255, 0.15)' : 'rgba(0, 0, 0, 0.1)';
      const borderColor = luminance < 0.5 ? 'rgba(255, 255, 255, 0.7)' : 'rgba(0, 0, 0, 0.5)';

      // ボタンの背景色と文字色も明るさに応じて反転
      const buttonBg = luminance < 0.5 ? 'white' : '#007BFF';
      const buttonColor = luminance < 0.5 ? '#007BFF' : 'white';
      const buttonBorder = luminance < 0.5 ? '#007BFF' : '#0056b3';

      // 反映
      document.querySelectorAll('.main-content, #settings').forEach(el => {
        el.style.backgroundColor = mainBgColor;
        el.style.borderColor = borderColor;
        el.style.color = textColor;
      });

      document.querySelectorAll('button').forEach(btn => {
        btn.style.backgroundColor = buttonBg;
        btn.style.color = buttonColor;
        btn.style.borderColor = buttonBorder;
      });
    }

    document.getElementById("bgColorPicker").addEventListener("input", e => {
      const color = e.target.value;
      document.body.style.backgroundColor = color;
      updateUIColor(color);
      localStorage.setItem("bgColor", color);
    });

    document.getElementById("darkModeToggle").addEventListener("change", e => {
      if (e.target.checked) {
        const darkBg = "#121212";
        document.body.style.backgroundColor = darkBg;
        updateUIColor(darkBg);
      } else {
        const picker = document.getElementById("bgColorPicker");
        const bgColor = picker.value;
        document.body.style.backgroundColor = bgColor;
        updateUIColor(bgColor);
      }
    });

    document.getElementById("musicFile").addEventListener("change", e => {
      const file = e.target.files[0];
      if (file) {
        const url = URL.createObjectURL(file);
        bgm.src = url;
        bgm.volume = 0.3;
        localStorage.setItem("music", url);
      }
    });

    function toggleMusic() {
      if (bgm.paused) {
        bgm.play();
        musicToggleButton.textContent = "音楽 停止";
      } else {
        bgm.pause();
        musicToggleButton.textContent = "音楽 再生";
      }
    }

    document.getElementById("useLongBreak").addEventListener("change", e => {
      const checked = e.target.checked;
      document.getElementById("longBreakInput").disabled = !checked;
      document.getElementById("longBreakInterval").disabled = !checked;
      localStorage.setItem("useLongBreak", checked);
    });

    window.onload = () => {
      if (Notification && Notification.permission !== "granted") {
        Notification.requestPermission();
      }
      const savedColor = localStorage.getItem("bgColor");
      if (savedColor) {
        document.body.style.backgroundColor = savedColor;
        document.getElementById("bgColorPicker").value = savedColor;
        updateUIColor(savedColor);
      }
      const useLong = localStorage.getItem("useLongBreak") === "true";
      document.getElementById("useLongBreak").checked = useLong;
      document.getElementById("longBreakInput").disabled = !use
      document.getElementById("longBreakInput").disabled = !useLong;
      document.getElementById("longBreakInterval").disabled = !useLong;
    };
  </script>
</body>
</html>
