<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
<title>Janito in Love - 3 Levels</title>
<style>
* { margin: 0; padding: 0; box-sizing: border-box; }
body { 
    overflow: hidden; 
    background: #000; 
    font-family: Arial;
    touch-action: none;
}
@keyframes shake {
    0%, 100% { transform: translateX(-50%) translateY(0); }
    25% { transform: translateX(-50%) translateY(-5px); }
    75% { transform: translateX(-50%) translateY(5px); }
}
#game { 
    position: fixed;
    top: 0; 
    left: 0;
    width: 100vw; 
    height: 100vh; 
}
canvas { 
    display: block; 
    width: 100%; 
    height: 100%; 
    background: linear-gradient(#87CEEB, #90EE90);
}
#hud {
    position: fixed;
    top: 10px;
    left: 50%;
    transform: translateX(-50%);
    background: linear-gradient(135deg, rgba(139,69,19,0.95), rgba(101,67,33,0.95));
    padding: 12px 25px;
    border-radius: 15px;
    color: gold;
    font-size: 22px;
    font-weight: bold;
    z-index: 10;
    border: 4px solid gold;
    box-shadow: 0 5px 20px rgba(0,0,0,0.6), 0 0 15px rgba(255,215,0,0.3);
    text-shadow: 2px 2px 4px rgba(0,0,0,0.7);
}
#controls {
    position: fixed;
    bottom: 0;
    width: 100%;
    height: 120px;
    background: rgba(0,0,0,0.8);
    display: flex;
    justify-content: space-between;
    padding: 20px;
    z-index: 10;
}
.btn {
    width: 65px;
    height: 65px;
    background: linear-gradient(135deg, #8B4513, #654321);
    border: 4px solid gold;
    border-radius: 50%;
    color: gold;
    font-size: 28px;
    font-weight: bold;
    display: flex;
    align-items: center;
    justify-content: center;
    user-select: none;
    box-shadow: 0 5px 15px rgba(0,0,0,0.5), inset 0 2px 5px rgba(255,215,0,0.3);
    transition: all 0.1s;
}
.btn:active { 
    background: linear-gradient(135deg, #654321, #4a2f1a);
    transform: scale(0.9);
    box-shadow: 0 2px 8px rgba(0,0,0,0.5);
}
.jump { 
    width: 75px; 
    height: 75px; 
    background: linear-gradient(135deg, #e91e63, #c71585);
    font-size: 16px;
    box-shadow: 0 6px 20px rgba(233,30,99,0.5), inset 0 2px 5px rgba(255,255,255,0.3);
}
#dialog {
    position: fixed;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    background: linear-gradient(135deg, rgba(139,69,19,0.98), rgba(101,67,33,0.98));
    padding: 25px;
    border: 5px solid gold;
    border-radius: 20px;
    display: none;
    max-width: 85vw;
    z-index: 20;
    box-shadow: 0 10px 40px rgba(0,0,0,0.7), 0 0 20px rgba(255,215,0,0.5);
}
#dialog h2 { 
    color: gold; 
    text-align: center; 
    margin-bottom: 15px; 
    font-size: 22px;
    text-shadow: 2px 2px 4px rgba(0,0,0,0.7);
}
#dialog p { 
    color: white; 
    text-align: center; 
    margin-bottom: 15px; 
    font-size: 17px;
    text-shadow: 1px 1px 2px rgba(0,0,0,0.5);
}
.answer {
    padding: 15px;
    margin: 10px 0;
    background: linear-gradient(135deg, #654321, #4a2f1a);
    border: 3px solid gold;
    border-radius: 12px;
    color: gold;
    font-size: 16px;
    text-align: center;
    font-weight: bold;
    box-shadow: 0 4px 8px rgba(0,0,0,0.4);
    transition: all 0.2s;
}
.answer:active { 
    background: linear-gradient(135deg, #4a2f1a, #654321);
    transform: scale(0.95);
    box-shadow: 0 2px 4px rgba(0,0,0,0.4);
}
#start, #levelComplete {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: linear-gradient(#8B4513, #4a2f1a);
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    z-index: 30;
}
#start h1, #levelComplete h1 { 
    color: gold; 
    font-size: 36px; 
    margin-bottom: 20px; 
    text-align: center; 
    padding: 0 20px;
}
#start p, #levelComplete p {
    color: #FFD700;
    font-size: 18px;
    margin-bottom: 30px;
    text-align: center;
    padding: 0 30px;
    max-width: 500px;
}
#start button, #levelComplete button {
    padding: 15px 40px;
    font-size: 20px;
    background: #e91e63;
    color: white;
    border: 3px solid gold;
    border-radius: 10px;
    font-weight: bold;
}
#levelComplete {
    display: none;
}
#final {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: linear-gradient(#ff69b4, #c71585);
    display: none;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    z-index: 30;
}
#final h1 { 
    color: white; 
    font-size: 36px; 
    text-align: center; 
    margin-bottom: 20px;
    text-shadow: 3px 3px 6px rgba(0,0,0,0.5);
}
#final button {
    padding: 15px 40px;
    font-size: 20px;
    background: white;
    color: #e91e63;
    border: 3px solid white;
    border-radius: 10px;
    font-weight: bold;
}
</style>
</head>
<body>
<div id="start">
    <h1>🌹 Janito in Love 🦆</h1>
    <p>3 Levels of Wine Expertise</p>
    <button onclick="startGame()">Start Level 1 ❤️</button>
