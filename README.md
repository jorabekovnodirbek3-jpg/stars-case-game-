# stars-case-game-.
<!DOCTYPE html>
<html lang="uz">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Stars Casino</title>
    <script src="https://telegram.org/js/telegram-web-app.js"></script>
    <style>
        body { font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; background: #0f0f0f; color: white; text-align: center; padding: 20px; }
        .balance-box { background: #1e1e1e; padding: 10px; border-radius: 15px; margin-bottom: 20px; border: 1px solid #333; }
        .stars { color: #ffd700; font-size: 22px; font-weight: bold; }
        .case-area { width: 180px; height: 180px; background: radial-gradient(#333, #111); border: 3px dashed #444; border-radius: 50%; margin: 30px auto; display: flex; align-items: center; justify-content: center; font-size: 60px; transition: 0.3s; }
        .input-group { margin-bottom: 20px; }
        input { background: #222; border: 1px solid #ffaa00; color: white; padding: 10px; border-radius: 8px; width: 60px; text-align: center; font-size: 18px; }
        button { background: linear-gradient(45deg, #ffaa00, #ff8800); border: none; padding: 15px 40px; border-radius: 12px; color: black; font-weight: bold; cursor: pointer; font-size: 18px; box-shadow: 0 4px 15px rgba(255, 170, 0, 0.3); }
        button:disabled { background: #555; cursor: not-allowed; }
        #status { margin-top: 20px; font-weight: bold; height: 24px; }
    </style>
</head>
<body>

    <div class="balance-box">
        Balans: <span id="balance" class="stars">100</span> ⭐
    </div>

    <div class="case-area" id="case">📦</div>

    <div class="input-group">
        <p>Stavka (max 50 ⭐):</p>
        <input type="number" id="betInput" value="10" min="1" max="50">
    </div>

    <button id="openBtn" onclick="playGame()">Case Ochish</button>
    
    <div id="status"></div>

    <script>
        const tg = window.Telegram.WebApp;
        tg.expand();

        let balance = 100;

        function playGame() {
            const bet = parseInt(document.getElementById('betInput').value);
            const status = document.getElementById('status');
            const caseEmoji = document.getElementById('case');
            const btn = document.getElementById('openBtn');

            // Tekshiruvlar
            if (isNaN(bet) || bet < 1 || bet > 50) {
                alert("Stavka 1 dan 50 gacha bo'lishi kerak!");
                return;
            }
            if (bet > balance) {
                alert("Mablag' yetarli emas!");
                return;
            }

            // O'yin boshlandi
            balance -= bet;
            updateBalance();
            btn.disabled = true;
            status.innerText = "Case ochilmoqda...";
            caseEmoji.style.transform = "scale(1.2) rotate(10deg)";

            setTimeout(() => {
                // Yutish ehtimoli: 30% (0.3 dan kichik bo'lsa yutadi)
                const winChance = Math.random();
                
                if (winChance < 0.3) {
                    const winAmount = bet * 2; // 2 barobar yutuq
                    balance += winAmount;
                    caseEmoji.innerText = "💰";
                    status.style.color = "#00ff00";
                    status.innerText = `YUTDINGIZ! +${winAmount} Stars`;
                } else {
                    caseEmoji.innerText = "💀";
                    status.style.color = "#ff4444";
                    status.innerText = `YUTQAZDINGIZ! -${bet} Stars`;
                }

                updateBalance();
                caseEmoji.style.transform = "scale(1) rotate(0deg)";
                btn.disabled = false;
                
                setTimeout(() => {
                    caseEmoji.innerText = "📦";
                }, 2000);

            }, 1200);
        }

        function updateBalance() {
            document.getElementById('balance').innerText = balance;
        }
    </script>
</body>
</html>
