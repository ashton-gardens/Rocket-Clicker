THIS GAME IS A CLICKER GAME WERE YOU CLICK A ROCKET TO KM(MONEY)
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-select=none">
    <title>Rocket Clicker: Deep Space ULTIMATE (Responsive)</title>
    <style>
        :root {
            --space-bg: #020205;
            --neon-blue: #00d2ff;
            --gold-bg: #ffd54f;
            --prestige-purple: #a100ff;
            --panel-bg: rgba(255, 255, 255, 0.95);
        }

        /* FIX 1: Allow scrolling on mobile but hide it on desktop */
        body {
            margin: 0;
            min-height: 100vh;
            display: flex;
            background-color: var(--space-bg);
            font-family: 'Segoe UI', Arial, sans-serif;
            color: #333;
            user-select: none;
            overflow-x: hidden;
        }

        #starfield {
            position: fixed; /* Changed to fixed so it stays in background */
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
            position: fixed;
            top: 10px;
            right: 10px;
            z-index: 1000;
            background: rgba(255, 255, 255, 0.2);
            border: 1px solid var(--neon-blue);
            color: white;
            padding: 5px 10px;
            border-radius: 8px;
            cursor: pointer;
            font-size: 12px;
            backdrop-filter: blur(5px);
        }

        /* FIX 2: Sidebar becomes top-bar on small screens */
        .sidebar {
            width: 320px;
            min-width: 320px;
            background: var(--panel-bg);
            padding: 15px;
            display: flex;
            flex-direction: column;
            gap: 10px;
            z-index: 100;
            border-right: 5px solid var(--neon-blue);
            box-shadow: 5px 0 25px rgba(0,0,0,0.5);
            height: 100vh;
            box-sizing: border-box;
            overflow-y: auto;
        }

        .stats {
            text-align: center;
            background: var(--neon-blue);
            padding: 15px;
            border-radius: 15px;
            margin-bottom: 5px;
        }

        .km-count {
            font-size: clamp(24px, 5vw, 32px);
            font-weight: 900;
            color: #ffffff; 
            text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
            margin: 0;
        }

        .tabs { display: flex; gap: 5px; margin-bottom: 5px; }
        .tab-btn { flex: 1; padding: 8px 2px; font-size: 11px; font-weight: bold; cursor: pointer; border: none; border-radius: 5px; background: #ccc; }
        .tab-btn.active { background: var(--neon-blue); color: white; }
        
        .tab-content { flex: 1; display: none; padding-right: 5px; }
        .tab-content.active { display: block; }

        .upgrade-item, .skin-item, .planet-item, .ach-item {
            background: #fff;
            border: 2px solid #ddd;
            padding: 8px;
            border-radius: 12px;
            cursor: pointer;
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 8px;
            font-size: 12px;
        }

        .disabled { opacity: 0.5 !important; cursor: not-allowed; filter: grayscale(1); }
        .price-tag { background: var(--gold-bg); color: #000; padding: 3px 6px; border-radius: 6px; font-weight: 900; }

        .game-area { flex: 1; display: flex; justify-content: center; align-items: center; z-index: 50; position: relative; min-height: 350px; }
        
        /* FIX 3: Rocket scales with screen width */
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
            font-size: clamp(80px, 20vw, 150px); 
            cursor: pointer; 
            transition: transform 0.05s; 
            z-index: 10; 
        }
        #rocket:active { transform: scale(0.85) translateY(-20px); }

        .satellite {
            position: absolute; font-size: 25px; pointer-events: none;
            animation: orbit var(--dur) linear infinite;
        }
        @keyframes orbit {
            from { transform: rotate(0deg) translateX(var(--dist)) rotate(0deg); }
            to   { transform: rotate(360deg) translateX(var(--dist)) rotate(-360deg); }
        }

        .float-text { position: absolute; color: #fff; font-weight: 900; font-size: 24px; text-shadow: 2px 2px #000; pointer-events: none; animation: float 0.8s forwards; z-index: 200; }
        @keyframes float { 0% { transform: translateY(0); opacity: 1; } 100% { transform: translateY(-120px); opacity: 0; } }
        
        #prestige-btn { display: none; background: var(--prestige-purple); color: white; border: none; padding: 10px; border-radius: 10px; font-weight: bold; cursor: pointer; width: 100%; margin-top: 5px; }
        #save-indicator { position: fixed; bottom: 10px; right: 10px; color: white; font-size: 10px; opacity: 0; transition: 0.5s; z-index: 1000; }

        /* FINAL RESPONSIVE OVERRIDE */
        @media (max-width: 768px) {
            body { flex-direction: column; overflow-y: auto; }
            .sidebar { width: 100%; height: auto; min-width: 100%; border-right: none; border-bottom: 5px solid var(--neon-blue); }
            .game-area { padding: 50px 0; }
        }
    </style>
</head>
<body>

<div id="starfield"></div>
<button id="fullscreen-btn" onclick="toggleFullscreen()">⛶ Fullscreen</button>
<div id="save-indicator">Data Synced 🛰️</div>

<div class="sidebar">
    <div class="stats">
        <div class="km-count" id="score">0</div>
        <div style="color:white; font-size: 10px; font-weight: bold;">KILOMETERS</div>
        <div style="color:white; font-size: 9px; margin-top:4px;">+<span id="click-stat">1</span>/click | <span id="auto-stat">0</span> km/s</div>
        <button id="prestige-btn" onclick="prestige()">PRESTIGE (+<span id="pending-coins">0</span>)</button>
    </div>

    <div class="tabs">
        <button class="tab-btn active" onclick="showTab('shop')">Shop</button>
        <button class="tab-btn" onclick="showTab('skins')">Skins</button>
        <button class="tab-btn" onclick="showTab('planets')">Planets</button>
        <button class="tab-btn" onclick="showTab('ach')">🏆</button>
    </div>

    <div id="shop" class="tab-content active"><div id="shop-container"></div></div>
    
    <div id="skins" class="tab-content">
        <div class="skin-item" onclick="changeSkin('🚀', 0)"><span>Classic</span><span>Free</span></div>
        <div class="skin-item" onclick="changeSkin('🛸', 50000)"><span>UFO</span><span class="price-tag">50k</span></div>
        <div class="skin-item" onclick="changeSkin('🛰️', 500000)"><span>Probe</span><span class="price-tag">500k</span></div>
        <div class="skin-item" onclick="changeSkin('👾', 10000000)"><span>Alien</span><span class="price-tag">10M</span></div>
    </div>

    <div id="planets" class="tab-content">
        <div class="planet-item" onclick="travel('Earth', 0)"><span>Earth</span><span>📍</span></div>
        <div class="planet-item" onclick="travel('Mars', 1000000)"><span>Mars</span><span class="price-tag">1M</span></div>
        <div class="planet-item" onclick="travel('Jupiter', 50000000)"><span>Jupiter</span><span class="price-tag">50M</span></div>
    </div>

    <div id="ach" class="tab-content">
        <div class="ach-item"><span>1,000km Reached</span><span id="ach-1">🔒</span></div>
        <div class="ach-item"><span>Satellite Squad</span><span id="ach-2">🔒</span></div>
    </div>

    <button onclick="resetGame()" style="background:none; border:none; color:red; cursor:pointer; font-size:10px; margin-top: 20px; align-self: center;">Hard Reset</button>
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
        if(event) event.currentTarget.classList.add('active');
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
            if(data.upgradeCosts) data.upgradeCosts.forEach((c, i) => upgrades[i].cost = c);
            for(let i=0; i<(data.satelliteCount || 0); i++) spawnVisualSatellite('🛰️', true);
            
            let offline = Math.floor((Date.now() - (data.time || Date.now())) / 1000);
            if (offline > 30 && autoPower > 0) {
                let gain = offline * autoPower;
                score += gain;
                alert(`Offline distance: ${gain.toLocaleString()} km!`);
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
            u.cost = Math.floor(u.cost * 1.6);
            updateUI(); saveGame();
        }
    }

    function spawnVisualSatellite(emoji, load = false) {
        if(!load) satelliteCount++;
        const sat = document.createElement('div');
        sat.className = 'satellite'; sat.innerText = emoji;
        sat.style.setProperty('--dist', (120 + Math.random() * 40) + 'px');
        sat.style.setProperty('--dur', (4 + Math.random() * 4) + 's');
        document.getElementById('rocket-holder').appendChild(sat);
    }

    function updateUI() {
        document.getElementById('score').innerText = Math.floor(score).toLocaleString();
        document.getElementById('click-stat').innerText = clickPower;
        document.getElementById('auto-stat').innerText = autoPower;
        
        upgrades.forEach((u, i) => {
            const el = document.getElementById(`upg-${i}`);
            if(el) {
                const pr = document.getElementById(`price-${i}`);
                pr.innerText = Math.floor(u.cost).toLocaleString();
                score >= u.cost ? el.classList.remove('disabled') : el.classList.add('disabled');
            }
        });

        if (score >= PRESTIGE_THRESHOLD) {
            document.getElementById('prestige-btn').style.display = 'block';
            document.getElementById('pending-coins').innerText = Math.floor(score / PRESTIGE_THRESHOLD);
        }
        if (score >= 1000) document.getElementById('ach-1').innerText = '✅';
        if (satelliteCount >= 5) document.getElementById('ach-2').innerText = '✅';
    }

    function prestige() {
        if(confirm("Prestige? You lose km but gain permanent 2x speed!")) {
            prestigeMultiplier *= 2;
            score = 0; clickPower = 1; autoPower = 0; satelliteCount = 0;
            localStorage.removeItem(SAVE_KEY); location.reload();
        }
    }

    function renderShop() {
        document.getElementById('shop-container').innerHTML = upgrades.map((u, i) => `
            <div id="upg-${i}" class="upgrade-item" onclick="buy(${i})">
                <div><b>${u.name}</b><br><small>${u.desc}</small></div>
                <div id="price-${i}" class="price-tag">${u.cost}</div>
            </div>`).join('');
    }

    function changeSkin(e, c) { if(score >= c) { score -= c; currentSkin = e; document.getElementById('rocket').innerText = e; updateUI(); saveGame(); } }
    function travel(n, c) { alert(score >= c ? "Reached "+n : "Too far!"); }
    
    function spawnText(x, y, t) {
        const div = document.createElement('div'); div.className = 'float-text';
        div.style.left = x+'px'; div.style.top = y+'px'; div.innerText = t;
        document.body.appendChild(div); setTimeout(() => div.remove(), 800);
    }

    function init() {
        const sf = document.getElementById('starfield');
        for(let i=0; i<100; i++) {
            const s = document.createElement('div'); s.className = 'star';
            s.style.width = s.style.height = Math.random()*2+'px';
            s.style.left = Math.random()*100+'%'; s.style.top = Math.random()*100+'%';
            s.style.setProperty('--t', (2+Math.random()*3)+'s'); sf.appendChild(s);
        }
        document.getElementById('rocket').onclick = (e) => {
            let p = clickPower * prestigeMultiplier; score += p;
            spawnText(e.clientX, e.clientY, "+"+p); updateUI();
        };
        loadGame();
        setInterval(() => { if(autoPower > 0) { score += (autoPower * prestigeMultiplier)/10; updateUI(); } }, 100);
    }

    function resetGame() { if(confirm("Reset everything?")) { localStorage.clear(); location.reload(); } }
    init();
</script>
</body>
</html>
