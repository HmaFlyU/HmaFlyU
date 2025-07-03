<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

</body>
</html>
<!DOCTYPE html>
<html lang="tr">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1, user-scalable=no" />
<title>Mobil Top Kaçış Oyunu - Özelleştirilebilir</title>
<style>
  html, body {
    margin: 0; padding: 0; height: 100%; overflow: hidden; background: #111;
    display: flex; justify-content: center; align-items: center;
    user-select: none;
    font-family: sans-serif;
  }
  #gameContainer {
    position: relative;
    width: 360px;
    height: 600px;
    background: #222;
    border-radius: 12px;
    overflow: hidden;
    box-shadow: 0 0 20px rgba(0,0,0,0.8);
  }
  canvas {
    display: block;
    background: linear-gradient(180deg, #1a1a1a 0%, #000000 100%);
  }
  #leftBtn, #rightBtn {
    position: absolute;
    bottom: 20px;
    width: 80px;
    height: 80px;
    background: rgba(255,255,255,0.15);
    border-radius: 50%;
    font-size: 40px;
    color: white;
    text-align: center;
    line-height: 80px;
    user-select: none;
    z-index: 5;
  }
  #leftBtn { left: 20px; }
  #rightBtn { right: 20px; }

  #scoreDisplay {
    position: absolute;
    top: 10px;
    left: 10px;
    color: white;
    font-size: 22px;
    user-select: none;
    z-index: 5;
    text-shadow: 0 0 8px lime;
  }
  #moneyDisplay {
    position: absolute;
    top: 40px;
    left: 10px;
    color: gold;
    font-size: 20px;
    user-select: none;
    z-index: 5;
    text-shadow: 0 0 6px gold;
  }
  #restartBtn {
    position: absolute;
    top: 10px;
    right: 10px;
    padding: 8px 14px;
    font-size: 16px;
    border-radius: 6px;
    border: none;
    background: #007bff;
    color: white;
    cursor: pointer;
    display: none;
    z-index: 5;
    transition: background 0.3s ease;
  }
  #restartBtn:active {
    background: #0056b3;
  }

  #menuBackground {
    display: none;
    position: absolute;
    top: 0; left: 0; right: 0; bottom: 0;
    background: rgba(0,0,0,0.6);
    z-index: 19;
    border-radius: 12px;
  }

  #menuOverlay {
    display: none;
    position: absolute;
    top: 50%;
    left: 50%;
    width: 360px;
    max-height: 90vh;
    overflow-y: auto;
    background: #333;
    color: white;
    padding: 20px 25px;
    border-radius: 12px;
    box-shadow: 0 0 15px #000;
    z-index: 20;
    transform: translate(-50%, -50%);
    user-select: none;
  }
  #menuOverlay h2 {
    margin: 0 0 15px 0;
    font-size: 22px;
    text-align: center;
  }
  #menuOverlay label {
    font-size: 18px;
    display: block;
    margin-top: 15px;
  }
  #menuOverlay select, #menuOverlay input[type=range] {
    width: 100%;
    margin-top: 5px;
    font-size: 16px;
    padding: 5px;
    border-radius: 6px;
    border: none;
  }
  #menuOverlay input[type=checkbox] {
    margin-top: 10px;
    transform: scale(1.3);
    cursor: pointer;
  }
  #menuOverlay button {
    margin-top: 15px;
    padding: 12px 0;
    font-size: 18px;
    border-radius: 8px;
    border: none;
    cursor: pointer;
    width: 100%;
    background: #007bff;
    color: white;
    transition: background 0.3s ease;
  }
  #menuOverlay button:hover {
    background: #0056b3;
  }
  #menuOverlay button:active {
    background: #003d80;
  }
  #storeBtn {
    position: absolute;
    top: 70px;
    right: 10px;
    padding: 6px 10px;
    font-size: 14px;
    border-radius: 6px;
    border: none;
    background: #28a745;
    color: white;
    cursor: pointer;
    z-index: 5;
  }
  #storeBtn:active {
    background: #1e7e34;
  }

  /* Mağaza stili */
  #storeOverlay {
    display: none;
    position: absolute;
    top: 50%;
    left: 50%;
    width: 360px;
    max-height: 90vh;
    overflow-y: auto;
    background: #222;
    color: white;
    padding: 20px 25px;
    border-radius: 12px;
    box-shadow: 0 0 15px #000;
    z-index: 21;
    transform: translate(-50%, -50%);
    user-select: none;
  }
  #storeOverlay h2 {
    margin: 0 0 15px 0;
    font-size: 22px;
    text-align: center;
  }
  #storeOverlay button {
    margin-top: 15px;
    padding: 10px 0;
    font-size: 16px;
    border-radius: 8px;
    border: none;
    cursor: pointer;
    width: 100%;
    background: #17a2b8;
    color: white;
    transition: background 0.3s ease;
  }
  #storeOverlay button:hover {
    background: #117a8b;
  }
  #storeOverlay button:disabled {
    background: #444;
    cursor: default;
  }
  #storeOverlay #storeMenuBtn {
    background: #007bff;
  }
