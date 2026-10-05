<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
  <meta http-equiv="Cache-Control" content="no-cache, no-store, must-revalidate">
  <meta http-equiv="Pragma" content="no-cache">
  <meta http-equiv="Expires" content="0">
  <title>Pact & Passion</title>
  <script src="https://telegram.org/js/telegram-web-app.js?v=3"></script>
  <style>
    :root {
      --bg-color: #0b0b0e;
      --card-bg: #161620;
      --accent-color: #ff0044;
      --text-main: #ffffff;
      --text-muted: #8e8e9f;
      --border-color: #262636;
    }

    * {
      box-sizing: border-box;
      -webkit-tap-highlight-color: transparent;
    }

    body {
      margin: 0;
      padding: 16px 16px 80px 16px;
      font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
      background-color: var(--bg-color);
      color: var(--text-main);
      overflow-x: hidden;
    }

    /* ЕКРАН ЗАСТАВКИ (SPLASH SCREEN) */
    #splash-screen {
      position: fixed;
      inset: 0;
      background: #070709;
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      z-index: 99999;
      transition: opacity 0.5s ease, visibility 0.5s;
    }

    #splash-screen.hidden {
      opacity: 0;
      visibility: hidden;
      pointer-events: none;
    }

    .contract-stage {
      position: relative;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
    }

    .fire-icon {
      font-size: 60px;
      margin-bottom: 12px;
      animation: pulseFire 1.5s infinite alternate ease-in-out;
    }

    @keyframes pulseFire {
      0% { transform: scale(0.95); opacity: 0.8; }
      100% { transform: scale(1.1); opacity: 1; }
    }

    /* 1. ДРУКУЄТЬСЯ НАЗВА */
    .typewriter {
      font-size: 28px;
      font-weight: 900;
      color: #ffffff;
      letter-spacing: 2px;
      text-transform: uppercase;
      margin: 0;
      display: inline-block;
      white-space: nowrap;
      overflow: hidden;
      border-right: 3px solid #ff0044;
      width: 0;
      animation: 
        typeText 1.2s steps(13, end) 0.2s forwards,
        blinkCursor 0.5s step-end infinite;
    }

    @keyframes typeText {
      from { width: 0; }
      to { width: 100%; }
    }

    @keyframes blinkCursor {
      from, to { border-color: transparent; }
      50% { border-color: #ff0044; }
    }

    /* 2. ПЕЧАТКА БАХАЄ ПОВЕРХ НАЗВИ */
    .stamp-overlay {
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%) scale(3) rotate(-20deg);
      opacity: 0;
      pointer-events: none;
      animation: hardSlam 0.2s cubic-bezier(0.1, 0.9, 0.2, 1) 1.5s forwards;
      z-index: 10;
    }

    .seal-body {
      width: 140px;
      height: 140px;
      border: 5px solid #ff0044;
      border-radius: 50%;
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      background: rgba(18, 0, 6, 0.88);
      position: relative;
      box-shadow: 0 0 25px rgba(255, 0, 68, 0.6);
    }

    .seal-inner-ring {
      position: absolute;
      inset: 5px;
      border: 2px dashed #ff0044;
      border-radius: 50%;
    }

    .seal-number {
      font-size: 48px;
      font-weight: 900;
      color: #ff0044;
      line-height: 1;
      letter-spacing: -2px;
    }

    .seal-banner {
      background: #ff0044;
      color: #ffffff;
      font-size: 10px;
      font-weight: 900;
      padding: 2px 10px;
      margin-top: 2px;
      letter-spacing: 2px;
      text-transform: uppercase;
      transform: rotate(-5deg);
      border-radius: 2px;
    }

    /* СПАЛАХ/ХВИЛЯ ВІД УДАРУ ПЕЧАТКИ */
    .seal-flash {
      position: absolute;
      inset: -10px;
      border-radius: 50%;
      border: 5px solid #ff0044;
      opacity: 0;
      animation: flashRing 0.4s ease-out 1.5s forwards;
    }

    @keyframes hardSlam {
      0% { transform: translate(-50%, -50%) scale(3) rotate(-25deg); opacity: 0; }
      100% { transform: translate(-50%, -50%) scale(1) rotate(-12deg); opacity: 1; }
    }

    @keyframes flashRing {
      0% { transform: scale(0.8); opacity: 1; }
      100% { transform: scale(1.7); opacity: 0; }
    }

    /* ПРОГРЕС-БАР */
    .loading-line-bg {
      width: 160px;
      height: 4px;
      background: #1c1c28;
      border-radius: 4px;
      margin-top: 28px;
      overflow: hidden;
    }

    .loading-line-fill {
      width: 0%;
      height: 100%;
      background: #ff0044;
      border-radius: 4px;
      animation: fillBar 2.2s ease 1.8s forwards;
    }

    @keyframes fillBar {
      to { width: 100%; }
    }

    /* ІНТЕРФЕЙС ГРИ */
    .header { text-align: center; margin-bottom: 20px; }
    .title {
      font-size: 24px; font-weight: 800; color: var(--accent-color);
      letter-spacing: 1px; margin: 0; text-transform: uppercase;
      display: flex; align-items: center; justify-content: center; gap: 8px;
    }
    .header-age {
      font-size: 11px; border: 1px solid var(--accent-color);
      padding: 2px 6px; border-radius: 6px; color: var(--accent-color);
    }
    .user-welcome { font-size: 14px; color: var(--text-muted); margin-top: 6px; }

    .card {
      background: var(--card-bg); border: 1px solid var(--border-color);
      border-radius: 16px; padding: 20px; margin-bottom: 16px;
    }

    .stats-container { display: flex; justify-content: space-around; text-align: center; }
    .stat-item .val { font-size: 22px; font-weight: 700; color: var(--accent-color); }
    .stat-item .lbl { font-size: 12px; color: var(--text-muted); margin-top: 4px; }

    .contract-list { display: flex; flex-direction: column; gap: 10px; }
    .contract-item {
      background: #101018; border: 1px solid var(--border-color);
      padding: 14px; border-radius: 10px; display: flex;
      justify-content: space-between; align-items: center;
    }
    .contract-info .name { font-size: 14px; font-weight: 600; }
    .contract-info .reward { font-size: 12px; color: var(--accent-color); margin-top: 2px; }

    .small-btn {
      background: transparent; border: 1px solid var(--accent-color);
      color: var(--accent-color); padding: 6px 12px; border-radius: 8px;
      font-size: 12px; font-weight: 600; cursor: pointer;
    }

    .tab-content { display: none; }
    .tab-content.active { display: block; }

    .nav-bar {
      position: fixed; bottom: 0; left: 0; right: 0; height: 65px;
      background: #101018; border-top: 1px solid var(--border-color);
      display: flex; justify-content: space-around; align-items: center; z-index: 100;
    }
    .nav-item {
      display: flex; flex-direction: column; align-items: center;
      color: var(--text-muted); font-size: 11px; cursor: pointer;
    }
    .nav-item.active { color: var(--accent-color); }
    .nav-icon { font-size: 20px; margin-bottom: 3px; }
  </style>
