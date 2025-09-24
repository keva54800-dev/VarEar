<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>VarEar — Demo (Acode)</title>
  <style>
    body {
      margin: 0;
      font-family: Arial, sans-serif;
      background: #0d1117;
      color: #fff;
      display: flex;
      flex-direction: column;
      align-items: center;
      padding: 20px;
    }
    h1 {
      color: #4ade80;
    }
    .nav {
      margin: 15px 0;
    }
    .nav button {
      background: #22c55e;
      border: none;
      color: white;
      padding: 8px 16px;
      margin: 0 5px;
      border-radius: 8px;
      cursor: pointer;
    }
    .card {
      background: #161b22;
      padding: 20px;
      border-radius: 12px;
      box-shadow: 0 4px 12px rgba(0,0,0,0.3);
      max-width: 400px;
      width: 100%;
      margin: 15px 0;
    }
    canvas {
      background: #0d1117;
      border-radius: 8px;
      display: block;
      margin: 0 auto 15px;
    }
    input {
      display: block;
      width: 100%;
      padding: 8px;
      margin: 8px 0;
      border-radius: 6px;
      border: 1px solid #333;
      background: #0d1117;
      color: white;
    }
    .instructions {
      font-size: 14px;
      color: #ccc;
    }
    .link-btn {
      display: block;
      text-align: center;
      text-decoration: none;
      background: #2563eb;
      color: #fff;
      padding: 10px;
      margin: 8px 0;
      border-radius: 8px;
    }
  </style>
</head>
<body>
  <h1>VarEar — Demo</h1>
  <div class="nav">
    <button onclick="showSection('preview')">Превью</button>
    <button onclick="showSection('register')">Регистрация</button>
    <button onclick="showSection('profile')">Профиль</button>
    <button onclick="showSection('links')">Ссылки</button>
  </div>

  <!-- Превью -->
  <div class="card" id="preview">
    <h2>Превью скина</h2>
    <canvas id="skinCanvas" width="250" height="250"></canvas>
    <button onclick="makeGreen()">Сделать зелёного</button>
    <button onclick="downloadPreview()">Скачать превью</button>
    <input type="file" accept="image/*" onchange="uploadImage(event)">
    <p class="instructions">Размер превью: 250px</p>
  </div>

  <!-- Регистрация -->
  <div class="card" id="register" style="display:none;">
    <h2>Регистрация</h2>
    <input type="text" id="username" placeholder="Имя пользователя">
    <input type="password" id="password" placeholder="Пароль">
    <button onclick="register()">Зарегистрироваться</button>
    <p class="instructions">Данные сохраняются в localStorage</p>
  </div>

  <!-- Профиль -->
  <div class="card" id="profile" style="display:none;">
    <h2>Профиль</h2>
    <p id="profileInfo">Вы не вошли</p>
    <button onclick="logout()">Выйти</button>
  </div>

  <!-- Ссылки -->
  <div class="card" id="links" style="display:none;">
    <h2>Ссылки</h2>
    <a class="link-btn" href="https://add.aternos.org/WarEar" target="_blank">Зайти на сервер</a>
    <a class="link-btn" href="https://t.me/WarEar1" target="_blank">Телеграм</a>
    <h3>Адреса</h3>
    <p><b>WarEar.aternos.me</b><br>Порт: <b>64093</b></p>
  </div>

  <script>
    const canvas = document.getElementById("skinCanvas");
    const ctx = canvas.getContext("2d");

    function drawCharacter(color="#22c55e") {
      ctx.fillStyle = "#0d1117";
      ctx.fillRect(0, 0, canvas.width, canvas.height);
      ctx.fillStyle = color;
      ctx.fillRect(90, 100, 70, 100);
      ctx.beginPath();
      ctx.arc(125, 70, 35, 0, Math.PI*2);
      ctx.fill();
      ctx.fillStyle = "#000";
      ctx.beginPath();
      ctx.arc(115, 70, 5, 0, Math.PI*2);
      ctx.arc(135, 70, 5, 0, Math.PI*2);
      ctx.fill();
    }

    function makeGreen() { drawCharacter("#22c55e"); }

    function uploadImage(event) {
      const file = event.target.files[0];
      const reader = new FileReader();
      reader.onload = function(e) {
        const img = new Image();
        img.onload = function() {
          ctx.drawImage(img, 0, 0, canvas.width, canvas.height);
        };
        img.src = e.target.result;
      };
      reader.readAsDataURL(file);
    }

    function downloadPreview() {
      const link = document.createElement("a");
      link.download = "preview.png";
      link.href = canvas.toDataURL();
      link.click();
    }

    function showSection(id) {
      ["preview","register","profile","links"].forEach(sec => {
        document.getElementById(sec).style.display = "none";
      });
      document.getElementById(id).style.display = "block";
    }

    function register() {
      const username = document.getElementById("username").value;
      const password = document.getElementById("password").value;
      if (!username || !password) {
        alert("Заполните все поля!");
        return;
      }
      localStorage.setItem("user", JSON.stringify({username, password}));
      alert("Регистрация успешна!");
      showSection("profile");
      loadProfile();
    }

    function loadProfile() {
      const user = JSON.parse(localStorage.getItem("user"));
      if (user) {
        document.getElementById("profileInfo").innerText = 
          "Привет, " + user.username + "!";
      }
    }

    function logout() {
      localStorage.removeItem("user");
      document.getElementById("profileInfo").innerText = "Вы не вошли";
    }

    drawCharacter();
    loadProfile();
  </script>
</body>
</html>