</div>
<div id="game">
    <canvas id="canvas"></canvas>
    <div id="hud">
        <span id="hearts">❤️❤️❤️</span> | 🌹 <span id="roses">0/10</span> | <span id="level">Level 1</span>
    </div>
    <div id="controls">
        <div style="display:flex;gap:10px;">
            <div class="btn" ontouchstart="moveLeft=true" ontouchend="moveLeft=false">◄</div>
            <div class="btn" ontouchstart="moveRight=true" ontouchend="moveRight=false">►</div>
        </div>
        <div class="btn jump" ontouchstart="jump()">JUMP</div>
    </div>
</div>
<div id="dialog">
    <h2 id="qtitle"></h2>
    <p id="qtext"></p>
    <div id="answers"></div>
</div>
<div id="levelComplete">
    <h1 id="levelMessage"></h1>
    <p id="levelSubtext"></p>
    <button id="nextLevelBtn" onclick="nextLevel()">Continue ➜</button>
</div>
<div id="final">
    <h1 id="finalMessage">HAPPY VALENTINE'S DAY ❤️</h1>
    <button onclick="location.reload()">Play Again 🌹</button>
</div>
<script>
const canvas = document.getElementById('canvas');
const ctx = canvas.getContext('2d');

// Sistema de Audio
const audioCtx = new (window.AudioContext || window.webkitAudioContext)();

function playSound(freq, duration, type='sine') {
    try {
        const osc = audioCtx.createOscillator();
        const gain = audioCtx.createGain();
        osc.connect(gain);
        gain.connect(audioCtx.destination);
        osc.frequency.value = freq;
        osc.type = type;
        gain.gain.setValueAtTime(0.3, audioCtx.currentTime);
        gain.gain.exponentialRampToValueAtTime(0.01, audioCtx.currentTime + duration);
        osc.start(audioCtx.currentTime);
        osc.stop(audioCtx.currentTime + duration);
    } catch(e) {}
}

function playJump() {
    playSound(400, 0.1);
    setTimeout(() => playSound(500, 0.1), 50);
}

function playCorrect() {
    playSound(523, 0.1);
    setTimeout(() => playSound(659, 0.1), 100);
    setTimeout(() => playSound(784, 0.15), 200);
}

function playWrong() {
    playSound(200, 0.3, 'sawtooth');
}

function playCollect() {
    playSound(800, 0.1);
    setTimeout(() => playSound(1000, 0.1), 50);
}

function resize() {
    canvas.width = window.innerWidth;
    canvas.height = window.innerHeight;
}
resize();
window.addEventListener('resize', resize);

let moveLeft = false, moveRight = false;
let game = {
    lives: 3,
    roses: 0,
    rosesPerLevel: 10,
    paused: false,
    camera: 0,
    currentLevel: 1,
    maxLevel: 3,
    loopRunning: false  // FIX: flag to prevent double game loops
};

let player = { x: 100, y: 100, vx: 0, vy: 0, w: 40, h: 40, onGround: false };

