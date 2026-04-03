#
qwertyuiooiuytrsasrtyuiopoiuytrewertyui543ertyuicvbnmnbvdtyujnbvcdtyujhvcdrtyujnbvfrtyuydsxcvbnmjhgfdertyuytrsxcvbnjhgfdrtyuhgfvbnmmcxdftygfdsw4rdszzdfbvcdrtyhjkliuhgfdserthjkiuytfdsert
THIS GAME IS A CLICKER GAME WERE YOU CLICK A ROCKET TO KM(MONEY)
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Rocket Clicker: Deep Space</title>
    <style>
        :root {
            --space-bg: #020205;
            --neon-blue: #00d2ff;
            --gold-bg: #ffd54f;
            --prestige-purple: #a100ff;
            --panel-bg: rgba(255, 255, 255, 0.95);
        }

        body {
            margin: 0;
            height: 100vh;
            display: flex;
            background-color: var(--space-bg);
            font-family: 'Segoe UI', Arial, sans-serif;
            overflow: hidden;
            color: #333;
            user-select: none;
        }

        #starfield {
            position: absolute;
            width: 100%;
            height: 100%;
            z-index: 1;
            background: radial-gradient(circle at center, #0a0a20 0%, #020205 100%);
        }

        .star {
            position: absolute;
            background: white;
            border-radius: 50%;
            animation: twinkle var(--t) infinite ease-in-out;
        }

        @keyframes twinkle {
            0%, 100% { opacity: 0.3; transform: scale(1); }
            50% { opacity: 1; transform: scale(1.2); }
        }

        .sidebar {
            width: 280px;
            background: var(--panel-bg);
            padding: 15px;
            display: flex;
            flex-direction: column;
            gap: 10px;
            z-index: 100;
            border-right: 5px solid var(--neon-blue);
            box-shadow: 5px 0 25px rgba(0,0,0,0.5);
        }

        h1 { margin: 0; font-size: 22px; color: #007bff; }
        .intro-text { font-size: 10px; color: #666; margin-bottom: 5px; }

        .fullscreen-btn {
            background: #222;
            color: #fff;
            border: none;
            padding: 8px;
            border-radius: 8px;
            cursor: pointer;
            font-size: 11px;
            font-weight: bold;
            margin-bottom: 5px;
        }

        .stats {
            text-align: center;
            background: var(--neon-blue);
            padding: 12px;
            border-radius: 12px;
            color: white;
        }

        .km-count { font-size: 28px; font-weight: 900; margin: 0; }

        .tabs { display: flex; gap: 4px; margin-top: 5px; }
        .tab-btn { flex: 1; padding: 6px 2px; font-size: 10px; font-weight: bold; cursor: pointer; border: none; border-radius: 5px; background: #ccc; }
        .tab-btn.active { background: var(--neon-blue); color: white; }
        
        .tab-content { flex: 1; overflow-y: auto; display: none; padding-top: 10px; }
        .tab-content.active { display: block; }

        .upgrade-item, .skin-item {
            background: #fff;
            border: 1px solid #ddd;
            padding: 8px;
            border-radius: 10px;
            cursor: pointer;
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 6px;
            font-size: 12px;
        }

        .upgrade-item:hover:not(.disabled) { border-color: var(--neon-blue); }
        .disabled { opacity: 0.5; cursor: not-allowed; filter: grayscale(1); }
        .price-tag { background: var(--gold-bg); padding: 2px 5px; border-radius: 4px; font-weight: 900; font-size: 10px; color: #000; }

        .game-area { flex: 1; display: flex; justify-content: center; align-items: center; z-index: 50; position: relative; }
        
        /* KEPT THE SMALLER SIZE */
        .rocket-container { 
            position: relative; 
            width: 200px; 
            height: 200px; 
            display: flex; 
            justify-content: center; 
            align-items: center;
            margin-top: -40px; 
        }

        #rocket { 
            font-size: 80px; 
            cursor: pointer; 
            transition: transform 0.05s; 
            z-index: 10; 
        }
        #rocket:active { transform: scale(0.9); }

        .satellite {
            position: absolute; 
            font-size: 22px; 
            pointer-events: none; 
            left: 50%; top: 50%; 
            margin-left: -11px; margin-top: -11px;
            animation: orbit var(--dur) linear infinite;
        }
        @keyframes orbit {
            from { transform: rotate(0deg) translateX(var(--dist)) rotate(0deg); }
            to   { transform: rotate(360deg) translateX(var(--dist)) rotate(-360deg); }
        }

        .float-text { position: absolute; color: #fff; font-weight: 900; font-size: 20px; text-shadow: 1px 1px #000; pointer-events: none; animation: float 0.7s forwards; z-index: 200; }
        @keyframes float { 0% { transform: translateY(0); opacity: 1; } 100% { transform: translateY(-80px); opacity: 0; } }
        
        #save-indicator { position: absolute; bottom: 10px; right: 10px; color: white; font-size: 10px; opacity: 0; transition: opacity 0.5s; z-index: 1000; }
    </style>
</head>
<body>

<div id="starfield"></div>
<div id="save-indicator">Progress Saved...</div>

<div class="sidebar">
    <h1>Rocket Clicker</h1>
    <div class="intro-text">CLICK THE ROCKET TO EARN KM!</div>
    
    <button class="fullscreen-btn" onclick="openInNewTab()">🚀 Launch Fullscreen Tab</button>

    <div class="stats">
        <div class="km-count" id="score">0</div>
        <div style="font-size: 9px; font-weight: bold;">KILOMETERS</div>
        <div style="font-size: 9px; margin-top:4px;">+<span id="click-stat">1</span>/click | <span id="auto-stat">0</span> km/s</div>
    </div>

    <div class="tabs">
        <button class="tab-btn active" onclick="showTab('shop')">Shop</button>
        <button class="tab-btn" onclick="showTab('skins')">Skins</button>
    </div>

    <div id="shop" class="tab-content active">
        <div id="shop-container"></div>
    </div>
    
    <div id="skins" class="tab-content">
        <div class="skin-item" onclick="changeSkin('🚀', 0)"><span>Classic</span><span>Free</span></div>
        <div class="skin-item" onclick="changeSkin('🛸', 5000)"><span>UFO</span><span class="price-tag">5k</span></div>
        <div class="skin-item" onclick="changeSkin('🛰️', 25000)"><span>Probe</span><span class="price-tag">25k</span></div>
    </div>

    <button onclick="resetGame()" style="background:none; border:none; color:red; cursor:pointer; font-size:9px; margin-top: auto; align-self: center;">Hard Reset</button>
</div>

<div class="game-area">
    <div class="rocket-container" id="rocket-holder">
        <div id="rocket">🚀</div>
    </div>
</div>

<script>
    let score = 0;
    let clickPower = 1;
    let autoPower = 0;
    let satelliteCount = 0;
    let currentSkin = '🚀';

    let upgrades = [
        { id: 'c1', name: 'Ion Thrusters', cost: 50, power: 2, type: 'click' },
        { id: 'a1', name: 'Satellites', cost: 100, power: 5, type: 'auto', visual: '🛰️' },
        { id: 'c2', name: 'Fusion Engine', cost: 1000, power: 20, type: 'click' },
        { id: 'a2', name: 'Space Station', cost: 5000, power: 100, type: 'auto' }
    ];

    function openInNewTab() { saveGame(); window.open(window.location.href, '_blank'); }

    function showTab(tabId) {
        document.querySelectorAll('.tab-content').forEach(c => c.classList.remove('active'));
        document.querySelectorAll('.tab-btn').forEach(b => b.classList.remove('active'));
        document.getElementById(tabId).classList.add('active');
        event.currentTarget.classList.add('active');
    }

    function buy(index) {
        const u = upgrades[index];
        if (score >= u.cost) {
            score -= u.cost;
            if (u.type === 'click') clickPower += u.power;
            else { autoPower += u.power; if (u.visual) spawnVisualSatellite(u.visual); }
            u.cost = Math.floor(u.cost * 1.5);
            updateUI();
            saveGame();
        }
    }

    function changeSkin(emoji, cost) {
        if (score >= cost) {
            score -= cost;
            currentSkin = emoji;
            document.getElementById('rocket').innerText = emoji;
            updateUI();
            saveGame();
        }
    }

    function spawnVisualSatellite(emoji) {
        satelliteCount++;
        const holder = document.getElementById('rocket-holder');
        const sat = document.createElement('div');
        sat.className = 'satellite';
        sat.innerText = emoji;
        sat.style.setProperty('--dist', (70 + Math.random() * 30) + 'px');
        sat.style.setProperty('--dur', (3 + Math.random() * 4) + 's');
        holder.appendChild(sat);
    }

    function updateUI() {
        document.getElementById('score').innerText = Math.floor(score).toLocaleString();
        document.getElementById('click-stat').innerText = clickPower;
        document.getElementById('auto-stat').innerText = autoPower;
        
        const container = document.getElementById('shop-container');
        container.innerHTML = upgrades.map((u, i) => `
            <div class="upgrade-item ${score < u.cost ? 'disabled' : ''}" onclick="buy(${i})">
                <div><b>${u.name}</b></div>
                <div class="price-tag">${Math.floor(u.cost).toLocaleString()}</div>
            </div>
        `).join('');
    }

    function saveGame() {
        const data = { score, clickPower, autoPower, satelliteCount, currentSkin, upgrades };
        localStorage.setItem('rocketClicker_Final', JSON.stringify(data));
        const ind = document.getElementById('save-indicator');
        ind.style.opacity = "1"; setTimeout(() => ind.style.opacity = "0", 1500);
    }

    function loadGame() {
        const saved = localStorage.getItem('rocketClicker_Final');
        if (saved) {
            const data = JSON.parse(saved);
            score = data.score || 0;
            clickPower = data.clickPower || 1;
            autoPower = data.autoPower || 0;
            upgrades = data.upgrades || upgrades;
            currentSkin = data.currentSkin || '🚀';
            document.getElementById('rocket').innerText = currentSkin;
            for(let i=0; i<(data.satelliteCount || 0); i++) spawnVisualSatellite('🛰️');
        }
    }

    document.getElementById('rocket').onclick = (e) => {
        score += clickPower;
        const t = document.createElement('div');
        t.className = 'float-text';
        t.style.left = e.clientX + 'px'; t.style.top = e.clientY + 'px';
        t.innerText = `+${clickPower}`;
        document.body.appendChild(t);
        setTimeout(() => t.remove(), 700);
        updateUI();
    };

    function init() {
        loadGame();
        const field = document.getElementById('starfield');
        for (let i = 0; i < 100; i++) {
            const star = document.createElement('div');
            star.className = 'star';
            star.style.width = star.style.height = Math.random() * 2 + 'px';
            star.style.left = Math.random() * 100 + '%';
            star.style.top = Math.random() * 100 + '%';
            star.style.setProperty('--t', (2 + Math.random() * 3) + 's');
            field.appendChild(star);
        }
        setInterval(() => { if (autoPower > 0) { score += autoPower / 10; updateUI(); } }, 100);
        setInterval(saveGame, 10000);
        updateUI();
    }

    function resetGame() { if(confirm("Hard Reset?")) { localStorage.clear(); location.reload(); } }
    init();
</script>
</body>
</html>