</head>
<body>

  <!-- SPLASH SCREEN -->
  <div id="splash-screen">
    <div class="contract-stage">
      <div class="fire-icon">🔥</div>
      
      <!-- 1. НАЗВА -->
      <h1 class="typewriter">Pact & Passion</h1>

      <!-- 2. ПЕЧАТКА ПОВЕРХ НАЗВИ -->
      <div class="stamp-overlay">
        <div class="seal-flash"></div>
        <div class="seal-body">
          <div class="seal-inner-ring"></div>
          <span class="seal-number">18+</span>
          <div class="seal-banner">RESTRICTED</div>
        </div>
      </div>

      <!-- 3. ІНДИКАТОР ЗАВАНТАЖЕННЯ -->
      <div class="loading-line-bg">
        <div class="loading-line-fill"></div>
      </div>
    </div>
  </div>

  <!-- ОСНОВНИЙ ІНТЕРФЕЙС -->
  <div class="header">
    <h1 class="title">
      Pact & Passion
      <span class="header-age">18+</span>
    </h1>
    <div class="user-welcome" id="user-greeting">Привіт, Гравцю!</div>
  </div>

  <div id="tab-pacts" class="tab-content active">
    <div class="card">
      <div class="stats-container">
        <div class="stat-item">
          <div class="val" id="points-val">1,250</div>
          <div class="lbl">Пристрасть</div>
        </div>
        <div class="stat-item">
          <div class="val" id="contracts-val">2</div>
          <div class="lbl">Активні Пакти</div>
        </div>
      </div>
    </div>

    <div class="card">
      <div style="font-weight: 600; margin-bottom: 12px;">Поточні пакти</div>
      <div class="contract-list">
        <div class="contract-item">
          <div class="contract-info">
            <div class="name">Ранковий ритуал</div>
            <div class="reward">+100 Пристрасті</div>
          </div>
          <button class="small-btn" onclick="completeTask(100, this)">Виконати</button>
        </div>
        <div class="contract-item">
          <div class="contract-info">
            <div class="name">Нічна сповідь</div>
            <div class="reward">+250 Пристрасті</div>
          </div>
          <button class="small-btn" onclick="completeTask(250, this)">Виконати</button>
        </div>
      </div>
    </div>
  </div>

  <div id="tab-leaderboard" class="tab-content">
    <div class="card">
      <div style="font-weight: 600; margin-bottom: 12px;">Топ Гравців</div>
      <div class="contract-list">
        <div class="contract-item">
          <div>1. Alex_VIP</div>
          <div style="color: var(--accent-color); font-weight: bold;">15,400</div>
        </div>
        <div class="contract-item">
          <div>2. Ви</div>
          <div style="color: var(--accent-color); font-weight: bold;" id="my-rank-val">1,250</div>
        </div>
      </div>
    </div>
  </div>

  <div id="tab-profile" class="tab-content">
    <div class="card" style="text-align: center;">
      <div style="font-size: 40px; margin-bottom: 10px;">🔥</div>
      <div style="font-size: 18px; font-weight: 700;">Рівень 1: Адепт</div>
      <div style="margin-top: 15px; font-size: 11px; color: var(--text-muted); border-top: 1px solid var(--border-color); padding-top: 10px;">
        Контент призначений строго для осіб старше 18 років.
      </div>
    </div>
  </div>

  <div class="nav-bar">
    <div class="nav-item active" onclick="switchTab('pacts', this)">
      <div class="nav-icon">📜</div>
      <div>Пакти</div>
    </div>
    <div class="nav-item" onclick="switchTab('leaderboard', this)">
      <div class="nav-icon">🏆</div>
      <div>Рейтинг</div>
    </div>
    <div class="nav-item" onclick="switchTab('profile', this)">
      <div class="nav-icon">👤</div>
      <div>Профіль</div>
    </div>
  </div>

  <script>
    const tg = window.Telegram.WebApp;
    if (tg) {
      tg.expand();
      if (tg.initDataUnsafe && tg.initDataUnsafe.user) {
        document.getElementById('user-greeting').innerText = `Гравець: ${tg.initDataUnsafe.user.first_name}`;
      }
    }

    // Вібровідгук рівно в момент удару печатки (1.5 сек)
    setTimeout(() => {
      if (tg && tg.HapticFeedback) {
        tg.HapticFeedback.notificationOccurred('warning');
      }
    }, 1500);

    // Приховування заставки через 5 секунд
    setTimeout(() => {
      const splash = document.getElementById('splash-screen');
      if (splash) splash.classList.add('hidden');
    }, 5000);

    let points = 1250;

    function completeTask(reward, btn) {
      points += reward;
      document.getElementById('points-val').innerText = points.toLocaleString();
      btn.innerText = "Готово ✓";
      btn.disabled = true;
      btn.style.opacity = "0.5";
      btn.style.borderColor = "#4ccc54";
      btn.style.color = "#4ccc54";
      if (tg && tg.HapticFeedback) tg.HapticFeedback.impactOccurred('medium');
    }

    function switchTab(tabName, el) {
      document.querySelectorAll('.tab-content').forEach(tab => tab.classList.remove('active'));
      document.querySelectorAll('.nav-item').forEach(item => item.classList.remove('active'));
      
      document.getElementById('tab-' + tabName).classList.add('active');
      el.classList.add('active');
      if (tg && tg.HapticFeedback) tg.HapticFeedback.selectionChanged();
    }
  </script>
</body>
</html>
