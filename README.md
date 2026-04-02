THIS GAME IS A CLICKER GAME WERE YOU CLICK A ROCKET TO KM(MONEY)                                                                        
                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 I HOPE YOU HAVE FUN PLAYING MY GAME
                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                
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
            width: 320px;
            background: var(--panel-bg);
            padding: 20px;
            display: flex;
            flex-direction: column;
            gap: 10px;
            z-index: 100;
            border-right: 5px solid var(--neon-blue);
            box-shadow: 5px 0 25px rgba(0,0,0,0.5);
        }

        .fullscreen-btn {
            background: #222;
            color: #fff;
            border: none;
            padding: 8px;
            border-radius: 8px;
            cursor: pointer;
            font-size: 12px;
            font-weight: bold;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 8px;
            margin-bottom: 5px;
            transition: background 0.3s;
        }
        .fullscreen-btn:hover { background: #444; }

        .stats {
            text-align: center;
            background: var(--neon-blue);
            padding: 15px;
            border-radius: 15px;
            margin-bottom: 5px;
        }

        .km-count {
            font-size: 32px;
            font-weight: 900;
            color: #ffffff; 
            text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
            margin: 0;
        }

        .tabs { display: flex; gap: 5px; margin-bottom: 5px; }
        .tab-btn { flex: 1; padding: 8px 2px; font-size: 11px; font-weight: bold; cursor: pointer; border: none; border-radius: 5px; background: #ccc; transition: 0.2s; }
        .tab-btn.active { background: var(--neon-blue); color: white; }
        
        .tab-content { flex: 1; overflow-y: auto; display: none; padding-right: 5px; }
        .tab-content.active { display: block; }

        .upgrade-item, .skin-item, .planet-item, .ach-item {
            background: #fff;
            border: 2px solid #ddd;
            padding: 10px;
            border-radius: 12px;
            cursor: pointer;
            transition: 0.2s;
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 8px;
            color: #333;
            font-size: 13px;
        }

        .upgrade-item:hover:not(.disabled) { border-color: var(--neon-blue); transform: scale(1.02); }
        .disabled { opacity: 0.5 !important; cursor: not-allowed; filter: grayscale(1); }
        .price-tag { background: var(--gold-bg); color: #000; padding: 3px 6px; border-radius: 6px; font-weight: 900; }

        #boss-comet {
            position: absolute; font-size: 50px; z-index: 85; cursor: pointer; display: none;
            filter: drop-shadow(0 0 15px orange);
        }

        .game-area { flex: 1; display: flex; justify-content: center; align-items: center; z-index: 50; position: relative; }
        .rocket-container { position: relative; width: 300px; height: 300px; display: flex; justify-content: center; align-items: center; }
        #rocket { font-size: 150px; cursor: pointer; transition: transform 0.05s; z-index: 10; }
        #rocket:active { transform: scale(0.85) translateY(-20px); }

        .satellite {
            position: absolute; font-size: 30px; pointer-events: none; left: 50%; top: 50%; margin-left: -20px; margin-top: -20px;
            animation: orbit var(--dur) linear infinite;
            z-index: 5;
        }
        @keyframes orbit {
            from { transform: rotate(0deg) translateX(var(--dist)) rotate(0deg); }
            to   { transform: rotate(360deg) translateX(var(--dist)) rotate(-360deg); }
        }

        .float-text { position: absolute; color: #fff; font-weight: 900; font-size: 24px; text-shadow: 2px 2px #000; pointer-events: none; animation: float 0.8s forwards; z-index: 200; }
        @keyframes float { 0% { transform: translateY(0); opacity: 1; } 100% { transform: translateY(-120px); opacity: 0; } }
        
        #prestige-btn { display: none; background: var(--prestige-purple); color: white; border: none; padding: 10px; border-radius: 10px; font-weight: bold; cursor: pointer; width: 100%; margin-top: 5px; }
        #save-indicator { position: absolute; bottom: 10px; right: 10px; color: white; font-size: 10px; opacity: 0; transition: opacity 0.5s; z-index: 1000; }
    </style>
</head>
<body>

<div id="starfield"></div>
<div id="boss-comet">☄️</div>
<div id="save-indicator">Progress Saved...</div>

<div class="sidebar">
    <button class="fullscreen-btn" onclick="openInNewTab()">
        <span>🚀 Launch Fullscreen Tab</span>
    </button>

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

    <div id="shop" class="tab-content active">
        <div id="shop-container"></div>
    </div>
    
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
        <div class="ach-item"><span>First Satellite</span><span id="ach-2">🔒</span></div>
    </div>

    <button onclick="resetGame()" style="background:none; border:none; color:red; cursor:pointer; font-size:10px; margin-top: auto; align-self: center;">Hard Reset</button>
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
    
    let prestigeCoins = 0;
    let prestigeCount = 0;
    let prestigeMultiplier = 1;
    const PRESTIGE_COST = 100000000;

    let upgrades = [
        { id: 'click1', name: 'Ion Thrusters', desc: '+2 per click', cost: 50, power: 2, type: 'click' },
        { id: 'auto1', name: 'Satellites', desc: 'Auto Clicker', cost: 100, power: 5, type: 'auto', visual: '🛰️' },
        { id: 'click2', name: 'Fusion Engine', desc: '+15 per click', cost: 1500, power: 15, type: 'click' },
        { id: 'auto2', name: 'Space Station', desc: '+100 km/s', cost: 5000, power: 100, type: 'auto' },
        { id: 'click3', name: 'Warp Core', desc: '+150 per click', cost: 45000, power: 150, type: 'click' },
        { id: 'auto3', name: 'Space Port', desc: '+1,200 km/s', cost: 120000, power: 1200, type: 'auto' }
    ];

    function openInNewTab() {
        // Saves progress before opening new tab
        saveGame();
        window.open(window.location.href, '_blank');
    }

    function showTab(tabId) {
        document.querySelectorAll('.tab-content').forEach(c => c.classList.remove('active'));
        document.querySelectorAll('.tab-btn').forEach(b => b.classList.remove('active'));
        document.getElementById(tabId).classList.add('active');
        if(event) event.currentTarget.classList.add('active');
    }

    function saveGame() {
        const gameData = { 
            score, clickPower, autoPower, satelliteCount, upgrades, 
            prestigeCoins, prestigeCount, prestigeMultiplier, 
            lastSaveTime: Date.now(), currentSkin 
        };
        localStorage.setItem('rocketClickerV4', JSON.stringify(gameData));
        const indicator = document.getElementById('save-indicator');
        if(indicator) {
            indicator.style.opacity = "1";
            setTimeout(() => indicator.style.opacity = "0", 2000);
        }
    }

    function loadGame() {
        const savedData = localStorage.getItem('rocketClickerV4');
        if (savedData) {
            const data = JSON.parse(savedData);
            score = Number(data.score) || 0;
            clickPower = Number(data.clickPower) || 1;
            autoPower = Number(data.autoPower) || 0;
            satelliteCount = 0; 
            upgrades = data.upgrades || upgrades;
            prestigeCoins = data.prestigeCoins || 0;
            prestigeCount = data.prestigeCount || 0;
            prestigeMultiplier = data.prestigeMultiplier || 1;
            currentSkin = data.currentSkin || '🚀';
            document.getElementById('rocket').innerText = currentSkin;

            const visualCount = data.satelliteCount || 0;
            for (let i = 0; i < visualCount; i++) {
                spawnVisualSatellite('🛰️');
            }
        }
    }

    function buy(index) {
        const u = upgrades[index];
        if (score >= u.cost) {
            score -= u.cost;
            if (u.type === 'click') {
                clickPower += u.power;
            } else {
                autoPower += u.power;
                if (u.visual) spawnVisualSatellite(u.visual);
            }
            u.cost = Math.floor(u.cost * 1.6);
            updateUI();
            saveGame();
        }
    }

    function changeSkin(emoji, cost = 0) {
        if (score >= cost) {
            score -= cost;
            currentSkin = emoji;
            document.getElementById('rocket').innerText = emoji;
            updateUI();
            saveGame();
        } else { alert("Not enough distance!"); }
    }

    function travel(name, cost) {
        if (score >= cost) {
            alert(`Engaging warp drive to ${name}!`);
        } else {
            alert(`Insufficient fuel. You need ${cost.toLocaleString()} km to reach ${name}.`);
        }
    }

    function spawnComet() {
        const comet = document.getElementById('boss-comet');
        comet.style.display = 'block';
        comet.style.top = Math.random() * 80 + '%';
        comet.style.left = '-100px';
        comet.onclick = () => {
            let bonus = Math.floor((autoPower * 60) + 1000);
            score += bonus;
            spawnText(window.innerWidth/2, window.innerHeight/2, "COMET SMASH! +" + bonus.toLocaleString());
            comet.style.display = 'none';
            updateUI();
        };
        let pos = -100;
        let interval = setInterval(() => {
            pos += 6;
            comet.style.left = pos + 'px';
            if (pos > window.innerWidth) { clearInterval(interval); comet.style.display = 'none'; }
        }, 30);
    }

    function prestige() {
        if (score >= PRESTIGE_COST) {
            prestigeCount++;
            prestigeCoins += Math.floor(score / PRESTIGE_COST);
            score = 0;
            saveGame();
            location.reload(); 
        }
    }

    function renderShop() {
        const container = document.getElementById('shop-container');
        if (!container) return;
        container.innerHTML = upgrades.map((u, i) => `
            <div class="upgrade-item ${score < u.cost ? 'disabled' : ''}" onclick="buy(${i})">
                <div><b>${u.name}</b><br><small>${u.desc}</small></div>
                <div class="price-tag">${Math.floor(u.cost).toLocaleString()}</div>
            </div>
        `).join('');
    }

    function spawnVisualSatellite(emoji) {
        satelliteCount++;
        const holder = document.getElementById('rocket-holder');
        const sat = document.createElement('div');
        sat.className = 'satellite';
        sat.innerText = emoji;
        sat.style.setProperty('--dist', (140 + Math.random() * 50) + 'px');
        sat.style.setProperty('--dur', (3 + Math.random() * 5) + 's');
        holder.appendChild(sat);
    }

    function updateUI() {
        document.getElementById('score').innerText = Math.floor(score).toLocaleString();
        document.getElementById('click-stat').innerText = clickPower;
        document.getElementById('auto-stat').innerText = autoPower;
        
        if (score >= PRESTIGE_COST) {
            document.getElementById('prestige-btn').style.display = 'block';
            document.getElementById('pending-coins').innerText = Math.floor(score / PRESTIGE_COST);
        }
        
        if (score >= 1000) document.getElementById('ach-1').innerText = '✅';
        if (satelliteCount >= 1) document.getElementById('ach-2').innerText = '✅';
        
        renderShop();
    }

    function spawnText(x, y, txt) {
        const t = document.createElement('div');
        t.className = 'float-text';
        t.style.left = x + 'px'; t.style.top = y + 'px';
        t.innerText = txt;
        document.body.appendChild(t);
        setTimeout(() => t.remove(), 800);
    }

    function init() {
        loadGame();
        const field = document.getElementById('starfield');
        for (let i = 0; i < 150; i++) {
            const star = document.createElement('div');
            star.className = 'star';
            star.style.width = star.style.height = Math.random() * 3 + 'px';
            star.style.left = Math.random() * 100 + '%';
            star.style.top = Math.random() * 100 + '%';
            star.style.setProperty('--t', (2 + Math.random() * 4) + 's');
            field.appendChild(star);
        }
        
        document.getElementById('rocket').onclick = (e) => {
            score += clickPower;
            spawnText(e.clientX, e.clientY, `+${clickPower}`);
            updateUI();
        };

        setInterval(() => { 
            if (autoPower > 0) { 
                score += autoPower / 10; 
                updateUI(); 
            } 
        }, 100);

        setInterval(spawnComet, 60000); 
        setInterval(saveGame, 10000);
        
        updateUI();
    }

    function resetGame() { 
        if(confirm("This will delete all progress. Are you sure?")) { 
            localStorage.clear(); 
            location.reload(); 
        } 
    }

    init();
</script>
</body>
</html>
