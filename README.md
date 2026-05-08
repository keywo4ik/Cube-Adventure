# Cube-Adventure
мини-игра платформер в неоновом стиле где у тебя 5 сердец и тебе надо избегать красных кубов чтобы не получить урон. Есть бонусы виде шлема который добавляет 2 сердца которые регенирируются. 
<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <title>⚡ NEON CUBE | Железные сердца + Шлем ⚡</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            user-select: none;
        }

        body {
            min-height: 100vh;
            background: radial-gradient(ellipse at center, #0a0a1a 0%, #000000 100%);
            font-family: 'Courier New', 'Monaco', monospace;
            display: flex;
            justify-content: center;
            align-items: center;
            overflow: hidden;
        }

        .neon-bg {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: -1;
            background: 
                repeating-linear-gradient(90deg, rgba(0,255,255,0.02) 0px, rgba(0,255,255,0.02) 2px, transparent 2px, transparent 8px),
                repeating-linear-gradient(0deg, rgba(255,0,255,0.02) 0px, rgba(255,0,255,0.02) 2px, transparent 2px, transparent 8px);
            animation: bgScroll 20s linear infinite;
        }

        @keyframes bgScroll {
            0% { transform: translate(0, 0); }
            100% { transform: translate(8px, 8px); }
        }

        .game-wrapper {
            background: rgba(10, 10, 30, 0.7);
            backdrop-filter: blur(10px);
            border-radius: 30px;
            padding: 20px;
            box-shadow: 
                0 0 20px rgba(0, 255, 255, 0.3),
                0 0 40px rgba(255, 0, 255, 0.2),
                inset 0 0 20px rgba(0, 255, 255, 0.1);
            border: 1px solid rgba(0, 255, 255, 0.5);
            animation: borderPulse 3s ease-in-out infinite;
        }

        @keyframes borderPulse {
            0%, 100% { border-color: rgba(0, 255, 255, 0.5); box-shadow: 0 0 20px rgba(0, 255, 255, 0.3); }
            50% { border-color: rgba(255, 0, 255, 0.5); box-shadow: 0 0 40px rgba(255, 0, 255, 0.5); }
        }

        canvas {
            display: block;
            margin: 0 auto;
            border-radius: 15px;
            box-shadow: 0 0 30px rgba(0, 255, 255, 0.3);
            cursor: none;
        }

        .menu-screen {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.95);
            backdrop-filter: blur(8px);
            display: flex;
            justify-content: center;
            align-items: center;
            z-index: 1000;
            transition: all 0.5s cubic-bezier(0.68, -0.55, 0.265, 1.55);
        }

        .menu-screen.hidden {
            opacity: 0;
            visibility: hidden;
            pointer-events: none;
        }

        .neon-menu {
            text-align: center;
            padding: 50px 80px;
            background: rgba(0, 0, 0, 0.6);
            border-radius: 50px;
            border: 2px solid;
            animation: menuGlow 2s ease-in-out infinite;
            box-shadow: 0 0 50px rgba(0, 255, 255, 0.5), inset 0 0 30px rgba(255, 0, 255, 0.2);
        }

        @keyframes menuGlow {
            0%, 100% { box-shadow: 0 0 30px rgba(0, 255, 255, 0.5), inset 0 0 20px rgba(0, 255, 255, 0.2); border-color: #0ff; }
            50% { box-shadow: 0 0 60px rgba(255, 0, 255, 0.7), inset 0 0 30px rgba(255, 0, 255, 0.3); border-color: #f0f; }
        }

        .neon-title {
            font-size: 4rem;
            font-weight: bold;
            margin-bottom: 20px;
            text-transform: uppercase;
            animation: titleFlicker 3s infinite;
        }

        .neon-title span:nth-child(1) { color: #0ff; text-shadow: 0 0 10px #0ff, 0 0 20px #0ff; }
        .neon-title span:nth-child(2) { color: #f0f; text-shadow: 0 0 10px #f0f, 0 0 20px #f0f; }
        .neon-title span:nth-child(3) { color: #0ff; text-shadow: 0 0 10px #0ff, 0 0 20px #0ff; }
        .neon-title span:nth-child(4) { color: #ff0; text-shadow: 0 0 10px #ff0; }

        @keyframes titleFlicker {
            0%, 100% { opacity: 1; }
            95% { opacity: 1; }
            96% { opacity: 0.5; }
            97% { opacity: 1; }
        }

        .neon-subtitle {
            color: #0ff;
            font-size: 1rem;
            margin-bottom: 40px;
            letter-spacing: 3px;
            text-shadow: 0 0 5px #0ff;
        }

        .menu-buttons {
            display: flex;
            gap: 30px;
            justify-content: center;
            flex-wrap: wrap;
            margin-bottom: 30px;
        }

        .neon-btn {
            background: transparent;
            border: 2px solid;
            padding: 12px 35px;
            font-size: 1.3rem;
            font-weight: bold;
            font-family: monospace;
            cursor: pointer;
            transition: all 0.3s ease;
            text-transform: uppercase;
            letter-spacing: 2px;
        }

        .neon-btn-start {
            border-color: #0ff;
            color: #0ff;
            text-shadow: 0 0 5px #0ff;
            box-shadow: 0 0 15px rgba(0, 255, 255, 0.3);
        }

        .neon-btn-start:hover {
            background: #0ff;
            color: #000;
            box-shadow: 0 0 40px #0ff;
            transform: scale(1.05);
        }

        .neon-btn-info {
            border-color: #f0f;
            color: #f0f;
            text-shadow: 0 0 5px #f0f;
        }

        .neon-btn-info:hover {
            background: #f0f;
            color: #000;
            box-shadow: 0 0 40px #f0f;
            transform: scale(1.05);
        }

        .controls-info {
            margin-top: 30px;
            padding: 20px;
            border-top: 1px solid rgba(0, 255, 255, 0.3);
            color: #ff0;
            font-size: 0.8rem;
            text-shadow: 0 0 3px #ff0;
        }

        .controls-info span {
            display: inline-block;
            margin: 0 10px;
            padding: 5px 10px;
            background: rgba(0, 255, 255, 0.1);
            border-radius: 10px;
            border: 1px solid #0ff;
        }

        .info-panel {
            margin-top: 15px;
            background: rgba(0, 0, 0, 0.6);
            backdrop-filter: blur(5px);
            border-radius: 15px;
            padding: 12px 25px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            flex-wrap: wrap;
            gap: 15px;
            border: 1px solid rgba(0, 255, 255, 0.5);
            box-shadow: 0 0 20px rgba(0, 255, 255, 0.2);
        }

        .stats {
            display: flex;
            gap: 25px;
            align-items: center;
            flex-wrap: wrap;
        }

        .stat {
            background: rgba(0, 0, 0, 0.7);
            padding: 5px 18px;
            border-radius: 30px;
            display: flex;
            align-items: center;
            gap: 10px;
            font-weight: bold;
            border: 1px solid;
        }

        .stat:nth-child(1) { border-color: #ff0; color: #ff0; }
        .stat:nth-child(2) { border-color: #0ff; color: #0ff; }
        .stat:nth-child(3) { border-color: #f0f; color: #f0f; }
        .stat:nth-child(4) { border-color: #0f0; color: #0f0; }

        .stat span { font-size: 1.2rem; font-weight: bold; }

        .controls-hint {
            background: rgba(0, 0, 0, 0.7);
            padding: 5px 15px;
            border-radius: 30px;
            font-family: monospace;
            font-size: 0.75rem;
            border: 1px solid #ff0;
            color: #ff0;
        }

        button {
            background: rgba(255, 0, 255, 0.2);
            border: 1px solid #f0f;
            padding: 8px 25px;
            border-radius: 30px;
            font-weight: bold;
            cursor: pointer;
            font-family: monospace;
            transition: all 0.3s;
            color: #f0f;
        }

        button:hover {
            background: #f0f;
            color: #000;
            box-shadow: 0 0 20px #f0f;
            transform: scale(1.02);
        }

        .message {
            background: rgba(0, 0, 0, 0.8);
            padding: 8px 20px;
            border-radius: 30px;
            font-size: 0.8rem;
            backdrop-filter: blur(4px);
            border: 1px solid #0ff;
            color: #0ff;
        }
    </style>
</head>
<body>
<div class="neon-bg"></div>

<div class="menu-screen" id="menuScreen">
    <div class="neon-menu">
        <div class="neon-title">
            <span>⚡</span><span>NEON</span><span> CUBE</span><span> ⚡</span>
        </div>
        <div class="neon-subtitle">
            🛡️ ЖЕЛЕЗНЫЕ СЕРДЦА | РЕГЕНЕРАЦИЯ 🛡️
        </div>
        <div class="menu-buttons">
            <button class="neon-btn neon-btn-start" id="startGameBtn">▶ СТАРТ</button>
            <button class="neon-btn neon-btn-info" id="infoBtn">ℹ️ УПРАВЛЕНИЕ</button>
        </div>
        <div class="controls-info" id="infoPanel" style="display: none;">
            <div>🎮 УПРАВЛЕНИЕ:</div>
            <span>A / D</span> или <span>← / →</span> — движение<br>
            <span>ПРОБЕЛ</span> или <span>W</span> — НЕОНОВЫЙ ПРЫЖОК<br>
            <span>R</span> — рестарт<br>
            <span>ESC</span> — меню
            <div style="margin-top: 10px; color: #0ff;">🛡️ НАЙДИ ШЛЕМ! +2 ЖЕЛЕЗНЫХ СЕРДЦА (регенерация 1 сердце/сек через 10 сек)</div>
        </div>
    </div>
</div>

<div>
    <div class="game-wrapper">
        <canvas id="gameCanvas" width="900" height="500"></canvas>
        <div class="info-panel">
            <div class="stats">
                <div class="stat">⭐ МОНЕТЫ: <span id="coinCount">0</span></div>
                <div class="stat">💎 АЛМАЗЫ: <span id="gemCount">0</span></div>
                <div class="stat health-bar" id="healthDisplay">❤️ ЗДОРОВЬЕ: <span id="healthText"></span></div>
                <div class="stat" id="helmetStat">🛡️ ШЛЕМ: <span id="helmetStatus">❌</span></div>
            </div>
            <div class="controls-hint">
                🎮 A/D или ←/→ | ПРОБЕЛ/W — ПРЫЖОК | R — рестарт | ESC — меню
            </div>
            <button id="resetBtnGame">🔄 Новая игра</button>
        </div>
        <div class="info-panel" style="margin-top: 10px; justify-content: center;">
            <div class="message" id="gameMessage">
                🛡️ НАЙДИ ЗОЛОТОЙ ШЛЕМ НА ВЕРХНЕЙ ПЛАТФОРМЕ! +2 ЖЕЛЕЗНЫХ СЕРДЦА!
            </div>
        </div>
    </div>
</div>

<script>
    (function(){
        const canvas = document.getElementById('gameCanvas');
        const ctx = canvas.getContext('2d');
        
        const WIDTH = 900;
        const HEIGHT = 500;
        
        const menuScreen = document.getElementById('menuScreen');
        const startBtn = document.getElementById('startGameBtn');
        const infoBtn = document.getElementById('infoBtn');
        const infoPanel = document.getElementById('infoPanel');
        let gameStarted = false;
        
        startBtn.addEventListener('click', () => {
            menuScreen.classList.add('hidden');
            gameStarted = true;
            resetGameData();
        });
        
        infoBtn.addEventListener('click', () => {
            if(infoPanel.style.display === 'none') {
                infoPanel.style.display = 'block';
                infoBtn.textContent = '🔽 СКРЫТЬ';
            } else {
                infoPanel.style.display = 'none';
                infoBtn.textContent = 'ℹ️ УПРАВЛЕНИЕ';
            }
        });
        
        document.addEventListener('keydown', (e) => {
            if(e.key === 'Escape' && gameStarted) {
                menuScreen.classList.remove('hidden');
                gameStarted = false;
            }
        });
        
        // Платформы
        const platforms = [];
        platforms.push({ x: 0, y: HEIGHT - 40, w: WIDTH, h: 40, type: 'ground' });
        platforms.push({ x: 80, y: HEIGHT - 100, w: 80, h: 20, type: 'normal' });
        platforms.push({ x: 220, y: HEIGHT - 160, w: 80, h: 20, type: 'normal' });
        platforms.push({ x: 360, y: HEIGHT - 220, w: 80, h: 20, type: 'normal' });
        platforms.push({ x: 500, y: HEIGHT - 280, w: 80, h: 20, type: 'normal' });
        platforms.push({ x: 640, y: HEIGHT - 340, w: 80, h: 20, type: 'normal' });
        platforms.push({ x: 780, y: HEIGHT - 400, w: 80, h: 20, type: 'normal' });
        platforms.push({ x: 30, y: HEIGHT - 200, w: 70, h: 20, type: 'normal' });
        platforms.push({ x: 750, y: HEIGHT - 250, w: 70, h: 20, type: 'normal' });
        platforms.push({ x: 830, y: HEIGHT - 300, w: 60, h: 20, type: 'normal' });
        platforms.push({ x: 150, y: HEIGHT - 380, w: 60, h: 20, type: 'floating' });
        platforms.push({ x: 450, y: HEIGHT - 420, w: 60, h: 20, type: 'floating' });
        platforms.push({ x: 650, y: HEIGHT - 440, w: 60, h: 20, type: 'rare' });
        platforms.push({ x: 300, y: HEIGHT - 460, w: 50, h: 20, type: 'rare' });
        
        let player = {
            x: 100, y: HEIGHT - 80, width: 28, height: 28,
            vx: 0, vy: 0, onGround: false, facingRight: true,
            health: 5, maxHealth: 5, hasHelmet: false, ironHearts: 0,
            regenTimer: 0, regenCounter: 0, regenActive: false, invincibleTimer: 0
        };
        
        let helmetItem = { x: 830, y: HEIGHT - 425, w: 20, h: 20, collected: false };
        let coins = [], gems = [], enemies = [];
        let coinScore = 0, gemScore = 0, gameRunning = true;
        
        const keys = { ArrowLeft: false, ArrowRight: false, a: false, d: false, space: false, w: false };
        let jumpPressed = false, canJump = true;
        let floatingMessage = { text: "", timer: 0 };
        
        function updateHealthDisplay() {
            const healthDiv = document.getElementById('healthText');
            let display = '';
            for(let i = 0; i < player.health; i++) display += '❤️ ';
            for(let i = 0; i < player.ironHearts; i++) display += '🛡️ ';
            if(display === '') display = '💀';
            healthDiv.innerHTML = display;
            document.getElementById('helmetStatus').innerHTML = player.hasHelmet ? '✅ +2 🛡️' : '❌';
        }
        
        function takeDamage(amount) {
            if(player.invincibleTimer > 0) return false;
            if(player.health <= 0) return true;
            let remainingDamage = amount;
            while(remainingDamage > 0 && player.ironHearts > 0) {
                player.ironHearts--;
                remainingDamage--;
                showMessage(`🛡️ Железное сердце поглотило удар!`, false);
            }
            if(remainingDamage > 0) {
                player.health -= remainingDamage;
                if(player.health < 0) player.health = 0;
                showMessage(`❤️‍🔥 -${remainingDamage} здоровья!`, false);
            }
            updateHealthDisplay();
            if(player.health <= 0) {
                gameRunning = false;
                showMessage(`💀 GAME OVER! Нажми R или Новая игра 💀`, false);
                return true;
            }
            player.invincibleTimer = 30;
            if(player.hasHelmet && player.ironHearts < 2) {
                player.regenTimer = 600;
                player.regenActive = false;
                player.regenCounter = 0;
                showMessage(`🛡️ Шлем активирует регенерацию через 10 секунд!`, true);
            }
            return false;
        }
        
        function updateRegeneration() {
            if(!player.hasHelmet) return;
            if(player.ironHearts >= 2) {
                player.regenTimer = 0;
                player.regenActive = false;
                player.regenCounter = 0;
                return;
            }
            if(player.regenTimer > 0) {
                player.regenTimer--;
                if(player.regenTimer === 540) showMessage(`⚙️ Регенерация через 9 секунд...`, true);
                else if(player.regenTimer === 300) showMessage(`⚙️ Регенерация через 5 секунд...`, true);
                else if(player.regenTimer === 60) showMessage(`⚙️ Регенерация через 1 секунду!`, true);
            } else if(player.regenTimer === 0 && player.ironHearts < 2) {
                if(!player.regenActive) {
                    player.regenActive = true;
                    player.regenCounter = 0;
                }
                if(player.regenCounter >= 60 && player.ironHearts < 2) {
                    player.ironHearts = Math.min(2, player.ironHearts + 1);
                    player.regenCounter = 0;
                    updateHealthDisplay();
                    showMessage(`🛡️ Железное сердце восстановлено! ${player.ironHearts}/2`, true);
                    if(player.ironHearts === 2) {
                        player.regenActive = false;
                        showMessage(`✨ Железные сердца полностью восстановлены! ✨`, true);
                    }
                } else if(player.ironHearts < 2) {
                    player.regenCounter++;
                }
            } else {
                player.regenActive = false;
                player.regenCounter = 0;
            }
        }
        
        function pickUpHelmet() {
            if(!helmetItem.collected) {
                const dist = Math.hypot(player.x - helmetItem.x, player.y - helmetItem.y);
                if(dist < 30) {
                    helmetItem.collected = true;
                    player.hasHelmet = true;
                    player.ironHearts = 2;
                    updateHealthDisplay();
                    showMessage(`🛡️ ТЫ НАШЁЛ ШЛЕМ! +2 ЖЕЛЕЗНЫХ СЕРДЦА! 🛡️`, true);
                }
            }
        }
        
        function resetGameData() {
            player = {
                x: 100, y: HEIGHT - 80, width: 28, height: 28,
                vx: 0, vy: 0, onGround: false, facingRight: true,
                health: 5, maxHealth: 5, hasHelmet: false, ironHearts: 0,
                regenTimer: 0, regenCounter: 0, regenActive: false, invincibleTimer: 0
            };
            helmetItem = { x: 830, y: HEIGHT - 425, w: 20, h: 20, collected: false };
            coinScore = 0;
            gemScore = 0;
            gameRunning = true;
            generateItems();
            updateHealthDisplay();
            updateUI();
            showMessage("🛡️ НАЙДИ ЗОЛОТОЙ ШЛЕМ НА ВЕРХНЕЙ ПЛАТФОРМЕ!", true);
        }
        
        function generateItems() {
            coins = [];
            gems = [];
            enemies = [];
            for(let i=0; i<55; i++) {
                let platform = platforms[Math.floor(Math.random() * platforms.length)];
                coins.push({ x: platform.x + 10 + Math.random() * (platform.w - 30), y: platform.y - 10, w: 10, h: 10, collected: false });
            }
            for(let i=0; i<12; i++) {
                let highPlatforms = platforms.filter(p => p.y < HEIGHT-150);
                if(highPlatforms.length) {
                    let platform = highPlatforms[Math.floor(Math.random() * highPlatforms.length)];
                    gems.push({ x: platform.x + platform.w/2 - 8, y: platform.y - 14, w: 16, h: 16, collected: false });
                }
            }
            for(let i=0; i<15; i++) {
                enemies.push({ x: 40 + Math.random() * (WIDTH-80), y: HEIGHT-70, w: 25, h: 25, type: 'cactus', collected: false });
            }
            for(let i=0; i<6; i++) {
                let plat = platforms[Math.floor(Math.random() * platforms.length)];
                if(plat.type !== 'ground')
                    enemies.push({ x: plat.x + plat.w/2 - 12, y: plat.y - 15, w: 24, h: 15, type: 'lava', collected: false });
            }
        }
        
        function showMessage(msg, good=true) {
            floatingMessage = { text: msg, timer: 90 };
            const msgDiv = document.getElementById('gameMessage');
            msgDiv.innerHTML = msg;
            msgDiv.style.background = good ? "rgba(0,255,0,0.3)" : "rgba(255,0,0,0.3)";
            msgDiv.style.borderColor = good ? "#0f0" : "#f00";
            setTimeout(() => { if(floatingMessage.text === msg) {
                msgDiv.style.background = "rgba(0,0,0,0.8)";
                msgDiv.style.borderColor = "#0ff";
            }}, 1000);
        }
        
        function updateUI() {
            document.getElementById('coinCount').innerText = coinScore;
            document.getElementById('gemCount').innerText = gemScore;
        }
        
        function applyPhysicsAndCollisions() {
            player.vy += 0.5;
            player.x += player.vx;
            for(let plat of platforms) {
                if(player.x < plat.x + plat.w && player.x + player.width > plat.x) {
                    if(player.y + player.height > plat.y && player.y < plat.y + plat.h) {
                        if(player.vx > 0) player.x = plat.x - player.width;
                        if(player.vx < 0) player.x = plat.x + plat.w;
                    }
                }
            }
            player.y += player.vy;
            player.onGround = false;
            for(let plat of platforms) {
                if(player.x < plat.x + plat.w && player.x + player.width > plat.x) {
                    if(player.vy >= 0 && player.y + player.height > plat.y && player.y + player.height - player.vy <= plat.y + 10) {
                        player.y = plat.y - player.height;
                        player.vy = 0;
                        player.onGround = true;
                        canJump = true;
                    } else if(player.vy < 0 && player.y < plat.y + plat.h && player.y - player.vy >= plat.y + plat.h - 8) {
                        player.y = plat.y + plat.h;
                        player.vy = 0;
                    }
                }
            }
            if(player.x < 0) player.x = 0;
            if(player.x + player.width > WIDTH) player.x = WIDTH - player.width;
            if(player.y + player.height > HEIGHT) {
                player.y = HEIGHT - player.height;
                player.vy = 0;
                player.onGround = true;
                canJump = true;
            }
            if(player.y < 0) { player.y = 0; if(player.vy < 0) player.vy = 0; }
        }
        
        function handleInput() {
            if(!gameRunning || !gameStarted) return;
            let move = 0;
            if(keys.ArrowLeft || keys.a) move = -1;
            if(keys.ArrowRight || keys.d) move = 1;
            player.vx = move * 4.2;
            if(move !== 0) player.facingRight = move > 0;
            let jumpRequest = (keys.space || keys.w);
            if(jumpRequest && !jumpPressed && player.onGround && canJump) {
                player.vy = -11.5;
                player.onGround = false;
                canJump = false;
                jumpPressed = true;
            }
            if(!jumpRequest) jumpPressed = false;
            if(!player.onGround) canJump = false;
            else canJump = true;
        }
        
        function updateCollectibles() {
            for(let c of coins) {
                if(!c.collected && player.x < c.x + c.w && player.x + player.width > c.x && player.y < c.y + c.h && player.y + player.height > c.y) {
                    c.collected = true; coinScore++; updateUI(); showMessage(`+1 монета`, true);
                }
            }
            coins = coins.filter(c => !c.collected);
            for(let g of gems) {
                if(!g.collected && player.x < g.x + g.w && player.x + player.width > g.x && player.y < g.y + g.h && player.y + player.height > g.y) {
                    g.collected = true; gemScore++; updateUI(); showMessage(`💎 АЛМАЗ! +1 💎`, true);
                }
            }
            gems = gems.filter(g => !g.collected);
            for(let e of enemies) {
                if(!e.collected && player.x < e.x + e.w && player.x + player.width > e.x && player.y < e.y + e.h && player.y + player.height > e.y) {
                    e.collected = true;
                    takeDamage(1);
                }
            }
            enemies = enemies.filter(e => !e.collected);
            if(player.invincibleTimer > 0) player.invincibleTimer--;
        }
        
        // Отрисовка (сокращённо, но все элементы присутствуют)
        function drawBackground() {
            const grad = ctx.createLinearGradient(0, 0, 0, HEIGHT);
            grad.addColorStop(0, "#0a0a2a");
            grad.addColorStop(1, "#1a0a2a");
            ctx.fillStyle = grad;
            ctx.fillRect(0, 0, WIDTH, HEIGHT);
            ctx.strokeStyle = "#0ff";
            ctx.lineWidth = 1;
            for(let i=0; i<WIDTH; i+=50) {
                ctx.beginPath();
                ctx.moveTo(i, 0);
                ctx.lineTo(i, HEIGHT);
                ctx.stroke();
                ctx.beginPath();
                ctx.moveTo(0, i%HEIGHT);
                ctx.lineTo(WIDTH, i%HEIGHT);
                ctx.stroke();
            }
        }
        function drawPlatforms() {
            for(let plat of platforms) {
                if(plat.type === 'ground') { ctx.fillStyle = "#ff00ff88"; ctx.shadowBlur = 10; ctx.shadowColor = "#f0f"; }
                else if(plat.type === 'rare') { ctx.fillStyle = "#00ffff88"; ctx.shadowColor = "#0ff"; }
                else { ctx.fillStyle = "#ffff0088"; ctx.shadowColor = "#ff0"; }
                ctx.fillRect(plat.x, plat.y, plat.w, plat.h);
                ctx.strokeStyle = "#fff";
                ctx.strokeRect(plat.x, plat.y, plat.w, plat.h);
            }
            ctx.shadowBlur = 0;
        }
        function drawHelmet() {
            if(!helmetItem.collected) {
                ctx.fillStyle = "#FFD700";
                ctx.shadowBlur = 15;
                ctx.shadowColor = "#ff0";
                ctx.fillRect(helmetItem.x, helmetItem.y, helmetItem.w, helmetItem.w);
                ctx.fillStyle = "#FFA500";
                ctx.fillRect(helmetItem.x+4, helmetItem.y+4, 12, 4);
                ctx.fillStyle = "#FFF";
                ctx.fillRect(helmetItem.x+8, helmetItem.y+10, 4, 6);
                ctx.font = "bold 14px monospace";
                ctx.fillStyle = "#ff0";
                ctx.fillText("🛡️", helmetItem.x+2, helmetItem.y-2);
            }
        }
        function drawCoins() {
            for(let c of coins) {
                ctx.fillStyle = "#FFD700";
                ctx.shadowBlur = 8;
                ctx.shadowColor = "#ff0";
                ctx.beginPath();
                ctx.ellipse(c.x+5, c.y+5, 6, 8, 0, 0, Math.PI*2);
                ctx.fill();
            }
        }
        function drawGems() {
            for(let g of gems) {
                ctx.fillStyle = "#0ff";
                ctx.shadowBlur = 10;
                ctx.shadowColor = "#0ff";
                ctx.beginPath();
                ctx.moveTo(g.x+8, g.y);
                ctx.lineTo(g.x+16, g.y+8);
                ctx.lineTo(g.x+8, g.y+16);
                ctx.lineTo(g.x, g.y+8);
                ctx.fill();
            }
        }
        function drawEnemies() {
            for(let e of enemies) {
                ctx.fillStyle = e.type === 'cactus' ? "#ff4444" : "#ff6600";
                ctx.shadowColor = e.type === 'cactus' ? "#f00" : "#f60";
                ctx.shadowBlur = 8;
                ctx.fillRect(e.x, e.y, e.w, e.h);
            }
        }
        function drawPlayer() {
            ctx.save();
            if(player.invincibleTimer > 0 && (Math.floor(Date.now()/60)%3 === 0)) ctx.globalAlpha = 0.5;
            ctx.fillStyle = player.hasHelmet ? "#ffaa44" : "#0ff";
            ctx.shadowColor = player.hasHelmet ? "#ff0" : "#0ff";
            ctx.shadowBlur = 15;
            ctx.fillRect(player.x, player.y, player.width, player.height);
            if(player.hasHelmet) {
                ctx.fillStyle = "#FFD700";
                ctx.fillRect(player.x+4, player.y-5, 20, 8);
                ctx.fillStyle = "#FFA500";
                ctx.fillRect(player.x+8, player.y-3, 12, 4);
            }
            ctx.fillStyle = "#fff";
            ctx.fillRect(player.x+5, player.y+5, 6, 6);
            ctx.fillRect(player.x+17, player.y+5, 6, 6);
            ctx.fillStyle = "#000";
            ctx.fillRect(player.x+7, player.y+15, 14, 5);
            if(player.facingRight) {
                ctx.fillStyle = "#ff0";
                ctx.fillRect(player.x+24, player.y+12, 6, 4);
            } else {
                ctx.fillRect(player.x-2, player.y+12, 6, 4);
            }
            ctx.restore();
        }
        function drawUItext() {
            if(floatingMessage.timer > 0) {
                ctx.font = "bold 16px 'Courier New'";
                ctx.fillStyle = "#0ff";
                ctx.shadowBlur = 5;
                ctx.fillText(floatingMessage.text, player.x-40, player.y-30);
                floatingMessage.timer--;
            }
            if(!gameRunning && gameStarted) {
                ctx.font = "bold 36 monospace";
                ctx.fillStyle = "#f0f";
                ctx.fillText("GAME OVER", WIDTH/2-100, HEIGHT/2-50);
                ctx.font = "16px monospace";
                ctx.fillStyle = "#0ff";
                ctx.fillText("Нажми R или Новая игра", WIDTH/2-100, HEIGHT/2+20);
            }
        }
        
        function updateGame() {
            if(gameRunning && gameStarted) {
                handleInput();
                applyPhysicsAndCollisions();
                pickUpHelmet();
                updateCollectibles();
                updateRegeneration();
                if(player.health <= 0) gameRunning = false;
            }
        }
        
        function draw() {
            drawBackground();
            drawPlatforms();
            drawHelmet();
            drawCoins();
            drawGems();
            drawEnemies();
            drawPlayer();
            drawUItext();
        }
        
        function gameLoop() {
            updateGame();
            draw();
            requestAnimationFrame(gameLoop);
        }
        
        window.addEventListener('keydown', (e) => {
            const key = e.key;
            if(['ArrowLeft','ArrowRight','a','d',' ','w','W','r','R'].includes(key)) e.preventDefault();
            if(key === 'ArrowLeft') keys.ArrowLeft = true;
            if(key === 'ArrowRight') keys.ArrowRight = true;
            if(key === 'a') keys.a = true;
            if(key === 'd') keys.d = true;
            if(key === ' ') keys.space = true;
            if(key === 'w' || key === 'W') keys.w = true;
            if(key === 'r' || key === 'R') { if(gameStarted) resetGameData(); }
        });
        
        window.addEventListener('keyup', (e) => {
            const key = e.key;
            if(key === 'ArrowLeft') keys.ArrowLeft = false;
            if(key === 'ArrowRight') keys.ArrowRight = false;
            if(key === 'a') keys.a = false;
            if(key === 'd') keys.d = false;
            if(key === ' ') keys.space = false;
            if(key === 'w' || key === 'W') keys.w = false;
        });
        
        document.getElementById('resetBtnGame').addEventListener('click', () => { if(gameStarted) resetGameData(); });
        
        generateItems();
        updateHealthDisplay();
        updateUI();
        gameLoop();
    })();
</script>
</body>
</html>