</style>
</head>
<body>

<div id="gameContainer">
  <canvas id="gameCanvas" width="360" height="600"></canvas>
  <div id="scoreDisplay">Puan: 0</div>
  <div id="moneyDisplay">Para: 0</div>
  <button id="restartBtn">Yeniden Başlat</button>
  <button id="storeBtn">Mağaza</button>
  <div id="leftBtn">&#8592;</div>
  <div id="rightBtn">&#8594;</div>

  <div id="menuBackground"></div>
  <div id="menuOverlay">
    <h2>Menü</h2>

    <label for="difficultySelect">Zorluk Seçimi</label>
    <select id="difficultySelect">
      <option value="easy">Kolay</option>
      <option value="medium" selected>Orta</option>
      <option value="hard">Zor</option>
    </select>

    <label for="volumeRange">Ses Seviyesi</label>
    <input type="range" id="volumeRange" min="0" max="1" step="0.01" value="0.3" />

    <label for="gyroToggle">Jiroskop Kontrolü</label>
    <input type="checkbox" id="gyroToggle" />

    <button id="resumeBtn">Oyuna Dön</button>
    <button id="exitBtn">Oyundan Çık</button>
  </div>

  <div id="storeOverlay">
    <h2>Mağaza</h2>

    <button id="buyShieldBtn">Kalkan Satın Al (50 Lira)</button>
    <div style="margin-top:10px; font-size: 16px; color: #ccc;">
      <strong>Kalan Kalkan:</strong> <span id="shieldCount">0</span><br />
      <small>Kalkan oyunda 1 kere kullanılır, sonra biter.</small>
    </div>

    <h3 style="margin-top:20px;">Top Renkleri (Her biri 100 Lira)</h3>
    <button class="buyColorBtn" data-color="lime" style="background: lime; color: black;">Yeşil</button>
    <button class="buyColorBtn" data-color="deepskyblue" style="background: deepskyblue; color: black;">Mavi</button>
    <button class="buyColorBtn" data-color="orange" style="background: orange; color: black;">Turuncu</button>
    <button class="buyColorBtn" data-color="violet" style="background: violet; color: black;">Mor</button>
    <button class="buyColorBtn" data-color="crimson" style="background: crimson; color: black;">Kırmızı</button>

    <button id="storeMenuBtn" style="margin-top: 20px;">Menüye Dön</button>
    <button id="closeStoreBtn" style="margin-top: 10px;">Mağazayı Kapat</button>
  </div>
</div>

