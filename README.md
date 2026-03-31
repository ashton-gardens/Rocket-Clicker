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
            height: 100vh;
            display: flex;
            background-color: var(--space-bg);
            font-family: 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
            overflow: hidden;
            color: #333;
            user-select: none;
        }

        /* Improved Background */
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

        #fullscreen-btn {
            position: absolute;
            top: 20px;
            right: 20px;
            z-index: 1000;
            background: rgba(255, 255, 255, 0.1);
            border: 1px solid var(--neon-blue);
            color: white;
            padding: 8px 16px;
            border-radius: 20px;
            cursor: pointer;
            font-weight: bold;
            backdrop-filter: blur(5px);
            transition: 0.3s;
        }
        #fullscreen-btn:hover { background: var(--neon-blue); color: black; }

        .sidebar {
            width: 320px;
            background: var(--panel-bg);
            padding: 20px;
            display: flex;
            flex-direction: column;
            gap: 10px;
            z-index: 100;
            border-right: 5px solid var(--neon-blue);
            box-shadow: 10px 0 30px rgba(0,0,0,0.5);
        }

        .stats {
            text-align: center;
            background: linear-gradient(135deg, var(--neon-blue), #00a2ff);
            padding: 20px;
            border-radius: 15px;
            margin-bottom: 5px;
            box-shadow: 0 4px 15px rgba(0,210,255,0.3);
        }

        .km-count {
            font-size: 36px;
            font-weight: 900;
            color: #ffffff; 
            text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
            margin: 0;
        }

        .tabs { display: flex; gap: 5px; margin-bottom: 5px; }
        .tab-btn { flex: 1; padding: 10px 2px; font-size: 12px; font-weight: bold; cursor: pointer; border: none; border-radius: 8px; background: #e0e0e0; transition: 0.2s; }
        .tab-btn.active { background: var(--neon-blue); color: white; }
        
        .tab-content { flex: 1; overflow-y: auto; display: none; padding-right: 5px; }
        .tab-content.active { display: block; }

        .upgrade-item, .skin-item, .planet-item, .ach-item {
            background: #fff;
            border: 1px solid #eee;
            padding: 12px;
            border-radius: 12px;
            cursor: pointer;
            transition: 0.2s;
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 10px;
            font-size: 13px;
            box-shadow: 0 2px 5px rgba(0,0,0,0.05);
        }
        .upgrade-item:hover:not(.disabled) { transform: translateX(5px); border-color: var(--neon-blue); }

        .disabled { opacity: 0.5 !important; cursor: not-allowed; filter: grayscale(1); background: #f5f5f5 !important; }
        .price-tag { background: var(--gold-bg); color: #000; padding: 4px 8px; border-radius: 6px; font-weight: 900; }

        .game-area { flex: 1; display: flex; justify-content: center; align-items: center; z-index: 50; position: relative; }
        .rocket-container { position: relative; width: 400px; height: 400px; display: flex; justify-content: center; align-items: center; }
        
        #rocket { 
            font-size: 120px; 
            cursor: pointer; 
            transition: transform 0.1s cubic-bezier(0.175, 0.885, 0.32, 1.275); 
            z-index: 10; 
            filter: drop-shadow(0 0 20px rgba(0,210,255,0.4));
        }
        #rocket:active { transform: scale(0.8) translateY(-30px); }

        .satellite {
            position: absolute; font-size: 30px; pointer-events: none; left: 50%; top: 50%; margin-left: -20px; margin-top: -20px;
            animation: orbit var(--dur) linear infinite;
        }
        @keyframes orbit {
            from { transform: rotate(0deg) translateX(var(--dist)) rotate(0deg); }
            to   { transform: rotate(360deg) translateX(var(--dist)) rotate(-360deg); }
        }

        .float-text { position: absolute; color: #fff; font-weight: 900; font-size: 28px; text-shadow: 2px 2px #000; pointer-events: none; animation: float 0.8s ease-out forwards; z-index: 200; }
        @keyframes float { 0% { transform: translateY(0) scale(1); opacity: 1; } 100% { transform: translateY(-150px) scale(1.2); opacity: 0; } }
        
        #prestige-btn { display: none; background: var(--prestige-purple); color: white; border: none; padding: 12px; border-radius: 10px; font-weight: bold; cursor: pointer; width: 100%; margin-top: 10px; animation: pulse 2s infinite; }
        @keyframes pulse { 0% { opacity: 0.8; } 50% { opacity: 1; box-shadow: 0 0 20px var(--prestige-purple); } 100% { opacity: 0.8; } }

        #save-indicator { position: absolute; bottom: 20px; right: 20px; color: white; font-size: 12px; opacity: 0; transition: opacity 0.5s; z-index: 1000; background: rgba(0,0,0,0.5); padding: 5px 10px; border-radius: 20px; }

        /* Custom Scrollbar */
        .tab-content::-webkit-scrollbar { width: 5px; }
        .tab-content::-webkit-scrollbar-thumb { background: #ccc; border-radius: 10px; }
    </style>
</head>
<body>

<div id="starfield"></div>
<button id="fullscreen-btn" onclick="toggleFullscreen()">⛶ Fullscreen</button>
<div id="save-indicator">Data Synced 🛰️</div>

<div class="sidebar">
    <div class="stats">
        <div class="km-count" id="score">0</div>
        <div style="color:white; font-size: 12px; font-weight: bold; letter-spacing: 2px;">KILOMETERS</div>
        <div style="color:rgba(255,255,255,0.8); font-size: 11px; margin-top:8px;">
            🚀 <span id="click-stat">1</span> km/c | 🛰️ <span id="auto-stat">0</span> km/s
        </div>
        <button id="prestige-btn" onclick="prestige()">ASCEND (+<span id="pending-coins">0</span> Multiplier)</button>
    </div>

    <div class="tabs">
        <button class="tab-btn active" onclick="showTab('shop')">Upgrades</button>
        <button class="tab-btn" onclick="showTab('skins')">Skins</button>
        <button class="tab-btn" onclick="showTab('planets')">Travel</button>
        <button class="tab-btn" onclick="showTab('ach')">🏆</button>
    </div>

    <div id="shop" class="tab-content active"><div id="shop-container"></div></div>
    
    <div id="skins" class="tab-content">
        <div class="skin-item" onclick="changeSkin('🚀', 0)"><span>Classic Rocket</span><span>Free</span></div>
        <div class="skin-item" onclick="changeSkin('🛸', 50000)"><span>UFO</span><span class="price-tag">50k</span></div>
        <div class="skin-item" onclick="changeSkin('🛰️', 500000)"><span>Probe</span><span class="price-tag">500k</span></div>
        <div class="skin-item" onclick="changeSkin('👾', 10000000)"><span>Invader</span><span class="price-tag">10M</span></div>
        <div class="skin-item" onclick="changeSkin('☄️', 100000000)"><span>Comet</span><span class="price-tag">100M</span></div>
    </div>

    <div id="planets" class="tab-content">
        <div class="planet-item" onclick="travel('Earth', 0)"><span>Earth Orbit</span><span>📍</span></div>
        <div class="planet-item" onclick="travel('Mars', 1000000)"><span>Red Planet</span><span class="price-tag">1M</span></div>
        <div class="planet-item" onclick="travel('Jupiter', 50000000)"><span>Gas Giant</span><span class="price-tag">50M</span></div>
        <div class="planet-item" onclick="travel('Pluto', 500000000)"><span>The Edge</span><span class="price-tag">500M</span></div>
    </div>

    <div id="ach" class="tab-content">
        <div class="ach-item"><span>1,000km Reached</span><span id="ach-1">🔒</span></div>
        <div class="ach-item"><span>Satellite Squad (5)</span><span id="ach-2">🔒</span></div>
        <div class="ach-item"><span>Millionaire</span><span id="ach-3">🔒</span></div>
    </div>

    <button onclick="resetGame()" style="background:none; border:none; color:#ff4444; cursor:pointer; font-size:11px; margin-top: auto; align-self: center; padding: 10px;">Hard Reset Data</button>
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
    let prestigeMultiplier = 1;
    const SAVE_KEY = "RocketClicker_Master_Save";
    const PRESTIGE_THRESHOLD = 100000000;

    let upgrades = [
        { id: 'click1', name: 'Ion Thrusters', desc: '+2 per click', cost: 50, power: 2, type: 'click' },
        { id: 'auto1', name: 'Satellites', desc: 'Auto Clicker', cost: 100, power: 5, type: 'auto', visual: '🛰️' },
        { id: 'click2', name: 'Fusion Engine', desc: '+15 per click', cost: 1500, power: 15, type: 'click' },
        { id: 'auto2', name: 'Space Station', desc: '+100 km/s', cost: 5000, power: 100, type: 'auto' },
        { id: 'click3', name: 'Warp Core', desc: '+150 per click', cost: 45000, power: 150, type: 'click' },
        { id: 'auto3', name: 'Space Port', desc: '+1,200 km/s', cost: 120000, power: 1200, type: 'auto' }
    ];

    function toggleFullscreen() {
        if (!document.fullscreenElement) document.documentElement.requestFullscreen();
        else if (document.exitFullscreen) document.exitFullscreen();
    }

    function showTab(tabId) {
        document.querySelectorAll('.tab-content').forEach(c => c.classList.remove('active'));
        document.querySelectorAll('.tab-btn').forEach(b => b.classList.remove('active'));
        document.getElementById(tabId).classList.add('active');
        event.currentTarget.classList.add('active');
    }

    function saveGame() {
        const data = {
            score, clickPower, autoPower, satelliteCount, currentSkin, prestigeMultiplier,
            upgradeCosts: upgrades.map(u => u.cost),
            time: Date.now()
        };
        localStorage.setItem(SAVE_KEY, JSON.stringify(data));
        const ind = document.getElementById('save-indicator');
        ind.style.opacity = 1; setTimeout(() => ind.style.opacity = 0, 1000);
    }

    function loadGame() {
        const saved = localStorage.getItem(SAVE_KEY);
        if (saved) {
            const data = JSON.parse(saved);
            score = data.score || 0;
            clickPower = data.clickPower || 1;
            autoPower = data.autoPower || 0;
            currentSkin = data.currentSkin || '🚀';
            prestigeMultiplier = data.prestigeMultiplier || 1;
            document.getElementById('rocket').innerText = currentSkin;
            if(data.upgradeCosts) data.upgradeCosts.forEach((c, i) => { if(upgrades[i]) upgrades[i].cost = c; });
            for(let i=0; i<(data.satelliteCount || 0); i++) spawnVisualSatellite('🛰️', true);
            
            let offline = Math.floor((Date.now() - (data.time || Date.now())) / 1000);
            if (offline > 30 && autoPower > 0) {
                let gain = offline * autoPower;
                score += gain;
                alert(`Welcome back! Your crew traveled ${gain.toLocaleString()} km while you were away!`);
            }
        }
        renderShop();
        updateUI();
    }

    function buy(i) {
        let u = upgrades[i];
        if (score >= u.cost) {
            score -= u.cost;
            if (u.type === 'click') clickPower += u.power;
            else { autoPower += u.power; if(u.visual) spawnVisualSatellite(u.visual); }
            u.cost = Math.floor(u.cost * 1.55);
            updateUI(); saveGame();
        }
    }

    function spawnVisualSatellite(emoji, load = false) {
        if(!load) satelliteCount++;
        const sat = document.createElement('div');
        sat.className = 'satellite'; sat.innerText = emoji;
        sat.style.setProperty('--dist', (140 + Math.random() * 60) + 'px');
        sat.style.setProperty('--dur', (3 + Math.random() * 5) + 's');
        document.getElementById('rocket-holder').appendChild(sat);
    }

    function updateUI() {
        document.getElementById('score').innerText = Math.floor(score).toLocaleString();
        document.getElementById('click-stat').innerText = (clickPower * prestigeMultiplier).toLocaleString();
        document.getElementById('auto-stat').innerText = (autoPower * prestigeMultiplier).toLocaleString();
        
        upgrades.forEach((u, i) => {
            const el = document.getElementById(`upg-${i}`);
            const pr = document.getElementById(`price-${i}`);
            if(el) {
                pr.innerText = Math.floor(u.cost).toLocaleString();
                score >= u.cost ? el.classList.remove('disabled') : el.classList.add('disabled');
            }
        });

        if (score >= PRESTIGE_THRESHOLD) {
            document.getElementById('prestige-btn').style.display = 'block';
            document.getElementById('pending-coins').innerText = (prestigeMultiplier * 2) + "x";
        }
        
        // Achievements
        if (score >= 1000) document.getElementById('ach-1').innerText = '✅';
        if (satelliteCount >= 5) document.getElementById('ach-2').innerText = '✅';
        if (score >= 1000000) document.getElementById('ach-3').innerText = '✅';
    }

    function prestige() {
        if(confirm("Prestige? You will lose all your current Kilometers and Upgrades, but your speed will DOUBLE permanently!")) {
            prestigeMultiplier *= 2;
            score = 0; clickPower = 1; autoPower = 0; satelliteCount = 0;
            // Reset upgrade costs to base
            upgrades[0].cost = 50; upgrades[1].cost = 100;
            localStorage.removeItem(SAVE_KEY); 
            location.reload();
        }
    }

    function renderShop() {
        document.getElementById('shop-container').innerHTML = upgrades.map((u, i) => `
            <div id="upg-${i}" class="upgrade-item" onclick="buy(${i})">
                <div><b>${u.name}</b><br><small style="color:#666">${u.desc}</small></div>
                <div id="price-${i}" class="price-tag">${u.cost}</div>
            </div>`).join('');
    }

    function changeSkin(e, c) { 
        if(score >= c) { 
            score -= c; 
            currentSkin = e; 
            document.getElementById('rocket').innerText = e; 
            updateUI(); 
            saveGame(); 
        } else {
            alert("Not enough distance traveled!");
        }
    }

    function travel(n, c) { alert(score >= c ? "Engaging warp drive to " + n + "!" : "Engine failure! You need " + c.toLocaleString() + " km to reach " + n); }

    function spawnText(x, y, t) {
        const div = document.createElement('div'); div.className = 'float-text';
        div.style.left = (x - 20) + 'px'; div.style.top = (y - 20) + 'px'; div.innerText = t;
        document.body.appendChild(div); setTimeout(() => div.remove(), 800);
    }

    function init() {
        // Create Stars
        const starfield = document.getElementById('starfield');
        for(let i=0; i<150; i++) {
            const s = document.createElement('div'); s.className = 'star';
            s.style.width = s.style.height = Math.random()*3+'px';
            s.style.left = Math.random()*100+'%'; s.style.top = Math.random()*100+'%';
            s.style.setProperty('--t', (2+Math.random()*4)+'s'); starfield.appendChild(s);
        }

        document.getElementById('rocket').onclick = (e) => {
            let p = clickPower * prestigeMultiplier; 
            score += p;
            spawnText(e.clientX, e.clientY, "+" + p); 
            updateUI();
        };

        loadGame();
        
        // Main Loop
        setInterval(() => { 
            if(autoPower > 0) { 
                score += (autoPower * prestigeMultiplier) / 10; 
                updateUI(); 
            } 
        }, 100);

        // Auto Save
        setInterval(saveGame, 30000);
    }

    function resetGame() { if(confirm("Are you sure? This deletes ALL progress forever.")) { localStorage.clear(); location.reload(); } }
    
    init();
</script>
</body>
</html>