// NIVEL 1: Preguntas Básicas
const level1Questions = [
    {name:'Merlot', q:'What type of wine is Merlot?', correct:'Red Wine', options:['Red Wine','White Wine','Rosé Wine','Sparkling Wine']},
    {name:'Chardonnay', q:'What type of wine is Chardonnay?', correct:'White Wine', options:['Red Wine','White Wine','Rosé Wine','Sparkling Wine']},
    {name:'Cabernet', q:'What type of wine is Cabernet Sauvignon?', correct:'Red Wine', options:['Red Wine','White Wine','Rosé Wine','Dessert Wine']},
    {name:'Riesling', q:'What type of wine is Riesling?', correct:'White Wine', options:['Red Wine','White Wine','Rosé Wine','Port Wine']},
    {name:'Pinot Noir', q:'What type of wine is Pinot Noir?', correct:'Red Wine', options:['Red Wine','White Wine','Rosé Wine','Sherry']},
    {name:'Sauvignon', q:'What type of wine is Sauvignon Blanc?', correct:'White Wine', options:['Red Wine','White Wine','Rosé Wine','Port']},
    {name:'Malbec', q:'What type of wine is Malbec?', correct:'Red Wine', options:['Red Wine','White Wine','Rosé Wine','Sparkling']},
    {name:'Prosecco', q:'What type of wine is Prosecco?', correct:'Sparkling Wine', options:['Red Wine','White Wine','Rosé Wine','Sparkling Wine']},
    {name:'Rosé', q:'What type of wine is Rosé?', correct:'Rosé Wine', options:['Red Wine','White Wine','Rosé Wine','Sparkling']},
    {name:'Syrah', q:'What type of wine is Syrah?', correct:'Red Wine', options:['Red Wine','White Wine','Rosé Wine','Dessert']}
];

// NIVEL 2: Preguntas de Sommelier (Regiones y Características)
const level2Questions = [
    {name:'Bordeaux', q:'Bordeaux wines are primarily from which country?', correct:'France', options:['France','Italy','Spain','Germany']},
    {name:'Chianti', q:'Chianti is a wine from which Italian region?', correct:'Tuscany', options:['Piedmont','Tuscany','Veneto','Sicily']},
    {name:'Rioja', q:'Rioja wines come from which country?', correct:'Spain', options:['Portugal','Spain','France','Italy']},
    {name:'Barolo', q:'Barolo is made from which grape?', correct:'Nebbiolo', options:['Sangiovese','Nebbiolo','Barbera','Merlot']},
    {name:'Champagne', q:'True Champagne can only come from which region?', correct:'Champagne, France', options:['Champagne, France','California, USA','Prosecco, Italy','Cava, Spain']},
    {name:'Sauternes', q:'Sauternes is known for what type of wine?', correct:'Sweet Dessert Wine', options:['Dry Red','Sweet Dessert Wine','Sparkling','Rosé']},
    {name:'Port', q:'Port wine originates from which country?', correct:'Portugal', options:['Spain','Portugal','France','Italy']},
    {name:'Chablis', q:'Chablis is made from which grape variety?', correct:'Chardonnay', options:['Sauvignon Blanc','Chardonnay','Riesling','Pinot Grigio']},
    {name:'Mosel', q:'Mosel valley is famous for which wine?', correct:'Riesling', options:['Riesling','Gewürztraminer','Pinot Noir','Chardonnay']},
    {name:'Brunello', q:'Brunello di Montalcino is made from which grape?', correct:'Sangiovese', options:['Nebbiolo','Sangiovese','Barbera','Merlot']}
];

// NIVEL 3: Preguntas Expertas de Sommelier
const level3Questions = [
    {name:'Grand Cru', q:'What does "Grand Cru" indicate in Burgundy?', correct:'Highest quality vineyard classification', options:['Highest quality vineyard classification','Aged 10+ years','Sweetest wine','Sparkling method']},
    {name:'Phylloxera', q:'What devastated European vineyards in the 1800s?', correct:'Phylloxera insect', options:['Fungal disease','Phylloxera insect','Drought','Cold winters']},
    {name:'Malolactic', q:'Malolactic fermentation converts malic acid to what?', correct:'Lactic acid', options:['Acetic acid','Lactic acid','Citric acid','Tartaric acid']},
    {name:'Terroir', q:'What does "terroir" primarily refer to?', correct:'Environmental factors affecting wine', options:['Wine aging process','Environmental factors affecting wine','Barrel type','Grape variety']},
    {name:'Botrytis', q:'Botrytis cinerea is used to make which wine style?', correct:'Noble rot dessert wines', options:['Sparkling wines','Noble rot dessert wines','Dry reds','Fortified wines']},
    {name:'Sur lie', q:'What does "sur lie" aging mean?', correct:'Wine aged on its lees/yeast', options:['Aged in oak barrels','Wine aged on its lees/yeast','Aged in bottle 10+ years','Aged underground']},
    {name:'Tannins', q:'Tannins in wine primarily come from what?', correct:'Grape skins and seeds', options:['Water','Grape skins and seeds','Yeast','Added sulfites']},
    {name:'Assemblage', q:'In Champagne, what is "assemblage"?', correct:'Blending different wines', options:['Riddling process','Blending different wines','Disgorgement','Dosage addition']},
    {name:'Lieu-dit', q:'What is a "lieu-dit" in French wine terminology?', correct:'Named vineyard parcel', options:['Wine merchant','Named vineyard parcel','Tasting note','Barrel maker']},
    {name:'Aszú', q:'Tokaji Aszú is a famous wine from which country?', correct:'Hungary', options:['Austria','Hungary','Czech Republic','Romania']}
];

