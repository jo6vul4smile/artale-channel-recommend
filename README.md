<!DOCTYPE html>
<html lang="zh-Hant">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>頻道推薦系統</title>
  <style>
    body {
      font-family: "Microsoft JhengHei", sans-serif;
      text-align: center;
      padding: 2rem;
      background-color: #f8f9fa;
    }
    input {
      padding: 0.5rem;
      font-size: 1rem;
      margin: 0.5rem;
    }
    button {
      padding: 0.5rem 1rem;
      font-size: 1rem;
      background-color: #007bff;
      color: white;
      border: none;
      border-radius: 4px;
      cursor: pointer;
    }
    button:hover {
      background-color: #0056b3;
    }
    #result {
      margin-top: 2rem;
      padding: 1rem;
      background: white;
      border: 1px solid #ccc;
      border-radius: 6px;
      width: 90%;
      max-width: 400px;
      margin-left: auto;
      margin-right: auto;
      font-size: 1.2rem;
      color: #333;
      line-height: 1.6;
    }
  </style>
</head>
<body>
  <h1>🎯 ARTALE 頻道推薦器</h1>
  <p>輸入你的出生年月日，即可獲得今日幸運頻道</p>
  <input type="date" id="birthdate" />
  <button onclick="recommendChannel()">獲得推薦</button>
  <div id="result"></div>

  <script>
    function getZodiac(year) {
      const animals = ["猴", "雞", "狗", "豬", "鼠", "牛", "虎", "兔", "龍", "蛇", "馬", "羊"];
      return animals[year % 12];
    }

    function getHoroscope(month, day) {
      const signs = [
        ["摩羯", 19], ["水瓶", 18], ["雙魚", 20], ["牡羊", 19], ["金牛", 20],
        ["雙子", 20], ["巨蟹", 22], ["獅子", 22], ["處女", 22], ["天秤", 23],
        ["天蠍", 22], ["射手", 21], ["摩羯", 31]
      ];
      return day <= signs[month - 1][1] ? signs[month - 1][0] : signs[month][0];
    }

    function recommendChannel() {
      const birth = document.getElementById("birthdate").value;
      const date = new Date(birth);
      if (isNaN(date.getTime())) {
        document.getElementById("result").innerText = "❗ 請正確輸入生日 (格式: YYYY-MM-DD)";
        return;
      }

      const today = new Date();
      const year = date.getFullYear();
      const month = date.getMonth() + 1;
      const day = date.getDate();

      const zodiac = getZodiac(year);
      const horoscope = getHoroscope(month, day);

      const base = (year + month + day + today.getDate()) % 3000;
      const mainChannel = (base + 729) % 3000;
      const alt1 = (base + 229) % 3000;
      const alt2 = (base + 909) % 3000;

      const mainStr = mainChannel.toString().padStart(3, "0");
      const alt1Str = alt1.toString().padStart(3, "0");
      const alt2Str = alt2.toString().padStart(3, "0");

      const tailOptions = ["09", "29", "39", "00", "99"];

      document.getElementById("result").innerHTML = `
        🔮 <b>生肖：</b>${zodiac}<br>
        🌌 <b>星座：</b>${horoscope}<br><br>
        📡 <b>今日推薦頻道號碼：</b><br>
        ✅ 主推：<b style="color:#007bff; font-size:1.4rem">${mainStr}</b><br>
        ✨ 次選：${alt1Str}、${alt2Str}<br><br>
        🔢 <b>推薦尾號：</b>${tailOptions.map(n => `<code>${n}</code>`).join("、")}
      `;
    }
  </script>
</body>
</html>
