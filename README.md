<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Xayrland - Донат</title>
  <link href="https://fonts.googleapis.com/css2?family=Rubik:wght@400;600&display=swap" rel="stylesheet">
  <style>
    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
    }

    body {
      background-color: #0e0e0e;
      color: #e6ffe6;
      font-family: 'Rubik', sans-serif;
    }

    header {
      background: linear-gradient(90deg, #1b3b1b, #0f2f0f);
      padding: 1.5rem;
      text-align: center;
      font-size: 2.5rem;
      color: #00ff88;
      font-weight: 600;
      box-shadow: 0 4px 10px rgba(0, 255, 100, 0.2);
    }

    .container {
      max-width: 1200px;
      margin: 3rem auto;
      padding: 2rem;
      background-color: #141414;
      border-radius: 12px;
      box-shadow: 0 0 20px rgba(0, 255, 120, 0.1);
    }

    .section {
      margin-bottom: 2rem;
      background-color: #1c1c1c;
      padding: 2rem;
      border-radius: 10px;
      box-shadow: 0 0 10px rgba(0, 255, 140, 0.05);
    }

    label {
      display: block;
      margin-bottom: 0.5rem;
      font-weight: 600;
      color: #9fff9f;
    }

    select, button, input[type="text"] {
      width: 100%;
      padding: 0.75rem;
      margin-top: 0.5rem;
      margin-bottom: 1rem;
      border: none;
      border-radius: 8px;
      background-color: #203820;
      color: #b6ffb6;
      font-size: 1rem;
    }

    button {
      background-color: #00cc66;
      font-weight: bold;
      transition: background-color 0.3s ease;
    }

    button:hover {
      background-color: #00e676;
    }

    h3 {
      margin-bottom: 1rem;
      color: #66ffb2;
    }

    .price-display {
      font-size: 1.2rem;
      color: #aaffaa;
      margin-bottom: 1rem;
    }

    .instructions {
      color: #b0ffb0;
      margin-top: 1rem;
      font-size: 0.95rem;
    }

    .summary-box {
      background-color: #1f2f1f;
      border: 1px solid #00cc66;
      padding: 1rem;
      border-radius: 8px;
      margin-top: 1rem;
      color: #e0ffe0;
    }
  </style>
</head>
<body>
  <header>Xayrland | Донат Магазин</header>
  <div class="container">
    <div class="section">
      <label for="privilege">Выберите привилегию:</label>
      <select id="privilege" onchange="updatePrice()">
        <option value="ANARCHIST">ANARCHIST</option>
        <option value="DOMINION">DOMINION</option>
        <option value="WARLORD">WARLORD</option>
        <option value="RAVAGER">RAVAGER</option>
        <option value="PHANTOM">PHANTOM</option>
        <option value="KRAKEN">KRAKEN</option>
        <option value="ELITE">ELITE</option>
        <option value="PHOENIX">PHOENIX</option>
      </select>

      <label for="duration">Выберите срок:</label>
      <select id="duration" onchange="updatePrice()">
        <option value="1">1 месяц</option>
        <option value="3">3 месяца</option>
        <option value="forever">Навсегда</option>
      </select>

      <label for="nickname">Ваш ник на сервере:</label>
      <input type="text" id="nickname" placeholder="Введите ваш ник" required>

      <label for="payment">Метод оплаты:</label>
      <select id="payment">
        <option value="donationalerts">Donation Alerts</option>
        <option value="ukrcard">Карта (Украина)</option>
      </select>

      <div class="price-display" id="priceDisplay">Цена: —</div>

      <button onclick="buyNow()">Перейти к оплате</button>

      <div class="instructions">
        Перед оплатой обязательно введите ваш ник. После оплаты донат будет выдан в течение 5 минут. При проблемах — обращайтесь: @xayrllano
      </div>

      <div id="summary" class="summary-box" style="display:none;"></div>
    </div>

    <div class="section">
      <h3>Купить донат-кейсы:</h3>
      <select id="cases">
        <option value="1">1 Кейc — 29 Руб</option>
        <option value="3">3 Кейса — 64 Руб</option>
        <option value="5">5 Кeйсов — 120 Руб (+5 Хаиров)</option>
        <option value="10">10+ Кeйсов — 200 Руб (+25 Хаиров)</option>
      </select>
      <button onclick="buyCases()">Купить кейсы</button>
    </div>
  </div>

  <script>
    const priceTable = {
      ANARCHIST: { "1": 7, "3": 10, "forever": 14 },
      DOMINION: { "1": 21, "3": 31, "forever": 42 },
      WARLORD: { "1": 44, "3": 66, "forever": 88 },
      RAVAGER: { "1": 54, "3": 78, "forever": 104 },
      PHANTOM: { "1": 67, "3": 100, "forever": 134 },
      KRAKEN: { "1": 72, "3": 108, "forever": 144 },
      ELITE: { "1": 85, "3": 127, "forever": 170 },
      PHOENIX: { "1": 94, "3": 141, "forever": 188 },
    };

    function updatePrice() {
      const privilege = document.getElementById('privilege').value;
      const duration = document.getElementById('duration').value;
      const price = priceTable[privilege][duration];
      document.getElementById('priceDisplay').textContent = `Цена: ${price} руб.`;
    }

    function buyNow() {
      const payment = document.getElementById('payment').value;
      const privilege = document.getElementById('privilege').value;
      const duration = document.getElementById('duration').value;
      const nickname = document.getElementById('nickname').value.trim();
      const price = priceTable[privilege][duration];

      if (!nickname) {
        alert('Пожалуйста, введите ваш ник на сервере.');
        return;
      }

      const durationText = duration === 'forever' ? 'Навсегда' : `${duration} месяц(а)`;
      const description = `${nickname} - ${privilege} - ${durationText}`;

      const summaryBox = document.getElementById('summary');
      summaryBox.innerHTML = `
        <strong>Скопируйте и вставьте это в поле "Описание доната":</strong><br><br>
        ${description}<br><br>
        <strong>Укажите цену:</strong> ${price} руб.<br><br>
        <em>Если возникли проблемы — пишите в Telegram: @andrei_capbed</em>
      `;
      summaryBox.style.display = 'block';

      if (payment === 'donationalerts') {
        navigator.clipboard.writeText(description).then(() => {
          alert('Описание скопировано. Вставьте его в поле "Комментарий" на Donation Alerts.');
          window.open('https://www.donationalerts.com/r/xayrllano', '_blank');
        }).catch(() => {
          alert(`Скопируйте вручную:\n${description}`);
          window.open('https://www.donationalerts.com/r/xayrllano', '_blank');
        });
      } else {
        alert('Для оплаты картой (Украина) свяжитесь с админом в Telegram: @xayrllano');
      }
    }

    function buyCases() {
      window.open('https://www.donationalerts.com/r/xayrllano', '_blank');
    }
  </script>
</body>
</html>