let tables = [];
let currentLevelQuestions = [];

function initTables() {
    tables = [];
    
    // Seleccionar preguntas según el nivel
    if(game.currentLevel === 1) {
        currentLevelQuestions = level1Questions;
    } else if(game.currentLevel === 2) {
        currentLevelQuestions = level2Questions;
    } else {
        currentLevelQuestions = level3Questions;
    }
    
    for(let i=0; i<10; i++) {
        tables.push({
            x: 300 + i*350,
            y: canvas.height - 200,
            wine: currentLevelQuestions[i],
            got: false
        });
    }
}

function startGame() {
    document.getElementById('start').style.display = 'none';
    game.currentLevel = 1;
    game.roses = 0;
    game.lives = 3;
    game.paused = false;   // FIX: ensure not paused
    game.loopRunning = false;
    updateHUD();
    initTables();
    gameLoop();
}

function nextLevel() {
    document.getElementById('levelComplete').style.display = 'none';
    game.currentLevel++;
    game.roses = 0;
    game.lives = 3;
    game.camera = 0;
    game.paused = false;   // FIX: unpause the game so player can move
    player.x = 100;
    player.y = 100;
    player.vx = 0;
    player.vy = 0;
    updateHUD();
    initTables();
    // FIX: restart the game loop only if it's not already running
    if(!game.loopRunning) {
        gameLoop();
    }
}

function updateHUD() {
    let h = '';
    for(let i=0; i<game.lives; i++) h += '❤️';
    document.getElementById('hearts').textContent = h;
    document.getElementById('roses').textContent = game.roses + '/' + game.rosesPerLevel;
    document.getElementById('level').textContent = 'Level ' + game.currentLevel;
}

function jump() {
    if(player.onGround && !game.paused) {
        player.vy = -15;
        playJump();
    }
}

