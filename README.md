<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NeuroFlash — Entrenamiento de Memoria Eidética</title>
<link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@400;700;900&family=Rajdhani:wght@300;400;600&display=swap" rel="stylesheet">
<style>
  :root {
    --bg: #05080f;
    --panel: #0a0f1e;
    --border: #1a2540;
    --accent: #00f5c4;
    --accent2: #7b61ff;
    --accent3: #ff4b6e;
    --text: #e0eaff;
    --text-dim: #5a6a8a;
    --glow: 0 0 20px rgba(0,245,196,0.4);
    --glow2: 0 0 20px rgba(123,97,255,0.4);
  }

  * { margin:0; padding:0; box-sizing:border-box; }

  body {
    background: var(--bg);
    color: var(--text);
    font-family: 'Rajdhani', sans-serif;
    min-height: 100vh;
    overflow-x: hidden;
  }

  /* Animated background grid */
  body::before {
    content:'';
    position:fixed; inset:0;
    background-image:
      linear-gradient(rgba(0,245,196,0.03) 1px, transparent 1px),
      linear-gradient(90deg, rgba(0,245,196,0.03) 1px, transparent 1px);
    background-size: 50px 50px;
    animation: gridPulse 8s ease-in-out infinite;
    pointer-events:none; z-index:0;
  }
  @keyframes gridPulse {
    0%,100%{opacity:0.5} 50%{opacity:1}
  }

  /* ─── HEADER ─── */
  header {
    position:relative; z-index:10;
    padding: 20px 40px;
    display:flex; align-items:center; justify-content:space-between;
    border-bottom: 1px solid var(--border);
    background: rgba(5,8,15,0.8);
    backdrop-filter: blur(10px);
  }
  .logo {
    font-family:'Orbitron',sans-serif;
    font-size:1.4rem; font-weight:900;
    letter-spacing:3px;
    color: var(--accent);
    text-shadow: var(--glow);
  }
  .logo span { color: var(--accent2); }
  .stats-bar {
    display:flex; gap:30px; align-items:center;
  }
  .stat {
    text-align:center;
  }
  .stat-val {
    font-family:'Orbitron',sans-serif;
    font-size:1.2rem; font-weight:700;
    color: var(--accent);
    text-shadow: var(--glow);
  }
  .stat-lbl {
    font-size:0.7rem; color:var(--text-dim);
    letter-spacing:2px; text-transform:uppercase;
  }

  /* ─── MAIN LAYOUT ─── */
  main {
    position:relative; z-index:5;
    display:flex; flex-direction:column;
    align-items:center; justify-content:center;
    min-height: calc(100vh - 80px);
    padding: 30px 20px;
  }

  /* ─── SCREENS ─── */
  .screen { display:none; width:100%; max-width:900px; }
  .screen.active { display:flex; flex-direction:column; align-items:center; }

  /* ─── HOME SCREEN ─── */
  .home-title {
    font-family:'Orbitron',sans-serif;
    font-size: clamp(2rem, 5vw, 3.5rem);
    font-weight:900;
    text-align:center;
    margin-bottom:10px;
    background: linear-gradient(135deg, var(--accent), var(--accent2));
    -webkit-background-clip: text; -webkit-text-fill-color:transparent;
    animation: titlePulse 3s ease-in-out infinite;
  }
  @keyframes titlePulse { 0%,100%{filter:brightness(1)} 50%{filter:brightness(1.3)} }

  .home-sub {
    color: var(--text-dim); font-size:1.1rem; letter-spacing:3px;
    text-transform:uppercase; margin-bottom:50px; text-align:center;
  }

  .mode-grid {
    display:grid; grid-template-columns: repeat(auto-fit, minmax(220px,1fr));
    gap:20px; width:100%; margin-bottom:40px;
  }
  .mode-card {
    background: var(--panel);
    border: 1px solid var(--border);
    border-radius:12px;
    padding:30px 25px;
    cursor:pointer;
    transition: all 0.3s;
    position:relative; overflow:hidden;
    text-align:center;
  }
  .mode-card::before {
    content:'';
    position:absolute; inset:0;
    background: linear-gradient(135deg, transparent, rgba(0,245,196,0.05));
    opacity:0; transition:0.3s;
  }
  .mode-card:hover { border-color:var(--accent); transform:translateY(-4px); box-shadow: var(--glow); }
  .mode-card:hover::before { opacity:1; }
  .mode-card.purple:hover { border-color:var(--accent2); box-shadow: var(--glow2); }
  .mode-card.red:hover { border-color:var(--accent3); box-shadow: 0 0 20px rgba(255,75,110,0.4); }
  .mode-card.gold:hover { border-color:#ffbe00; box-shadow: 0 0 20px rgba(255,190,0,0.4); }
  .mode-card.gold .mode-name { color:#ffbe00; }

  /* ─── DIGIT SELECTOR ─── */
  .digit-selector {
    margin-top: 16px;
    border-top: 1px solid var(--border);
    padding-top: 14px;
  }
  .digit-selector-label {
    font-size: 0.6rem; letter-spacing: 2px; color: var(--text-dim);
    text-transform: uppercase; margin-bottom: 10px;
  }
  .digit-selector-btns {
    display: flex; flex-wrap: wrap; gap: 6px; justify-content: center;
  }
  .dsb {
    width: 34px; height: 34px;
    background: var(--bg); border: 1px solid var(--border);
    border-radius: 6px; color: var(--text-dim);
    font-family: 'Orbitron', sans-serif; font-size: 0.75rem; font-weight: 700;
    cursor: pointer; transition: all 0.15s;
  }
  .dsb:hover { border-color: #ffbe00; color: #ffbe00; }
  .dsb.active { border-color: #ffbe00; color: #ffbe00; background: rgba(255,190,0,0.12); box-shadow: 0 0 8px rgba(255,190,0,0.3); }

  .mode-icon { font-size:2.5rem; margin-bottom:15px; }
  .mode-name {
    font-family:'Orbitron',sans-serif; font-size:0.9rem;
    font-weight:700; letter-spacing:2px; color:var(--accent);
    margin-bottom:8px;
  }
  .mode-card.purple .mode-name { color:var(--accent2); }
  .mode-card.red .mode-name { color:var(--accent3); }
  .mode-desc { font-size:0.85rem; color:var(--text-dim); line-height:1.5; }

  .difficulty-row {
    display:flex; gap:12px; margin-bottom:30px;
  }
  .diff-btn {
    padding:10px 24px;
    background:transparent; border:1px solid var(--border);
    color:var(--text-dim); border-radius:6px;
    cursor:pointer; font-family:'Rajdhani',sans-serif;
    font-size:0.9rem; font-weight:600; letter-spacing:2px;
    text-transform:uppercase; transition:0.2s;
  }
  .diff-btn.active, .diff-btn:hover {
    border-color:var(--accent); color:var(--accent); background: rgba(0,245,196,0.08);
  }
  .diff-btn.hard.active, .diff-btn.hard:hover { border-color:var(--accent3); color:var(--accent3); background:rgba(255,75,110,0.08); }
  .diff-btn.medium.active, .diff-btn.medium:hover { border-color:var(--accent2); color:var(--accent2); background:rgba(123,97,255,0.08); }

  /* ─── GAME SCREEN ─── */
  .game-header {
    display:flex; justify-content:space-between; align-items:center;
    width:100%; margin-bottom:30px;
  }
  .game-info {
    font-family:'Orbitron',sans-serif; font-size:0.8rem;
    color:var(--text-dim); letter-spacing:2px;
  }
  .game-info span { color:var(--accent); }

  /* Timer ring */
  .timer-container {
    position:relative; width:90px; height:90px;
  }
  .timer-svg { transform:rotate(-90deg); }
  .timer-track { fill:none; stroke:var(--border); stroke-width:4; }
  .timer-bar {
    fill:none; stroke:var(--accent); stroke-width:4;
    stroke-linecap:round;
    stroke-dasharray:251;
    stroke-dashoffset:0;
    transition: stroke-dashoffset 0.1s linear, stroke 0.3s;
  }
  .timer-num {
    position:absolute; inset:0;
    display:flex; align-items:center; justify-content:center;
    font-family:'Orbitron',sans-serif; font-size:1.4rem; font-weight:700;
    color:var(--accent);
  }
  .timer-num.urgent { color:var(--accent3); animation: blink 0.5s step-end infinite; }
  @keyframes blink { 50%{opacity:0.3} }

  /* Phase label */
  .phase-label {
    font-family:'Orbitron',sans-serif; font-size:0.75rem;
    letter-spacing:3px; color:var(--text-dim); text-transform:uppercase;
    margin-bottom:20px; text-align:center;
  }
  .phase-label.memorize { color:var(--accent); }
  .phase-label.recall { color:var(--accent2); }
  .phase-label.result-good { color:var(--accent); }
  .phase-label.result-bad { color:var(--accent3); }

  /* ─── GRID OF SYMBOLS ─── */
  .symbol-grid {
    display:grid;
    gap:10px;
    margin-bottom:20px;
  }
  .cell {
    background: var(--panel);
    border: 1px solid var(--border);
    border-radius:10px;
    display:flex; align-items:center; justify-content:center;
    font-size:2.2rem;
    cursor:default;
    transition:all 0.2s;
    aspect-ratio:1;
    position:relative; overflow:hidden;
    user-select:none;
  }
  .cell::after {
    content:''; position:absolute; inset:0;
    background:radial-gradient(circle at center, rgba(255,255,255,0.05), transparent 70%);
    opacity:0; transition:0.2s;
  }
  .cell.clickable { cursor:pointer; }
  .cell.clickable:hover { border-color:var(--accent2); transform:scale(1.04); box-shadow:var(--glow2); }
  .cell.clickable:hover::after { opacity:1; }
  .cell.selected { border-color:var(--accent); background:rgba(0,245,196,0.1); box-shadow:var(--glow); }
  .cell.correct { border-color:var(--accent); background:rgba(0,245,196,0.15); animation:pop 0.3s ease; }
  .cell.wrong { border-color:var(--accent3); background:rgba(255,75,110,0.15); animation:shake 0.3s ease; }
  .cell.highlight { border-color:var(--accent2); background:rgba(123,97,255,0.15); }
  .cell.hidden-cell { background:var(--panel); }
  .cell.hidden-cell .cell-content { opacity:0; }
  .cell.flash { animation:flashIn 0.15s ease; }

  @keyframes pop { 0%{transform:scale(1.2)} 100%{transform:scale(1)} }
  @keyframes shake { 0%,100%{transform:translateX(0)} 25%{transform:translateX(-5px)} 75%{transform:translateX(5px)} }
  @keyframes flashIn { 0%{opacity:0;transform:scale(0.8)} 100%{opacity:1;transform:scale(1)} }

  /* Progress bar */
  .progress-bar {
    width:100%; height:4px;
    background:var(--border); border-radius:2px; margin-bottom:10px;
  }
  .progress-fill {
    height:100%; border-radius:2px;
    background:linear-gradient(90deg, var(--accent), var(--accent2));
    transition:width 0.3s;
  }

  /* Round indicator */
  .rounds-row {
    display:flex; gap:6px; margin-bottom:30px;
  }
  .round-dot {
    width:10px; height:10px; border-radius:50%;
    background:var(--border); transition:0.3s;
  }
  .round-dot.done { background:var(--accent); box-shadow:var(--glow); }
  .round-dot.current { background:var(--accent2); box-shadow:var(--glow2); animation:pulse 1s infinite; }
  @keyframes pulse { 0%,100%{transform:scale(1)} 50%{transform:scale(1.3)} }

  /* Instructions overlay */
  .instruction {
    font-size:1rem; color:var(--text-dim); text-align:center;
    margin-bottom:20px; letter-spacing:1px;
    min-height:30px;
  }
  .instruction strong { color:var(--text); }

  /* ─── BUTTONS ─── */
  .btn {
    padding:14px 40px;
    border:none; border-radius:8px;
    font-family:'Orbitron',sans-serif; font-size:0.85rem;
    font-weight:700; letter-spacing:3px; text-transform:uppercase;
    cursor:pointer; transition:all 0.2s;
    position:relative; overflow:hidden;
  }
  .btn-primary {
    background:linear-gradient(135deg, var(--accent), #00c49a);
    color:#05080f;
  }
  .btn-primary:hover { transform:translateY(-2px); box-shadow:var(--glow); }
  .btn-secondary {
    background:transparent; border:1px solid var(--accent);
    color:var(--accent);
  }
  .btn-secondary:hover { background:rgba(0,245,196,0.1); }
  .btn-purple {
    background:linear-gradient(135deg, var(--accent2), #5b41df);
    color:#fff;
  }
  .btn-purple:hover { transform:translateY(-2px); box-shadow:var(--glow2); }

  /* ─── RESULT SCREEN ─── */
  .result-score {
    font-family:'Orbitron',sans-serif;
    font-size: clamp(3rem, 8vw, 6rem);
    font-weight:900;
    margin:20px 0;
    animation: scoreReveal 0.8s cubic-bezier(0.34,1.56,0.64,1) both;
  }
  @keyframes scoreReveal { 0%{transform:scale(0);opacity:0} 100%{transform:scale(1);opacity:1} }
  .result-score.great { color:var(--accent); text-shadow:var(--glow); }
  .result-score.ok { color:var(--accent2); text-shadow:var(--glow2); }
  .result-score.bad { color:var(--accent3); text-shadow:0 0 20px rgba(255,75,110,0.4); }

  .result-title {
    font-family:'Orbitron',sans-serif; font-size:1.2rem;
    letter-spacing:4px; color:var(--text-dim); text-align:center;
    margin-bottom:5px;
  }
  .result-detail {
    color:var(--text-dim); font-size:1rem; text-align:center;
    margin-bottom:30px; letter-spacing:1px;
  }
  .result-detail strong { color:var(--text); }

  .stats-grid {
    display:grid; grid-template-columns:repeat(3,1fr);
    gap:15px; width:100%; max-width:500px; margin-bottom:30px;
  }
  .stat-box {
    background:var(--panel); border:1px solid var(--border);
    border-radius:10px; padding:15px; text-align:center;
  }
  .stat-box .val { font-family:'Orbitron',sans-serif; font-size:1.4rem; color:var(--accent); }
  .stat-box .lbl { font-size:0.7rem; color:var(--text-dim); letter-spacing:2px; text-transform:uppercase; margin-top:4px; }

  .btn-row { display:flex; gap:15px; flex-wrap:wrap; justify-content:center; }

  /* ─── SEQUENCE MODE ─── */
  .sequence-track {
    display:flex; gap:8px; justify-content:center;
    flex-wrap:wrap; margin-bottom:20px; min-height:60px;
    align-items:center;
  }
  .seq-item {
    width:50px; height:50px;
    border:1px solid var(--border);
    border-radius:8px;
    display:flex; align-items:center; justify-content:center;
    font-size:1.5rem;
    background:var(--panel);
    transition:0.2s;
  }
  .seq-item.filled { border-color:var(--accent2); background:rgba(123,97,255,0.1); }
  .seq-item.correct-seq { border-color:var(--accent); background:rgba(0,245,196,0.1); }
  .seq-item.wrong-seq { border-color:var(--accent3); background:rgba(255,75,110,0.1); }

  /* ─── COLORS MODE ─── */
  .color-cell {
    width:80px; height:80px; border-radius:12px;
    border:2px solid transparent;
    cursor:pointer; transition:all 0.2s;
    display:flex; align-items:center; justify-content:center;
  }
  .color-cell:hover { transform:scale(1.08); border-color:rgba(255,255,255,0.4); }
  .color-cell.selected { border-color:#fff; box-shadow:0 0 15px rgba(255,255,255,0.5); }
  .color-cell.correct { box-shadow:0 0 20px rgba(0,245,196,0.6); }
  .color-cell.wrong { box-shadow:0 0 20px rgba(255,75,110,0.6); }

  /* ─── NUMBER FLASH MODE ─── */
  .numflash-display {
    font-family: Arial, sans-serif;
    font-size: clamp(3rem, 12vw, 7rem);
    font-weight:900;
    color: #ffffff;
    text-shadow: 0 0 40px rgba(255,255,255,0.4), 0 0 80px rgba(255,255,255,0.15);
    letter-spacing:8px;
    text-align:center;
    padding:30px 40px;
    background:var(--panel);
    border:1px solid rgba(255,190,0,0.3);
    border-radius:16px;
    margin:10px 0 20px;
    min-width:300px;
    animation: numAppear 0.05s ease both;
    position:relative;
  }
  @keyframes numAppear {
    0%{opacity:0;transform:scale(1.1)}
    100%{opacity:1;transform:scale(1)}
  }
  .numflash-display.hidden-num {
    color:transparent;
    text-shadow:none;
    background:rgba(10,15,30,0.9);
    border-color:var(--border);
  }
  .numflash-display.hidden-num::after {
    content:'?';
    position:absolute; inset:0;
    display:flex; align-items:center; justify-content:center;
    font-size:inherit; color:var(--text-dim);
  }
  .numflash-input-row {
    display:flex; gap:10px; align-items:center; justify-content:center;
    margin-bottom:15px; flex-wrap:wrap;
  }
  .digit-btn {
    width:60px; height:60px;
    background:var(--panel); border:1px solid var(--border);
    border-radius:10px; color:var(--text);
    font-family: Arial, sans-serif; font-size:1.3rem; font-weight:700;
    cursor:pointer; transition:all 0.15s;
  }
  .digit-btn:hover { border-color:#ffbe00; color:#ffbe00; background:rgba(255,190,0,0.08); transform:scale(1.05); }
  .digit-btn:active { transform:scale(0.95); }
  .digit-btn.del { color:var(--accent3); font-size:1rem; }
  .digit-btn.del:hover { border-color:var(--accent3); background:rgba(255,75,110,0.08); }
  .numflash-answer {
    font-family: Arial, sans-serif;
    font-size: clamp(2rem, 8vw, 4rem);
    font-weight:900; color:#ffffff;
    letter-spacing:10px; text-align:center;
    min-height:70px; line-height:70px;
    background:rgba(255,190,0,0.05);
    border:1px solid rgba(255,190,0,0.2);
    border-radius:10px; padding:0 20px;
    min-width:280px; margin-bottom:15px;
    transition:0.2s;
  }
  .numflash-answer.wrong-ans { color:var(--accent3); border-color:var(--accent3); background:rgba(255,75,110,0.1); }
  .numflash-answer.correct-ans { color:var(--accent); border-color:var(--accent); background:rgba(0,245,196,0.1); }
  .numflash-correct-reveal {
    font-family:'Orbitron',sans-serif; font-size:1rem;
    color:var(--text-dim); letter-spacing:3px; text-align:center;
    margin-top:5px;
  }
  .numflash-correct-reveal strong { color:var(--accent); }
  .flash-countdown {
    font-family:'Orbitron',sans-serif; font-size:4rem; font-weight:900;
    color:var(--accent2); text-shadow:var(--glow2);
    animation:countAnim 0.9s ease both;
    text-align:center; padding:20px;
  }
  @keyframes countAnim {0%{transform:scale(1.5);opacity:0}50%{opacity:1}100%{transform:scale(0.8);opacity:0}}
  .exposure-badge {
    display:inline-block;
    background:rgba(255,190,0,0.15); border:1px solid rgba(255,190,0,0.4);
    color:#ffbe00; font-family:'Orbitron',sans-serif;
    font-size:0.7rem; font-weight:700; letter-spacing:2px;
    padding:4px 12px; border-radius:20px; margin-bottom:15px;
  }
  .numflash-digits-row {
    display:flex; gap:6px; justify-content:center; margin-bottom:8px;
  }
  .digit-slot {
    width:16px; height:4px; border-radius:2px;
    background:var(--border); transition:0.2s;
  }
  .digit-slot.filled { background:#ffbe00; box-shadow:0 0 6px rgba(255,190,0,0.5); }
  body::after {
    content:''; position:fixed; inset:0;
    background:repeating-linear-gradient(0deg, transparent, transparent 2px, rgba(0,0,0,0.03) 2px, rgba(0,0,0,0.03) 4px);
    pointer-events:none; z-index:100;
  }

  /* Score popup */
  .score-popup {
    position:fixed; top:50%; left:50%;
    transform:translate(-50%,-50%);
    font-family:'Orbitron',sans-serif; font-size:2rem; font-weight:900;
    pointer-events:none; z-index:200;
    animation: popupAnim 0.8s ease forwards;
  }
  .score-popup.plus { color:var(--accent); text-shadow:var(--glow); }
  .score-popup.minus { color:var(--accent3); }
  @keyframes popupAnim { 0%{opacity:1;transform:translate(-50%,-50%) scale(1)} 100%{opacity:0;transform:translate(-50%,-120%) scale(0.8)} }

  /* Confetti particles */
  .particle {
    position:fixed; pointer-events:none; z-index:150;
    width:8px; height:8px; border-radius:2px;
    animation: particleFly 1s ease forwards;
  }
  @keyframes particleFly {
    0%{opacity:1; transform:translate(0,0) rotate(0deg)}
    100%{opacity:0; transform:translate(var(--tx),var(--ty)) rotate(720deg)}
  }

  @media(max-width:600px) {
    header { padding:15px 20px; }
    .logo { font-size:1rem; }
    .stats-bar { gap:15px; }
    .mode-grid { grid-template-columns:1fr; }
  }

  /* ─── NUMFLASH SETUP SCREEN ─── */
  .setup-section {
    width: 100%; max-width: 480px;
    margin-bottom: 28px;
  }
  .setup-section-title {
    font-family: 'Orbitron', sans-serif; font-size: 0.7rem;
    font-weight: 700; letter-spacing: 3px; color: var(--text-dim);
    text-transform: uppercase; margin-bottom: 14px; text-align: center;
  }
  .setup-options {
    display: flex; gap: 12px; justify-content: center;
  }
  .setup-opt {
    background: var(--panel);
    border: 1px solid var(--border);
    border-radius: 12px;
    padding: 16px 20px;
    cursor: pointer;
    transition: all 0.2s;
    text-align: center;
    min-width: 90px;
    flex: 1;
  }
  .setup-opt:hover { border-color: #ffbe00; transform: translateY(-2px); }
  .setup-opt.active {
    border-color: #ffbe00;
    background: rgba(255,190,0,0.1);
    box-shadow: 0 0 16px rgba(255,190,0,0.25);
  }
  .setup-opt-val {
    font-family: 'Orbitron', sans-serif;
    font-size: 1.3rem; font-weight: 900;
    color: #ffbe00; margin-bottom: 4px;
  }
  .setup-opt-lbl {
    font-size: 0.7rem; letter-spacing: 2px;
    color: var(--text-dim); text-transform: uppercase;
  }
  .setup-opt.dsm { min-width: 56px; flex: none; padding: 14px 10px; }
  .setup-opt.dsm .setup-opt-val { font-size: 1.1rem; }
  .setup-preview {
    background: rgba(255,190,0,0.06);
    border: 1px solid rgba(255,190,0,0.2);
    border-radius: 10px;
    padding: 14px 24px;
    color: var(--text-dim); font-size: 0.95rem;
    letter-spacing: 1px; text-align: center;
    margin-bottom: 24px; max-width: 420px; width: 100%;
  }
  .setup-preview strong { color: #ffbe00; }

  /* ─── BACK BUTTON ─── */
  .back-btn {
    background: transparent;
    border: 1px solid var(--border);
    color: var(--text-dim);
    font-family: 'Orbitron', sans-serif;
    font-size: 0.65rem;
    font-weight: 700;
    letter-spacing: 2px;
    padding: 7px 14px;
    border-radius: 6px;
    cursor: pointer;
    transition: all 0.2s;
  }
  .back-btn:hover {
    border-color: var(--accent3);
    color: var(--accent3);
    background: rgba(255,75,110,0.08);
  }

  /* ─── CONFIRM MODAL ─── */
  .modal-overlay {
    position: fixed; inset: 0; z-index: 300;
    background: rgba(5,8,15,0.85);
    backdrop-filter: blur(6px);
    display: flex; align-items: center; justify-content: center;
    animation: fadeIn 0.15s ease;
  }
  @keyframes fadeIn { from{opacity:0} to{opacity:1} }
  .modal-box {
    background: var(--panel);
    border: 1px solid var(--border);
    border-radius: 16px;
    padding: 40px 36px;
    text-align: center;
    max-width: 360px; width: 90%;
    box-shadow: 0 0 60px rgba(0,0,0,0.6);
    animation: slideUp 0.2s cubic-bezier(0.34,1.56,0.64,1);
  }
  @keyframes slideUp { from{transform:translateY(20px);opacity:0} to{transform:translateY(0);opacity:1} }
  .modal-icon { font-size: 2.5rem; margin-bottom: 12px; }
  .modal-title {
    font-family: 'Orbitron', sans-serif; font-size: 1rem;
    font-weight: 700; letter-spacing: 3px; color: var(--text);
    margin-bottom: 8px;
  }
  .modal-sub {
    color: var(--text-dim); font-size: 0.9rem;
    line-height: 1.5; margin-bottom: 28px;
  }
  .modal-btns { display: flex; gap: 12px; justify-content: center; }

  /* ─── NUMFLASH CONFIG SCREEN ─── */
  .numcfg-back { align-self:flex-start; margin-bottom: 10px; }
  .numcfg-icon { font-size: 3rem; margin-bottom: 10px; }
  .numcfg-title {
    font-family: 'Orbitron', sans-serif; font-size: clamp(1.4rem,4vw,2rem);
    font-weight: 900; color: #ffbe00;
    text-shadow: 0 0 30px rgba(255,190,0,0.5);
    letter-spacing: 4px; margin-bottom: 6px; text-align:center;
  }
  .numcfg-sub {
    color: var(--text-dim); font-size: 0.9rem; letter-spacing: 2px;
    margin-bottom: 40px; text-align:center;
  }
  .numcfg-section {
    width: 100%; max-width: 480px;
    margin-bottom: 32px;
  }
  .numcfg-label {
    font-family: 'Orbitron', sans-serif; font-size: 0.7rem;
    letter-spacing: 3px; color: var(--text-dim);
    text-transform: uppercase; margin-bottom: 14px; text-align:center;
  }
  .numcfg-options {
    display: flex; gap: 14px; justify-content: center;
  }
  .numcfg-opt {
    flex: 1; max-width: 130px;
    background: var(--panel); border: 1px solid var(--border);
    border-radius: 12px; padding: 18px 12px;
    cursor: pointer; text-align: center; transition: all 0.2s;
  }
  .numcfg-opt:hover { border-color: #ffbe00; transform: translateY(-2px); }
  .numcfg-opt.active {
    border-color: #ffbe00; background: rgba(255,190,0,0.1);
    box-shadow: 0 0 20px rgba(255,190,0,0.3);
  }
  .numcfg-opt-val {
    font-family: 'Orbitron', sans-serif; font-size: 1.3rem;
    font-weight: 900; color: #ffbe00; margin-bottom: 4px;
  }
  .numcfg-opt-lbl {
    font-size: 0.75rem; color: var(--text-dim); letter-spacing: 2px;
    text-transform: uppercase;
  }
  .numcfg-digits {
    display: flex; flex-wrap: wrap; gap: 8px; justify-content: center;
  }
  .numcfg-preview {
    background: rgba(255,190,0,0.06); border: 1px solid rgba(255,190,0,0.2);
    border-radius: 10px; padding: 14px 24px;
    font-size: 0.95rem; color: var(--text-dim); text-align: center;
    letter-spacing: 1px; margin-bottom: 10px;
    max-width: 400px; width: 100%;
  }
  .numcfg-preview strong { color: #ffbe00; }
</style>
</head>
<body>

<header>
  <div class="logo">NEURO<span>FLASH</span></div>
  <div class="stats-bar">
    <div class="stat">
      <div class="stat-val" id="hdr-score">0</div>
      <div class="stat-lbl">Puntos</div>
    </div>
    <div class="stat">
      <div class="stat-val" id="hdr-streak">0🔥</div>
      <div class="stat-lbl">Racha</div>
    </div>
    <div class="stat">
      <div class="stat-val" id="hdr-level">1</div>
      <div class="stat-lbl">Nivel</div>
    </div>
  </div>
</header>

<main>

  <!-- ═══ HOME ═══ -->
  <div class="screen active" id="screen-home">
    <div class="home-title">MEMORIA EIDÉTICA</div>
    <div class="home-sub">Entrena tu velocidad de percepción visual</div>

    <div class="rounds-row" id="diff-row">
      <button class="diff-btn active" data-diff="easy" onclick="setDiff(this,'easy')">Fácil</button>
      <button class="diff-btn medium" data-diff="medium" onclick="setDiff(this,'medium')">Medio</button>
      <button class="diff-btn hard" data-diff="hard" onclick="setDiff(this,'hard')">Difícil</button>
    </div>

    <div class="mode-grid">
      <div class="mode-card" onclick="startGame('flash')">
        <div class="mode-icon">⚡</div>
        <div class="mode-name">Flash</div>
        <div class="mode-desc">Memoriza símbolos que aparecen brevemente y encuéntralos entre distractores</div>
      </div>
      <div class="mode-card purple" onclick="startGame('sequence')">
        <div class="mode-icon">🔢</div>
        <div class="mode-name">Secuencia</div>
        <div class="mode-desc">Recuerda el orden exacto en que aparecieron los elementos</div>
      </div>
      <div class="mode-card red" onclick="startGame('color')">
        <div class="mode-icon">🎨</div>
        <div class="mode-name">Colores</div>
        <div class="mode-desc">Memoriza el color exacto de cada posición en la cuadrícula</div>
      </div>
      <div class="mode-card" onclick="startGame('odd')">
        <div class="mode-icon">🔍</div>
        <div class="mode-name">Intruso</div>
        <div class="mode-desc">Detecta el elemento diferente que se ocultó entre los demás</div>
      </div>
      <div class="mode-card gold" onclick="startGame('numflash')">
        <div class="mode-icon">🔢</div>
        <div class="mode-name">Números</div>
        <div class="mode-desc" id="numflash-desc">Un número aparece brevemente — ¿puedes recordarlo?</div>
      </div>
    </div>

    <div style="color:var(--text-dim);font-size:0.85rem;letter-spacing:2px;text-align:center">
      MEJOR PUNTUACIÓN: <span id="best-score" style="color:var(--accent);font-family:'Orbitron',sans-serif;">0</span>
    </div>
  </div>

  <!-- ═══ NUMFLASH SETUP ═══ -->
  <div class="screen" id="screen-numsetup">
    <button class="back-btn" style="align-self:flex-start;margin-bottom:24px" onclick="goHome()">&#8592; INICIO</button>
    <div style="font-size:3rem;margin-bottom:10px">🔢</div>
    <div class="home-title" style="font-size:clamp(1.4rem,4vw,2.2rem);margin-bottom:6px">MEMORIA DE NÚMEROS</div>
    <div class="home-sub" style="margin-bottom:36px;font-size:0.9rem">Configura tu ejercicio</div>

    <!-- EXPOSURE TIME -->
    <div class="setup-section">
      <div class="setup-section-title">⏱ TIEMPO DE EXPOSICIÓN</div>
      <div class="setup-options" id="exposure-opts">
        <div class="setup-opt" data-val="1000" onclick="selectSetup(this,'exposure')">
          <div class="setup-opt-val">1 s</div>
          <div class="setup-opt-lbl">Fácil</div>
        </div>
        <div class="setup-opt" data-val="500" onclick="selectSetup(this,'exposure')">
          <div class="setup-opt-val">0.5 s</div>
          <div class="setup-opt-lbl">Medio</div>
        </div>
        <div class="setup-opt active" data-val="100" onclick="selectSetup(this,'exposure')">
          <div class="setup-opt-val">0.1 s</div>
          <div class="setup-opt-lbl">Difícil</div>
        </div>
      </div>
    </div>

    <!-- DIGIT COUNT -->
    <div class="setup-section">
      <div class="setup-section-title">🔢 CANTIDAD DE DÍGITOS</div>
      <div class="setup-options" id="digits-opts" style="flex-wrap:wrap;max-width:420px">
        <div class="setup-opt dsm" data-val="2" onclick="selectSetup(this,'digits')"><div class="setup-opt-val">2</div></div>
        <div class="setup-opt dsm" data-val="3" onclick="selectSetup(this,'digits')"><div class="setup-opt-val">3</div></div>
        <div class="setup-opt dsm active" data-val="4" onclick="selectSetup(this,'digits')"><div class="setup-opt-val">4</div></div>
        <div class="setup-opt dsm" data-val="5" onclick="selectSetup(this,'digits')"><div class="setup-opt-val">5</div></div>
        <div class="setup-opt dsm" data-val="6" onclick="selectSetup(this,'digits')"><div class="setup-opt-val">6</div></div>
        <div class="setup-opt dsm" data-val="7" onclick="selectSetup(this,'digits')"><div class="setup-opt-val">7</div></div>
        <div class="setup-opt dsm" data-val="8" onclick="selectSetup(this,'digits')"><div class="setup-opt-val">8</div></div>
        <div class="setup-opt dsm" data-val="9" onclick="selectSetup(this,'digits')"><div class="setup-opt-val">9</div></div>
      </div>
    </div>

    <!-- PREVIEW -->
    <div class="setup-preview" id="setup-preview">
      Memoriza un número de <strong id="prev-digits">4</strong> dígitos en <strong id="prev-exposure">0.1 segundos</strong>
    </div>

    <button class="btn btn-primary" style="margin-top:10px;font-size:1rem;padding:16px 60px" onclick="launchNumflash()">COMENZAR</button>
  </div>

  <!-- ═══ NUMFLASH CONFIG ═══ -->
  <div class="screen" id="screen-numconfig">
    <div class="numcfg-back">
      <button class="back-btn" onclick="goHome()">&#8592; INICIO</button>
    </div>
    <div class="numcfg-icon">🔢</div>
    <div class="numcfg-title">MEMORIA DE NÚMEROS</div>
    <div class="numcfg-sub">Configura tu sesión antes de empezar</div>

    <div class="numcfg-section">
      <div class="numcfg-label">⏱ TIEMPO DE EXPOSICIÓN</div>
      <div class="numcfg-options">
        <div class="numcfg-opt" id="exp-1000" onclick="selectExposure(1000,this)">
          <div class="numcfg-opt-val">1 seg</div>
          <div class="numcfg-opt-lbl">Fácil</div>
        </div>
        <div class="numcfg-opt" id="exp-500" onclick="selectExposure(500,this)">
          <div class="numcfg-opt-val">0.5 seg</div>
          <div class="numcfg-opt-lbl">Medio</div>
        </div>
        <div class="numcfg-opt" id="exp-100" onclick="selectExposure(100,this)">
          <div class="numcfg-opt-val">0.1 seg</div>
          <div class="numcfg-opt-lbl">Difícil</div>
        </div>
      </div>
    </div>

    <div class="numcfg-section">
      <div class="numcfg-label">🔢 CANTIDAD DE DÍGITOS</div>
      <div class="numcfg-digits">
        <button class="dsb" onclick="setNumDigits(2,this)">2</button>
        <button class="dsb" onclick="setNumDigits(3,this)">3</button>
        <button class="dsb active" onclick="setNumDigits(4,this)">4</button>
        <button class="dsb" onclick="setNumDigits(5,this)">5</button>
        <button class="dsb" onclick="setNumDigits(6,this)">6</button>
        <button class="dsb" onclick="setNumDigits(7,this)">7</button>
        <button class="dsb" onclick="setNumDigits(8,this)">8</button>
        <button class="dsb" onclick="setNumDigits(9,this)">9</button>
      </div>
    </div>

    <div class="numcfg-preview" id="numcfg-preview">
      Verás un número de <strong id="prev-digits">4</strong> dígitos durante <strong id="prev-time">1 segundo</strong>
    </div>

    <button class="btn btn-primary" style="margin-top:10px;font-size:1rem;padding:16px 60px;" onclick="launchNumflash()">
      ▶ COMENZAR
    </button>
  </div>

  <!-- ═══ GAME ═══ -->
  <div class="screen" id="screen-game">
    <div class="game-header">
      <div style="display:flex;flex-direction:column;align-items:flex-start;gap:8px;">
        <button class="back-btn" onclick="confirmGoHome()">&#8592; INICIO</button>
        <div class="game-info">RONDA <span id="g-round">1</span>/<span id="g-total">10</span></div>
      </div>
      <div class="timer-container">
        <svg class="timer-svg" width="90" height="90" viewBox="0 0 90 90">
          <circle class="timer-track" cx="45" cy="45" r="40"/>
          <circle class="timer-bar" id="timer-bar" cx="45" cy="45" r="40"/>
        </svg>
        <div class="timer-num" id="timer-num">3</div>
      </div>
      <div class="game-info">PUNTOS <span id="g-score">0</span></div>
    </div>

    <div class="rounds-row" id="round-dots"></div>
    <div class="phase-label" id="phase-label">MEMORIZA</div>
    <div class="progress-bar"><div class="progress-fill" id="progress-fill" style="width:0%"></div></div>
    <div class="instruction" id="game-instruction"></div>
    <div class="symbol-grid" id="game-grid"></div>
    <div class="sequence-track" id="seq-track" style="display:none"></div>
  </div>

  <!-- ═══ RESULT ═══ -->
  <div class="screen" id="screen-result">
    <div class="result-title">SESIÓN COMPLETADA</div>
    <div class="result-score" id="result-score">0%</div>
    <div class="result-detail" id="result-detail"></div>
    <div class="stats-grid">
      <div class="stat-box"><div class="val" id="r-correct">0</div><div class="lbl">Correctas</div></div>
      <div class="stat-box"><div class="val" id="r-time">0ms</div><div class="lbl">Tiempo prom.</div></div>
      <div class="stat-box"><div class="val" id="r-streak">0</div><div class="lbl">Racha máx.</div></div>
    </div>
    <div class="btn-row">
      <button class="btn btn-primary" onclick="playAgain()">JUGAR DE NUEVO</button>
      <button class="btn btn-secondary" onclick="goHome()">INICIO</button>
    </div>
  </div>

</main>

<script>
// ═══════════════════════════════════════════════
// DATA
// ═══════════════════════════════════════════════
const SYMBOLS = ['🌟','🔥','💎','🌙','⚡','🎯','🦋','🌊','🎭','🔮','🌺','🦊','🐉','🎪','🌈','🏆','💫','🎲','🌸','🦁','🔷','🌿','🎸','🦄','🍄','🎃','🌵','🐺','💠','🎵','🦅','🌻','🔱','💥','🌍','🦈','🎯','🗝️','⚗️','🔬'];
const COLORS_LIST = ['#ff4b6e','#7b61ff','#00f5c4','#ffbe00','#ff8c00','#00b8ff','#ff69b4','#32cd32','#dc143c','#9400d3','#00ced1','#ff6347'];

// ═══════════════════════════════════════════════
// STATE
// ═══════════════════════════════════════════════
let state = {
  mode: 'flash',
  diff: 'easy',
  round: 0, totalRounds: 10,
  score: 0, streak: 0, maxStreak: 0,
  correct: 0, totalAnswered: 0,
  phase: 'memorize', // memorize | recall | feedback
  targetSymbols: [], gridSymbols: [],
  targetColors: [], userColors: [],
  sequence: [], userSequence: [],
  timer: null, timeLeft: 0, maxTime: 3,
  roundStartTime: 0,
  totalTime: 0,
  gridSize: 4,
  selectedCells: [],
  bestScore: parseInt(localStorage.getItem('nf_best')||'0'),
  numFlashTarget: '', numFlashAnswer: '',
  numDigits: 4,
  exposureMs: 1000,
  numExposureMs: 100,
};

// ═══════════════════════════════════════════════
// DIFFICULTY CONFIG
// ═══════════════════════════════════════════════
const DIFF_CONFIG = {
  easy:   { gridSize:3, memorizeTime:3000, targetCount:2, rounds:8,  numDigits:4, exposureMs:1000 },
  medium: { gridSize:4, memorizeTime:2000, targetCount:3, rounds:10, numDigits:4, exposureMs:500  },
  hard:   { gridSize:5, memorizeTime:1200, targetCount:4, rounds:12, numDigits:4, exposureMs:100  },
};

// ═══════════════════════════════════════════════
// UTILS
// ═══════════════════════════════════════════════
function shuffle(arr) {
  let a = [...arr];
  for (let i=a.length-1;i>0;i--){const j=Math.floor(Math.random()*(i+1));[a[i],a[j]]=[a[j],a[i]];}
  return a;
}
function pick(arr, n) { return shuffle(arr).slice(0,n); }
function rnd(min,max){ return Math.floor(Math.random()*(max-min+1))+min; }

function setDiff(btn, diff) {
  document.querySelectorAll('.diff-btn').forEach(b=>b.classList.remove('active'));
  btn.classList.add('active');
  state.diff = diff;
  updateNumflashDesc();
}

function setNumDigits(n, btn) {
  state.numDigits = n;
  document.querySelectorAll('.dsb').forEach(b=>b.classList.remove('active'));
  if(btn) btn.classList.add('active');
  updateNumcfgPreview();
}

function selectExposure(ms, btn) {
  state.exposureMs = ms;
  document.querySelectorAll('.numcfg-opt').forEach(o=>o.classList.remove('active'));
  const el = btn || document.getElementById('exp-'+ms);
  if(el) el.classList.add('active');
  updateNumcfgPreview();
}

function updateNumcfgPreview() {
  const labels = { 1000:'1 segundo', 500:'0.5 segundos', 100:'0.1 segundos' };
  const d = document.getElementById('prev-digits');
  const t = document.getElementById('prev-time');
  if(d) d.textContent = state.numDigits;
  if(t) t.textContent = labels[state.exposureMs] || state.exposureMs+'ms';
}

function launchNumflash() {
  const cfg = DIFF_CONFIG[state.diff];
  state.mode = 'numflash';
  state.round = 0;
  state.totalRounds = cfg.rounds;
  state.correct = 0;
  state.totalAnswered = 0;
  state.streak = 0;
  state.maxStreak = 0;
  state.totalTime = 0;
  state.gridSize = cfg.gridSize;
  buildRoundDots();
  showScreen('game');
  setTimeout(nextRound, 300);
}

function updateNumflashDesc() {
  const desc = document.getElementById('numflash-desc');
  if(desc) desc.innerHTML = `Un número aparece brevemente — ¿puedes recordarlo?`;
}

function updateHeader() {
  document.getElementById('hdr-score').textContent = state.score;
  document.getElementById('hdr-streak').textContent = state.streak + '🔥';
  document.getElementById('hdr-level').textContent = Math.floor(state.score/500)+1;
  document.getElementById('best-score').textContent = state.bestScore;
}

function showScreen(id) {
  document.querySelectorAll('.screen').forEach(s=>s.classList.remove('active'));
  document.getElementById('screen-'+id).classList.add('active');
}

// ═══════════════════════════════════════════════
// GAME START
// ═══════════════════════════════════════════════
function startGame(mode) {
  if(mode === 'numflash') {
    showScreen('numconfig');
    selectExposure(state.exposureMs, document.getElementById('exp-'+state.exposureMs));
    updateNumcfgPreview();
    return;
  }
  const cfg = DIFF_CONFIG[state.diff];
  state.mode = mode;
  state.round = 0;
  state.totalRounds = cfg.rounds;
  state.correct = 0;
  state.totalAnswered = 0;
  state.streak = 0;
  state.maxStreak = 0;
  state.totalTime = 0;
  state.gridSize = cfg.gridSize;

  buildRoundDots();
  showScreen('game');
  setTimeout(nextRound, 300);
}

function buildRoundDots() {
  const el = document.getElementById('round-dots');
  el.innerHTML = '';
  for(let i=0;i<state.totalRounds;i++){
    const d=document.createElement('div');
    d.className='round-dot';
    d.id=`dot-${i}`;
    el.appendChild(d);
  }
}

function nextRound() {
  if(state.round >= state.totalRounds){ endGame(); return; }
  const cfg = DIFF_CONFIG[state.diff];

  // update dots
  for(let i=0;i<state.totalRounds;i++){
    const d=document.getElementById(`dot-${i}`);
    if(!d)continue;
    d.className='round-dot'+(i<state.round?' done':i===state.round?' current':'');
  }
  document.getElementById('g-round').textContent = state.round+1;
  document.getElementById('g-total').textContent = state.totalRounds;
  document.getElementById('g-score').textContent = state.score;
  document.getElementById('progress-fill').style.width = (state.round/state.totalRounds*100)+'%';

  if(state.mode==='flash') flashRound(cfg);
  else if(state.mode==='sequence') sequenceRound(cfg);
  else if(state.mode==='color') colorRound(cfg);
  else if(state.mode==='odd') oddRound(cfg);
  else if(state.mode==='numflash') numFlashRound(cfg);
}

// ═══════════════════════════════════════════════
// FLASH MODE
// ═══════════════════════════════════════════════
function flashRound(cfg) {
  const total = cfg.gridSize * cfg.gridSize;
  const targets = pick(SYMBOLS.slice(0,25), cfg.targetCount);
  const distractors = pick(SYMBOLS.filter(s=>!targets.includes(s)), total - cfg.targetCount);
  const recallSymbols = shuffle([...targets, ...distractors]);

  state.targetSymbols = targets;
  state.gridSymbols = recallSymbols;
  state.selectedCells = [];
  state.phase = 'memorize';

  setPhaseLabel('memorize', '⚡ MEMORIZA');
  setInstruction(`Recuerda <strong>${cfg.targetCount}</strong> símbolo${cfg.targetCount>1?'s':''} que aparecerán`);

  // Show targets
  const grid = document.getElementById('game-grid');
  grid.style.gridTemplateColumns = `repeat(${Math.ceil(Math.sqrt(cfg.targetCount))}, 1fr)`;
  grid.style.maxWidth = cfg.targetCount<=4 ? '200px' : '300px';
  document.getElementById('seq-track').style.display='none';

  grid.innerHTML = '';
  for(let t of targets){
    const cell = makeCell(t, false);
    cell.style.width='80px'; cell.style.height='80px';
    cell.classList.add('flash');
    grid.appendChild(cell);
  }

  startTimer(cfg.memorizeTime/1000, ()=>{
    // Recall phase
    state.phase='recall';
    setPhaseLabel('recall', '🔍 ENCUENTRA LOS SÍMBOLOS');
    setInstruction(`Selecciona los <strong>${cfg.targetCount}</strong> símbolo${cfg.targetCount>1?'s':''} que viste`);

    grid.style.gridTemplateColumns = `repeat(${cfg.gridSize}, 1fr)`;
    grid.style.maxWidth = cfg.gridSize<=4?'360px':'440px';
    grid.innerHTML='';
    state.roundStartTime=performance.now();

    recallSymbols.forEach((sym,i)=>{
      const cell=makeCell(sym,true);
      cell.dataset.index=i;
      cell.dataset.sym=sym;
      cell.addEventListener('click',()=>flashCellClick(cell, targets, cfg));
      grid.appendChild(cell);
    });
  });
}

function flashCellClick(cell, targets, cfg) {
  if(state.phase!=='recall') return;
  const sym = cell.dataset.sym;
  const isTarget = targets.includes(sym);

  if(cell.classList.contains('selected') || cell.classList.contains('correct') || cell.classList.contains('wrong')) return;

  if(isTarget){ cell.classList.add('selected'); }
  else {
    cell.classList.add('wrong');
    state.selectedCells.push({cell,correct:false});
    recordAnswer(false);
    setTimeout(()=>finishFlashRound(targets), 600);
    return;
  }

  state.selectedCells.push({cell,correct:true});

  if(state.selectedCells.filter(x=>x.correct).length === targets.length){
    state.selectedCells.forEach(x=>x.cell.classList.add('correct'));
    const dt = performance.now()-state.roundStartTime;
    state.totalTime += dt;
    recordAnswer(true);
    showScorePopup('+100', true);
    spawnParticles();
    setTimeout(()=>{ state.round++; nextRound(); }, 700);
  }
}

function finishFlashRound(targets) {
  // highlight correct
  document.querySelectorAll('#game-grid .cell').forEach(c=>{
    if(targets.includes(c.dataset.sym)) c.classList.add('highlight');
  });
  setTimeout(()=>{ state.round++; nextRound(); }, 900);
}

// ═══════════════════════════════════════════════
// SEQUENCE MODE
// ═══════════════════════════════════════════════
function sequenceRound(cfg) {
  const n = cfg.targetCount + 1;
  const seqItems = pick(SYMBOLS.slice(0,20), n);
  state.sequence = seqItems;
  state.userSequence = [];
  state.phase = 'memorize';

  setPhaseLabel('memorize','🔢 MEMORIZA LA SECUENCIA');
  setInstruction('Observa el orden de aparición');

  const grid = document.getElementById('game-grid');
  const track = document.getElementById('seq-track');
  track.style.display='flex';
  grid.style.gridTemplateColumns=`repeat(${Math.min(n,5)}, 1fr)`;
  grid.style.maxWidth=`${Math.min(n,5)*80}px`;
  grid.innerHTML=''; track.innerHTML='';

  // show sequence one by one
  let idx=0;
  const showNext = ()=>{
    grid.innerHTML='';
    if(idx>=seqItems.length){
      // recall
      state.phase='recall';
      setPhaseLabel('recall','👆 REPITE LA SECUENCIA');
      setInstruction('Toca los símbolos en el mismo orden');

      const recallItems = shuffle([...seqItems]);
      grid.innerHTML=''; track.innerHTML='';
      seqItems.forEach((_,i)=>{
        const slot=document.createElement('div');
        slot.className='seq-item'; slot.id=`slot-${i}`;
        track.appendChild(slot);
      });

      state.roundStartTime=performance.now();
      recallItems.forEach((sym,i)=>{
        const cell=makeCell(sym,true);
        cell.style.width='68px'; cell.style.height='68px';
        cell.dataset.sym=sym;
        cell.addEventListener('click',()=>seqClick(sym, seqItems));
        grid.appendChild(cell);
      });
      return;
    }
    const cell=makeCell(seqItems[idx],false);
    cell.style.width='80px'; cell.style.height='80px';
    cell.classList.add('flash');
    grid.appendChild(cell);
    idx++;
    setTimeout(showNext, 700);
  };
  clearTimer();
  setTimerDisplay(0);
  showNext();
}

function seqClick(sym, sequence) {
  if(state.phase!=='recall') return;
  const pos = state.userSequence.length;
  const slot = document.getElementById(`slot-${pos}`);
  if(!slot) return;

  state.userSequence.push(sym);
  slot.textContent = sym;
  slot.classList.add('filled');

  if(sym !== sequence[pos]){
    slot.classList.remove('filled'); slot.classList.add('wrong-seq');
    // show correct
    sequence.forEach((s,i)=>{
      const sl=document.getElementById(`slot-${i}`);
      if(sl){ sl.textContent=s; sl.className='seq-item correct-seq'; }
    });
    recordAnswer(false);
    showScorePopup('-50', false);
    setTimeout(()=>{ state.round++; nextRound(); }, 1000);
    return;
  }

  slot.classList.add('correct-seq');
  if(state.userSequence.length === sequence.length){
    const dt=performance.now()-state.roundStartTime;
    state.totalTime+=dt;
    recordAnswer(true);
    showScorePopup('+120', true);
    spawnParticles();
    setTimeout(()=>{ state.round++; nextRound(); }, 800);
  }
}

// ═══════════════════════════════════════════════
// COLOR MODE
// ═══════════════════════════════════════════════
function colorRound(cfg) {
  const n = cfg.gridSize; // n×n but use n cells for simplicity
  const cellCount = Math.min(n*2, 8);
  const colorSet = pick(COLORS_LIST, Math.min(cellCount,6));
  const assignment = Array.from({length:cellCount},()=>colorSet[rnd(0,colorSet.length-1)]);
  const targetIdx = rnd(0,cellCount-1);

  state.phase='memorize';
  setPhaseLabel('memorize','🎨 MEMORIZA LOS COLORES');
  setInstruction('¿De qué color es la celda con el símbolo?');

  const grid=document.getElementById('game-grid');
  document.getElementById('seq-track').style.display='none';
  grid.style.gridTemplateColumns=`repeat(${Math.min(cellCount,4)}, 1fr)`;
  grid.style.maxWidth=`${Math.min(cellCount,4)*90}px`;
  grid.innerHTML='';

  assignment.forEach((col,i)=>{
    const cell=document.createElement('div');
    cell.className='color-cell';
    cell.style.background=col;
    if(i===targetIdx) { cell.innerHTML='<span style="font-size:1.8rem">⭐</span>'; }
    grid.appendChild(cell);
  });

  const targetColor = assignment[targetIdx];
  state.targetColors = [targetColor];

  startTimer(DIFF_CONFIG[state.diff].memorizeTime/1000, ()=>{
    // Recall: show blank cells, ask which color
    state.phase='recall';
    setPhaseLabel('recall','🎨 ¿QUÉ COLOR ERA LA ESTRELLA?');
    setInstruction('Selecciona el color correcto');

    grid.style.gridTemplateColumns=`repeat(${Math.min(COLORS_LIST.length,4)}, 1fr)`;
    grid.style.maxWidth='380px'; grid.innerHTML='';
    state.roundStartTime=performance.now();

    const options = shuffle([...new Set([...pick(COLORS_LIST.filter(c=>c!==targetColor),3), targetColor])]);
    options.forEach(col=>{
      const cell=document.createElement('div');
      cell.className='color-cell';
      cell.style.background=col;
      cell.style.width='70px'; cell.style.height='70px';
      cell.addEventListener('click',()=>{
        const correct=(col===targetColor);
        cell.classList.add(correct?'correct':'wrong');
        if(!correct){
          options.forEach(c=>{
            if(c===targetColor){
              grid.querySelectorAll('.color-cell').forEach(cc=>{
                if(cc.style.background===targetColor||getComputedStyle(cc).backgroundColor===targetColor) cc.classList.add('correct');
              });
            }
          });
        }
        if(correct){ showScorePopup('+80',true); spawnParticles(); }
        else showScorePopup('-40',false);
        const dt=performance.now()-state.roundStartTime;
        state.totalTime+=dt;
        recordAnswer(correct);
        setTimeout(()=>{ state.round++; nextRound(); }, 700);
      });
      grid.appendChild(cell);
    });
  });
}

// ═══════════════════════════════════════════════
// ODD MODE (intruso)
// ═══════════════════════════════════════════════
function oddRound(cfg) {
  const total = cfg.gridSize * cfg.gridSize;
  const mainSym = pick(SYMBOLS.slice(0,20),1)[0];
  const oddSym = pick(SYMBOLS.filter(s=>s!==mainSym).slice(0,20),1)[0];
  const oddIdx = rnd(0,total-1);
  const symbols = Array.from({length:total},(_,i)=>i===oddIdx?oddSym:mainSym);

  state.phase='memorize';
  setPhaseLabel('memorize','🔍 ENCUENTRA EL INTRUSO');
  setInstruction(`Encuentra el símbolo diferente y recuerda su posición`);

  const grid=document.getElementById('game-grid');
  document.getElementById('seq-track').style.display='none';
  grid.style.gridTemplateColumns=`repeat(${cfg.gridSize},1fr)`;
  grid.style.maxWidth=cfg.gridSize<=4?'360px':'440px';
  grid.innerHTML='';

  symbols.forEach((sym,i)=>{
    const cell=makeCell(sym,false);
    grid.appendChild(cell);
  });

  startTimer(DIFF_CONFIG[state.diff].memorizeTime/1000, ()=>{
    state.phase='recall';
    setPhaseLabel('recall','👆 TOCA EL INTRUSO');
    setInstruction('¿Dónde estaba el símbolo diferente?');

    grid.innerHTML='';
    state.roundStartTime=performance.now();
    symbols.forEach((sym,i)=>{
      const cell=makeCell(sym,true);
      cell.addEventListener('click',()=>{
        const correct=(i===oddIdx);
        cell.classList.add(correct?'correct':'wrong');
        if(!correct) grid.children[oddIdx]?.classList.add('highlight');
        if(correct){ showScorePopup('+100',true); spawnParticles(); }
        else showScorePopup('-30',false);
        const dt=performance.now()-state.roundStartTime;
        state.totalTime+=dt;
        recordAnswer(correct);
        setTimeout(()=>{ state.round++; nextRound(); }, 800);
      });
      grid.appendChild(cell);
    });
  });
}

// ═══════════════════════════════════════════════
// NUMBER FLASH MODE
// ═══════════════════════════════════════════════
function numFlashRound(cfg) {
  const digits = state.numDigits;
  const exposureMs = state.exposureMs;
  const exposureLabel = exposureMs >= 1000 ? '1 segundo' : exposureMs === 500 ? '0.5 segundos' : '0.1 segundos';
  const min = Math.pow(10, digits-1);
  const max = Math.pow(10, digits) - 1;
  const target = rnd(min, max).toString();
  state.numFlashTarget = target;
  state.numFlashAnswer = '';
  state.phase = 'memorize';

  const grid = document.getElementById('game-grid');
  const seqTrack = document.getElementById('seq-track');
  seqTrack.style.display='none';
  grid.style.maxWidth='500px';
  grid.style.gridTemplateColumns='1fr';
  grid.innerHTML='';
  clearTimer();

  setPhaseLabel('memorize','⚡ MEMORIZA EL NÚMERO');
  setInstruction('Prepárate...');

  // countdown 3-2-1
  let count = 3;
  const showCount = () => {
    grid.innerHTML='';
    if(count > 0){
      const cd = document.createElement('div');
      cd.className='flash-countdown'; cd.textContent=count;
      grid.appendChild(cd);
      count--;
      setTimeout(showCount, 700);
    } else {
      // Mostrar interfaz completa con número visible en el cuadro
      setInstruction(`El número desaparece en <strong>${exposureLabel}</strong>`);
      grid.innerHTML='';

      // digit slots — todos activos (llenos) durante el flash
      const slotsRow = document.createElement('div');
      slotsRow.className='numflash-digits-row'; slotsRow.id='nf-slots';
      for(let i=0;i<digits;i++){
        const s=document.createElement('div');
        s.className='digit-slot filled'; s.id=`nf-slot-${i}`;
        slotsRow.appendChild(s);
      }
      grid.appendChild(slotsRow);

      // cuadro de respuesta — muestra el número durante el flash
      const ansDiv = document.createElement('div');
      ansDiv.className='numflash-answer'; ansDiv.id='nf-answer';
      ansDiv.textContent = target;
      ansDiv.style.borderColor = 'rgba(255,255,255,0.5)';
      ansDiv.style.background = 'rgba(255,255,255,0.06)';
      grid.appendChild(ansDiv);

      const revealDiv = document.createElement('div');
      revealDiv.className='numflash-correct-reveal'; revealDiv.id='nf-reveal';
      grid.appendChild(revealDiv);

      // teclado numérico — atenuado e inactivo durante el flash
      const pad = document.createElement('div');
      pad.id='nf-pad';
      pad.style.cssText='display:flex;flex-direction:column;align-items:center;gap:8px;opacity:0.2;pointer-events:none;transition:opacity 0.4s;';
      const rows = [['7','8','9'],['4','5','6'],['1','2','3'],['⌫','0','✓']];
      rows.forEach(row=>{
        const rowDiv=document.createElement('div');
        rowDiv.className='numflash-input-row';
        row.forEach(key=>{
          const btn=document.createElement('button');
          btn.className='digit-btn'+(key==='⌫'?' del':'')+(key==='✓'?' btn btn-primary':'');
          btn.textContent=key;
          if(key==='✓') btn.style.cssText='width:60px;height:60px;padding:0;border-radius:10px;font-size:1.2rem;';
          btn.addEventListener('click',()=>numPadPress(key, target, digits));
          rowDiv.appendChild(btn);
        });
        pad.appendChild(rowDiv);
      });
      grid.appendChild(pad);

      // Tras el tiempo de exposición: borrar número y activar teclado
      setTimeout(()=>{
        state.phase='recall';
        setPhaseLabel('recall','🧠 ESCRIBE EL NÚMERO');
        setInstruction('¿Qué número viste?');

        ansDiv.textContent='';
        ansDiv.style.borderColor='';
        ansDiv.style.background='';
        for(let i=0;i<digits;i++){
          const sl=document.getElementById(`nf-slot-${i}`);
          if(sl) sl.className='digit-slot';
        }

        pad.style.opacity='1';
        pad.style.pointerEvents='auto';
        state.roundStartTime = performance.now();
      }, exposureMs);
    }
  };
  showCount();
}

function buildNumInput(target, digits, grid) {
  // digit slots indicator
  const slotsRow = document.createElement('div');
  slotsRow.className='numflash-digits-row'; slotsRow.id='nf-slots';
  for(let i=0;i<digits;i++){
    const s=document.createElement('div'); s.className='digit-slot'; s.id=`nf-slot-${i}`;
    slotsRow.appendChild(s);
  }
  grid.appendChild(slotsRow);

  // answer display
  const ansDiv = document.createElement('div');
  ansDiv.className='numflash-answer'; ansDiv.id='nf-answer';
  ansDiv.textContent='';
  grid.appendChild(ansDiv);

  const revealDiv = document.createElement('div');
  revealDiv.className='numflash-correct-reveal'; revealDiv.id='nf-reveal';
  grid.appendChild(revealDiv);

  // numpad
  const pad = document.createElement('div');
  pad.style.cssText='display:flex;flex-direction:column;align-items:center;gap:8px;';

  const rows = [['7','8','9'],['4','5','6'],['1','2','3'],['⌫','0','✓']];
  rows.forEach(row=>{
    const rowDiv=document.createElement('div');
    rowDiv.className='numflash-input-row';
    row.forEach(key=>{
      const btn=document.createElement('button');
      btn.className='digit-btn'+(key==='⌫'?' del':'')+(key==='✓'?' btn btn-primary':'');
      btn.textContent=key;
      if(key==='✓'){
        btn.style.cssText='width:60px;height:60px;padding:0;border-radius:10px;font-size:1.2rem;';
      }
      btn.addEventListener('click',()=>numPadPress(key, target, digits));
      rowDiv.appendChild(btn);
    });
    pad.appendChild(rowDiv);
  });
  grid.appendChild(pad);
}

function numPadPress(key, target, digits) {
  if(state.phase!=='recall') return;
  const ansDiv=document.getElementById('nf-answer');
  const revDiv=document.getElementById('nf-reveal');
  if(!ansDiv) return;

  if(key==='⌫'){
    state.numFlashAnswer=state.numFlashAnswer.slice(0,-1);
  } else if(key==='✓'){
    // submit
    submitNumAnswer(target, digits);
    return;
  } else {
    if(state.numFlashAnswer.length >= digits) return;
    state.numFlashAnswer += key;
    // auto-submit when enough digits
    if(state.numFlashAnswer.length === digits){
      setTimeout(()=>submitNumAnswer(target, digits), 200);
    }
  }

  ansDiv.textContent = state.numFlashAnswer || '';
  // update slots
  for(let i=0;i<digits;i++){
    const slot=document.getElementById(`nf-slot-${i}`);
    if(slot) slot.className='digit-slot'+(i<state.numFlashAnswer.length?' filled':'');
  }
}

function submitNumAnswer(target, digits) {
  if(state.phase!=='recall') return;
  state.phase='feedback';
  const ansDiv=document.getElementById('nf-answer');
  const revDiv=document.getElementById('nf-reveal');
  const answer=state.numFlashAnswer;
  const correct=(answer===target);

  // disable pad
  document.querySelectorAll('.digit-btn').forEach(b=>b.disabled=true);

  if(correct){
    ansDiv.classList.add('correct-ans');
    showScorePopup('+150',true);
    spawnParticles();
    if(revDiv) revDiv.innerHTML='✅ ¡Correcto!';
  } else {
    ansDiv.classList.add('wrong-ans');
    ansDiv.textContent = answer || '—';
    showScorePopup('-50',false);
    if(revDiv) revDiv.innerHTML=`❌ Era: <strong>${target}</strong>`;
  }

  const dt=performance.now()-state.roundStartTime;
  state.totalTime+=dt;
  recordAnswer(correct);
  setTimeout(()=>{ state.round++; nextRound(); }, 1200);
}


function makeCell(sym, clickable) {
  const cell=document.createElement('div');
  cell.className='cell'+(clickable?' clickable':'');
  const span=document.createElement('span');
  span.className='cell-content'; span.textContent=sym;
  cell.appendChild(span);
  return cell;
}

function setPhaseLabel(cls, text) {
  const el=document.getElementById('phase-label');
  el.className='phase-label '+cls; el.textContent=text;
}
function setInstruction(html) { document.getElementById('game-instruction').innerHTML=html; }

function recordAnswer(correct) {
  state.totalAnswered++;
  if(correct){
    state.correct++;
    state.streak++;
    state.maxStreak=Math.max(state.maxStreak,state.streak);
    const bonus = state.streak>=3?50:0;
    state.score += correct ? (100 + bonus) : 0;
  } else {
    state.streak=0;
    state.score=Math.max(0, state.score-30);
  }
  document.getElementById('g-score').textContent=state.score;
  updateHeader();
}

// ─── TIMER ───
function startTimer(seconds, callback) {
  clearTimer();
  state.timeLeft = seconds; state.maxTime = seconds;
  updateTimerUI();
  state.timer = setInterval(()=>{
    state.timeLeft -= 0.1;
    updateTimerUI();
    if(state.timeLeft<=0){ clearTimer(); callback(); }
  }, 100);
}
function clearTimer() { if(state.timer){ clearInterval(state.timer); state.timer=null; } }
function setTimerDisplay(v) {
  document.getElementById('timer-num').textContent=Math.ceil(v)||'';
  document.getElementById('timer-bar').style.strokeDashoffset=0;
}
function updateTimerUI() {
  const pct = state.timeLeft/state.maxTime;
  const bar=document.getElementById('timer-bar');
  const num=document.getElementById('timer-num');
  bar.style.strokeDashoffset = 251*(1-pct);
  bar.style.stroke = pct>0.5?'var(--accent)':pct>0.25?'var(--accent2)':'var(--accent3)';
  num.textContent=Math.ceil(state.timeLeft);
  num.className='timer-num'+(state.timeLeft<=2?' urgent':'');
}

// ─── SCORE POPUP ───
function showScorePopup(text, positive) {
  const el=document.createElement('div');
  el.className='score-popup '+(positive?'plus':'minus');
  el.textContent=text;
  document.body.appendChild(el);
  setTimeout(()=>el.remove(), 900);
}

// ─── PARTICLES ───
function spawnParticles() {
  const colors=[`var(--accent)`,`var(--accent2)`,`var(--accent3)`,'#ffbe00'];
  for(let i=0;i<12;i++){
    const p=document.createElement('div');
    p.className='particle';
    p.style.cssText=`left:${45+rnd(-20,20)}vw;top:${40+rnd(-10,10)}vh;background:${colors[rnd(0,3)]};--tx:${rnd(-100,100)}px;--ty:${rnd(-150,-50)}px;animation-delay:${rnd(0,200)}ms`;
    document.body.appendChild(p);
    setTimeout(()=>p.remove(),1200);
  }
}

// ═══════════════════════════════════════════════
// END GAME
// ═══════════════════════════════════════════════
function endGame() {
  clearTimer();
  const pct = Math.round(state.correct/state.totalRounds*100);
  const avgTime = state.totalAnswered>0 ? Math.round(state.totalTime/state.totalAnswered) : 0;

  if(state.score > state.bestScore){
    state.bestScore=state.score;
    localStorage.setItem('nf_best',state.score);
  }

  const sc=document.getElementById('result-score');
  sc.textContent=pct+'%';
  sc.className='result-score '+(pct>=80?'great':pct>=50?'ok':'bad');

  const msgs={great:['¡Memoria excepcional!','¡Rendimiento sobresaliente!'],ok:['¡Buen intento!','Sigue practicando'],bad:['Necesitas más práctica','¡No te rindas!']};
  const cat=pct>=80?'great':pct>=50?'ok':'bad';
  document.getElementById('result-detail').innerHTML=msgs[cat][rnd(0,1)]+` — <strong>${state.correct}/${state.totalRounds}</strong> rondas correctas`;

  document.getElementById('r-correct').textContent=state.correct;
  document.getElementById('r-time').textContent=avgTime+'ms';
  document.getElementById('r-streak').textContent=state.maxStreak;

  updateHeader();
  showScreen('result');
  if(pct>=80) spawnParticles();
}

function confirmGoHome() {
  const overlay = document.createElement('div');
  overlay.className = 'modal-overlay';
  overlay.id = 'confirm-modal';
  overlay.innerHTML = `
    <div class="modal-box">
      <div class="modal-icon">🏠</div>
      <div class="modal-title">¿SALIR DEL JUEGO?</div>
      <div class="modal-sub">Tu progreso en esta sesión se perderá.<br>¿Quieres regresar al inicio?</div>
      <div class="modal-btns">
        <button class="btn btn-secondary" onclick="closeModal()">CANCELAR</button>
        <button class="btn" style="background:linear-gradient(135deg,var(--accent3),#c0334f);color:#fff;padding:14px 28px;border:none;border-radius:8px;font-family:'Orbitron',sans-serif;font-size:0.8rem;font-weight:700;letter-spacing:2px;cursor:pointer;" onclick="closeModalAndGoHome()">SALIR</button>
      </div>
    </div>`;
  document.body.appendChild(overlay);
}

function closeModal() {
  const m = document.getElementById('confirm-modal');
  if(m) m.remove();
}

function closeModalAndGoHome() {
  closeModal();
  goHome();
}

function playAgain() {
  if(state.mode === 'numflash') { showScreen('numsetup'); return; }
  startGame(state.mode);
}
function goHome() {
  clearTimer();
  showScreen('home');
  updateHeader();
}

// Init
document.getElementById('best-score').textContent=state.bestScore;
updateHeader();
updateNumflashDesc();
</script>
</body>
</html>
