<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
    <title>Колесо Фортуны | Раз в сутки</title>
    <style>
        * {
            box-sizing: border-box;
            user-select: none;
        }

        body {
            background: linear-gradient(145deg, #1a472a 0%, #0e2a1a 100%);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            font-family: 'Segoe UI', 'Poppins', 'Montserrat', system-ui, -apple-system, sans-serif;
            margin: 0;
            padding: 20px;
        }

        .game-container {
            background: rgba(0, 0, 0, 0.35);
            backdrop-filter: blur(4px);
            border-radius: 68px;
            padding: 20px 20px 35px;
            box-shadow: 0 25px 45px rgba(0, 0, 0, 0.5), inset 0 1px 2px rgba(255, 255, 255, 0.1);
        }

        .wheel-wrapper {
            position: relative;
            display: flex;
            justify-content: center;
            margin-bottom: 20px;
        }

        canvas {
            display: block;
            margin: 0 auto;
            border-radius: 50%;
            box-shadow: 0 20px 35px rgba(0, 0, 0, 0.5), 0 0 0 12px #f9e0a0, 0 0 0 18px #b57c1c;
            transition: box-shadow 0.2s;
            background: #ecb87c;
            cursor: pointer;
        }

        canvas:active {
            transform: scale(0.99);
        }

        .pointer {
            position: absolute;
            top: -28px;
            left: 50%;
            transform: translateX(-50%);
            width: 0;
            height: 0;
            border-left: 28px solid transparent;
            border-right: 28px solid transparent;
            border-top: 58px solid #ffd966;
            filter: drop-shadow(0 6px 6px rgba(0, 0, 0, 0.3));
            z-index: 10;
        }

        .pointer::after {
            content: "⚡";
            position: absolute;
            top: -54px;
            left: -18px;
            font-size: 28px;
            color: #e67e22;
            text-shadow: 0 2px 3px #ffd700;
        }

        .info-panel {
            background: #2c1a10e0;
            backdrop-filter: blur(12px);
            border-radius: 60px;
            padding: 12px 20px;
            margin-top: 20px;
            display: flex;
            flex-wrap: wrap;
            align-items: center;
            justify-content: space-between;
            gap: 15px;
            border: 1px solid rgba(255, 215, 130, 0.6);
            box-shadow: 0 8px 20px rgba(0, 0, 0, 0.3);
        }

        .prize-result {
            background: #1f1107;
            border-radius: 40px;
            padding: 8px 20px;
            color: #ffecb3;
            font-weight: bold;
            font-size: 1.25rem;
            letter-spacing: 1px;
            flex: 2;
            text-align: center;
            box-shadow: inset 0 1px 4px rgba(0,0,0,0.5), 0 2px 2px rgba(255,215,0,0.3);
        }

        .prize-result span {
            color: #ffb347;
            font-weight: 800;
            font-size: 1.35rem;
            text-shadow: 0 0 2px gold;
        }

        button {
            background: radial-gradient(circle at 30% 10%, #ffb347, #ff7e05);
            border: none;
            padding: 12px 28px;
            border-radius: 50px;
            font-weight: bold;
            font-size: 1.3rem;
            font-family: inherit;
            color: #2c1a0b;
            cursor: pointer;
            transition: all 0.2s ease;
            box-shadow: 0 8px 0 #9b4a00;
            letter-spacing: 1px;
            display: inline-flex;
            align-items: center;
            gap: 8px;
        }

        button:active {
            transform: translateY(4px);
            box-shadow: 0 4px 0 #9b4a00;
        }

        button:disabled {
            opacity: 0.6;
            transform: translateY(0);
            cursor: not-allowed;
            filter: grayscale(0.1);
            box-shadow: 0 8px 0 #9b4a00;
        }

        .prizes-list {
            background: #261a0e80;
            border-radius: 40px;
            padding: 6px 18px;
            font-size: 0.8rem;
            color: #ffdeae;
            display: flex;
            align-items: center;
            gap: 8px;
            flex-wrap: wrap;
        }

        .prizes-list b {
            color: #ffc857;
        }

        /* Таймер обратного отсчета */
        .cooldown-timer {
            background: #0e0b07cc;
            border-radius: 40px;
            padding: 6px 18px;
            font-size: 0.9rem;
            font-weight: bold;
            color: #ffcf8a;
            display: flex;
            align-items: center;
            gap: 8px;
            letter-spacing: 0.5px;
        }

        .cooldown-timer span {
            font-family: monospace;
            font-size: 1.1rem;
            background: #00000066;
            padding: 2px 8px;
            border-radius: 30px;
            color: #ffeaac;
        }

        @media (max-width: 650px) {
            .info-panel {
                flex-direction: column;
                text-align: center;
            }
            .prize-result {
                width: 100%;
            }
            .prizes-list {
                justify-content: center;
            }
            .pointer {
                top: -22px;
                border-left: 22px solid transparent;
                border-right: 22px solid transparent;
                border-top: 46px solid #ffd966;
            }
            .pointer::after {
                top: -44px;
                left: -15px;
                font-size: 24px;
            }
            .cooldown-timer {
                justify-content: center;
            }
        }
    </style>
</head>
<body>
<div>
    <div class="game-container">
        <div class="wheel-wrapper">
            <canvas id="wheelCanvas" width="550" height="550" style="width:100%; height:auto; max-width:550px; aspect-ratio:1/1"></canvas>
            <div class="pointer"></div>
        </div>
        <div class="info-panel">
            <div class="prize-result">
                🎁 ВЫИГРЫШ: <span id="winDisplay">—</span>
            </div>
            <button id="spinBtn">🌀 КРУТИТЬ!</button>
        </div>
        <div class="prizes-list">
            <b>🏆 ПРИЗЫ:</b> 
            <span id="prizeNamesList">загрузка...</span>
        </div>
        <div class="cooldown-timer" id="cooldownBox">
            ⏳ Следующий раз: <span id="cooldownCountdown">—</span>
        </div>
    </div>
</div>

<script>
    (function() {
        // ----- НАСТРАИВАЕМЫЕ ПРИЗЫ И ЦВЕТА -----
        const SECTORS = [
            { name: "Скидка 5%",   color: "#F94144" },
            { name: "Скидка 10%",  color: "#F9C74F" },
            { name: "Бесплатная доставка", color: "#90BE6D" },
            { name: "Спасибо за участие", color: "#577590" },
            { name: "Подарок",     color: "#F9844A" },
            { name: "Скидка 20%",  color: "#43AA8B" },
            { name: "Кофе в подарок", color: "#F8961E" },
            { name: "100 бонусов", color: "#9B5DE5" }
        ];
        
        // ----- НАСТРОЙКИ ВРАЩЕНИЯ (7 секунд) -----
        const DURATION = 7000;            // 7 секунд
        const EXTRA_SPINS = 12;           // минимальное кол-во полных оборотов
        const MAX_EXTRA = 18;             // максимальное кол-во полных оборотов
        
        // ----- КЛЮЧ ДЛЯ LOCALSTORAGE (ограничение раз в сутки)-----
        const STORAGE_KEY = "wheel_spin_last_timestamp";
        const ONE_DAY_MS = 24 * 60 * 60 * 1000;
        
        // DOM элементы
        const canvas = document.getElementById('wheelCanvas');
        const ctx = canvas.getContext('2d');
        const spinBtn = document.getElementById('spinBtn');
        const winDisplaySpan = document.getElementById('winDisplay');
        const prizeListSpan = document.getElementById('prizeNamesList');
        const cooldownSpan = document.getElementById('cooldownCountdown');
        
        let width, height, centerX, centerY, radius;
        let currentRotation = 0;
        let animating = false;
        let animationId = null;
        let startTime = 0;
        let startRotation = 0;
        let targetRotation = 0;
        
        const SEG_COUNT = SECTORS.length;
        const ANGLE_PER_SEG = 360 / SEG_COUNT;
        
        // Переменная для таймера обратного отсчета
        let countdownInterval = null;
        
        // ---- Функции ограничения раз в сутки ----
        function canSpinNow() {
            const lastSpin = localStorage.getItem(STORAGE_KEY);
            if (!lastSpin) return true;
            const lastTime = parseInt(lastSpin, 10);
            if (isNaN(lastTime)) return true;
            const now = Date.now();
            return (now - lastTime) >= ONE_DAY_MS;
        }
        
        function getRemainingTimeMs() {
            const lastSpin = localStorage.getItem(STORAGE_KEY);
            if (!lastSpin) return 0;
            const lastTime = parseInt(lastSpin, 10);
            if (isNaN(lastTime)) return 0;
            const now = Date.now();
            const elapsed = now - lastTime;
            if (elapsed >= ONE_DAY_MS) return 0;
            return ONE_DAY_MS - elapsed;
        }
        
        function saveSpinTimestamp() {
            localStorage.setItem(STORAGE_KEY, Date.now().toString());
        }
        
        // Обновляем отображение таймера и состояние кнопки
        function updateCooldownUI() {
            if (animating) return; // во время вращения не трогаем кнопку, она и так disabled
            
            const can = canSpinNow();
            if (can) {
                spinBtn.disabled = false;
                cooldownSpan.innerText = "Доступно!";
                if (countdownInterval) {
                    clearInterval(countdownInterval);
                    countdownInterval = null;
                }
            } else {
                spinBtn.disabled = true;
                const remainingMs = getRemainingTimeMs();
                if (remainingMs > 0) {
                    const hours = Math.floor(remainingMs / (1000 * 60 * 60));
                    const minutes = Math.floor((remainingMs % (1000 * 60 * 60)) / (1000 * 60));
                    const seconds = Math.floor((remainingMs % (1000 * 60)) / 1000);
                    cooldownSpan.innerText = `${hours}ч ${minutes}м ${seconds}с`;
                } else {
                    // на всякий случай, если время вышло но флаг не обновился
                    spinBtn.disabled = false;
                    cooldownSpan.innerText = "Доступно!";
                }
            }
        }
        
        // Запускаем цикл обновления таймера каждую секунду
        function startCountdownUpdater() {
            if (countdownInterval) clearInterval(countdownInterval);
            updateCooldownUI();
            countdownInterval = setInterval(() => {
                updateCooldownUI();
                // дополнительно: если кнопка разблокировалась, но интервал висит, ничего страшного
                if (canSpinNow() && spinBtn.disabled && !animating) {
                    spinBtn.disabled = false;
                    cooldownSpan.innerText = "Доступно!";
                    if (countdownInterval) clearInterval(countdownInterval);
                    countdownInterval = null;
                }
            }, 1000);
        }
        
        // ---- Функции отрисовки колеса ----
        function getShortName(name, maxLen = 18) {
            if (name.length <= maxLen) return name;
            return name.slice(0, maxLen-2) + "..";
        }
        
        function drawWheel() {
            if (!ctx) return;
            ctx.clearRect(0, 0, width, height);
            const angleRadStep = (ANGLE_PER_SEG * Math.PI) / 180;
            const startAngleRad = (currentRotation * Math.PI) / 180;
            
            for (let i = 0; i < SEG_COUNT; i++) {
                const start = startAngleRad + i * angleRadStep;
                const end = start + angleRadStep;
                ctx.beginPath();
                ctx.moveTo(centerX, centerY);
                ctx.arc(centerX, centerY, radius, start, end);
                ctx.closePath();
                ctx.fillStyle = SECTORS[i].color;
                ctx.fill();
                ctx.save();
                ctx.shadowBlur = 0;
                ctx.strokeStyle = "#FFF5E0";
                ctx.lineWidth = 2.5;
                ctx.stroke();
                
                ctx.beginPath();
                ctx.moveTo(centerX, centerY);
                ctx.arc(centerX, centerY, radius, start, end);
                ctx.lineTo(centerX, centerY);
                ctx.strokeStyle = "rgba(255,240,180,0.8)";
                ctx.lineWidth = 2;
                ctx.stroke();
                
                ctx.save();
                ctx.translate(centerX, centerY);
                ctx.rotate(start + angleRadStep / 2);
                ctx.textAlign = "center";
                ctx.textBaseline = "middle";
                const textRadius = radius * 0.68;
                const fontSize = Math.max(12, Math.min(28, radius * 0.08));
                ctx.font = `bold ${fontSize}px "Segoe UI", "Poppins", system-ui`;
                ctx.fillStyle = "#1F1A15";
                let displayText = getShortName(SECTORS[i].name, radius < 180 ? 10 : 14);
                ctx.fillText(displayText, textRadius, 6);
                ctx.restore();
            }
            
            ctx.beginPath();
            ctx.arc(centerX, centerY, radius * 0.12, 0, 2 * Math.PI);
            ctx.fillStyle = "#FFD966";
            ctx.shadowBlur = 10;
            ctx.fill();
            ctx.beginPath();
            ctx.arc(centerX, centerY, radius * 0.09, 0, 2 * Math.PI);
            ctx.fillStyle = "#B65F00";
            ctx.fill();
            ctx.shadowBlur = 0;
            
            ctx.beginPath();
            ctx.arc(centerX, centerY, radius, 0, 2 * Math.PI);
            ctx.strokeStyle = "#ffcf8a";
            ctx.lineWidth = 4;
            ctx.stroke();
            ctx.beginPath();
            ctx.arc(centerX, centerY, radius-3, 0, 2 * Math.PI);
            ctx.strokeStyle = "#FCE2A4";
            ctx.lineWidth = 2;
            ctx.stroke();
        }
        
        const POINTER_ANGLE = -90;
        
        function getCurrentPrizeIndex() {
            let rawAngle = POINTER_ANGLE - currentRotation;
            rawAngle = ((rawAngle % 360) + 360) % 360;
            for (let i = 0; i < SEG_COUNT; i++) {
                let startDeg = i * ANGLE_PER_SEG;
                let endDeg = (i + 1) * ANGLE_PER_SEG;
                if (rawAngle >= startDeg && rawAngle < endDeg) return i;
            }
            return Math.floor(rawAngle / ANGLE_PER_SEG) % SEG_COUNT;
        }
        
        function updateWinDisplay() {
            const idx = getCurrentPrizeIndex();
            winDisplaySpan.innerText = SECTORS[idx].name;
        }
        
        // ---- Анимация вращения (7 секунд) ----
        function easeOutCubic(t) {
            return 1 - Math.pow(1 - t, 3);
        }
        
        function animateSpin(now) {
            const elapsed = now - startTime;
            let t = Math.min(1, elapsed / DURATION);
            const ease = easeOutCubic(t);
            const deltaAngle = targetRotation - startRotation;
            let newRotation = startRotation + deltaAngle * ease;
            newRotation = ((newRotation % 360) + 360) % 360;
            currentRotation = newRotation;
            drawWheel();
            
            if (t < 1) {
                animationId = requestAnimationFrame(animateSpin);
            } else {
                currentRotation = ((targetRotation % 360) + 360) % 360;
                drawWheel();
                animating = false;
                // После завершения вращения - сохраняем метку времени (раз в сутки)
                saveSpinTimestamp();
                updateWinDisplay();
                // Обновляем UI таймера (кнопка заблокируется, запустим обратный отсчет)
                startCountdownUpdater();
                // Вибрация дисплея
                winDisplaySpan.style.transform = "scale(1.1)";
                setTimeout(() => { winDisplaySpan.style.transform = ""; }, 300);
                spinBtn.disabled = true;   // уже заблокируется через updateCooldownUI, но для надежности
                animationId = null;
                // Дополнительно вызовем обновление, чтобы таймер сразу показал 24ч
                updateCooldownUI();
            }
        }
        
        // Запуск вращения (с проверкой ограничения)
        function startSpin() {
            if (animating) return;
            // Проверка: можно ли крутить сегодня
            if (!canSpinNow()) {
                // Показываем сообщение, что нельзя
                winDisplaySpan.innerText = "❌ Можно раз в сутки!";
                setTimeout(() => {
                    if (!animating) updateWinDisplay();
                }, 1500);
                return;
            }
            
            // Генерируем случайный приз (честный рандом по индексу)
            const randomPrizeIndex = Math.floor(Math.random() * SEG_COUNT);
            // Для плавной остановки на нужном секторе:
            const desiredRawAngle = randomPrizeIndex * ANGLE_PER_SEG + ANGLE_PER_SEG/2;
            let rawTarget = POINTER_ANGLE - desiredRawAngle;
            rawTarget = ((rawTarget % 360) + 360) % 360;
            const fullTurns = (EXTRA_SPINS + Math.random() * (MAX_EXTRA - EXTRA_SPINS)) * 360;
            let finalTarget = rawTarget + fullTurns;
            
            startRotation = currentRotation;
            targetRotation = finalTarget;
            startTime = performance.now();
            animating = true;
            spinBtn.disabled = true;
            winDisplaySpan.innerText = "🌀 Крутим 7 секунд...";
            
            if (animationId) cancelAnimationFrame(animationId);
            animationId = requestAnimationFrame(animateSpin);
        }
        
        // ---- Ресайз и инициализация ----
        function resizeAndDraw() {
            const container = canvas.parentElement;
            const maxSize = Math.min(container.clientWidth, 550);
            canvas.width = maxSize;
            canvas.height = maxSize;
            canvas.style.width = `${maxSize}px`;
            canvas.style.height = `${maxSize}px`;
            width = maxSize;
            height = maxSize;
            centerX = width / 2;
            centerY = height / 2;
            radius = width * 0.46;
            drawWheel();
        }
        
        function updatePrizeListText() {
            const names = SECTORS.map(s => s.name);
            if (names.length <= 6) {
                prizeListSpan.innerText = names.join(" • ");
            } else {
                prizeListSpan.innerText = names.slice(0,5).join(" • ") + " • ...";
            }
        }
        
        // Старт
        window.addEventListener('resize', () => {
            if (!animating) resizeAndDraw();
        });
        
        resizeAndDraw();
        updatePrizeListText();
        updateWinDisplay();
        spinBtn.addEventListener('click', startSpin);
        
        // Инициализация ограничения: блокируем кнопку, если уже крутили сегодня
        startCountdownUpdater();
        
        // Доп. синхронизация после загрузки, чтобы таймер точно показал остаток
        if (!canSpinNow()) {
            spinBtn.disabled = true;
            const remaining = getRemainingTimeMs();
            if (remaining > 0 && countdownInterval) {
                // уже обновится в интервале
            }
        } else {
            spinBtn.disabled = false;
            cooldownSpan.innerText = "Доступно!";
        }
        
        console.log("✅ Колесо фортуны | 7 секунд вращения | 1 раз в сутки (localStorage)");
    })();
</script>
</body>
</html>
