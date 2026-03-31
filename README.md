<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Rocket Clicker: Deep Space ULTIMATE</title>
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
            min-height: 100vh;
            display: flex;
            background-color: var(--space-bg);
            font-family: 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
            color: #333;
            user-select: none;
            overflow-x: hidden; /* Prevents side-scrolling */
        }

        #starfield {
            position: fixed; /* Fixed so it stays behind everything when scrolling */
            width: 100vw;
            height: 100vh;
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

        #fullscreen-btn {
            position: fixed;
            top: 10px;
            right: 10px;
            z-index: 1000;
            background: rgba(255, 255, 255, 0.1);
            border: 1px solid var(--neon-blue);
            color: white;
            padding: 6px 12px;
            border-radius: 20px;
            cursor: pointer;
            font-size: 12px;
            backdrop-filter: blur(5px);
        }

        .sidebar {
            width: 320px;
            min-width: 320px;
            background: var(--panel-bg);
            padding: 20px;
            display: flex;
            flex-direction: column;
            gap: 10px;
            z-index: 100;
            border-right: 5px solid var(--neon-blue);
            box-shadow: 10px 0 30px rgba(0,0,0,0.5);
            height: 100vh;
            box-sizing: border-box;
            overflow-y: auto;
        }

        .stats {
            text-align: center;
            background: linear-gradient(135deg, var(--neon-blue), #00a2ff);
            padding: 15px;
            border-radius: 15px;
            color: white;
        }

        .km-count { font-size: 28px; font-weight: 900; margin: 0; }

        .tabs { display: flex; gap: 4px; margin-top: 10px; }
        .tab-btn { flex: 1; padding: 8px 2px; font-size: 11px; font-weight: bold; cursor: pointer; border: none; border-radius: 6px; background: #e0e0e0; }
        .tab-btn.active { background: var(--neon-blue); color: white; }
        
        .tab-content { flex: 1; display: none; padding-top: 10px; }
        .tab-content.active { display: block; }

        .upgrade-item, .skin-item, .planet-item {
            background: #fff;
            padding: 10px;
            border-radius: 10px;
            margin-bottom: 8px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            border: 1px solid #ddd;
            font-size: 13px;
        }

        .price-tag { background: var(--gold-bg); padding: 2px 6px; border-radius: 4px; font-weight: bold; }
        .disabled { opacity: 0.5; grayscale(1); pointer-events: none; }

        .game-area { 
            flex: 1; 
            display: flex; 
            justify-content: center; 
            align-items: center; 
            position: relative; 
            z-index: 50; 
            min-height: 400px;
        }

        .rocket-container { 
            position: relative; 
            width: 80%; 
            max-width: 300px; 
            aspect-ratio: 1/1; 
            display: flex; 
            justify-content: center; 
            align-items: center; 
        }
        
        #rocket { 
            font-size: clamp(80px, 15vw, 120px); 
            cursor: pointer; 
            transition: transform 0.1s; 
        }
        #rocket:active { transform: scale(0.8) translateY(-20px); }

        .satellite {
            position: absolute; font-size: 25px; pointer-events: none;
            animation: orbit var(--dur) linear infinite;
        }

        @keyframes orbit {
            from { transform: rotate(0deg) translateX(var(--dist)) rotate(0deg); }
            to   { transform: rotate(360deg) translateX(var(--dist)) rotate(-360deg); }
        }

        .float-text { position: absolute; color: #fff; font-weight: 900; pointer-events: none; animation: float 0.8s forwards; z-index: 200; }
        @keyframes float { 0% { transform: translateY(0); opacity: 1; } 100% { transform: translateY(-100px); opacity: 0; } }

        /* RESPONSIVE DESIGN FOR SMALL SCREENS */
        @media (max-width: 768px) {
            body { flex-direction: column; overflow-y: auto; }
            .sidebar { 
                width: 100%; 
                min-width: 100%; 
                height: auto; 
                border-right: none; 
                border-bottom: 5px solid var(--neon-blue); 
            }
            .game-area { padding: 40px 0; }
            #fullscreen-btn { display: none; } /* Fullscreen is buggy on small mobile embeds */
        }

        #save-indicator { position: fixed; bottom: 10px; right: 10px; color: white; font-size: 10px; opacity: 0; transition: 0.5s; z-index: 1000; }
    </style>
</head>
<body>

<div id="starfield"></div>
<button id="fullscreen-btn" onclick="toggleFullscreen()">⛶ Fullscreen</button>
<div id="save-indicator">Data Synced 🛰️</div>

<div class="sidebar">
    <div class="stats">
        <div class="km-count" id="score">0</div>
        <div style="font-size: 10px; letter-spacing: 1px;">KILOMETERS</div>
        <div style="font-size: 10px; margin-top:5px;">
            +<span id="click-stat">1</span>/click | <span id="auto-stat">0</span>/sec
        </div>
        <button id="prestige-btn" onclick="prestige()" style="display:none; width:100%; margin-top:10px; background:var(--prestige-purple); color:white; border:none; padding:5px; border-radius:5px; cursor:pointer;">PRESTIGE</button>
    </div>

    <div class="tabs">
        <button class="tab-btn active" onclick="showTab('shop')">Shop</button>
        <button class="tab-btn" onclick="showTab('skins')">Skins</button>
        <button class="tab-btn" onclick="showTab('planets')">Travel</button>
    </div>

    <div id="shop" class="tab-content active"><div id="shop-container"></div></div>
    
    <div id="skins" class="tab-content">
        <div class="skin-item" onclick="changeSkin('🚀', 0)"><span>Classic</span><span>Free</span></div>
        <div class="skin-item" onclick="changeSkin('🛸', 50000)"><span>UFO</span><span class="price-tag">50k</span></div>
    </div>

    <div id="planets" class="tab-content">
        <div class="planet-item" onclick="travel('Mars', 1000000)"><span>Mars</span><span class="price-tag">1M</span></div>
    </div>

    <button onclick="resetGame()" style="margin-top:auto; background:none; border:none; color:red; cursor:pointer; font-size:10px; padding:10px;">Reset Data</button>
</div>

<div class="game-area">
    <div class="rocket-container" id="rocket-holder">
        <div id="rocket">🚀</div>
    </div>
</div>

<script>
    // --- GAME LOGIC ---
    let score = 0;
    let clickPower = 1;
    let autoPower = 0;
    let satelliteCount = 0;
    let prestigeMultiplier = 1;
    const SAVE_KEY = "RocketClicker_Save_Final";

    let upgrades = [
        { name: 'Thrusters', cost: 50, power: 2, type: 'click' },
        { name: 'Drone', cost: 100, power: 5, type: 'auto', visual: '🛰️' },
        { name: 'Reactor', cost: 2000, power: 20, type: 'click' },
        { name: 'Station', cost: 10000, power: 150, type: 'auto' }
    ];

    function updateUI() {
        document.getElementById('score').innerText = Math.floor(score).toLocaleString();
        document.getElementById('click-stat').innerText = (clickPower * prestigeMultiplier);
        document.getElementById('auto-stat').innerText = (autoPower * prestigeMultiplier);
        
        upgrades.forEach((u, i) => {
            const el = document.getElementById(`upg-${i}`);
            if(el) score >= u.cost ? el.classList.remove('disabled') : el.classList.add('disabled');
        });
    }

    function buy(i) {
        let u = upgrades[i];
        if (score >= u.cost) {
            score -= u.cost;
            if (u.type === 'click') clickPower += u.power;
            else { autoPower += u.power; if(u.visual) spawnVisualSatellite(u.visual); }
            u.cost = Math.floor(u.cost * 1.6);
            renderShop(); updateUI(); saveGame();
        }
    }

    function spawnVisualSatellite(emoji) {
        const sat = document.createElement('div');
        sat.className = 'satellite'; sat.innerText = emoji;
        sat.style.setProperty('--dist', (100 + Math.random() * 50) + 'px');
        sat.style.setProperty('--dur', (3 + Math.random() * 3) + 's');
        document.getElementById('rocket-holder').appendChild(sat);
    }

    function renderShop() {
        document.getElementById('shop-container').innerHTML = upgrades.map((u, i) => `
            <div id="upg-${i}" class="upgrade-item" onclick="buy(${i})">
                <div><b>${u.name}</b></div>
                <div class="price-tag">${u.cost.toLocaleString()}</div>
            </div>`).join('');
    }

    function showTab(id) {
        document.querySelectorAll('.tab-content').forEach(c => c.classList.remove('active'));
        document.querySelectorAll('.tab-btn').forEach(b => b.classList.remove('active'));
        document.getElementById(id).classList.add('active');
        event.currentTarget.classList.add('active');
    }

    function spawnText(x, y, t) {
        const div = document.createElement('div'); div.className = 'float-text';
        div.style.left = x + 'px'; div.style.top = y + 'px'; div.innerText = t;
        document.body.appendChild(div); setTimeout(() => div.remove(), 800);
    }

    function saveGame() {
        localStorage.setItem(SAVE_KEY, JSON.stringify({score, clickPower, autoPower, upgrades}));
        const ind = document.getElementById('save-indicator');
        ind.style.opacity = 1; setTimeout(() => ind.style.opacity = 0, 1000);
    }

    function loadGame() {
        const saved = JSON.parse(localStorage.getItem(SAVE_KEY));
        if(saved) {
            score = saved.score; clickPower = saved.clickPower; autoPower = saved.autoPower;
            if(saved.upgrades) upgrades = saved.upgrades;
        }
    }

    function toggleFullscreen() {
        if (!document.fullscreenElement) document.documentElement.requestFullscreen();
        else document.exitFullscreen();
    }

    function init() {
        // Starfield
        for(let i=0; i<80; i++) {
            const s = document.createElement('div'); s.className = 'star';
            s.style.width = s.style.height = Math.random()*2+'px';
            s.style.left = Math.random()*100+'%'; s.style.top = Math.random()*100+'%';
            s.style.setProperty('--t', (2+Math.random()*2)+'s');
            document.getElementById('starfield').appendChild(s);
        }

        document.getElementById('rocket').onclick = (e) => {
            let p = clickPower * prestigeMultiplier; score += p;
            spawnText(e.clientX, e.clientY, "+"+p); updateUI();
        };

        loadGame(); renderShop(); updateUI();
        setInterval(() => { if(autoPower > 0) { score += (autoPower * prestigeMultiplier)/10; updateUI(); } }, 100);
    }

    function resetGame() { if(confirm("Clear all progress?")) { localStorage.clear(); location.reload(); } }

    init();
</script>
</body>
</html>