function draw() {
    ctx.clearRect(0,0,canvas.width,canvas.height);
    
    // Cielo con gradiente
    let gradient = ctx.createLinearGradient(0, 0, 0, canvas.height-150);
    gradient.addColorStop(0, '#87CEEB');
    gradient.addColorStop(0.7, '#E0F6FF');
    gradient.addColorStop(1, '#FFE4B5');
    ctx.fillStyle = gradient;
    ctx.fillRect(0, 0, canvas.width, canvas.height-150);
    
    // Sol
    ctx.fillStyle = '#FFD700';
    ctx.beginPath();
    ctx.arc(canvas.width-100, 80, 40, 0, Math.PI*2);
    ctx.fill();
    
    // Nubes
    let time = Date.now() / 2000;
    for(let i=0; i<8; i++) {
        let cx = ((time * 15 + i * 180) % (canvas.width + 100)) - 50;
        let cy = 50 + (i % 3) * 40;
        ctx.fillStyle = 'rgba(255,255,255,0.8)';
        ctx.fillRect(cx, cy, 40, 20);
        ctx.fillRect(cx+15, cy-10, 40, 20);
        ctx.fillRect(cx+30, cy, 35, 20);
    }
    
    // Viñedos
    ctx.strokeStyle = '#654321';
    ctx.lineWidth = 4;
    for(let i=0; i<12; i++) {
        let vx = i * 180 - game.camera * 0.5;
        if(vx > -100 && vx < canvas.width + 100) {
            ctx.fillStyle = '#654321';
            ctx.fillRect(vx, 100, 10, canvas.height-190-100);
            ctx.fillStyle = '#228B22';
            ctx.fillRect(vx-30, canvas.height-240, 70, 4);
            for(let j=0; j<4; j++) {
                let lx = vx - 25 + j * 20;
                ctx.fillRect(lx, canvas.height-250, 8, 10);
            }
            ctx.fillStyle = '#8B008B';
            ctx.fillRect(vx-10, canvas.height-230, 6, 8);
        }
    }
    
    // Ground
    let groundY = canvas.height-150;
    ctx.fillStyle = '#90EE90';
    ctx.fillRect(-game.camera, groundY, 5000, 20);
    ctx.fillStyle = '#7CFC00';
    for(let i=0; i<100; i++) {
        ctx.fillRect(i*50 - game.camera, groundY, 20, 20);
    }
    ctx.fillStyle = '#8B4513';
    ctx.fillRect(-game.camera, groundY+20, 5000, 60);
    ctx.fillStyle = '#654321';
    for(let i=0; i<80; i++) {
        ctx.fillRect(i*60 - game.camera + 10, groundY+30, 8, 8);
    }
    ctx.fillStyle = '#FF69B4';
    for(let i=0; i<50; i++) {
        ctx.fillRect(i*100 - game.camera + 20, groundY+5, 4, 4);
    }
    
    // Tables
    tables.forEach(t => {
        if(t.got) return;
        let x = t.x - game.camera;
        
        ctx.fillStyle = 'rgba(0,0,0,0.2)';
        ctx.fillRect(x-2, t.y+42, 66, 4);
        
        ctx.fillStyle = '#A0522D';
        ctx.fillRect(x, t.y, 62, 12);
        ctx.fillStyle = '#8B4513';
        ctx.fillRect(x, t.y, 62, 3);
        
        ctx.fillStyle = '#654321';
        ctx.fillRect(x+5, t.y+12, 12, 30);
        ctx.fillRect(x+45, t.y+12, 12, 30);
        
        let bounce = Math.sin(Date.now()/500) * 2;
        ctx.fillStyle = 'rgba(200,200,255,0.3)';
        ctx.fillRect(x+22, t.y-22+bounce, 18, 20);
        ctx.fillStyle = '#8B0000';
        ctx.fillRect(x+24, t.y-18+bounce, 14, 14);
        ctx.fillStyle = 'rgba(255,255,255,0.8)';
        ctx.fillRect(x+26, t.y-20+bounce, 3, 6);
        ctx.fillStyle = '#C0C0C0';
        ctx.fillRect(x+29, t.y-2+bounce, 4, 8);
        ctx.fillStyle = '#A9A9A9';
        ctx.fillRect(x+25, t.y+6+bounce, 12, 3);
        
        ctx.fillStyle = '#FFD700';
        ctx.fillRect(x+5, t.y-32, 52, 8);
        ctx.fillStyle = '#000';
        ctx.font = 'bold 11px Arial';
        ctx.textAlign = 'center';
        ctx.fillText(t.wine.name, x+31, t.y-25);
    });
    
    // Player
    let px = player.x - game.camera;
    ctx.fillStyle = 'rgba(0,0,0,0.2)';
    ctx.ellipse(px+20, player.y+40, 15, 5, 0, 0, Math.PI*2);
    ctx.fill();
    ctx.fillStyle = '#FFD700';
    ctx.fillRect(px+5, player.y+5, 30, 30);
    ctx.fillStyle = '#FFA500';
    ctx.fillRect(px+5, player.y+20, 30, 3);
    ctx.fillStyle = '#FFF';
    ctx.fillRect(px+10, player.y+10, 8, 8);
    ctx.fillRect(px+22, player.y+10, 8, 8);
    ctx.fillStyle = '#000';
    ctx.fillRect(px+13, player.y+13, 4, 4);
    ctx.fillRect(px+25, player.y+13, 4, 4);
    ctx.fillStyle = '#FF8C00';
    ctx.fillRect(px+15, player.y+22, 10, 6);
    ctx.fillRect(px+10, player.y+35, 6, 8);
    ctx.fillRect(px+24, player.y+35, 6, 8);
}

