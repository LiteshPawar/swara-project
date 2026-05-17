<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>SWARA - AI Speech Therapy</title>
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800;900&display=swap" rel="stylesheet">
<style>
  :root {
    --primary: #6c47ff;
    --primary-dark: #5035cc;
    --primary-light: #ede9ff;
    --accent: #ff6b9d;
    --accent2: #00d4aa;
    --success: #00c47a;
    --warn: #ffb020;
    --danger: #ff4757;
    --bg: #0f0c29;
    --surface: rgba(255,255,255,0.07);
    --surface-solid: #1a1640;
    --border: rgba(255,255,255,0.12);
    --text: #fff;
    --text-muted: rgba(255,255,255,0.55);
    --card-shadow: 0 8px 32px rgba(0,0,0,0.35);
  }

  * { box-sizing: border-box; margin: 0; padding: 0; }

  body {
    font-family: 'Inter', 'Segoe UI', sans-serif;
    background: linear-gradient(135deg, #0f0c29 0%, #302b63 50%, #24243e 100%);
    min-height: 100vh;
    color: var(--text);
  }

  /* ── SCROLLBAR ── */
  ::-webkit-scrollbar { width: 6px; }
  ::-webkit-scrollbar-track { background: transparent; }
  ::-webkit-scrollbar-thumb { background: rgba(255,255,255,0.2); border-radius: 99px; }

  /* ── HEADER ── */
  .header {
    padding: 18px 24px;
    display: flex; align-items: center; gap: 14px;
    background: rgba(255,255,255,0.05);
    backdrop-filter: blur(20px);
    border-bottom: 1px solid var(--border);
    position: sticky; top: 0; z-index: 100;
  }
  .hlogo {
    width: 48px; height: 48px; border-radius: 16px;
    background: linear-gradient(135deg, var(--primary), var(--accent));
    display: flex; align-items: center; justify-content: center;
    font-size: 24px; box-shadow: 0 4px 20px rgba(108,71,255,0.5);
    flex-shrink: 0;
  }
  .htitle {
    font-size: 18px; font-weight: 900; letter-spacing: -.3px;
    background: linear-gradient(90deg, #fff, #c4b5fd);
    -webkit-background-clip: text; -webkit-text-fill-color: transparent;
  }
  .hsub { color: var(--text-muted); font-size: 11px; font-weight: 500; margin-top: 2px; }
  .hstreak {
    margin-left: auto;
    background: linear-gradient(135deg, #ff6b35, #ff8c42);
    border-radius: 99px; padding: 7px 16px;
    color: #fff; font-size: 13px; font-weight: 800;
    box-shadow: 0 4px 16px rgba(255,107,53,0.4);
    white-space: nowrap; flex-shrink: 0;
  }

  /* ── LAYOUT ── */
  .body { padding: 20px 16px 40px; max-width: 660px; margin: 0 auto; }

  /* ── TABS ── */
  .tab-row {
    display: flex; gap: 4px; margin-bottom: 20px;
    background: rgba(255,255,255,0.06);
    padding: 5px; border-radius: 16px;
    border: 1px solid var(--border);
  }
  .tab {
    flex: 1; padding: 9px 4px;
    border: none; border-radius: 11px;
    background: transparent; font-family: inherit;
    font-size: 11px; font-weight: 700;
    color: var(--text-muted); cursor: pointer;
    text-align: center; transition: all .2s;
  }
  .tab:hover { color: #fff; background: rgba(255,255,255,0.08); }
  .tab.active {
    background: linear-gradient(135deg, var(--primary), #9b73ff);
    color: #fff;
    box-shadow: 0 4px 14px rgba(108,71,255,0.45);
  }

  .panel { display: none; }
  .panel.show { display: block; }

  /* ── CARDS ── */
  .card {
    background: rgba(255,255,255,0.07);
    backdrop-filter: blur(20px);
    border: 1px solid var(--border);
    border-radius: 20px; padding: 20px;
    margin-bottom: 14px;
    box-shadow: var(--card-shadow);
    transition: transform .2s;
  }
  .card:hover { transform: translateY(-1px); }

  .clabel {
    font-size: 10px; font-weight: 800; letter-spacing: 1.2px;
    text-transform: uppercase; margin-bottom: 14px;
    color: var(--text-muted);
    display: flex; align-items: center; gap: 6px;
  }
  .clabel::after {
    content: ''; flex: 1; height: 1px;
    background: linear-gradient(90deg, var(--border), transparent);
  }

  /* ── INPUTS ── */
  .user-row { display: flex; gap: 10px; align-items: center; }
  .user-label { font-size: 12px; color: var(--text-muted); font-weight: 600; white-space: nowrap; }
  .user-input {
    flex: 1; padding: 10px 14px;
    border: 1.5px solid var(--border); border-radius: 12px;
    font-family: inherit; font-size: 14px; font-weight: 600;
    outline: none; background: rgba(255,255,255,0.08);
    color: #fff; transition: border-color .2s, box-shadow .2s;
  }
  .user-input::placeholder { color: var(--text-muted); }
  .user-input:focus {
    border-color: var(--primary);
    box-shadow: 0 0 0 3px rgba(108,71,255,0.25);
  }

  select {
    width: 100%; padding: 12px 16px;
    border: 1.5px solid var(--border); border-radius: 12px;
    font-family: inherit; font-size: 15px; font-weight: 600;
    color: #fff; background: rgba(255,255,255,0.1);
    outline: none; cursor: pointer; appearance: none;
    transition: border-color .2s;
  }
  select:focus { border-color: var(--primary); }
  select option { background: #302b63; color: #fff; }

  /* ── LEVEL TOGGLE ── */
  .level-toggle {
    display: flex; gap: 0; margin-bottom: 18px;
    border: 1.5px solid var(--border); border-radius: 16px;
    overflow: hidden; background: rgba(255,255,255,0.05);
  }
  .level-btn {
    flex: 1; padding: 13px 10px;
    border: none; background: transparent;
    font-family: inherit; font-size: 13px; font-weight: 800;
    color: var(--text-muted); cursor: pointer;
    transition: all .25s; text-align: center;
    position: relative;
  }
  .level-btn:not(:last-child) { border-right: 1px solid var(--border); }
  .level-btn.active {
    background: linear-gradient(135deg, var(--primary), #9b73ff);
    color: #fff;
    box-shadow: inset 0 1px 0 rgba(255,255,255,0.15);
  }
  .level-badge {
    display: inline-block; font-size: 9px; font-weight: 900; letter-spacing: .5px;
    background: var(--warn); color: #1a1200; border-radius: 6px;
    padding: 2px 6px; margin-left: 5px; vertical-align: middle;
    text-transform: uppercase;
  }
  .level-btn.active .level-badge { background: rgba(255,255,255,0.3); color: #fff; }

  /* ── DIFFICULTY ── */
  .diff-row { display: flex; gap: 8px; margin-top: 12px; }
  .diff-btn {
    flex: 1; padding: 10px 4px;
    border: 1.5px solid var(--border); border-radius: 12px;
    background: rgba(255,255,255,0.06); font-family: inherit;
    font-size: 12px; font-weight: 700; color: var(--text-muted);
    cursor: pointer; text-align: center; transition: all .2s;
  }
  .diff-btn:hover { color: #fff; border-color: rgba(255,255,255,0.3); }
  .diff-btn.active {
    background: linear-gradient(135deg, var(--primary), #9b73ff);
    border-color: transparent; color: #fff;
    box-shadow: 0 4px 14px rgba(108,71,255,0.4);
  }

  /* ── WORD GRID ── */
  .wgrid { display: grid; grid-template-columns: repeat(3,1fr); gap: 10px; margin-top: 4px; }
  .wbtn {
    padding: 16px 6px; border: 1.5px solid var(--border); border-radius: 16px;
    background: rgba(255,255,255,0.06); font-size: 20px; color: #fff;
    cursor: pointer; transition: all .2s; font-weight: 800;
    text-align: center; line-height: 1.4; font-family: inherit;
    position: relative; overflow: hidden;
  }
  .wbtn::before {
    content: ''; position: absolute; inset: 0;
    background: linear-gradient(135deg, var(--primary), #9b73ff);
    opacity: 0; transition: opacity .2s;
  }
  .wbtn:hover::before { opacity: 0.15; }
  .wbtn.active {
    background: linear-gradient(135deg, var(--primary), #9b73ff);
    border-color: transparent;
    box-shadow: 0 6px 20px rgba(108,71,255,0.5);
    transform: scale(1.02);
  }
  .wbtn span { position: relative; }
  .word-latin { display: block; font-size: 10px; font-weight: 600; opacity: .7; margin-top: 4px; }

  /* ── PARAGRAPH GRID ── */
  .pgrid { display: flex; flex-direction: column; gap: 10px; margin-top: 4px; }
  .pbtn {
    padding: 16px; border: 1.5px solid var(--border); border-radius: 16px;
    background: rgba(255,255,255,0.06); font-size: 14px; color: #fff;
    cursor: pointer; transition: all .2s; font-weight: 500;
    text-align: left; line-height: 1.7; font-family: inherit;
  }
  .pbtn:hover { border-color: rgba(255,255,255,0.3); background: rgba(255,255,255,0.1); }
  .pbtn.active {
    background: linear-gradient(135deg, rgba(108,71,255,0.3), rgba(155,115,255,0.2));
    border-color: var(--primary);
    box-shadow: 0 0 0 3px rgba(108,71,255,0.2);
  }
  .pbtn .pnum {
    font-size: 9px; font-weight: 800; letter-spacing: 1px;
    text-transform: uppercase; margin-bottom: 6px; display: block;
    color: var(--text-muted);
  }
  .pbtn.active .pnum { color: #c4b5fd; }

  .para-display {
    margin-top: 14px; padding: 18px;
    background: linear-gradient(135deg, rgba(108,71,255,0.2), rgba(155,115,255,0.1));
    border: 1.5px solid rgba(108,71,255,0.5);
    border-radius: 16px; font-size: 17px; font-weight: 700;
    color: #fff; line-height: 1.8; display: none;
    box-shadow: 0 0 30px rgba(108,71,255,0.15);
  }
  .para-display.show { display: block; }

  /* ── TTS BUTTON ── */
  .tts-btn {
    margin-top: 14px; width: 100%; padding: 13px;
    border: 1.5px solid rgba(0,212,170,0.4); border-radius: 14px;
    background: rgba(0,212,170,0.1); color: var(--accent2);
    font-family: inherit; font-size: 14px; font-weight: 700;
    cursor: pointer; display: flex; align-items: center;
    justify-content: center; gap: 8px; transition: all .2s;
  }
  .tts-btn:hover {
    background: rgba(0,212,170,0.2);
    box-shadow: 0 4px 16px rgba(0,212,170,0.25);
    transform: translateY(-1px);
  }

  /* ── SPEED ── */
  .speed-row { display: flex; align-items: center; gap: 12px; margin-top: 12px; }
  .speed-row label { font-size: 12px; color: var(--text-muted); font-weight: 600; white-space: nowrap; }
  .speed-row input[type=range] { flex: 1; accent-color: var(--primary); }
  .speed-val { font-size: 12px; font-weight: 800; color: var(--primary); min-width: 46px; text-align: right; }

  /* ── MIC ── */
  .mic-wrap { text-align: center; padding: 12px 0 8px; }
  .mic-btn {
    width: 90px; height: 90px; border-radius: 50%;
    border: none;
    background: linear-gradient(135deg, var(--primary), #9b73ff);
    color: #fff; font-size: 34px; cursor: pointer;
    display: inline-flex; align-items: center; justify-content: center;
    transition: all .25s;
    box-shadow: 0 8px 30px rgba(108,71,255,0.55), 0 0 0 0 rgba(108,71,255,0.3);
  }
  .mic-btn:hover { transform: scale(1.07); box-shadow: 0 12px 40px rgba(108,71,255,0.7); }
  .mic-btn.recording {
    background: linear-gradient(135deg, var(--danger), #ff6b6b);
    animation: rp 1.1s infinite;
    box-shadow: 0 8px 30px rgba(255,71,87,0.55);
  }
  @keyframes rp {
    0%,100% { box-shadow: 0 8px 30px rgba(255,71,87,0.55), 0 0 0 0 rgba(255,71,87,0.4); }
    50% { box-shadow: 0 8px 30px rgba(255,71,87,0.55), 0 0 0 20px rgba(255,71,87,0); }
  }
  .mic-label { margin-top: 12px; font-size: 13px; color: var(--text-muted); font-weight: 600; }
  .speak-hint {
    font-size: 14px; color: #c4b5fd; text-align: center;
    margin-top: 8px; font-weight: 700; min-height: 22px;
  }
  .countdown {
    font-size: 32px; font-weight: 900; color: var(--danger);
    text-align: center; margin-top: 10px; min-height: 40px;
    font-variant-numeric: tabular-nums;
  }

  /* ── TRANSCRIPT ── */
  .transcript-box {
    margin-top: 14px; padding: 14px 16px;
    background: rgba(255,255,255,0.06);
    border-radius: 14px; border: 1px solid var(--border);
    display: none;
  }
  .transcript-label {
    font-size: 10px; color: var(--text-muted);
    margin-bottom: 6px; font-weight: 700;
    text-transform: uppercase; letter-spacing: 1px;
  }
  .transcript-text { font-size: 17px; color: #fff; font-weight: 700; }

  /* ── ERROR BOX ── */
  .err-box {
    padding: 12px 16px;
    background: rgba(255,71,87,0.15); color: #ff8a95;
    border: 1px solid rgba(255,71,87,0.3);
    border-radius: 14px; font-size: 13px; margin-top: 12px;
    display: none; font-weight: 600;
  }
  .err-box.show { display: block; }

  /* ── SCORE CARD ── */
  .score-wrap { display: none; }
  .score-wrap.show { display: block; }

  .score-header {
    display: flex; justify-content: space-between; align-items: flex-start; margin-bottom: 14px;
  }
  .score-word {
    font-size: 15px; font-weight: 800; color: #fff;
    max-width: 60%;
  }
  .score-num {
    font-size: 44px; font-weight: 900; line-height: 1;
    background: linear-gradient(135deg, var(--primary), #9b73ff);
    -webkit-background-clip: text; -webkit-text-fill-color: transparent;
  }
  .score-num.zero {
    background: linear-gradient(135deg, var(--danger), #ff8a95);
    -webkit-background-clip: text; -webkit-text-fill-color: transparent;
  }
  .score-num.good {
    background: linear-gradient(135deg, var(--warn), #ffcc60);
    -webkit-background-clip: text; -webkit-text-fill-color: transparent;
  }
  .score-num.great {
    background: linear-gradient(135deg, var(--success), #00f5a0);
    -webkit-background-clip: text; -webkit-text-fill-color: transparent;
  }

  .pbar {
    height: 12px; background: rgba(255,255,255,0.1);
    border-radius: 99px; overflow: hidden; margin-bottom: 14px;
    box-shadow: inset 0 2px 4px rgba(0,0,0,0.2);
  }
  .pfill {
    height: 100%; border-radius: 99px;
    transition: width 1.1s cubic-bezier(.4,0,.2,1);
    background: linear-gradient(90deg, var(--primary), #9b73ff);
    box-shadow: 0 2px 8px rgba(108,71,255,0.5);
    position: relative; overflow: hidden;
  }
  .pfill::after {
    content: ''; position: absolute; top: 0; left: -100%; right: 0; bottom: 0;
    background: linear-gradient(90deg, transparent, rgba(255,255,255,0.3), transparent);
    animation: shimmer 2s infinite;
  }
  @keyframes shimmer { to { left: 200%; } }

  .fbbox {
    padding: 14px 18px; border-radius: 16px;
    font-size: 14px; font-weight: 700;
    display: flex; align-items: center; gap: 12px;
    border: 1px solid transparent;
  }
  .fb-perfect {
    background: linear-gradient(135deg, rgba(0,196,122,0.2), rgba(0,245,160,0.1));
    color: #00f5a0; border-color: rgba(0,196,122,0.4);
  }
  .fb-ok {
    background: rgba(0,196,122,0.12);
    color: #4ade80; border-color: rgba(0,196,122,0.25);
  }
  .fb-warn {
    background: rgba(255,176,32,0.12);
    color: #fbbf24; border-color: rgba(255,176,32,0.25);
  }
  .fb-err {
    background: rgba(255,71,87,0.12);
    color: #ff8a95; border-color: rgba(255,71,87,0.25);
  }
  .fb-wrong {
    background: linear-gradient(135deg, rgba(255,71,87,0.2), rgba(255,107,107,0.1));
    color: #ff6b6b; border-color: rgba(255,71,87,0.5);
  }

  .tips-box {
    margin-top: 12px; padding: 12px 16px;
    background: rgba(108,71,255,0.12); border-radius: 14px;
    border-left: 3px solid var(--primary);
    font-size: 13px; color: #c4b5fd; font-weight: 600; display: none;
  }
  .tips-box.show { display: block; }

  /* ── BUTTONS ── */
  .btn-row { display: flex; gap: 10px; margin-top: 14px; }
  .retry-btn {
    flex: 1; padding: 12px;
    border: 1.5px solid var(--border); border-radius: 14px;
    background: rgba(255,255,255,0.07); color: #fff;
    font-family: inherit; font-size: 14px; font-weight: 700;
    cursor: pointer; transition: all .2s;
  }
  .retry-btn:hover { background: rgba(255,255,255,0.12); transform: translateY(-1px); }
  .next-btn {
    flex: 1; padding: 12px; border: none; border-radius: 14px;
    background: linear-gradient(135deg, var(--primary), #9b73ff);
    color: #fff; font-family: inherit;
    font-size: 14px; font-weight: 700; cursor: pointer;
    transition: all .2s;
    box-shadow: 0 4px 16px rgba(108,71,255,0.4);
  }
  .next-btn:hover { transform: translateY(-2px); box-shadow: 0 8px 24px rgba(108,71,255,0.5); }

  /* ── CONFETTI ── */
  .confetti-wrap {
    text-align: center; padding: 10px 0 4px;
    font-size: 36px; display: none;
    animation: pop .4s cubic-bezier(.68,-.55,.27,1.55);
  }
  @keyframes pop { 0%{transform:scale(0.4) rotate(-10deg);opacity:0;} 100%{transform:scale(1) rotate(0);opacity:1;} }
  .confetti-wrap.show { display: block; }

  /* ── ZERO BANNER ── */
  .zero-banner {
    text-align: center; padding: 12px;
    background: linear-gradient(135deg, rgba(255,71,87,0.2), rgba(255,107,107,0.1));
    border: 1px solid rgba(255,71,87,0.4);
    border-radius: 14px; margin-bottom: 12px;
    font-size: 22px; display: none; font-weight: 800;
    color: #ff6b6b;
  }
  .zero-banner.show { display: block; }

  /* ── STATS ── */
  .stats-grid { display: grid; grid-template-columns: repeat(3,1fr); gap: 10px; margin-bottom: 16px; }
  .stat-card {
    background: rgba(255,255,255,0.06); border-radius: 16px;
    padding: 16px; text-align: center;
    border: 1px solid var(--border);
    transition: transform .2s;
  }
  .stat-card:hover { transform: translateY(-2px); }
  .stat-num {
    font-size: 26px; font-weight: 900;
    background: linear-gradient(135deg, var(--primary), #9b73ff);
    -webkit-background-clip: text; -webkit-text-fill-color: transparent;
  }
  .stat-label { font-size: 10px; color: var(--text-muted); font-weight: 700; margin-top: 4px; text-transform: uppercase; letter-spacing: .5px; }

  /* ── API STATUS ── */
  .api-status {
    display: flex; align-items: center; gap: 8px;
    font-size: 12px; font-weight: 700; margin-bottom: 14px;
    padding: 10px 14px; border-radius: 12px;
  }
  .api-ok  { background: rgba(0,196,122,0.12); color: #4ade80; border: 1px solid rgba(0,196,122,0.25); }
  .api-err { background: rgba(255,71,87,0.12); color: #ff8a95; border: 1px solid rgba(255,71,87,0.25); }
  .api-dot { width: 8px; height: 8px; border-radius: 50%; flex-shrink: 0; }
  .dot-ok  { background: var(--success); box-shadow: 0 0 8px var(--success); }
  .dot-err { background: var(--danger); box-shadow: 0 0 8px var(--danger); }

  /* ── CHART ── */
  .chart-bar-row { display: flex; align-items: center; gap: 10px; margin-bottom: 8px; }
  .chart-bar-label { font-size: 12px; color: var(--text-muted); font-weight: 600; width: 64px; text-align: right; overflow: hidden; text-overflow: ellipsis; white-space: nowrap; }
  .chart-bar-bg { flex: 1; background: rgba(255,255,255,0.08); border-radius: 99px; height: 10px; overflow: hidden; }
  .chart-bar-fill { height: 100%; border-radius: 99px; transition: width .7s; }
  .chart-bar-val { font-size: 12px; font-weight: 800; color: #c4b5fd; width: 38px; text-align: right; }

  /* ── HISTORY ── */
  .hist-item {
    display: flex; justify-content: space-between; align-items: center;
    padding: 12px 0; border-bottom: 1px solid rgba(255,255,255,0.06);
  }
  .hist-item:last-child { border-bottom: none; }
  .hist-word { font-size: 16px; font-weight: 800; color: #fff; }
  .hist-said { font-size: 12px; color: var(--text-muted); margin-top: 3px; }
  .hist-score { font-size: 16px; font-weight: 900; }
  .hs-ok  { color: var(--success); }
  .hs-warn{ color: var(--warn); }
  .hs-err { color: var(--danger); }
  .hist-time { font-size: 11px; color: var(--text-muted); text-align: right; margin-top: 3px; }

  /* ── CLEAR BTN ── */
  .clear-btn {
    margin-top: 14px; width: 100%; padding: 11px;
    border: 1.5px solid rgba(255,71,87,0.35); border-radius: 14px;
    background: rgba(255,71,87,0.08); color: #ff8a95;
    font-family: inherit; font-weight: 700; font-size: 13px; cursor: pointer;
    transition: all .2s;
  }
  .clear-btn:hover { background: rgba(255,71,87,0.16); }

  /* ── LEADERBOARD ── */
  .lb-row {
    display: flex; justify-content: space-between; align-items: center;
    padding: 12px 0; border-bottom: 1px solid rgba(255,255,255,0.06);
  }
  .lb-row:last-child { border-bottom: none; }
  .lb-rank { font-size: 20px; font-weight: 900; width: 36px; }
  .lb-user { font-size: 14px; font-weight: 700; color: #fff; }
  .lb-score { font-size: 16px; font-weight: 900; color: var(--success); }

  .no-hist { color: var(--text-muted); font-size: 14px; text-align: center; padding: 28px 0; }

  /* ── TOAST ── */
  .toast {
    position: fixed; bottom: 28px; left: 50%;
    transform: translateX(-50%) translateY(90px);
    background: linear-gradient(135deg, #1a1640, #302b63);
    color: #fff; padding: 12px 26px;
    border-radius: 99px; font-size: 14px; font-weight: 700;
    transition: transform .35s cubic-bezier(.68,-.55,.27,1.55);
    z-index: 999; pointer-events: none; white-space: nowrap;
    border: 1px solid var(--border);
    box-shadow: 0 8px 30px rgba(0,0,0,0.4);
  }
  .toast.show { transform: translateX(-50%) translateY(0); }

  /* ── SHAKE ── */
  @keyframes shake {
    0%   { transform: translateX(0); }
    12%  { transform: translateX(-9px) rotate(-1deg); }
    25%  { transform: translateX(9px) rotate(1deg); }
    37%  { transform: translateX(-7px) rotate(-.5deg); }
    50%  { transform: translateX(7px) rotate(.5deg); }
    62%  { transform: translateX(-4px); }
    75%  { transform: translateX(4px); }
    87%  { transform: translateX(-2px); }
    100% { transform: translateX(0); }
  }
  .shake { animation: shake 0.65s cubic-bezier(.36,.07,.19,.97) both; }

  /* ── ABOUT ── */
  .feat-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; margin-bottom: 16px; }
  .feat-item {
    background: rgba(255,255,255,0.06); border-radius: 14px;
    padding: 14px; border: 1px solid var(--border);
    display: flex; align-items: center; gap: 10px;
    font-size: 13px; font-weight: 600; color: #fff;
  }
  .feat-icon { font-size: 22px; flex-shrink: 0; }
  .about-stat-row { display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 10px; margin-bottom: 16px; }
  .about-stat {
    background: linear-gradient(135deg, rgba(108,71,255,0.25), rgba(155,115,255,0.1));
    border: 1px solid rgba(108,71,255,0.35); border-radius: 16px;
    padding: 16px; text-align: center;
  }
  .about-stat-num { font-size: 26px; font-weight: 900; color: #c4b5fd; }
  .about-stat-label { font-size: 11px; color: var(--text-muted); font-weight: 600; margin-top: 4px; }
  .about-warn {
    margin-top: 14px; padding: 14px 16px;
    background: rgba(255,176,32,0.1); border-radius: 14px;
    border: 1px solid rgba(255,176,32,0.3);
    font-size: 13px; color: #fbbf24; font-weight: 600;
  }
</style>
</head>
<body>

<div class="header">
  <div class="hlogo">🎤</div>
  <div>
    <div class="htitle">SWARA — AI Speech Therapy</div>
    <div class="hsub">Multilingual pronunciation practice</div>
  </div>
  <div class="hstreak" id="streakBadge">🔥 0 streak</div>
</div>

<div class="body">

  <div class="tab-row">
    <button class="tab active" onclick="switchTab('practice')">🎯 Practice</button>
    <button class="tab" onclick="switchTab('stats')">📊 Stats</button>
    <button class="tab" onclick="switchTab('history')">📋 History</button>
    <button class="tab" onclick="switchTab('leaderboard')">🏆 Board</button>
    <button class="tab" onclick="switchTab('about')">ℹ️ About</button>
  </div>

  <!-- ===== PRACTICE ===== -->
  <div id="tab-practice" class="panel show">

    <div class="card">
      <div class="clabel">👤 Your Name</div>
      <div class="user-row">
        <span class="user-label">Name:</span>
        <input class="user-input" id="userNameInput" type="text" placeholder="Enter your name..." value="Player1" oninput="updateUserId()">
      </div>
    </div>

    <div class="level-toggle">
      <button class="level-btn active" id="lvl1btn" onclick="setLevel(1)">
        🔤 Level 1 — Words
      </button>
      <button class="level-btn" id="lvl2btn" onclick="setLevel(2)">
        📄 Level 2 — Paragraphs <span class="level-badge">NEW</span>
      </button>
    </div>

    <div class="card">
      <div class="clabel">🌐 Language & Difficulty</div>
      <select id="lang" onchange="loadContent()">
        <option value="hindi">Hindi (हिंदी)</option>
        <option value="marathi">Marathi (मराठी)</option>
        <option value="english">English</option>
      </select>
      <div class="diff-row">
        <button class="diff-btn active" onclick="setDiff('easy',this)">🟢 Easy</button>
        <button class="diff-btn" onclick="setDiff('medium',this)">🟡 Medium</button>
        <button class="diff-btn" onclick="setDiff('hard',this)">🔴 Hard</button>
      </div>
    </div>

    <div class="card" id="wordCard">
      <div class="clabel">📝 Select Word to Practice</div>
      <div class="wgrid" id="wgrid"><div class="no-hist">Loading words...</div></div>
      <button class="tts-btn" onclick="speakCurrent()">🔊 Hear correct pronunciation</button>
      <div class="speed-row">
        <label>Speed:</label>
        <input type="range" min="40" max="120" value="82" step="1" id="speedSlider" oninput="updateSpeed(this.value)">
        <span class="speed-val" id="speedVal">Normal</span>
      </div>
    </div>

    <div class="card" id="paraCard" style="display:none;">
      <div class="clabel">📄 Select Paragraph to Practice</div>
      <div class="pgrid" id="pgrid"><div class="no-hist">Loading paragraphs...</div></div>
      <div class="para-display" id="paraDisplay"></div>
      <button class="tts-btn" onclick="speakCurrent()">🔊 Hear paragraph read aloud</button>
      <div class="speed-row">
        <label>Speed:</label>
        <input type="range" min="40" max="120" value="82" step="1" id="speedSlider2" oninput="updateSpeed(this.value)">
        <span class="speed-val" id="speedVal2">Normal</span>
      </div>
    </div>

    <div class="card">
      <div class="clabel">🎙️ Record Your Pronunciation</div>
      <div class="mic-wrap">
        <button class="mic-btn" id="micBtn" onclick="toggleMic()">🎤</button>
        <div class="mic-label" id="micLabel">Tap mic to start speaking</div>
      </div>
      <div class="countdown" id="countdown"></div>
      <div class="speak-hint" id="speakHint"></div>
      <div class="transcript-box" id="transcriptBox">
        <div class="transcript-label">You said:</div>
        <div class="transcript-text" id="transcriptText"></div>
      </div>
      <div class="err-box" id="errBox"></div>
    </div>

    <div class="card score-wrap" id="scoreCard">
      <div class="clabel">📊 Your Score</div>
      <div class="confetti-wrap" id="confetti">🎉🌟🎊✨🥳</div>
      <div class="zero-banner" id="zeroBanner">❌ 0% — No match at all!</div>
      <div class="score-header">
        <div class="score-word" id="scoreWordEl"></div>
        <div class="score-num" id="scoreNumEl"></div>
      </div>
      <div class="pbar"><div class="pfill" id="pfill" style="width:0%"></div></div>
      <div class="fbbox" id="fbbox"></div>
      <div class="tips-box" id="tipsBox"></div>
      <div class="btn-row">
        <button class="retry-btn" onclick="retryWord()">🔄 Try Again</button>
        <button class="next-btn" onclick="nextItem()">➡️ Next</button>
      </div>
    </div>

  </div>

  <!-- ===== STATS ===== -->
  <div id="tab-stats" class="panel">
    <div class="card">
      <div class="clabel">📊 Your Statistics</div>
      <div id="apiStatusStats" class="api-status api-ok">
        <div class="api-dot dot-ok"></div> Connected to Python backend
      </div>
      <div class="stats-grid">
        <div class="stat-card"><div class="stat-num" id="statTotal">0</div><div class="stat-label">Attempts</div></div>
        <div class="stat-card"><div class="stat-num" id="statAvg">—</div><div class="stat-label">Avg Score</div></div>
        <div class="stat-card"><div class="stat-num" id="statBest">—</div><div class="stat-label">Best Score</div></div>
        <div class="stat-card"><div class="stat-num" id="statMastered">0</div><div class="stat-label">Mastered</div></div>
        <div class="stat-card"><div class="stat-num" id="statStreak">0</div><div class="stat-label">Streak</div></div>
        <div class="stat-card"><div class="stat-num" id="statExcellent">0</div><div class="stat-label">Excellent</div></div>
      </div>
    </div>
    <div class="card">
      <div class="clabel">📈 Recent Scores Chart</div>
      <div id="chartWrap"><div class="no-hist">No data yet — start practicing!</div></div>
    </div>
  </div>

  <!-- ===== HISTORY ===== -->
  <div id="tab-history" class="panel">
    <div class="card">
      <div class="clabel">📋 Session History</div>
      <div id="histList"><div class="no-hist">No attempts yet — start practicing!</div></div>
      <button class="clear-btn" onclick="clearHist()">🗑️ Clear History</button>
    </div>
  </div>

  <!-- ===== LEADERBOARD ===== -->
  <div id="tab-leaderboard" class="panel">
    <div class="card">
      <div class="clabel">🏆 Top Players</div>
      <div id="lbList"><div class="no-hist">No scores yet — be the first!</div></div>
    </div>
  </div>

  <!-- ===== ABOUT ===== -->
  <div id="tab-about" class="panel">
    <div class="card">
      <div class="clabel">ℹ️ About SWARA</div>
      <p style="font-size:14px;color:rgba(255,255,255,0.7);line-height:1.8;margin-bottom:18px;">
        SWARA is an AI-powered multilingual speech therapy tool to help children
        practice pronunciation in Hindi, Marathi, and English using real-time voice recognition.
      </p>
      <div class="about-stat-row">
        <div class="about-stat"><div class="about-stat-num">3</div><div class="about-stat-label">Languages</div></div>
        <div class="about-stat"><div class="about-stat-num">2</div><div class="about-stat-label">Levels</div></div>
        <div class="about-stat"><div class="about-stat-num">AI</div><div class="about-stat-label">Scoring</div></div>
      </div>
      <div class="feat-grid">
        <div class="feat-item"><span class="feat-icon">🔤</span>Word Practice</div>
        <div class="feat-item"><span class="feat-icon">📄</span>Paragraph Mode</div>
        <div class="feat-item"><span class="feat-icon">🎙️</span>Live Recognition</div>
        <div class="feat-item"><span class="feat-icon">🔊</span>Text to Speech</div>
        <div class="feat-item"><span class="feat-icon">📊</span>Strict AI Scoring</div>
        <div class="feat-item"><span class="feat-icon">🏆</span>Leaderboard</div>
        <div class="feat-item"><span class="feat-icon">🔥</span>Streak Tracking</div>
        <div class="feat-item"><span class="feat-icon">🎮</span>3 Difficulty Levels</div>
      </div>
      <div class="about-warn">
        ⚠️ Works best in <strong>Google Chrome</strong> or <strong>Microsoft Edge</strong>.
        Allow microphone permission when prompted.
      </div>
    </div>
  </div>

</div>

<div class="toast" id="toast"></div>

<script>
const BACKEND_URL = window.location.origin;

let selLang      = 'hindi';
let selDiff      = 'easy';
let selWord      = null;
let selPara      = null;
let wordsList    = [];
let parasList    = [];
let recog        = null;
let isRecording  = false;
let streak       = 0;
let countdownTimer = null;
let speakRate    = 0.82;
let userId       = 'Player1';
let localHistory = [];
let currentLevel = 1;

const pronunciationTips = {
  hindi:   { default: 'Listen carefully first. Speak slowly and clearly. Focus on each syllable.' },
  marathi: { default: 'Listen to the word first. Then repeat it slowly syllable by syllable.' },
  english: { default: 'Speak clearly at normal pace. Make sure each syllable is distinct.' }
};

// ═══ AUDIO ════════════════════════════════════════════════════════
const AudioCtx = window.AudioContext || window.webkitAudioContext;
let audioCtx = null;
function getAudioCtx() { if (!audioCtx) audioCtx = new AudioCtx(); return audioCtx; }

function playErrorSound() {
  try {
    const ctx = getAudioCtx();
    [[0,380],[0.18,260]].forEach(([t,f]) => {
      const osc = ctx.createOscillator(), gain = ctx.createGain();
      osc.connect(gain); gain.connect(ctx.destination);
      osc.type = 'square'; osc.frequency.value = f;
      gain.gain.setValueAtTime(0.22, ctx.currentTime+t);
      gain.gain.exponentialRampToValueAtTime(0.001, ctx.currentTime+t+0.16);
      osc.start(ctx.currentTime+t); osc.stop(ctx.currentTime+t+0.18);
    });
  } catch(e) {}
}

function playSuccessSound() {
  try {
    const ctx = getAudioCtx();
    [523,659,784,1047].forEach((f,i) => {
      const osc = ctx.createOscillator(), gain = ctx.createGain();
      osc.connect(gain); gain.connect(ctx.destination);
      osc.type = 'sine'; osc.frequency.value = f;
      gain.gain.setValueAtTime(0.18, ctx.currentTime+i*0.1);
      gain.gain.exponentialRampToValueAtTime(0.001, ctx.currentTime+i*0.1+0.15);
      osc.start(ctx.currentTime+i*0.1); osc.stop(ctx.currentTime+i*0.1+0.2);
    });
  } catch(e) {}
}

// ═══ INIT ═════════════════════════════════════════════════════════
loadContent(); checkBackend();

function setLevel(n) {
  currentLevel = n;
  document.getElementById('lvl1btn').classList.toggle('active', n===1);
  document.getElementById('lvl2btn').classList.toggle('active', n===2);
  document.getElementById('wordCard').style.display = n===1?'':'none';
  document.getElementById('paraCard').style.display = n===2?'':'none';
  resetScoreUI(); loadContent();
}

function updateUserId() {
  userId = document.getElementById('userNameInput').value.trim() || 'Player1';
}

async function checkBackend() {
  try { const r = await fetch(BACKEND_URL+'/api/words?lang=hindi&diff=easy'); setApiStatus(r.ok); }
  catch(e) { setApiStatus(false); }
}

function setApiStatus(ok) {
  const el = document.getElementById('apiStatusStats'); if (!el) return;
  el.className = 'api-status '+(ok?'api-ok':'api-err');
  el.innerHTML = `<div class="api-dot ${ok?'dot-ok':'dot-err'}"></div> ${ok?'Connected to Python backend ✅':'Backend offline — using local mode ⚠️'}`;
}

function loadContent() {
  selLang = document.getElementById('lang').value;
  if (currentLevel===1) loadWords(); else loadParagraphs();
}

async function loadWords() {
  const grid = document.getElementById('wgrid');
  grid.innerHTML = '<div class="no-hist">Loading...</div>';
  try {
    const r = await fetch(`${BACKEND_URL}/api/words?lang=${selLang}&diff=${selDiff}`);
    const d = await r.json(); wordsList = d.words; renderWordGrid(wordsList);
  } catch(e) {
    const fb = {
      hindi:  {easy:['कमल','माला','रवि','आम','घर','पानी'],medium:['नमक','किताब','बादल','सपना','रंग','पहाड़'],hard:['विद्यालय','परिवार','आकाश','स्वास्थ्य','प्रकृति','उपहार']},
      marathi:{easy:['माती','नदी','फूल','पाणी','घर','आई'],medium:['आकाश','शाळा','पुस्तक','वारा','दगड','झाड'],hard:['विद्यार्थी','स्वयंपाक','समुद्र','महाराष्ट्र','परीक्षा','प्रयत्न']},
      english:{easy:['Lotus','Apple','River','Water','Mango','House'],medium:['Butterfly','Rainbow','Mountain','Elephant','Umbrella','Calendar'],hard:['Pronunciation','Comfortable','Particularly','Extraordinary','Vocabulary','Approximately']}
    };
    wordsList = fb[selLang][selDiff]; renderWordGrid(wordsList);
    showToast('⚠️ Offline mode');
  }
  resetScoreUI();
}

async function loadParagraphs() {
  const grid = document.getElementById('pgrid');
  grid.innerHTML = '<div class="no-hist">Loading...</div>';
  try {
    const r = await fetch(`${BACKEND_URL}/api/paragraphs?lang=${selLang}&diff=${selDiff}`);
    const d = await r.json(); parasList = d.paragraphs; renderParaGrid(parasList);
  } catch(e) {
    const fb = {
      hindi:  {easy:['मेरा नाम राम है। मैं स्कूल जाता हूँ।','यह एक सुंदर फूल है। इसका रंग लाल है।','आज मौसम अच्छा है। हम बाहर खेलेंगे।'],medium:['भारत एक महान देश है।','बच्चे रोज़ पढ़ते हैं।','पहाड़ों पर बर्फ पड़ती है।'],hard:['विद्यालय में शिक्षा का महत्व है।','प्रकृति की रक्षा करना जरूरी है।','स्वास्थ्य ही सबसे बड़ा धन है।']},
      marathi:{easy:['माझे नाव राम आहे.','हे एक सुंदर फूल आहे.','आज हवामान छान आहे.'],medium:['महाराष्ट्र सुंदर राज्य आहे.','मुले रोज अभ्यास करतात.','पर्वतांवर बर्फ पडतो.'],hard:['विद्यार्थ्यांनी शिक्षणाला महत्त्व द्यावे.','महाराष्ट्राची संस्कृती समृद्ध आहे.','निसर्गाचे संरक्षण करणे कर्तव्य आहे.']},
      english:{easy:['My name is Ram. I go to school every day.','This is a beautiful flower. Its colour is red.','Today the weather is nice. We will play outside.'],medium:['India is a great country.','Children study every day.','Snow falls on the mountains.'],hard:['Education is extremely important.','Environmental conservation is our responsibility.','Extraordinary achievements come from dedication.']}
    };
    parasList = (fb[selLang]||fb.english)[selDiff]; renderParaGrid(parasList);
    showToast('⚠️ Offline mode');
  }
  resetScoreUI();
}

function renderWordGrid(words) {
  const grid = document.getElementById('wgrid'); grid.innerHTML = '';
  words.forEach((w,i) => {
    const btn = document.createElement('button');
    btn.className = 'wbtn'+(i===0?' active':'');
    const roman = getRoman(w);
    btn.innerHTML = `<span>${w}${roman?`<span class="word-latin">${roman}</span>`:''}</span>`;
    btn.onclick = () => {
      document.querySelectorAll('.wbtn').forEach(b=>b.classList.remove('active'));
      btn.classList.add('active'); selWord = w; resetScoreUI();
    };
    grid.appendChild(btn);
    if (i===0) selWord = w;
  });
}

function renderParaGrid(paras) {
  const grid = document.getElementById('pgrid'); grid.innerHTML = '';
  paras.forEach((p,i) => {
    const btn = document.createElement('button');
    btn.className = 'pbtn'+(i===0?' active':'');
    btn.innerHTML = `<span class="pnum">Paragraph ${i+1}</span>${p}`;
    btn.onclick = () => {
      document.querySelectorAll('.pbtn').forEach(b=>b.classList.remove('active'));
      btn.classList.add('active'); selPara = p;
      const pd = document.getElementById('paraDisplay');
      pd.textContent = p; pd.classList.add('show'); resetScoreUI();
    };
    grid.appendChild(btn);
    if (i===0) {
      selPara = p;
      const pd = document.getElementById('paraDisplay');
      pd.textContent = p; pd.classList.add('show');
    }
  });
}

const romanMap = {
  'कमल':'Kamal','माला':'Mala','रवि':'Ravi','आम':'Aam','घर':'Ghar','पानी':'Paani',
  'नमक':'Namak','किताब':'Kitaab','बादल':'Baadal','सपना':'Sapna','रंग':'Rang','पहाड़':'Pahad',
  'विद्यालय':'Vidyalay','परिवार':'Parivaar','आकाश':'Aakash','स्वास्थ्य':'Swasthya','प्रकृति':'Prakriti','उपहार':'Uphaar',
  'माती':'Maati','नदी':'Nadi','फूल':'Phool','पाणी':'Paani','आई':'Aai',
  'शाळा':'Shaala','पुस्तक':'Pustak','वारा':'Vaara','दगड':'Dagad','झाड':'Zhaad',
  'विद्यार्थी':'Vidyarthi','स्वयंपाक':'Swayampak','समुद्र':'Samudra','महाराष्ट्र':'Maharashtra','परीक्षा':'Pariksha','प्रयत्न':'Prayatna'
};
function getRoman(w) { return romanMap[w]||''; }

function setDiff(d,el) {
  selDiff = d;
  document.querySelectorAll('.diff-btn').forEach(b=>b.classList.remove('active'));
  el.classList.add('active'); loadContent();
}

const langCode = { hindi:'hi-IN', marathi:'mr-IN', english:'en-US' };

function speakCurrent() {
  const text = currentLevel===1?selWord:selPara; if (!text) return;
  const utt = new SpeechSynthesisUtterance(text);
  utt.lang = langCode[selLang]; utt.rate = speakRate;
  speechSynthesis.cancel(); speechSynthesis.speak(utt);
  showToast('🔊 Playing pronunciation...');
}

function updateSpeed(val) {
  speakRate = val/100;
  const label = val<60?'Slow':val<95?'Normal':'Fast';
  document.getElementById('speedVal').textContent = label;
  const sv2 = document.getElementById('speedVal2'); if(sv2) sv2.textContent = label;
}

function toggleMic() { if (isRecording) { stopRec(); return; } startRec(); }

function startRec() {
  const target = currentLevel===1?selWord:selPara;
  if (!target) { showErr('Please select a word or paragraph first.'); return; }
  const SR = window.SpeechRecognition||window.webkitSpeechRecognition;
  if (!SR) { showErr('Speech recognition not supported. Use Google Chrome or Microsoft Edge.'); return; }
  recog = new SR();
  recog.lang = langCode[selLang]; recog.interimResults = false; recog.maxAlternatives = 5;
  const timeout = currentLevel===2?20:8;

  recog.onstart = () => {
    isRecording = true;
    document.getElementById('micBtn').classList.add('recording');
    document.getElementById('micBtn').textContent = '⏹';
    document.getElementById('micLabel').textContent = 'Listening... speak now!';
    document.getElementById('speakHint').textContent = currentLevel===1
      ? 'Say: '+selWord+(getRoman(selWord)?' ('+getRoman(selWord)+')':'')
      : 'Read the paragraph clearly...';
    document.getElementById('errBox').classList.remove('show');
    document.getElementById('transcriptBox').style.display = 'none';
    startCountdown(timeout);
  };
  recog.onresult = (e) => {
    clearCountdown(); let best='';
    const results = e.results[0];
    for (let i=0;i<results.length;i++) { const t=results[i].transcript.trim(); if(t){best=t;break;} }
    handleResult(best);
  };
  recog.onerror = (e) => {
    clearCountdown(); resetMic();
    if (e.error==='not-allowed') showErr('Microphone access denied. Allow microphone permission.');
    else if (e.error==='no-speech') showErr('No speech detected. Please speak clearly and try again.');
    else showErr('Error: '+e.error+'. Please try again.');
  };
  recog.onend = () => { clearCountdown(); if(isRecording) resetMic(); };
  try { recog.start(); } catch(e) { showErr('Could not start mic: '+e.message); resetMic(); }
}

function stopRec() { if(recog) recog.stop(); clearCountdown(); resetMic(); }

function resetMic() {
  isRecording = false;
  document.getElementById('micBtn').classList.remove('recording');
  document.getElementById('micBtn').textContent = '🎤';
  document.getElementById('micLabel').textContent = 'Tap mic to start speaking';
  document.getElementById('speakHint').textContent = '';
  document.getElementById('countdown').textContent = '';
}

function startCountdown(secs) {
  let t = secs;
  document.getElementById('countdown').textContent = t+'s';
  countdownTimer = setInterval(() => {
    t--;
    if (t<=0) { clearCountdown(); if(recog) recog.stop(); }
    else document.getElementById('countdown').textContent = t+'s';
  }, 1000);
}
function clearCountdown() { clearInterval(countdownTimer); document.getElementById('countdown').textContent=''; }

async function handleResult(transcript) {
  resetMic();
  document.getElementById('transcriptBox').style.display = 'block';
  document.getElementById('transcriptText').textContent = transcript||(currentLevel===2?'(nothing heard)':'(nothing heard — try again)');
  const target = currentLevel===1?selWord:selPara;
  const mode   = currentLevel===1?'word':'paragraph';
  try {
    const r = await fetch(BACKEND_URL+'/api/score', {
      method:'POST', headers:{'Content-Type':'application/json'},
      body: JSON.stringify({ target, spoken:transcript, lang:selLang, diff:selDiff, mode, user_id:userId })
    });
    const data = await r.json();
    updateStreak(data.score); showScore(data.score, data.feedback, transcript, target);
    localHistory.unshift({word:target,score:data.score,said:transcript,time:new Date().toLocaleTimeString(),lang:selLang,diff:selDiff,mode});
    if (localHistory.length>50) localHistory.pop();
  } catch(e) {
    const score = localCalcScore(target, transcript, mode);
    const feedback = localGetFeedback(score);
    updateStreak(score); showScore(score, feedback, transcript, target);
    localHistory.unshift({word:target,score,said:transcript,time:new Date().toLocaleTimeString(),lang:selLang,diff:selDiff,mode});
  }
}

function localCalcScore(target, said, mode) {
  if (!said||!said.trim()) return 0;
  const t=target.toLowerCase().trim(), s=said.toLowerCase().trim();
  if (t===s) return 100;
  if (mode==='paragraph') {
    const tW=t.split(/\s+/), sW=s.split(/\s+/);
    const matched=sW.filter(w=>tW.includes(w)).length;
    const ratio=matched/tW.length;
    return ratio<0.1?0:Math.min(100,Math.round(ratio*100));
  }
  const roman=getRoman(target).toLowerCase();
  if (roman&&s===roman) return 88;
  let matches=0; for(const c of s){if(t.includes(c))matches++;}
  const ratio=matches/Math.max(t.length,s.length);
  return ratio<0.15?0:Math.min(100,Math.round(ratio*100));
}
function localGetFeedback(score) {
  if (score===100) return {level:'perfect',  message:'💯 Perfect! Flawless pronunciation!'};
  if (score>=85)   return {level:'excellent',message:'🌟 Excellent! Fantastic job!'};
  if (score>=65)   return {level:'good',     message:'⚠️ Good try! Keep practicing.'};
  if (score>0)     return {level:'poor',     message:'❌ Needs more practice. Try again!'};
  return                  {level:'wrong',    message:'🚫 No match at all. Please try again!'};
}

function showScore(score, feedback, transcript, target) {
  const displayLabel = currentLevel===1
    ? (target+(getRoman(target)?' ('+getRoman(target)+')':''))
    : 'Paragraph';
  document.getElementById('scoreWordEl').textContent = displayLabel;
  const numEl = document.getElementById('scoreNumEl');
  numEl.textContent = score+'%';
  numEl.className = 'score-num'+(score===0?' zero':score<65?' zero':score<85?' good':' great');

  setTimeout(()=>{ document.getElementById('pfill').style.width=score+'%'; }, 50);

  const fill = document.getElementById('pfill');
  if (score>=85) {
    fill.style.background='linear-gradient(90deg,#00c47a,#00f5a0)';
    fill.style.boxShadow='0 2px 10px rgba(0,196,122,0.6)';
  } else if (score>=65) {
    fill.style.background='linear-gradient(90deg,#ffb020,#ffcc60)';
    fill.style.boxShadow='0 2px 10px rgba(255,176,32,0.5)';
  } else {
    fill.style.background='linear-gradient(90deg,#ff4757,#ff8a95)';
    fill.style.boxShadow='0 2px 10px rgba(255,71,87,0.5)';
  }

  const fb=document.getElementById('fbbox'), tips=document.getElementById('tipsBox');
  const conf=document.getElementById('confetti'), zero=document.getElementById('zeroBanner');
  const card=document.getElementById('scoreCard');
  zero.classList.remove('show'); conf.classList.remove('show');

  if (feedback.level==='perfect'||feedback.level==='excellent') {
    fb.className='fbbox '+(feedback.level==='perfect'?'fb-perfect':'fb-ok');
    fb.textContent=feedback.message;
    conf.classList.add('show');
    setTimeout(()=>conf.classList.remove('show'),2500);
    tips.classList.remove('show');
    playSuccessSound();
    showToast('🔥 '+(score===100?'Perfect! ':'Excellent! ')+'Streak +1');
  } else if (feedback.level==='good') {
    fb.className='fbbox fb-warn'; fb.textContent=feedback.message;
    tips.classList.add('show');
    tips.textContent='💡 Tip: '+pronunciationTips[selLang].default;
    triggerShake(card); playErrorSound();
  } else {
    fb.className='fbbox '+(score===0?'fb-wrong':'fb-err');
    fb.textContent=feedback.message;
    tips.classList.add('show');
    tips.textContent='💡 Tip: '+pronunciationTips[selLang].default;
    if (score===0) zero.classList.add('show');
    triggerShake(card); playErrorSound();
    showToast('❌ Wrong — listen and try again!');
  }
  card.classList.add('show');
}

function triggerShake(el) {
  el.classList.remove('shake'); void el.offsetWidth;
  el.classList.add('shake');
  el.addEventListener('animationend',()=>el.classList.remove('shake'),{once:true});
}

function updateStreak(score) {
  streak = score>=85?streak+1:0;
  document.getElementById('streakBadge').textContent='🔥 '+streak+' streak';
}

function retryWord() { resetScoreUI(); showToast('🔄 Try again! Listen first then speak.'); }
function nextItem() {
  if (currentLevel===1) {
    const btns=document.querySelectorAll('.wbtn'); let cur=0;
    btns.forEach((b,i)=>{if(b.classList.contains('active'))cur=i;});
    btns[(cur+1)%btns.length].click();
  } else {
    const btns=document.querySelectorAll('.pbtn'); let cur=0;
    btns.forEach((b,i)=>{if(b.classList.contains('active'))cur=i;});
    btns[(cur+1)%btns.length].click();
  }
  showToast('➡️ Next!');
}
function resetScoreUI() {
  document.getElementById('scoreCard').classList.remove('show');
  document.getElementById('transcriptBox').style.display='none';
  document.getElementById('errBox').classList.remove('show');
  document.getElementById('speakHint').textContent='';
  document.getElementById('confetti').classList.remove('show');
  document.getElementById('zeroBanner').classList.remove('show');
  document.getElementById('pfill').style.width='0%';
}

async function renderStats() {
  try {
    const r=await fetch(`${BACKEND_URL}/api/stats?user_id=${encodeURIComponent(userId)}`);
    const d=await r.json();
    document.getElementById('statTotal').textContent    = d.total;
    document.getElementById('statAvg').textContent      = d.avg?d.avg+'%':'—';
    document.getElementById('statBest').textContent     = d.best?d.best+'%':'—';
    document.getElementById('statMastered').textContent = d.mastered;
    document.getElementById('statExcellent').textContent= d.excellent;
    document.getElementById('statStreak').textContent   = streak;
    setApiStatus(true);
  } catch(e) {
    const total=localHistory.length;
    const avg=total?Math.round(localHistory.reduce((s,h)=>s+h.score,0)/total):0;
    const best=total?Math.max(...localHistory.map(h=>h.score)):0;
    document.getElementById('statTotal').textContent    = total;
    document.getElementById('statAvg').textContent      = avg?avg+'%':'—';
    document.getElementById('statBest').textContent     = best?best+'%':'—';
    document.getElementById('statMastered').textContent = localHistory.filter(h=>h.score>=85).length;
    document.getElementById('statExcellent').textContent= localHistory.filter(h=>h.score>=85).length;
    document.getElementById('statStreak').textContent   = streak;
    setApiStatus(false);
  }
  const chart=document.getElementById('chartWrap');
  if (!localHistory.length) { chart.innerHTML='<div class="no-hist">No data yet — start practicing!</div>'; return; }
  chart.innerHTML = localHistory.slice(0,10).reverse().map(h=>{
    const label=h.mode==='paragraph'?'¶ Para':(h.word.length>8?h.word.slice(0,8)+'…':h.word);
    const col=h.score>=85?'#00c47a':h.score>=65?'#ffb020':'#ff4757';
    return `<div class="chart-bar-row">
      <div class="chart-bar-label" title="${h.word}">${label}</div>
      <div class="chart-bar-bg"><div class="chart-bar-fill" style="width:${h.score}%;background:${col};"></div></div>
      <div class="chart-bar-val">${h.score}%</div>
    </div>`;
  }).join('');
}

async function renderHist() {
  const el=document.getElementById('histList'); let hist=[];
  try {
    const r=await fetch(`${BACKEND_URL}/api/history?user_id=${encodeURIComponent(userId)}`);
    const d=await r.json(); hist=d.history||[];
  } catch(e) { hist=localHistory; }
  if (!hist.length) { el.innerHTML='<div class="no-hist">No attempts yet — start practicing!</div>'; return; }
  el.innerHTML=hist.slice(0,30).map(h=>{
    const score=h.score, cls=score>=85?'hs-ok':score>=65?'hs-warn':'hs-err';
    const said=h.spoken||h.said||'—', time=h.timestamp?new Date(h.timestamp).toLocaleTimeString():(h.time||'');
    const target=h.target||h.word||'—';
    const modeTag=h.mode==='paragraph'?' <span style="font-size:9px;background:rgba(108,71,255,0.3);color:#c4b5fd;border-radius:5px;padding:1px 5px;font-weight:800;">¶ PARA</span>':'';
    const dt=target.length>30?target.slice(0,30)+'…':target;
    return `<div class="hist-item">
      <div>
        <div class="hist-word">${dt}${modeTag} <span style="font-size:11px;font-weight:500;color:rgba(255,255,255,0.35);">${h.lang||''}</span></div>
        <div class="hist-said">Said: "${said.length>40?said.slice(0,40)+'…':said}"</div>
      </div>
      <div style="text-align:right">
        <div class="hist-score ${cls}">${score}%</div>
        <div class="hist-time">${time}</div>
      </div>
    </div>`;
  }).join('');
}

async function clearHist() {
  try {
    await fetch(BACKEND_URL+'/api/reset',{method:'POST',headers:{'Content-Type':'application/json'},body:JSON.stringify({user_id:userId})});
  } catch(e) {}
  localHistory=[]; streak=0;
  document.getElementById('streakBadge').textContent='🔥 0 streak';
  renderHist(); showToast('🗑️ History cleared');
}

async function renderLeaderboard() {
  const el=document.getElementById('lbList');
  try {
    const r=await fetch(BACKEND_URL+'/api/leaderboard'), d=await r.json(), lb=d.leaderboard||[];
    if (!lb.length) { el.innerHTML='<div class="no-hist">No scores yet — be the first!</div>'; return; }
    const medals=['🥇','🥈','🥉'];
    el.innerHTML=lb.map((p,i)=>`<div class="lb-row">
      <div class="lb-rank">${medals[i]||'#'+(i+1)}</div>
      <div><div class="lb-user">${p.user_id}</div><div style="font-size:11px;color:rgba(255,255,255,0.4);">Best: ${(p.best_word||'').slice(0,28)}</div></div>
      <div class="lb-score">${p.best_score}%</div>
    </div>`).join('');
  } catch(e) { el.innerHTML='<div class="no-hist">Could not load leaderboard — backend offline.</div>'; }
}

function switchTab(t) {
  ['practice','stats','history','leaderboard','about'].forEach(x=>{
    document.getElementById('tab-'+x).classList.toggle('show',x===t);
  });
  document.querySelectorAll('.tab-row .tab').forEach((btn,i)=>{
    btn.classList.toggle('active',['practice','stats','history','leaderboard','about'][i]===t);
  });
  if (t==='stats')       renderStats();
  if (t==='history')     renderHist();
  if (t==='leaderboard') renderLeaderboard();
}

function showErr(msg) {
  const b=document.getElementById('errBox'); b.textContent=msg; b.classList.add('show');
}
function showToast(msg) {
  const t=document.getElementById('toast'); t.textContent=msg; t.classList.add('show');
  setTimeout(()=>t.classList.remove('show'),2200);
}
</script>
<script src="https://replit-cdn.com/replit-pill/replit-pill.global.js" data-repl-id="4447975d-2f0b-4175-aff7-2150e65340fe"></script></body>
</html>
