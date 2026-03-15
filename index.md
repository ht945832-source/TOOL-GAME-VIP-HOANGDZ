
<!DOCTYPE html>
<html lang="vi">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no">
<title>HOANGDZ VIPP - ROBOT 90° G14</title>
<link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@400;900&family=Share+Tech+Mono&display=swap" rel="stylesheet">
<style>
:root { --neon: #00ffcc; --tai: #ffffff; --xiu: #ffcc00; --bg: rgba(0, 15, 10, 0.95); }
* { box-sizing: border-box; -webkit-tap-highlight-color: transparent; font-family: 'Share Tech Mono', monospace; }
body, html { margin: 0; padding: 0; width: 100%; height: 100%; background: #000; overflow: hidden; color: #fff; }

/* HUB CHỌN GAME */
#gameHub { position: fixed; inset: 0; z-index: 100000; background: radial-gradient(circle, #001a12 0%, #000 100%); display: flex; flex-direction: column; align-items: center; justify-content: center; text-align: center; padding: 20px; }
.hub-title { font-family: 'Orbitron'; font-size: 26px; color: var(--neon); text-shadow: 0 0 15px var(--neon); margin-bottom: 20px; }
.game-grid { display: grid; grid-template-columns: repeat(3, 110px); gap: 10px; }
.game-card { background: rgba(0, 40, 30, 0.8); border: 1px solid var(--neon); border-radius: 12px; padding: 10px; cursor: pointer; transition: 0.3s; }
.game-card img { width: 50px; height: 50px; border-radius: 10px; margin-bottom: 5px; }

/* MAIN APP */
#mainApp { display: none; width: 100%; height: 100%; position: relative; }
#gameIframe { width: 100%; height: 100%; border: none; background: #000; }

/* TOOL ROBOT XOAY 90 ĐỘ */
#masterTool {
    position: absolute; left: 20px; top: 50%;
    z-index: 999999; transform-origin: left center;
    display: flex; align-items: center; gap: 10px;
    touch-action: none; transform: translateY(-50%) scale(0.8);
}

.robot-img { width: 85px; filter: drop-shadow(0 0 10px var(--neon)); animation: float 3s infinite ease-in-out; pointer-events: none; }
@keyframes float { 0%, 100% { transform: translateY(0); } 50% { transform: translateY(-10px); } }

.panel-horizontal {
    display: flex; flex-direction: row; align-items: center;
    background: var(--bg); backdrop-filter: blur(12px);
    border: 1.5px solid var(--neon); border-radius: 15px; padding: 8px 15px;
    gap: 12px; box-shadow: 0 0 25px rgba(0, 255, 204, 0.4);
}

.zoom-ctrl { display: flex; flex-direction: column; gap: 4px; }
.zoom-ctrl button { background: #000; border: 1px solid var(--neon); color: var(--neon); width: 28px; height: 28px; border-radius: 5px; font-weight: bold; cursor: pointer; }

.input-area { display: flex; flex-direction: column; gap: 4px; width: 140px; }
.input-md5 { background: #000; border: 1px solid rgba(0, 255, 204, 0.5); padding: 7px; color: var(--neon); text-align: center; border-radius: 8px; outline: none; font-size: 10px; }

/* HIỂN THỊ PHÂN TÍCH */
.result-display { min-width: 120px; text-align: center; background: rgba(0,0,0,0.7); border-radius: 10px; padding: 5px 8px; border: 1px solid var(--neon); }
#resTxt { font-family: 'Orbitron'; font-size: 18px; font-weight: 900; color: var(--neon); }
#analyzing { font-size: 9px; color: #00ff88; display: none; margin-bottom: 2px; }
.stats-info { font-size: 8px; color: #888; }
.stats-info b { color: var(--neon); }

.action-btns { display: flex; flex-direction: column; gap: 4px; }
.btn-v { padding: 5px 10px; border: none; border-radius: 5px; font-weight: 900; font-size: 8px; cursor: pointer; text-transform: uppercase; }
.btn-tai { background: #fff; color: #000; }
.btn-xiu { background: #ffcc00; color: #000; }

#backBtn { position: fixed; bottom: 15px; right: 15px; z-index: 1000; background: #000; border: 1px solid var(--neon); color: var(--neon); padding: 8px 12px; border-radius: 5px; font-size: 10px; cursor: pointer; }
</style>
</head>
<body>

<div id="gameHub">
    <div class="hub-title">HOANGDZ VIPP</div>
    <div class="game-grid">
        <div class="game-card" onclick="launch('https://play.betvip.fit/?utm_source=betvip.ceo&utm_campaign=betvip.ceo&utm_medium=betvip.ceo&utm_term=betvip.ceo')">
            <img src="https://i.postimg.cc/ZqqrCCjQ/A3EFE26C-7E67-4AFF-A21F-130E42D235EE.png"><div>BETVIP</div>
        </div>
        <div class="game-card" onclick="launch('https://lc79b.bet')">
            <img src="https://i.postimg.cc/JnLWCgjm/3E497BE2-D59F-46D3-BCCF-FDC2E3A9929C.png"><div>LC79</div>
        </div>
        <div class="game-card" onclick="launch('https://68gbvn88.bar')">
            <img src="https://i.postimg.cc/mD7XFp9t/68gamebai.png"><div>68GB</div>
        </div>
    </div>
</div>

<div id="mainApp">
    <iframe id="gameIframe" src=""></iframe>
    
    <div id="masterTool">
        <img class="robot-img" src="https://i.postimg.cc/63bdy9D9/robotics-1.gif">
        <div class="panel-horizontal">
            <div class="zoom-ctrl">
                <button onclick="zoomTool(0.1)">+</button>
                <button onclick="zoomTool(-0.1)">-</button>
            </div>
            <div class="input-area">
                <span style="font-size:8px; color:var(--neon); text-align:center">DÁN MÃ MD5 ĐỂ PHÂN TÍCH</span>
                <input type="text" id="md5Input" class="input-md5" placeholder="MÃ 32 KÝ TỰ..." autocomplete="off">
            </div>
            <div class="result-display">
                <div id="analyzing">ĐANG PHÂN TÍCH... <span id="timer">3</span></div>
                <div id="resTxt">SẴN SÀNG</div>
                <div class="stats-info">Tin cậy: <b id="percVal">0%</b></div>
            </div>
            <div class="action-btns">
                <button class="btn-v btn-tai" onclick="update(true)">TÀI</button>
                <button class="btn-v btn-xiu" onclick="update(false)">XỈU</button>
            </div>
            <div class="stats-info" style="border-left: 1px solid #333; padding-left: 10px;">
                ADJ: <b id="adjVal">0.000</b><br>
                SAI: <b id="failCnt">0</b>
            </div>
        </div>
    </div>
    <button id="backBtn" onclick="location.reload()">🎮 ĐỔI GAME</button>
</div>

<script>
let hist = []; let streak = 0; let adj = 0.0; let lastP = ""; let scale = 0.8;

function runAI(md5, a) {
    let sum = 0;
    for (let c of md5) if (!isNaN(parseInt(c, 16))) sum += parseInt(c, 16);
    let ratio = (sum % 100) / 100;
    ratio = Math.min(1, Math.max(0, ratio + a));
    return (ratio * 100).toFixed(2);
}

document.getElementById('md5Input').addEventListener('input', function() {
    let val = this.value.trim().toLowerCase();
    if (val.length === 32) {
        // HIỂN THỊ QUÁ TRÌNH PHÂN TÍCH
        let resTxt = document.getElementById('resTxt');
        let analyzing = document.getElementById('analyzing');
        let timer = document.getElementById('timer');
        
        resTxt.style.display = "none";
        analyzing.style.display = "block";
        let count = 3;
        timer.innerText = count;

        let countdown = setInterval(() => {
            count--;
            timer.innerText = count;
            if (count <= 0) {
                clearInterval(countdown);
                analyzing.style.display = "none";
                resTxt.style.display = "block";
                
                let tP = parseFloat(runAI(val, adj));
                let xP = (100 - tP).toFixed(2);
                lastP = tP > xP ? 'tài' : 'xiu';
                
                resTxt.innerText = lastP === 'tài' ? "TÀI" : "XỈU";
                resTxt.style.color = lastP === 'tài' ? "#fff" : "#ffcc00";
                document.getElementById('percVal').innerText = Math.max(tP, xP) + "%";
                document.getElementById('md5Input').value = "";
            }
        }, 800);
    }
});

function update(isTai) {
    let act = isTai ? 'tài' : 'xiu';
    if (act === lastP) streak = 0; else streak++;
    hist.push(act);
    let tc = hist.filter(v => v === 'tài').length, xc = hist.filter(v => v === 'xiu').length;
    adj = (tc > xc) ? Math.min(0.1, 0.06 + 0.01 * streak) : Math.max(-0.1, -0.09 - 0.01 * streak);
    if (streak >= 3) adj *= 2.5;
    document.getElementById('adjVal').innerText = adj.toFixed(3);
    document.getElementById('failCnt').innerText = streak;
}

function zoomTool(d) { scale = Math.max(0.2, Math.min(1.2, scale + d)); document.getElementById('masterTool').style.transform = `translateY(-50%) scale(${scale})`; }
function launch(u) { document.getElementById('gameHub').style.display = 'none'; document.getElementById('mainApp').style.display = 'block'; document.getElementById('gameIframe').src = u; }

let tool = document.getElementById('masterTool'), drag = false, sx, sy;
tool.addEventListener('touchstart', e => { drag = true; sx = e.touches[0].clientX - tool.offsetLeft; sy = e.touches[0].clientY - tool.offsetTop; });
document.addEventListener('touchmove', e => { if (drag) { tool.style.left = (e.touches[0].clientX - sx) + 'px'; tool.style.top = (e.touches[0].clientY - sy) + 'px'; }});
document.addEventListener('touchend', () => drag = false);
</script>
</body>
</html>