<script>
  const canvas = document.getElementById('gameCanvas');
  const ctx = canvas.getContext('2d');

  const width = canvas.width;
  const height = canvas.height;

  const ball = {
    x: width / 2,
    y: height - 50,
    radius: 20,
    speedX: 7
  };

  let obstacles = [];
  const obstacleWidth = 60;
  const obstacleHeight = 20;

  const difficulties = {
    easy: {
      obstacleSpeedBase: 3,
      obstacleSpawnRateBase: 0.015,
      ballSpeed: 6,
    },
    medium: {
      obstacleSpeedBase: 4,
      obstacleSpawnRateBase: 0.025,
      ballSpeed: 7,
    },
    hard: {
      obstacleSpeedBase: 5,
      obstacleSpawnRateBase: 0.035,
      ballSpeed: 8,
    }
  };

  let currentDifficulty = 'medium';
  let obstacleSpeed;
  let obstacleCreationProbability;
  let ballSpeed;

  let leftPressed = false;
  let rightPressed = false;
  let score = 0;
  let money = 0;
  let gameOver = false;

  let shieldCount = 0;
  let shieldActive = false;
  let shieldUsedThisGame = false;

  const scoreDisplay = document.getElementById('scoreDisplay');
  const moneyDisplay = document.getElementById('moneyDisplay');
  const restartBtn = document.getElementById('restartBtn');
  const leftBtn = document.getElementById('leftBtn');
  const rightBtn = document.getElementById('rightBtn');
  const storeBtn = document.getElementById('storeBtn');

  const menuOverlay = document.getElementById('menuOverlay');
  const menuBackground = document.getElementById('menuBackground');
  const volumeRange = document.getElementById('volumeRange');
  const resumeBtn = document.getElementById('resumeBtn');
  const exitBtn = document.getElementById('exitBtn');
  const difficultySelect = document.getElementById('difficultySelect');
  const gyroToggle = document.getElementById('gyroToggle');

  const storeOverlay = document.getElementById('storeOverlay');
  const buyShieldBtn = document.getElementById('buyShieldBtn');
  const shieldCountSpan = document.getElementById('shieldCount');
  const buyColorBtns = document.querySelectorAll('.buyColorBtn');
  const closeStoreBtn = document.getElementById('closeStoreBtn');
  const storeMenuBtn = document.getElementById('storeMenuBtn');

  const AudioContext = window.AudioContext || window.webkitAudioContext;
  const audioCtx = new AudioContext();
  let beepGain = audioCtx.createGain();
  beepGain.gain.value = volumeRange.value;
  beepGain.connect(audioCtx.destination);

  let ballColor = 'lime';

  // Oyuncunun sahip olduğu renkler (başlangıçta sadece lime var)
  let ownedColors = ['lime'];

  let useGyro = false;
  let gyroX = 0;

  function playBeep() {
    if(audioCtx.state === 'suspended') audioCtx.resume();
    const osc = audioCtx.createOscillator();
    osc.type = 'square';
    osc.frequency.setValueAtTime(800, audioCtx.currentTime);
    osc.connect(beepGain);
    osc.start();
    osc.stop(audioCtx.currentTime + 0.1);
  }

  function createObstacle(){
    const x = Math.random() * (width - obstacleWidth);
    obstacles.push({x: x, y: -obstacleHeight, width: obstacleWidth, height: obstacleHeight});
  }

  function saveScore(newScore){
    let scores = JSON.parse(localStorage.getItem('topScores')) || [];
    scores.push(newScore);
    scores.sort((a,b) => b - a);
    if(scores.length > 5) scores = scores.slice(0,5);
    localStorage.setItem('topScores', JSON.stringify(scores));
  }

  function loadScores(){
    let scores = JSON.parse(localStorage.getItem('topScores')) || [];
    // Skorları göstermek için ek bir alan yoksa bu kısmı atlayabiliriz
  }

  function clearScores(){
    if(confirm("Skorları temizlemek istediğine emin misin?")){
      localStorage.removeItem('topScores');
      loadScores();
    }
  }

  // Mağaza fonksiyonları
  function updateShieldDisplay(){
    shieldCountSpan.textContent = shieldCount;
  }

  function buyShield(){
    if(money >= 50){
      money -= 50;
      shieldCount++;
      updateShieldDisplay();
      moneyDisplay.textContent = "Para: " + money;
      alert("Kalkan satın alındı!");
    } else {
      alert("Yeterli paran yok!");
    }
  }

  function buyColor(color){
    if(ownedColors.includes(color)){
      alert("Bu renk zaten sende var!");
      return;
    }
    if(money >= 100){
      money -= 100;
      ownedColors.push(color);
      alert(color + " rengi satın alındı!");
      moneyDisplay.textContent = "Para: " + money;
      updateColorButtons();
    } else {
      alert("Yeterli paran yok!");
    }
  }

  function updateColorButtons(){
    buyColorBtns.forEach(btn => {
      const color = btn.getAttribute('data-color');
      if(ownedColors.includes(color)){
        btn.disabled = true;
        btn.textContent = color.charAt(0).toUpperCase() + color.slice(1) + " (Alındı)";
        btn.style.backgroundColor = color;
        btn.style.color = (color === 'violet' || color === 'crimson') ? 'white' : 'black';
      } else {
        btn.disabled = false;
        btn.textContent = color.charAt(0).toUpperCase() + color.slice(1) + " (100 Lira)";
        btn.style.backgroundColor = color;
        btn.style.color = (color === 'violet' || color === 'crimson') ? 'white' : 'black';
      }
    });
  }

  function updateDifficultyParameters(){
    const diff = difficulties[currentDifficulty];
    obstacleSpeed = diff.obstacleSpeedBase;
    obstacleCreationProbability = diff.obstacleSpawnRateBase;
    ball.speedX = diff.ballSpeed;
  }

  function resetGame(){
    if(gameOver && score > 0){
      saveScore(score);
      loadScores();
    }
    obstacles = [];
    ball.x = width / 2;
    score = 0;
    // Para sıfırlanmaz
    gameOver = false;
    shieldActive = false;
    shieldUsedThisGame = false;
    updateDifficultyParameters();
    scoreDisplay.textContent = "Puan: 0";
    moneyDisplay.textContent = "Para: " + money;
    restartBtn.style.display = 'none';
    if(menuOverlay.style.display === 'block'){
      toggleMenu();
    }
  }

  // Patlama efekti için değişkenler
  let explosion = null;

  function drawExplosion(){
    if(!explosion) return;
    const progress = (Date.now() - explosion.startTime) / explosion.duration;
    if(progress > 1){
      explosion = null;
      return;
    }
    const alpha = 1 - progress;
    const maxRadius = 50;
    const radius = maxRadius * progress;
    const gradient = ctx.createRadialGradient(explosion.x, explosion.y, 0, explosion.x, explosion.y, radius);
    gradient.addColorStop(0, `rgba(255, 69, 0, ${alpha})`);
    gradient.addColorStop(0.5, `rgba(255, 140, 0, ${alpha * 0.6})`);
    gradient.addColorStop(1, `rgba(255, 140, 0, 0)`);
    ctx.fillStyle = gradient;
    ctx.beginPath();
    ctx.arc(explosion.x, explosion.y, radius, 0, Math.PI*2);
    ctx.fill();
  }

  function handleOrientation(event) {
    gyroX = event.gamma || 0;
  }

  function update(){
    if(gameOver) return;
    if(menuOverlay.style.display === 'block') return;
    if(storeOverlay.style.display === 'block') return;

    if(useGyro){
      // gamma -90 ... +90, normalize -1 ... +1
      let normX = gyroX / 45;
      if(normX > 1) normX = 1;
      if(normX < -1) normX = -1;
      ball.x += normX * ball.speedX;
    } else {
      if(leftPressed && ball.x - ball.radius > 0){
        ball.x -= ball.speedX;
      }
      if(rightPressed && ball.x + ball.radius < width){
        ball.x += ball.speedX;
      }
    }

    // top sınırları aşmasın
    if(ball.x - ball.radius < 0) ball.x = ball.radius;
    if(ball.x + ball.radius > width) ball.x = width - ball.radius;

    // Engel oluşturma
    if(Math.random() < obstacleCreationProbability){
      createObstacle();
    }

    // Engel hareketi ve çarpışma kontrolü
    for(let i = obstacles.length -1; i>=0; i--){
      obstacles[i].y += obstacleSpeed;

      // Çarpışma kontrolü (top ile engel)
      if(
        ball.x + ball.radius > obstacles[i].x &&
        ball.x - ball.radius < obstacles[i].x + obstacles[i].width &&
        ball.y + ball.radius > obstacles[i].y &&
        ball.y - ball.radius < obstacles[i].y + obstacles[i].height
      ){
        if(shieldCount > 0 && !shieldUsedThisGame){
          shieldUsedThisGame = true;
          shieldCount--;
          updateShieldDisplay();
          playBeep();
          explosion = {x: ball.x, y: ball.y, startTime: Date.now(), duration: 400};
          obstacles.splice(i,1);
          continue;
        } else {
          gameOver = true;
          restartBtn.style.display = 'block';
          return;
        }
      }

      if(obstacles[i].y > height){
        obstacles.splice(i,1);
        score += 2; // 1 framede 2 puan arttırdık
        money += 1; // 1 puan = 2 para istedin, puanı 2 artırınca burası 1 olarak bırakıldı.
        scoreDisplay.textContent = "Puan: " + score;
        moneyDisplay.textContent = "Para: " + money;
      }
    }

    draw();
    drawExplosion();
  }

  function draw(){
    ctx.clearRect(0,0,width,height);

    // Top
    ctx.fillStyle = ballColor;
    ctx.beginPath();
    ctx.arc(ball.x, ball.y, ball.radius, 0, Math.PI * 2);
    ctx.fill();

    // Engeller
    ctx.fillStyle = "tomato";
    obstacles.forEach(obs => {
      ctx.fillRect(obs.x, obs.y, obs.width, obs.height);
    });

    // Kalkan varsa top etrafına çiz
    if(shieldCount > 0 && !shieldUsedThisGame){
      ctx.strokeStyle = 'cyan';
      ctx.lineWidth = 4;
      ctx.beginPath();
      ctx.arc(ball.x, ball.y, ball.radius + 8, 0, Math.PI * 2);
      ctx.stroke();
    }
  }

  function toggleMenu(){
    if(menuOverlay.style.display === 'block'){
      menuOverlay.style.display = 'none';
      menuBackground.style.display = 'none';
      if(useGyro){
        window.addEventListener('deviceorientation', handleOrientation);
      }
      restartBtn.style.display = gameOver ? 'block' : 'none';
    } else {
      menuOverlay.style.display = 'block';
      menuBackground.style.display = 'block';
      window.removeEventListener('deviceorientation', handleOrientation);
      restartBtn.style.display = 'none';
    }
  }

  function toggleStore(){
    if(storeOverlay.style.display === 'block'){
      storeOverlay.style.display = 'none';
      menuOverlay.style.display = 'block';
    } else {
      storeOverlay.style.display = 'block';
      menuOverlay.style.display = 'none';
    }
  }

  // Event Listeners
  leftBtn.addEventListener('touchstart', e => {e.preventDefault(); leftPressed = true;});
  leftBtn.addEventListener('touchend', e => {e.preventDefault(); leftPressed = false;});
  rightBtn.addEventListener('touchstart', e => {e.preventDefault(); rightPressed = true;});
  rightBtn.addEventListener('touchend', e => {e.preventDefault(); rightPressed = false;});

  window.addEventListener('keydown', e => {
    if(e.key === 'ArrowLeft') leftPressed = true;
    if(e.key === 'ArrowRight') rightPressed = true;
    if(e.key === 'Escape') toggleMenu();
  });
  window.addEventListener('keyup', e => {
    if(e.key === 'ArrowLeft') leftPressed = false;
    if(e.key === 'ArrowRight') rightPressed = false;
  });

  restartBtn.addEventListener('click', () => {
    resetGame();
  });

  storeBtn.addEventListener('click', () => {
    toggleStore();
  });

  resumeBtn.addEventListener('click', () => {
    toggleMenu();
  });

  exitBtn.addEventListener('click', () => {
    if(confirm("Oyundan çıkmak istediğine emin misin?")){
      window.close();
    }
  });

  difficultySelect.addEventListener('change', () => {
    currentDifficulty = difficultySelect.value;
    updateDifficultyParameters();
  });

  volumeRange.addEventListener('input', () => {
    beepGain.gain.value = volumeRange.value;
  });

  gyroToggle.addEventListener('change', async () => {
    if(gyroToggle.checked){
      // İzin talebi (iOS 13+ için)
      if (typeof DeviceOrientationEvent !== 'undefined' && typeof DeviceOrientationEvent.requestPermission === 'function') {
        try {
          const response = await DeviceOrientationEvent.requestPermission();
          if(response === 'granted'){
            useGyro = true;
            window.addEventListener('deviceorientation', handleOrientation);
          } else {
            alert("Jiroskop izni verilmedi.");
            gyroToggle.checked = false;
            useGyro = false;
          }
        } catch(e){
          alert("Jiroskop izni alınamadı.");
          gyroToggle.checked = false;
          useGyro = false;
        }
      } else {
        useGyro = true;
        window.addEventListener('deviceorientation', handleOrientation);
      }
    } else {
      useGyro = false;
      window.removeEventListener('deviceorientation', handleOrientation);
    }
  });

  buyShieldBtn.addEventListener('click', buyShield);

  buyColorBtns.forEach(btn => {
    btn.addEventListener('click', () => {
      buyColor(btn.getAttribute('data-color'));
    });
  });

  closeStoreBtn.addEventListener('click', () => {
    storeOverlay.style.display = 'none';
    menuOverlay.style.display = 'block';
  });

  storeMenuBtn.addEventListener('click', () => {
    storeOverlay.style.display = 'none';
    menuOverlay.style.display = 'block';
  });

  // Top rengini değiştirmek için tuşa uzun basma veya benzeri mekanizma yok.
  // Dilersen mağazada veya menüde seçeneği de ekleyebiliriz.

  // Otomatik animasyon
  function gameLoop(){
    update();
    requestAnimationFrame(gameLoop);
  }

  updateDifficultyParameters();
  updateShieldDisplay();
  updateColorButtons();
  resetGame();
  gameLoop();

</script>
</body>
</html>
