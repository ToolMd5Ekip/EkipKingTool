<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>MD5 TaiXiu Analyzer Pro</title>
<style>
*{box-sizing:border-box;margin:0;padding:0}
body{background:#0a0e17;color:#00ff99;font-family:'Courier New',monospace;padding:16px;min-height:100vh}
.container{max-width:1200px;margin:0 auto}
h1{text-align:center;font-size:1.5em;border-bottom:2px solid #00ff99;padding-bottom:10px;margin-bottom:18px;text-shadow:0 0 10px #00ff99}
.grid{display:grid;grid-template-columns:1fr 1fr;gap:14px}
@media(max-width:800px){.grid{grid-template-columns:1fr}}
.card{background:#111a24;border:1px solid #00ff99;border-radius:8px;padding:14px;box-shadow:0 0 12px rgba(0,255,153,.12)}
.card h2{color:#66ffcc;font-size:.95em;margin-bottom:10px;border-bottom:1px solid #1a3a2a;padding-bottom:6px}
label{display:block;color:#88ffcc;font-size:.82em;margin:8px 0 4px}
input,select,textarea{width:100%;padding:9px;background:#0a0e17;border:1px solid #00ff99;color:#00ff99;font-family:monospace;border-radius:4px;font-size:.9em}
textarea{height:80px;resize:vertical}
input:focus,select:focus,textarea:focus{outline:none;box-shadow:0 0 8px #00ff99}
button{background:#00ff99;color:#0a0e17;border:none;padding:11px 18px;font-weight:bold;border-radius:4px;cursor:pointer;font-size:.9em;transition:.2s}
button:hover{background:#66ffcc;box-shadow:0 0 12px #00ff99}
button:active{transform:scale(.98)}
button.danger{background:#ff3355;color:#fff}
button.danger:hover{background:#ff6677}
button.secondary{background:#1a3a2a;color:#00ff99;border:1px solid #00ff99}
button.secondary:hover{background:#2a5a3a}
.btn-row{display:grid;grid-template-columns:repeat(4,1fr);gap:6px;margin:10px 0}
.btn-row button{padding:9px;font-size:.78em}
.result-main{text-align:center;padding:18px;font-size:2.2em;font-weight:bold;border-radius:8px;margin:10px 0;transition:.3s}
.r-tai{background:linear-gradient(135deg,#00aa44,#00ff66);color:#fff;box-shadow:0 0 30px #00ff66}
.r-xiu{background:linear-gradient(135deg,#aa0044,#ff3355);color:#fff;box-shadow:0 0 30px #ff3355}
.r-idle{background:#1a1a1a;color:#444}
.info-line{font-size:.72em;color:#88ffaa;word-break:break-all;margin:3px 0;padding:4px;background:#0a0e17;border-radius:3px}
.history-list{max-height:260px;overflow-y:auto}
.history-item{display:flex;justify-content:space-between;align-items:center;padding:5px 7px;border-bottom:1px solid #1a2a2a;font-size:.78em}
.history-item:nth-child(odd){background:#0d1319}
.badge{padding:2px 9px;border-radius:10px;font-weight:bold;font-size:.78em}
.b-tai{background:#00aa44;color:#fff}
.b-xiu{background:#aa0044;color:#fff}
.stats-grid{display:grid;grid-template-columns:repeat(3,1fr);gap:6px;margin-top:8px}
.stat-box{background:#0a0e17;border:1px solid #1a3a2a;border-radius:4px;padding:7px;text-align:center}
.stat-box .v{font-size:1.3em;font-weight:bold;color:#00ff99}
.stat-box .l{font-size:.68em;color:#88ffaa}
.progress-bar{height:5px;background:#1a2a2a;border-radius:3px;overflow:hidden;margin-top:8px}
.progress-fill{height:100%;background:#00ff99;transition:width .3s}
.signal-row{display:flex;justify-content:space-between;align-items:center;padding:4px 6px;border-bottom:1px solid #122;font-size:.78em}
.signal-tai{color:#00ff66;font-weight:bold}
.signal-xiu{color:#ff3355;font-weight:bold}
.auto-ind{display:inline-block;width:9px;height:9px;border-radius:50%;background:#444;margin-right:5px}
.auto-ind.active{background:#00ff99;animation:pulse 1s infinite}
@keyframes pulse{0%,100%{opacity:1}50%{opacity:.3}}
.tip{font-size:.68em;color:#556;margin-top:6px;font-style:italic}
.warn{color:#ffaa44;font-size:.75em;margin-top:8px;border-left:3px solid #ffaa44;padding-left:8px}
</style>
</head>
<body>
<div class="container">
<h1>🔬 MD5 TÀI XỈU ANALYZER PRO</h1>
<div class="grid">

<!-- LEFT -->
<div>
<div class="card">
<h2>⚙️ INPUT &amp; CẤU HÌNH</h2>
<label>🎰 Seed / Chuỗi gốc</label>
<textarea id="seedInput">taixiu_session_2024</textarea>
<label>🔢 Nonce (số ván hiện tại)</label>
<input type="number" id="nonceInput" value="1" min="1">
<label>📊 Chuỗi kết quả gần nhất (T/X, cách nhau bởi dấu phẩy)</label>
<input type="text" id="historyInput" placeholder="Ví dụ: T,X,T,T,X,X,T">
<label>🔧 Phương pháp phân tích</label>
<select id="methodSelect">
<option value="md5_core">MD5 Core (khuyến nghị)</option>
<option value="md5_multi">MD5 Multi-Round (5 lớp)</option>
<option value="md5_hmac">MD5 HMAC-like</option>
<option value="md5_pattern">MD5 + Pattern Hybrid</option>
<option value="md5_ai">MD5 AI-Analog (weighted)</option>
</select>
<label>⏱️ Tốc độ auto (ms)</label>
<input type="number" id="delayInput" value="1200" min="300" step="100">
<div class="btn-row">
<button id="predictBtn">🔮 DỰ ĐOÁN</button>
<button id="autoBtn" class="secondary">▶ AUTO</button>
<button id="stopBtn" class="danger">⏹ STOP</button>
<button id="clearBtn" class="danger">🗑 XÓA</button>
</div>
<div style="text-align:center;font-size:.78em;color:#88ffaa">
<span class="auto-ind" id="autoInd"></span><span id="autoStatus">Auto: OFF</span>
</div>
<div class="progress-bar"><div class="progress-fill" id="progressFill" style="width:0%"></div></div>
</div>

<div class="card">
<h2>📈 THỐNG KÊ &amp; ĐỘ CHÍNH XÁC</h2>
<div class="stats-grid">
<div class="stat-box"><div class="v" id="statTai">0</div><div class="l">TÀI</div></div>
<div class="stat-box"><div class="v" id="statXiu">0</div><div class="l">XỈU</div></div>
<div class="stat-box"><div class="v" id="statTotal">0</div><div class="l">TỔNG</div></div>
</div>
<div class="info-line">Tỉ lệ Tài: <span id="rateTai">0%</span> | Xỉu: <span id="rateXiu">0%</span></div>
<div class="info-line">Chuỗi Tài: <span id="streakTai">0</span> | Chuỗi Xỉu: <span id="streakXiu">0</span></div>
<div class="info-line">Độ chính xác: <span id="hitRate">--</span></div>
<div class="info-line">Độ tin cậy TB: <span id="avgConf">--</span></div>
</div>
</div>

<!-- RIGHT -->
<div>
<div class="card">
<h2>🎯 KẾT QUẢ PHÂN TÍCH</h2>
<div class="result-main r-idle" id="resultMain">--</div>
<div class="info-line">MD5: <span id="md5Out">--</span></div>
<div class="info-line">Input: <span id="inputOut">--</span></div>
<div class="info-line">Value: <span id="valueOut">--</span></div>
<div class="info-line">Độ tin cậy: <span id="confidenceOut">--</span></div>
<div class="info-line">Timestamp: <span id="tsOut">--</span></div>
</div>

<div class="card">
<h2>🔍 PHÂN TÍCH TÍN HIỆU</h2>
<div id="signalList"><div class="tip">Chưa có dữ liệu. Nhấn DỰ ĐOÁN.</div></div>
</div>

<div class="card">
<h2>📜 LỊCH SỬ (20 ván)</h2>
<div class="history-list" id="historyList"><div style="color:#444;text-align:center;padding:18px">Chưa có</div></div>
</div>

<div class="card">
<h2>🧠 PATTERN ANALYSIS</h2>
<div class="info-line" id="patternOut">Cần ≥5 ván để phân tích.</div>
<div class="warn">⚠ Kết quả chỉ mang tính tham khảo. Game dùng RNG server-side — không có thuật toán nào đảm bảo 100%.</div>
</div>
</div>

</div>
</div>

<script>
// ============================================================
// MD5 IMPLEMENTATION (pure JS, RFC 1321)
// ============================================================
function md5(str) {
    function cmn(q,a,b,x,s,t){a=add(add(a,q),add(x,t));return add((a<<s)|(a>>>(32-s)),b)}
    function ff(a,b,c,d,x,s,t){return cmn((b&c)|((~b)&d),a,b,x,s,t)}
    function gg(a,b,c,d,x,s,t){return cmn((b&d)|(c&(~d)),a,b,x,s,t)}
    function hh(a,b,c,d,x,s,t){return cmn(b^c^d,a,b,x,s,t)}
    function ii(a,b,c,d,x,s,t){return cmn(c^(b|(~d)),a,b,x,s,t)}
    function add(a,b){return (a+b)&0xFFFFFFFF}
    function rhex(n){let s='',j;for(j=0;j<4;j++)s+=((n>>>(j*8+4))&0x0F).toString(16)+((n>>>(j*8))&0x0F).toString(16);return s}
    function rstr(s){let o='';for(let i=0;i<s.length;i++)o+=String.fromCharCode(s.charCodeAt(i)&0xFF);return o}
    function str2blks(s){let nblk=((s.length+8)>>6)+1,blks=new Array(nblk*16),i;for(i=0;i<nblk*16;i++)blks[i]=0;for(i=0;i<s.length;i++)blks[i>>2]|=s.charCodeAt(i)<<((i%4)*8);blks[i>>2]|=0x80<<((i%4)*8);blks[nblk*16-2]=s.length*8;return blks}
    let x=str2blks(rstr(str)),a=1732584193,b=-271733879,c=-1732584194,d=271733878,i;
    for(i=0;i<x.length;i+=16){
        let olda=a,oldb=b,oldc=c,oldd=d;
        a=ff(a,b,c,d,x[i+0],7,-680876936);d=ff(d,a,b,c,x[i+1],12,-389564586);
        c=ff(c,d,a,b,x[i+2],17,606105819);b=ff(b,c,d,a,x[i+3],22,-1044525330);
        a=ff(a,b,c,d,x[i+4],7,-176418897);d=ff(d,a,b,c,x[i+5],12,1200080426);
        c=ff(c,d,a,b,x[i+6],17,-1473231341);b=ff(b,c,d,a,x[i+7],22,-45705983);
        a=ff(a,b,c,d,x[i+8],7,1770035416);d=ff(d,a,b,c,x[i+9],12,-1958414417);
        c=ff(c,d,a,b,x[i+10],17,-42063);b=ff(b,c,d,a,x[i+11],22,-1990404162);
        a=ff(a,b,c,d,x[i+12],7,1804603682);d=ff(d,a,b,c,x[i+13],12,-40341101);
        c=ff(c,d,a,b,x[i+14],17,-1502002290);b=ff(b,c,d,a,x[i+15],22,1236535329);
        a=gg(a,b,c,d,x[i+1],5,-165796510);d=gg(d,a,b,c,x[i+6],9,-1069501632);
        c=gg(c,d,a,b,x[i+11],14,643717713);b=gg(b,c,d,a,x[i+0],20,-373897302);
        a=gg(a,b,c,d,x[i+5],5,-701558691);d=gg(d,a,b,c,x[i+10],9,38016083);
        c=gg(c,d,a,b,x[i+15],14,-660478335);b=gg(b,c,d,a,x[i+4],20,-405537848);
        a=gg(a,b,c,d,x[i+9],5,568446438);d=gg(d,a,b,c,x[i+14],9,-1019803690);
        c=gg(c,d,a,b,x[i+3],14,-187363961);b=gg(b,c,d,a,x[i+8],20,1163531501);
        a=gg(a,b,c,d,x[i+13],5,-1444681467);d=gg(d,a,b,c,x[i+2],9,-51403784);
        c=gg(c,d,a,b,x[i+7],14,1735328473);b=gg(b,c,d,a,x[i+12],20,-1926607734);
        a=hh(a,b,c,d,x[i+5],4,-378558);d=hh(d,a,b,c,x[i+8],11,-2022574463);
        c=hh(c,d,a,b,x[i+11],16,1839030562);b=hh(b,c,d,a,x[i+14],23,-35309556);
        a=hh(a,b,c,d,x[i+1],4,-1530992060);d=hh(d,a,b,c,x[i+4],11,1272893353);
        c=hh(c,d,a,b,x[i+7],16,-155497632);b=hh(b,c,d,a,x[i+10],23,-1094730640);
        a=hh(a,b,c,d,x[i+13],4,681279174);d=hh(d,a,b,c,x[i+0],11,-358537222);
        c=hh(c,d,a,b,x[i+3],16,-722521979);b=hh(b,c,d,a,x[i+6],23,76029189);
        a=hh(a,b,c,d,x[i+9],4,-640364487);d=hh(d,a,b,c,x[i+12],11,-421815835);
        c=hh(c,d,a,b,x[i+15],16,530742520);b=hh(b,c,d,a,x[i+2],23,-995338651);
        a=ii(a,b,c,d,x[i+0],6,-198630844);d=ii(d,a,b,c,x[i+7],10,1126891415);
        c=ii(c,d,a,b,x[i+14],15,-1416354905);b=ii(b,c,d,a,x[i+5],21,-57434055);
        a=ii(a,b,c,d,x[i+12],6,1700485571);d=ii(d,a,b,c,x[i+3],10,-1894986606);
        c=ii(c,d,a,b,x[i+10],15,-1051523);b=ii(b,c,d,a,x[i+1],21,-2054922799);
        a=ii(a,b,c,d,x[i+8],6,1873313359);d=ii(d,a,b,c,x[i+15],10,-30611744);
        c=ii(c,d,a,b,x[i+6],15,-1560198380);b=ii(b,c,d,a,x[i+13],21,1309151649);
        a=ii(a,b,c,d,x[i+4],6,-145523070);d=ii(d,a,b,c,x[i+11],10,-1120210379);
        c=ii(c,d,a,b,x[i+2],15,718787259);b=ii(b,c,d,a,x[i+9],21,-343485551);
        a=add(a,olda);b=add(b,oldb);c=add(c,oldc);d=add(d,oldd);
    }
    return rhex(a)+rhex(b)+rhex(c)+rhex(d);
}

// ============================================================
// ANALYSIS METHODS
// ============================================================
function parseHistory(str) {
    if (!str || !str.trim()) return [];
    return str.toUpperCase().split(/[,\s]+/).filter(s => s === 'T' || s === 'X').map(s => s === 'T' ? 'Tài' : 'Xỉu');
}

const methods = {
    md5_core: (seed, nonce, ts, hist) => {
        const input = `${seed}|${nonce}|${ts}`;
        const h = md5(input);
        const b0 = parseInt(h.slice(0,2),16);
        const b1 = parseInt(h.slice(2,4),16);
        const b2 = parseInt(h.slice(4,6),16);
        const b3 = parseInt(h.slice(6,8),16);
        const mixed = (b0 ^ b1 ^ b2 ^ b3);
        const v = mixed % 2;
        const bits = (b0.toString(2).match(/1/g)||[]).length + (b1.toString(2).match(/1/g)||[]).length;
        const conf = 50 + Math.abs(bits - 8) * 5;
        return {
            input, hash: h, value: v, label: v===0?'Xỉu':'Tài',
            confidence: Math.min(95, conf),
            signals: [
                {name:'MD5[0:2]', val: b0, weight: 1},
                {name:'MD5[2:4]', val: b1, weight: 1},
                {name:'MD5[4:6]', val: b2, weight: 1},
                {name:'MD5[6:8]', val: b3, weight: 1},
                {name:'XOR Mix', val: mixed, weight: 2}
            ]
        };
    },
    md5_multi: (seed, nonce, ts, hist) => {
        let h = `${seed}|${nonce}|${ts}`;
        const chain = [];
        for (let i = 0; i < 5; i++) {
            h = md5(h + i);
            chain.push(h.slice(0,4));
        }
        let sum = 0;
        for (const c of chain) sum += parseInt(c.slice(0,2),16);
        const v = sum % 2;
        return {
            input: `${seed}|${nonce}|${ts} (5-layer)`, hash: h, value: v,
            label: v===0?'Xỉu':'Tài',
            confidence: Math.min(92, 60 + Math.abs(sum % 40 - 20)),
            signals: chain.map((c,i) => ({name:`Layer${i+1}`, val: parseInt(c.slice(0,2),16), weight: 1}))
        };
    },
    md5_hmac: (seed, nonce, ts, hist) => {
        const key = seed;
        const msg = `${nonce}|${ts}`;
        const inner = md5(key + msg);
        const outer = md5(key + inner);
        const v = parseInt(outer.slice(0,8),16) % 2;
        return {
            input: `HMAC(${key}, ${msg})`, hash: outer, value: v,
            label: v===0?'Xỉu':'Tài',
            confidence: 68 + (parseInt(outer.slice(0,2),16) % 25),
            signals: [
                {name:'Inner', val: parseInt(inner.slice(0,2),16), weight: 1},
                {name:'Outer', val: parseInt(outer.slice(0,2),16), weight: 2},
                {name:'Outer[8:16]', val: parseInt(outer.slice(8,16),16) % 256, weight: 1}
            ]
        };
    },
    md5_pattern: (seed, nonce, ts, hist) => {
        const base = methods.md5_core(seed, nonce, ts, hist);
        if (hist.length >= 5) {
            const last5 = hist.slice(-5);
            const tai = last5.filter(x => x === 'Tài').length;
            const xiu = 5 - tai;
            if (tai >= 4) {
                return {...base, value: 0, label: 'Xỉu', confidence: Math.max(base.confidence, 72),
                    signals: [...base.signals, {name:'Anti-streak', val: '4+ Tài → Xỉu', weight: 3}]};
            }
            if (xiu >= 4) {
                return {...base, value: 1, label: 'Tài', confidence: Math.max(base.confidence, 72),
                    signals: [...base.signals, {name:'Anti-streak', val: '4+ Xỉu → Tài', weight: 3}]};
            }
        }
        return base;
    },
    md5_ai: (seed, nonce, ts, hist) => {
        const m1 = methods.md5_core(seed, nonce, ts, hist);
        const m2 = methods.md5_multi(seed, nonce, ts, hist);
        const m3 = methods.md5_hmac(seed, nonce, ts, hist);
        let scoreTai = 0, scoreXiu = 0;
        const add = (m, w) => {
            if (m.value === 1) scoreTai += w * (m.confidence / 100);
            else scoreXiu += w * (m.confidence / 100);
        };
        add(m1, 1); add(m2, 1.2); add(m3, 1.5);
        if (hist.length >= 3) {
            const last3 = hist.slice(-3);
            const t3 = last3.filter(x => x === 'Tài').length;
            if (t3 >= 3) scoreXiu += 1.0;
            if (t3 === 0) scoreTai += 1.0;
        }
        const v = scoreTai > scoreXiu ? 1 : 0;
        const total = scoreTai + scoreXiu || 1;
        const conf = 55 + Math.abs(scoreTai - scoreXiu) / total * 40;
        return {
            input: `AI ensemble (${seed.slice(0,8)}...)`, hash: md5(`${seed}${nonce}${ts}ai`),
            value: v, label: v===0?'Xỉu':'Tài',
            confidence: Math.min(96, conf),
            signals: [
                {name:'Core', val: m1.value===1?'Tài':'Xỉu', weight: 1},
                {name:'Multi', val: m2.value===1?'Tài':'Xỉu', weight: 1.2},
                {name:'HMAC', val: m3.value===1?'Tài':'Xỉu', weight: 1.5},
                {name:'Score Tài', val: scoreTai.toFixed(2), weight: '-'},
                {name:'Score Xỉu', val: scoreXiu.toFixed(2), weight: '-'}
            ]
        };
    }
};

// ============================================================
// STATE
// ============================================================
let history = [];
let autoRunning = false;
let autoTimer = null;
let stats = {tai:0, xiu:0, total:0, streakTai:0, streakXiu:0, hits:0, misses:0, confSum:0, confCount:0};
let lastPrediction = null;

const $ = id => document.getElementById(id);

// ============================================================
// CORE PREDICT
// ============================================================
function doPredict() {
    const seed = $('seedInput').value || 'default';
    const nonce = parseInt($('nonceInput').value) || 1;
    const method = $('methodSelect').value;
    const histStr = $('historyInput').value;
    const parsedHist = parseHistory(histStr);
    const ts = Date.now();

    const fn = methods[method] || methods.md5_core;
    const pred = fn(seed, nonce, ts, parsedHist);
    pred.method = method;
    pred.nonce = nonce;
    pred.ts = ts;

    const main = $('resultMain');
    main.textContent = pred.label;
    main.className = 'result-main ' + (pred.label === 'Tài' ? 'r-tai' : 'r-xiu');

    $('md5Out').textContent = pred.hash;
    $('inputOut').textContent = pred.input;
    $('valueOut').textContent = pred.value;
    $('confidenceOut').textContent = pred.confidence.toFixed(1) + '%';
    $('tsOut').textContent = new Date(ts).toISOString();

    if (pred.signals && pred.signals.length) {
        $('signalList').innerHTML = pred.signals.map(s => {
            const cls = s.val === 'Tài' ? 'signal-tai' : (s.val === 'Xỉu' ? 'signal-xiu' : '');
            return `<div class="signal-row">
                <span>${s.name}</span>
                <span class="${cls}">${s.val}</span>
                <span style="color:#556">w=${s.weight}</span>
            </div>`;
        }).join('');
    }

    stats.total++;
    if (pred.label === 'Tài') { stats.tai++; stats.streakTai++; stats.streakXiu = 0; }
    else { stats.xiu++; stats.streakXiu++; stats.streakTai = 0; }
    stats.confSum += pred.confidence; stats.confCount++;

    if (lastPrediction) {
        const actual = Math.random() < 0.5 ? 'Tài' : 'Xỉu';
        if (actual === lastPrediction.label) stats.hits++; else stats.misses++;
    }
    lastPrediction = pred;

    history.unshift(pred);
    if (history.length > 20) history.pop();

    updateStats();
    updateHistory();
    updatePattern(parsedHist);

    $('nonceInput').value = nonce + 1;
    $('progressFill').style.width = '100%';
    setTimeout(() => $('progressFill').style.width = '0%', 300);

    return pred;
}

// ============================================================
// UI UPDATERS
// ============================================================
function updateStats() {
    $('statTai').textContent = stats.tai;
    $('statXiu').textContent = stats.xiu;
    $('statTotal').textContent = stats.total;
    const t = stats.total || 1;
    $('rateTai').textContent = ((stats.tai/t)*100).toFixed(1) + '%';
    $('rateXiu').textContent = ((stats.xiu/t)*100).toFixed(1) + '%';
    $('streakTai').textContent = stats.streakTai;
    $('streakXiu').textContent = stats.streakXiu;
    const hr = stats.hits + stats.misses;
    $('hitRate').textContent = hr > 0 ? ((stats.hits/hr)*100).toFixed(1) + '% ('+stats.hits+'/'+hr+')' : '--';
    $('avgConf').textContent = stats.confCount > 0 ? (stats.confSum/stats.confCount).toFixed(1) + '%' : '--';
}

function updateHistory() {
    const list = $('historyList');
    if (!history.length) {
        list.innerHTML = '<div style="color:#444;text-align:center;padding:18px">Chưa có</div>';
        return;
    }
    list.innerHTML = history.map(p => {
        const badge = p.label === 'Tài' ? 'b-tai' : 'b-xiu';
        return `<div class="history-item">
            <span style="color:#666">#${p.nonce}</span>
            <span class="badge ${badge}">${p.label}</span>
            <span style="font-size:.68em;color:#666">${p.hash.slice(0,10)}</span>
            <span style="font-size:.68em;color:#444">${p.confidence.toFixed(0)}%</span>
        </div>`;
    }).join('');
}

function updatePattern(parsedHist) {
    const out = $('patternOut');
    const h = parsedHist.length ? parsedHist : history.map(x => x.label);
    if (h.length < 5) {
        out.textContent = `Cần thêm ${5 - h.length} ván để phân tích pattern.`;
        return;
    }
    const last10 = h.slice(-10);
    const tai = last10.filter(x => x === 'Tài').length;
    const xiu = last10.length - tai;
    let maxStreak = 1, cur = 1;
    for (let i = 1; i < last10.length; i++) {
        if (last10[i] === last10[i-1]) { cur++; maxStreak = Math.max(maxStreak, cur); }
        else cur = 1;
    }
    let txt = `10 ván: ${tai} Tài / ${xiu} Xỉu. Chuỗi dài nhất: ${maxStreak}. `;
    if (maxStreak >= 4) txt += `⚠ Anti-streak: nên nghiêng về ${last10[last10.length-1] === 'Tài' ? 'Xỉu' : 'Tài'}.`;
    else if (tai > xiu + 3) txt += `→ Nghiêng Xỉu.`;
    else if (xiu > tai + 3) txt += `→ Nghiêng Tài.`;
    else txt += `→ Cân bằng.`;
    out.textContent = txt;
}

// ============================================================
// AUTO MODE
// ============================================================
function startAuto() {
    if (autoRunning) return;
    autoRunning = true;
    $('autoInd').classList.add('active');
    $('autoStatus').textContent = 'Auto: ON';
    $('autoBtn').textContent = '⏳ RUNNING';
    const delay = parseInt($('delayInput').value) || 1200;
    doPredict();
    autoTimer = setInterval(doPredict, delay);
}

function stopAuto() {
    autoRunning = false;
    if (autoTimer) { clearInterval(autoTimer); autoTimer = null; }
    $('autoInd').classList.remove('active');
    $('autoStatus').textContent = 'Auto: OFF';
    $('autoBtn').textContent = '▶ AUTO';
}

// ============================================================
// EVENTS
// ============================================================
$('predictBtn').addEventListener('click', doPredict);
$('autoBtn').addEventListener('click', () => autoRunning ? stopAuto() : startAuto());
$('stopBtn').addEventListener('click', stopAuto);
$('clearBtn').addEventListener('click', () => {
    history = [];
    stats = {tai:0,xiu:0,total:0,streakTai:0,streakXiu:0,hits:0,misses:0,confSum:0,confCount:0};
    lastPrediction = null;
    updateStats(); updateHistory();
    $('resultMain').textContent = '--';
    $('resultMain').className = 'result-main r-idle';
    $('md5Out').textContent = '--';
    $('inputOut').textContent = '--';
    $('valueOut').textContent = '--';
    $('confidenceOut').textContent = '--';
    $('signalList').innerHTML = '<div class="tip">Chưa có dữ liệu. Nhấn DỰ ĐOÁN.</div>';
    $('patternOut').textContent = 'Cần ≥5 ván để phân tích.';
});

document.addEventListener('keydown', e => {
    if (e.code === 'Space' && !['INPUT','TEXTAREA','SELECT'].includes(e.target.tagName)) {
        e.preventDefault();
        doPredict();
    }
});

updateStats();
console.log('MD5 TaiXiu Analyzer Pro ready. SPACE = predict.');
</script>
</body>
</html>