function update() {
    if(game.paused) return;
    
    if(moveLeft) player.vx = -5;
    else if(moveRight) player.vx = 5;
    else player.vx = 0;
    
    player.x += player.vx;
    player.vy += 0.6;
    player.y += player.vy;
    
    let ground = canvas.height - 150;
    if(player.y + player.h > ground) {
        player.y = ground - player.h;
        player.vy = 0;
        player.onGround = true;
    } else {
        player.onGround = false;
    }
    
    game.camera = Math.max(0, player.x - canvas.width/3);
    
    tables.forEach(t => {
        if(!t.got && Math.abs(player.x - t.x) < 70 && Math.abs(player.y - t.y) < 70) {
            showQuestion(t);
        }
    });
}

function showQuestion(table) {
    game.paused = true;
    document.getElementById('qtitle').textContent = '🍷 ' + table.wine.name;
    document.getElementById('qtext').textContent = table.wine.q;
    
    let opts = [...table.wine.options].sort(() => Math.random()-0.5);
    let html = '';
    opts.forEach(o => {
        html += `<div class="answer" onclick="answer('${o.replace(/'/g, "\\'")}','${table.wine.correct.replace(/'/g, "\\'")}',${tables.indexOf(table)})">${o}</div>`;
    });
    document.getElementById('answers').innerHTML = html;
    document.getElementById('dialog').style.display = 'block';
}

function answer(selected, correct, idx) {
    document.getElementById('dialog').style.display = 'none';
    
    if(selected === correct) {
        playCorrect();
        playCollect();
        game.roses++;
        tables[idx].got = true;
        createParticles(tables[idx].x - game.camera + canvas.getBoundingClientRect().left, tables[idx].y);
        updateHUD();
        
        if(game.roses >= game.rosesPerLevel) {
            setTimeout(() => {
                if(game.currentLevel < game.maxLevel) {
                    showLevelComplete();
                } else {
                    showFinalScene();
                }
            }, 500);
            return; // FIX: don't unpause here if we're about to show level complete
        }
    } else {
        playWrong();
        game.lives--;
        let hud = document.getElementById('hud');
        hud.style.animation = 'shake 0.3s';
        setTimeout(() => hud.style.animation = '', 300);
        updateHUD();
        
        if(game.lives <= 0) {
            setTimeout(() => {
                alert('Game Over! 💔 Reloading...');
                location.reload();
            }, 300);
            return;
        }
    }
    
    game.paused = false;  // FIX: only unpause if game is continuing normally
}

function showLevelComplete() {
    game.paused = true;
    playCorrect();
    setTimeout(playCorrect, 200);
    
    let msg = document.getElementById('levelMessage');
    let sub = document.getElementById('levelSubtext');
    
    if(game.currentLevel === 1) {
        msg.textContent = '🌹 Level 1 Complete! 🌹';
        sub.textContent = 'Great start! Ready for sommelier questions?';
    } else if(game.currentLevel === 2) {
        msg.textContent = '💕 I love you so much my wine expert chick! 💕';
        sub.textContent = 'You really know your wines! One more level...';
    }
    
    document.getElementById('levelComplete').style.display = 'flex';
}

function showFinalScene() {
    game.paused = true;
    playCorrect();
    setTimeout(playCorrect, 200);
    setTimeout(playCorrect, 400);
    
    ctx.fillStyle = 'rgba(255, 105, 180, 0.95)';
    ctx.fillRect(0, 0, canvas.width, canvas.height);
    
    setTimeout(() => {
        let centerX = canvas.width / 2;
        let centerY = canvas.height / 2;
        
        ctx.font = '40px Arial';
        for(let i=0; i<15; i++) {
            ctx.fillText('❤️', Math.random() * canvas.width, Math.random() * canvas.height);
        }
        
        let janitoX = centerX - 150;
        let janitoY = centerY - 20;
        
        ctx.fillStyle = 'rgba(0,0,0,0.3)';
        ctx.fillRect(janitoX, janitoY + 80, 60, 10);
        ctx.fillStyle = '#FFD700';
        ctx.fillRect(janitoX, janitoY, 60, 60);
        ctx.fillStyle = '#FFA500';
        ctx.fillRect(janitoX, janitoY + 30, 60, 5);
        ctx.fillStyle = '#FFF';
        ctx.fillRect(janitoX + 10, janitoY + 15, 15, 15);
        ctx.fillRect(janitoX + 35, janitoY + 15, 15, 15);
        ctx.fillStyle = '#000';
        ctx.fillRect(janitoX + 15, janitoY + 20, 8, 8);
        ctx.fillRect(janitoX + 40, janitoY + 20, 8, 8);
        ctx.fillStyle = '#FF8C00';
        ctx.fillRect(janitoX + 22, janitoY + 35, 16, 10);
        ctx.fillRect(janitoX + 15, janitoY + 60, 10, 15);
        ctx.fillRect(janitoX + 35, janitoY + 60, 10, 15);
        
        let duckX = centerX + 90;
        let duckY = centerY - 30;
        
        ctx.fillStyle = 'rgba(0,0,0,0.3)';
        ctx.fillRect(duckX - 10, duckY + 100, 80, 10);
        ctx.fillStyle = '#FFEB3B';
        ctx.fillRect(duckX, duckY, 70, 70);
        ctx.fillStyle = '#FDD835';
        ctx.fillRect(duckX, duckY + 35, 70, 5);
        ctx.fillStyle = '#FFF';
        ctx.fillRect(duckX + 10, duckY + 15, 15, 15);
        ctx.fillRect(duckX + 45, duckY + 15, 15, 15);
        ctx.fillStyle = '#000';
        ctx.fillRect(duckX + 15, duckY + 20, 8, 8);
        ctx.fillRect(duckX + 50, duckY + 20, 8, 8);
        ctx.fillStyle = '#FF9800';
        ctx.fillRect(duckX + 70, duckY + 28, 20, 15);
        
        // BOTAS ROJAS
        ctx.fillStyle = '#FF0000';
        ctx.fillRect(duckX + 10, duckY + 70, 20, 30);
        ctx.fillRect(duckX + 40, duckY + 70, 20, 30);
        ctx.fillStyle = '#FF6666';
        ctx.fillRect(duckX + 12, duckY + 72, 8, 15);
        ctx.fillRect(duckX + 42, duckY + 72, 8, 15);
        ctx.fillStyle = '#000';
        ctx.fillRect(duckX + 8, duckY + 100, 24, 5);
        ctx.fillRect(duckX + 38, duckY + 100, 24, 5);
        
        // RAMO
        ctx.font = '100px Arial';
        ctx.fillText('💐', centerX - 50, centerY + 50);
        
        ctx.font = '50px Arial';
        ctx.fillText('❤️', centerX - 100, centerY - 80);
        ctx.fillText('❤️', centerX + 80, centerY - 90);
        ctx.fillText('💕', centerX - 80, centerY + 120);
        ctx.fillText('💕', centerX + 100, centerY + 130);
        
        setTimeout(() => {
            ctx.fillStyle = '#FF69B4';
            ctx.font = '60px Arial';
            ctx.fillText('💋', centerX - 20, centerY - 50);
        }, 500);
        
    }, 200);
    
    setTimeout(() => {
        document.getElementById('finalMessage').textContent = '💋 Duck Kiss 💋';
        setTimeout(() => {
            document.getElementById('finalMessage').textContent = 'HAPPY VALENTINE\'S DAY ❤️';
        }, 1500);
        document.getElementById('final').style.display = 'flex';
    }, 2000);
}

function createParticles(x, y) {
    for(let i=0; i<8; i++) {
        let p = document.createElement('div');
        p.textContent = '🌹';
        p.style.position = 'fixed';
        p.style.left = x + 'px';
        p.style.top = y + 'px';
        p.style.fontSize = '20px';
        p.style.pointerEvents = 'none';
        p.style.zIndex = '25';
        
        let angle = (Math.PI * 2 * i) / 8;
        let dist = 60;
        let tx = Math.cos(angle) * dist;
        let ty = Math.sin(angle) * dist;
        
        p.animate([
            { transform: 'translate(0, 0) scale(1)', opacity: 1 },
            { transform: `translate(${tx}px, ${ty}px) scale(0)`, opacity: 0 }
        ], {
            duration: 800,
            easing: 'ease-out'
        });
        
        document.body.appendChild(p);
        setTimeout(() => p.remove(), 800);
    }
}

function gameLoop() {
    game.loopRunning = true;  // FIX: mark loop as running
    update();
    draw();
    requestAnimationFrame(gameLoop);
}

document.addEventListener('keydown', e => {
    if(e.key === 'ArrowLeft') moveLeft = true;
    if(e.key === 'ArrowRight') moveRight = true;
    if(e.key === 'ArrowUp' || e.key === ' ') jump();
});
document.addEventListener('keyup', e => {
    if(e.key === 'ArrowLeft') moveLeft = false;
    if(e.key === 'ArrowRight') moveRight = false;
});
</script>
</body>
</html>
