<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, viewport-fit=cover, interactive-widget=resizes-content">
<title>한글 쓰기 — Korean Spelling Practice</title>
<link href="https://fonts.googleapis.com/css2?family=Gowun+Dodum&family=Quicksand:wght@400;600;700&family=Nanum+Myeongjo:wght@400;700&display=swap" rel="stylesheet">
<style>
  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

  :root {
    --ink:        #1c1a2e;
    --paper:      #f5f0e8;
    --ruled:      #ddd8cc;
    --red-stamp:  #c0392b;
    --red-light:  #f5e8e7;
    --blue-ink:   #2c4a8a;
    --blue-light: #e8edf8;
    --correct:    #2d7a4f;
    --correct-bg: #e8f5ee;
    --locked:     #1a5c38;
    --locked-bg:  #d4eddf;
    --gold:       #b8860b;
    --card-bg:    #ffffff;
    --muted:      #888;
    --muted-2:    #bbb;
    --red-stamp-dim: rgba(192,57,43,0.2);
  }

  [data-theme="dark"] {
    --ink:        #e8e3f0;
    --paper:      #14121f;
    --ruled:      #2c2940;
    --red-stamp:  #e0697a;
    --red-light:  #3a2228;
    --blue-ink:   #7b9ce8;
    --blue-light: #232c47;
    --correct:    #5fd394;
    --correct-bg: #1a3328;
    --locked:     #5fd394;
    --locked-bg:  #16291f;
    --gold:       #e0c068;
    --card-bg:    #1c1930;
    --muted:      #8a84a0;
    --muted-2:    #5c5670;
    --red-stamp-dim: rgba(224,105,122,0.25);
  }

  body { transition: background 0.2s ease, color 0.2s ease; }

  body {
    background: var(--paper);
    color: var(--ink);
    font-family: 'Quicksand', sans-serif;
    min-height: 100vh;
    min-height: 100dvh;
    display: flex;
    flex-direction: column;
    align-items: center;
    overscroll-behavior: none;
  }

  .app {
    position: relative;
    width: 100%;
    max-width: 580px;
    padding: 0 20px 40px;
  }

  /* ── HEADER ── */
  header { padding: 28px 0 16px; display: flex; align-items: baseline; gap: 12px; }
  .title-kr { font-family: 'Nanum Myeongjo', serif; font-size: 26px; font-weight: 700; color: var(--ink); letter-spacing: 2px; }
  .title-en { font-size: 12px; color: var(--muted); letter-spacing: 1px; text-transform: uppercase; }
  .score-badge { font-size: 13px; font-weight: 700; color: var(--gold); letter-spacing: 1px; }
  .theme-toggle {
    margin-left: auto;
    background: none; border: 1.5px solid var(--ruled); border-radius: 50%;
    width: 32px; height: 32px; font-size: 15px; cursor: pointer;
    display: flex; align-items: center; justify-content: center;
    transition: border-color 0.15s, background 0.15s;
    flex-shrink: 0;
  }
  .theme-toggle:hover { border-color: var(--blue-ink); background: var(--blue-light); }

  /* ── MODE TABS ── */
  .mode-tabs { display: flex; margin-bottom: 16px; border-bottom: 2px solid var(--ruled); }
  .mode-tab {
    padding: 8px 20px;
    font-family: 'Quicksand', sans-serif; font-size: 13px; font-weight: 700;
    letter-spacing: 1px; text-transform: uppercase;
    background: none; border: none; border-bottom: 3px solid transparent; margin-bottom: -2px;
    cursor: pointer; color: var(--muted-2); transition: color 0.2s, border-color 0.2s;
  }
  .mode-tab.active { color: var(--blue-ink); border-bottom-color: var(--blue-ink); }
  .due-badge {
    display: inline-flex;
    align-items: center;
    justify-content: center;
    min-width: 16px;
    height: 16px;
    padding: 0 4px;
    margin-left: 4px;
    border-radius: 8px;
    background: var(--red-stamp);
    color: #fff;
    font-size: 10px;
    font-weight: 700;
    line-height: 1;
    vertical-align: 2px;
  }

  /* ── TWO-COLUMN LAYOUT ── */
  .content-layout {
    display: flex; align-items: flex-start; gap: 0;
  }

  /* Left sidebar */
  .sidebar {
    flex-shrink: 0;
  }
  .selector-label {
    font-size: 8px; font-weight: 700; letter-spacing: 1px;
    text-transform: uppercase; color: var(--muted-2);
    text-align: center; margin-bottom: 6px;
    width: 44px;
  }

  /* Each chapter row: tab + optional chips side by side */
  .chapter-row {
    display: flex; flex-direction: row; align-items: center;
    gap: 5px; margin-bottom: 4px;
  }
  .chapter-tab-strip {
    display: flex; flex-direction: column; gap: 0;
  }
  .chapter-tab {
    padding: 6px 2px; border-radius: 6px;
    border: 1.5px solid var(--ruled); background: none;
    font-family: 'Quicksand', sans-serif; font-size: 10px; font-weight: 700;
    color: var(--muted); cursor: pointer; letter-spacing: 0.2px;
    text-align: center; line-height: 1.2; white-space: nowrap;
    transition: all 0.15s; width: 44px; flex-shrink: 0;
  }
  .chapter-tab.active {
    border-color: var(--blue-ink); background: var(--blue-light);
    color: var(--blue-ink);
  }
  .chapter-tab .tab-dot {
    display: block; width: 5px; height: 5px; border-radius: 50%;
    background: var(--blue-ink); margin: 3px auto 0;
  }

  /* Chips appear inline to the right of the active chapter tab */
  .chapter-chip-picker {
    display: flex; flex-direction: column; gap: 4px;
    animation: chipSlideIn 0.15s ease;
  }
  @keyframes chipSlideIn {
    from { opacity: 0; transform: translateX(-4px); }
    to   { opacity: 1; transform: translateX(0); }
  }
  .chapter-chip-picker .list-chip {
    display: block; width: 100%; text-align: center;
    padding: 5px 8px; font-size: 11px;
  }

  /* Right column — persistent selected chips bar + main content */
  .content-col {
    flex: 1; min-width: 0; padding-left: 8px;
  }

  /* Persistent selected chips bar */
  .selected-chips-bar {
    display: flex; gap: 5px; overflow-x: auto;
    padding: 0 0 8px 0; scrollbar-width: none;
    flex-wrap: nowrap;
  }
  .selected-chips-bar::-webkit-scrollbar { display: none; }
  .selected-chip {
    flex-shrink: 0; padding: 3px 8px; border-radius: 10px;
    background: var(--blue-ink); color: #fff;
    font-family: 'Quicksand', sans-serif; font-size: 10px; font-weight: 700;
    letter-spacing: 0.3px; white-space: nowrap;
  }
  .clear-lists-btn {
    flex-shrink: 0; padding: 2px 6px; border-radius: 10px;
    border: 1.5px solid var(--muted-2); background: none;
    font-family: 'Quicksand', sans-serif; font-size: 10px; font-weight: 700;
    color: var(--muted); cursor: pointer; line-height: 1;
    transition: border-color 0.15s, color 0.15s;
  }
  .clear-lists-btn:hover { border-color: var(--red-stamp); color: var(--red-stamp); }

  /* List chips inside picker */
  .list-chip {
    padding: 5px 12px; border-radius: 16px;
    border: 1.5px solid var(--blue-ink); background: var(--blue-light);
    font-family: 'Quicksand', sans-serif; font-size: 12px; font-weight: 700;
    color: var(--blue-ink); cursor: pointer; letter-spacing: 0.5px;
    transition: opacity 0.15s; user-select: none;
  }
  .list-chip.selected { opacity: 1; }
  .list-chip:not(.selected) { opacity: 0.3; }
  .list-chip:hover:not(.selected) { opacity: 0.5; }

  /* ── CATEGORY LABEL ── */
  .category-label {
    font-size: 11px; font-weight: 700; letter-spacing: 2px; text-transform: uppercase;
    color: var(--red-stamp); margin-bottom: 10px; display: flex; align-items: center; gap: 8px;
  }
  .category-label::after { content: ''; flex: 1; height: 1px; background: var(--red-stamp-dim, rgba(192,57,43,0.2)); }

  /* ── PROMPT CARD ── */
  .prompt-card {
    background: var(--card-bg); border: 1px solid var(--ruled); border-radius: 4px;
    padding: 22px 24px; margin-bottom: 24px; box-shadow: 2px 3px 0 var(--ruled); position: relative;
  }
  .prompt-card::before {
    content: ''; position: absolute; top: 0; bottom: 0; left: 0; width: 4px;
    background: var(--blue-ink); border-radius: 4px 0 0 4px;
  }
  .meaning-text { font-size: 22px; font-weight: 700; color: var(--ink); line-height: 1.3; }

  /* ── BLOCKS ── */
  .blocks-area { position: relative; margin-bottom: 20px; cursor: text; }
  .hidden-input { position: absolute; opacity: 0; width: 1px; height: 1px; top: 0; left: 0; pointer-events: none; font-size: 16px; }

  /* blocks-row uses flex with wrap; space and punct tokens sit inline.
     row-gap is larger than column-gap to leave room for the absolutely
     positioned per-syllable score label (used in Pronounce mode) sitting
     just below each block — otherwise a wrapped second row of blocks
     overlaps and hides that label. */
  .blocks-row {
    display: flex; flex-wrap: wrap;
    column-gap: 6px; row-gap: 26px;
    align-items: center; min-height: 68px;
  }

  /* A space between words */
  .block-space { width: 14px; flex-shrink: 0; }

  /* Punctuation shown between/after blocks */
  .block-punct {
    font-family: 'Gowun Dodum', sans-serif;
    font-size: 26px; color: var(--muted-2);
    line-height: 58px; flex-shrink: 0;
    user-select: none; margin: 0 1px;
  }

  .syl-block {
    width: 58px; height: 58px; flex-shrink: 0;
    border: 2px solid var(--ruled); border-radius: 4px; background: var(--card-bg);
    display: flex; align-items: center; justify-content: center;
    font-family: 'Gowun Dodum', sans-serif; font-size: 28px; color: var(--blue-ink);
    box-shadow: 2px 2px 0 var(--ruled);
    transition: border-color 0.12s, background 0.12s, color 0.12s;
    user-select: none;
  }
  .syl-block.empty   { border-color: var(--ruled); background: var(--card-bg); color: transparent; }
  .syl-block.active-block { border-color: var(--blue-ink); background: var(--blue-light); box-shadow: 2px 2px 0 var(--blue-ink); }
  .syl-block.filled  { border-color: var(--blue-ink); background: var(--blue-light); }
  .syl-block.locked  { border-color: var(--locked); background: var(--locked-bg); color: var(--locked); position: relative; }
  .syl-block.locked::after { content: '✓'; position: absolute; bottom: 2px; right: 4px; font-size: 9px; color: var(--locked); opacity: 0.7; font-family: sans-serif; }
  .syl-block.wrong   { border-color: var(--red-stamp); background: var(--red-light); color: var(--red-stamp); animation: shake 0.45s ease forwards; }
  .syl-block.correct { border-color: var(--correct); background: var(--correct-bg); color: var(--correct); }
  .syl-block.pop { animation: pop 0.3s ease; }

  @keyframes shake {
    0%,100%{ transform: translateX(0); }
    20%    { transform: translateX(-6px); }
    40%    { transform: translateX(6px); }
    60%    { transform: translateX(-4px); }
    80%    { transform: translateX(4px); }
  }
  @keyframes pop {
    0%  { transform: scale(1); }
    40% { transform: scale(1.18); }
    100%{ transform: scale(1); }
  }

  .tap-hint { font-size: 11px; color: var(--muted-2); margin-top: 8px; letter-spacing: 0.3px; transition: opacity 0.2s; }

  /* ── BUTTONS ── */
  .submit-row { display: flex; align-items: center; gap: 10px; margin-bottom: 10px; }
  .submit-btn {
    padding: 10px 24px; background: var(--blue-ink); color: #fff; border: none; border-radius: 4px;
    font-family: 'Quicksand', sans-serif; font-size: 13px; font-weight: 700; letter-spacing: 1px;
    cursor: pointer; transition: background 0.15s, transform 0.1s;
  }
  .submit-btn:hover  { background: #1e3570; }
  .submit-btn:active { transform: translateY(1px); }

  .feedback { height: 22px; font-size: 13px; font-weight: 600; letter-spacing: 0.5px; margin-bottom: 18px; display: flex; align-items: center; gap: 6px; }
  .feedback.correct { color: var(--correct); }
  .feedback.wrong   { color: var(--red-stamp); }

  .action-row { display: flex; gap: 8px; flex-wrap: wrap; margin-bottom: 6px; }
  .ghost-btn {
    padding: 7px 14px; background: none; border: 1.5px solid var(--ruled); border-radius: 4px;
    font-family: 'Quicksand', sans-serif; font-size: 12px; font-weight: 600;
    color: var(--muted); cursor: pointer; letter-spacing: 0.5px; transition: border-color 0.15s, color 0.15s;
  }
  .ghost-btn:hover { border-color: var(--muted-2); color: var(--ink); }

  /* ── PROGRESS ── */
  .progress-wrap { margin-bottom: 18px; }
  .progress-meta { display: flex; justify-content: space-between; font-size: 11px; color: var(--muted-2); font-weight: 600; letter-spacing: 1px; margin-bottom: 6px; }
  .progress-track { height: 4px; background: var(--ruled); border-radius: 2px; overflow: hidden; }
  .progress-fill { height: 100%; background: var(--blue-ink); border-radius: 2px; transition: width 0.4s ease; }

  /* ── STUDY LIST ── */
  .study-section { margin-bottom: 28px; }
  .study-section-title {
    font-size: 11px; font-weight: 700; letter-spacing: 2px; text-transform: uppercase;
    color: var(--red-stamp); padding-bottom: 8px; border-bottom: 1px solid var(--red-stamp-dim, rgba(192,57,43,0.2));
    margin-bottom: 0;
  }
  .study-row { display: flex; align-items: baseline; padding: 10px 4px; gap: 16px; border-bottom: 1px solid var(--ruled); }
  .study-kr { font-family: 'Gowun Dodum', sans-serif; font-size: 19px; color: var(--blue-ink); flex-shrink: 0; min-width: 160px; }
  .study-en { font-size: 13px; color: var(--muted); flex: 1; }

  /* ── PRONOUNCE MODE ── */
  .pronounce-card {
    text-align: center;
    padding: 8px 0 20px;
  }
  .record-btn {
    width: 84px; height: 84px;
    border-radius: 50%;
    border: none;
    background: var(--red-stamp);
    color: #fff;
    font-size: 30px;
    cursor: pointer;
    display: flex; align-items: center; justify-content: center;
    margin: 8px auto 14px;
    box-shadow: 0 4px 12px rgba(192,57,43,0.3);
    transition: transform 0.15s, background 0.15s;
  }
  .record-btn:hover { transform: scale(1.05); }
  .record-btn.recording {
    background: var(--red-stamp);
    animation: pulse-record 1.2s ease infinite;
  }
  @keyframes pulse-record {
    0%, 100% { box-shadow: 0 0 0 0 rgba(192,57,43,0.5); }
    50%      { box-shadow: 0 0 0 14px rgba(192,57,43,0); }
  }
  .record-label {
    font-size: 12px; color: var(--muted); letter-spacing: 0.5px;
    margin-bottom: 4px;
  }
  .pronounce-score {
    font-size: 44px; font-weight: 700;
    margin: 14px 0 4px;
  }
  .pronounce-score-label {
    font-size: 11px; letter-spacing: 1.5px; text-transform: uppercase;
    color: var(--muted); margin-bottom: 18px;
  }
  .syl-block.score-grad {
    /* background/color set inline per-block based on score */
    border-width: 2px;
  }
  .syl-score-label {
    position: absolute;
    bottom: -18px;
    left: 0; right: 0;
    text-align: center;
    font-family: 'Quicksand', sans-serif;
    font-size: 11px;
    font-weight: 700;
    letter-spacing: 0.3px;
  }
  .pronounce-status {
    font-size: 13px; color: var(--muted); min-height: 20px; margin-top: 8px;
  }

  /* ── END SCREEN ── */
  .end-screen { text-align: center; padding: 40px 0; }
  .stamp {
    display: inline-block; border: 3px solid var(--red-stamp); border-radius: 4px;
    padding: 8px 20px; font-family: 'Nanum Myeongjo', serif; font-size: 36px; font-weight: 700;
    color: var(--red-stamp); letter-spacing: 6px; transform: rotate(-4deg); margin-bottom: 24px; opacity: 0.85;
  }
  .end-score { font-size: 48px; font-weight: 700; color: var(--ink); line-height: 1; margin-bottom: 6px; }
  .end-label { font-size: 13px; color: var(--muted); letter-spacing: 1px; text-transform: uppercase; margin-bottom: 28px; }
  .end-breakdown {
    text-align: left; background: var(--card-bg); border: 1px solid var(--ruled);
    border-radius: 4px; padding: 16px 20px; margin-bottom: 24px; box-shadow: 2px 2px 0 var(--ruled);
  }
  .breakdown-row { display: flex; justify-content: space-between; align-items: center; font-size: 13px; padding: 5px 0; border-bottom: 1px solid var(--ruled); gap: 8px; }
  .breakdown-row:last-child { border-bottom: none; }
  .breakdown-row .kr { font-family: 'Gowun Dodum', sans-serif; font-size: 15px; color: var(--blue-ink); flex-shrink: 0; }
  .breakdown-row .en { color: var(--muted); font-size: 12px; flex: 1; }
  .breakdown-row .mark { font-size: 14px; flex-shrink: 0; }
  .play-recording-btn {
    background: none; border: none; cursor: pointer;
    font-size: 14px; flex-shrink: 0; padding: 2px 4px;
    border-radius: 4px; transition: background 0.15s;
    line-height: 1;
  }
  .play-recording-btn:hover { background: var(--blue-light); }
  .restart-btn {
    padding: 12px 32px; background: var(--red-stamp); color: #fff; border: none; border-radius: 4px;
    font-family: 'Quicksand', sans-serif; font-size: 14px; font-weight: 700; letter-spacing: 1px;
    cursor: pointer; transition: background 0.15s;
  }
  .restart-btn:hover { background: #a03020; }

  /* ── SETTINGS PANEL ── */
  .settings-overlay {
    position: fixed; inset: 0;
    background: rgba(0,0,0,0.45);
    z-index: 100;
    display: flex; align-items: flex-end; justify-content: center;
  }
  .settings-panel {
    background: var(--card-bg);
    width: 100%; max-width: 580px;
    border-radius: 16px 16px 0 0;
    padding: 20px 20px 28px;
    max-height: 80vh;
    overflow-y: auto;
  }
  .settings-panel-header {
    display: flex; align-items: center; justify-content: space-between;
    font-size: 15px; font-weight: 700; color: var(--ink);
    margin-bottom: 8px;
  }
  .settings-panel-hint {
    font-size: 12px; color: var(--muted); line-height: 1.5;
    margin-bottom: 18px;
  }
  .settings-list-row {
    display: flex; align-items: center; justify-content: space-between;
    padding: 10px 0; border-bottom: 1px solid var(--ruled);
    gap: 10px;
  }
  .settings-list-row:last-child { border-bottom: none; }
  .settings-list-name { font-size: 14px; font-weight: 600; color: var(--ink); }
  .settings-list-status { font-size: 11px; color: var(--muted); margin-top: 2px; }
  .settings-toggle-group { display: flex; gap: 4px; flex-shrink: 0; }
  .settings-toggle-btn {
    padding: 5px 10px; font-size: 11px; font-weight: 700;
    border: 1.5px solid var(--ruled); border-radius: 6px;
    background: none; color: var(--muted); cursor: pointer;
    letter-spacing: 0.3px; transition: all 0.15s;
  }
  .settings-toggle-btn.active-on  { border-color: var(--correct); background: var(--correct-bg); color: var(--correct); }
  .settings-toggle-btn.active-off { border-color: var(--red-stamp); background: var(--red-light); color: var(--red-stamp); }
  .settings-toggle-btn.active-auto { border-color: var(--blue-ink); background: var(--blue-light); color: var(--blue-ink); }

  /* ── STORIES MODE ── */
  .story-selector-list { display: flex; flex-direction: column; gap: 10px; margin-bottom: 20px; }
  .story-card {
    background: var(--card-bg); border: 1px solid var(--ruled);
    border-radius: 4px; padding: 16px 20px;
    box-shadow: 2px 2px 0 var(--ruled); cursor: pointer;
    transition: border-color 0.15s;
  }
  .story-card:hover { border-color: var(--blue-ink); }
  .story-card-title { font-size: 15px; font-weight: 700; color: var(--ink); margin-bottom: 4px; }
  .story-card-meta  { font-size: 12px; color: var(--muted); }
  .story-sub-tabs { display: flex; gap: 8px; margin-bottom: 20px; }
  .story-sub-tab {
    flex: 1; padding: 8px 0; text-align: center;
    font-family: 'Quicksand', sans-serif; font-size: 12px; font-weight: 700;
    letter-spacing: 0.5px; text-transform: uppercase;
    border: 1.5px solid var(--ruled); border-radius: 4px;
    background: none; color: var(--muted); cursor: pointer;
    transition: all 0.15s;
  }
  .story-sub-tab.active { border-color: var(--blue-ink); background: var(--blue-light); color: var(--blue-ink); }
  .story-read-line {
    display: flex; align-items: center; gap: 10px;
    padding: 10px 0; border-bottom: 1px solid var(--ruled);
  }
  .story-speaker {
    font-size: 11px; font-weight: 700; letter-spacing: 1px;
    color: var(--red-stamp); flex-shrink: 0; width: 22px; text-transform: uppercase;
  }
  .story-line-kr {
    font-family: 'Gowun Dodum', sans-serif;
    font-size: 18px; color: var(--ink); flex: 1; line-height: 1.5;
  }
  .story-line-listen {
    background: none; border: none; cursor: pointer;
    font-size: 16px; flex-shrink: 0; padding: 2px;
    opacity: 0.5; transition: opacity 0.15s;
  }
  .story-line-listen:hover { opacity: 1; }
  .story-context-line {
    font-family: 'Gowun Dodum', sans-serif;
    font-size: 16px; color: var(--muted-2);
    padding: 10px 0 14px; border-bottom: 1px solid var(--ruled);
    margin-bottom: 20px; line-height: 1.5;
  }
  .story-context-speaker {
    font-size: 11px; font-weight: 700; letter-spacing: 1px;
    color: var(--muted-2); text-transform: uppercase; margin-right: 6px;
  }
  .story-target-speaker {
    font-size: 11px; font-weight: 700; letter-spacing: 1px;
    color: var(--red-stamp); text-transform: uppercase; margin-bottom: 8px;
  }
  .mask-sentence {
    font-family: 'Gowun Dodum', sans-serif;
    font-size: 20px; line-height: 2; color: var(--ink); margin-bottom: 20px;
  }
  .mask-word { display: inline; }
  .mask-blank {
    display: inline-block;
    background: var(--blue-light); border-bottom: 2px solid var(--blue-ink);
    color: transparent; border-radius: 2px;
    min-width: 44px; margin: 0 2px; padding: 0 4px;
    vertical-align: baseline;
  }
  .mask-blank.revealed { color: var(--blue-ink); }
  .mask-blank.correct  { color: var(--correct); background: var(--correct-bg); border-color: var(--correct); }
  .mask-blank.wrong    { color: var(--red-stamp); background: var(--red-light); border-color: var(--red-stamp); }
  .story-back-btn {
    background: none; border: none; cursor: pointer;
    font-size: 12px; font-weight: 700; color: var(--muted);
    letter-spacing: 0.5px; padding: 0; margin-bottom: 16px;
    display: flex; align-items: center; gap: 4px;
  }
  .story-back-btn:hover { color: var(--ink); }
</style>
</head>
<body>
<div class="app">
  <header>
    <span class="title-kr">한글 쓰기</span>
    <span class="title-en">Spelling Practice</span>
    <span class="score-badge" id="scoreDisplay" style="display:none"></span>
    <button class="theme-toggle" id="settingsToggle" onclick="openSettingsPanel()" aria-label="Review settings">⚙️</button>
    <button class="theme-toggle" id="themeToggle" onclick="toggleTheme()" aria-label="Toggle dark mode">🔆</button>
  </header>

  <div id="settingsOverlay" class="settings-overlay" style="display:none" onclick="closeSettingsPanel(event)">
    <div class="settings-panel" onclick="event.stopPropagation()">
      <div class="settings-panel-header">
        <span>Review eligibility</span>
        <button class="ghost-btn" onclick="closeSettingsPanel()">Close</button>
      </div>
      <div class="settings-panel-hint">
        Lists become eligible for the Review tab automatically once you finish a full Quiz round on them. Override any list manually below.
      </div>
      <div id="settingsListRows"></div>
    </div>
  </div>

  <div class="mode-tabs">
    <button class="mode-tab active" id="tabStudy" onclick="switchMode('study')">Study</button>
    <button class="mode-tab"        id="tabQuiz"  onclick="switchMode('quiz')">Quiz</button>
    <button class="mode-tab"        id="tabPronounce" onclick="switchMode('pronounce')">Pronounce</button>
    <button class="mode-tab"        id="tabReview" onclick="switchMode('review')">Review<span id="reviewDueBadge" class="due-badge" style="display:none"></span></button>
    <button class="mode-tab"        id="tabStories" onclick="switchMode('stories')">Stories</button>
  </div>

  <!-- Two-column layout: chapter strip sidebar on left, content on right -->
  <div class="content-layout">
    <!-- Left: chapter strip (hidden in Review/Stories) -->
    <div class="sidebar" id="sidebar">
      <div class="selector-label" id="selectorLabel">Lists</div>
      <div id="listSelector"></div>
    </div>

    <!-- Right: selected chips bar + main content -->
    <div class="content-col" id="contentCol">
      <div class="selected-chips-bar" id="selectedChipsBar" style="display:none"></div>
      <div id="mainArea"></div>
    </div>
  </div>
</div>

<script>
// ── DATA ──────────────────────────────────────────────────────────────────
const LISTS = {
  "1.1 A": [
    { kr: "또 뵙겠습니다.",          en: "I'll see you again.",                    cat: "Expressions" },
    { kr: "병장.",                   en: "Sergeant (E-5); \"chief soldier\".",      cat: "Military" },
    { kr: "선생.",                   en: "A teacher.",                             cat: "Vocabulary" },
    { kr: "선생님.",                  en: "A teacher.",                             cat: "Vocabulary" },
    { kr: "안녕하세요.",              en: "Hello. Hi. How are you?",                cat: "Expressions" },
    { kr: "안녕하십니까?",            en: "Hello. Hi. How are you?",                cat: "Expressions" },
    { kr: "안녕히 가세요.",           en: "Good-bye. \"Go in peace.\"",             cat: "Expressions" },
    { kr: "안녕히 계세요.",           en: "Good-bye. \"Stay in peace.\"",           cat: "Expressions" },
    { kr: "어떻게 지내십니까?",       en: "How is it going? How are you?",          cat: "Expressions" },
    { kr: "오래간만입니다.",           en: "It's been a long time.",                 cat: "Expressions" },
    { kr: "요즘.",                   en: "Nowadays; lately.",                      cat: "Vocabulary" },
    { kr: "잘.",                     en: "Well.",                                  cat: "Vocabulary" },
    { kr: "지내다.",                  en: "To spend (time).",                       cat: "Vocabulary" },
  ],
  "1.1 B": [
    { kr: "고맙습니다.",              en: "Thank you (pure Korean).",               cat: "Expressions" },
    { kr: "감사합니다.",              en: "Thank you (Sino).",                      cat: "Expressions" },
    { kr: "네.",                     en: "Yes.",                                   cat: "Expressions" },
    { kr: "예.",                     en: "Yes (informal).",                        cat: "Expressions" },
    { kr: "아니요.",                  en: "No.",                                    cat: "Expressions" },
    { kr: "천만에요.",               en: "You're welcome.",                        cat: "Expressions" },
    { kr: "괜찮습니다.",              en: "OK / I'm okay.",                         cat: "Expressions" },
    { kr: "미안합니다.",              en: "I'm sorry.",                             cat: "Expressions" },
    { kr: "죄송합니다.",              en: "I'm sorry. (It's my fault.)",            cat: "Expressions" },
    { kr: "실례합니다.",              en: "Excuse me.",                             cat: "Expressions" },
    { kr: "만나서 반갑습니다.",       en: "Nice to meet you.",                      cat: "Introductions" },
    { kr: "처음 뵙겠습니다.",         en: "Nice to meet you (for the first time).", cat: "Introductions" },
    { kr: "고향.",                   en: "Hometown.",                              cat: "Vocabulary" },
    { kr: "이름.",                   en: "Name.",                                  cat: "Vocabulary" },
    { kr: "성함.",                   en: "Name (honorific).",                      cat: "Vocabulary" },
    { kr: "공군.",                   en: "Air Force.",                             cat: "Vocabulary" },
    { kr: "어디.",                   en: "Where.",                                 cat: "Vocabulary" },
    { kr: "캘리포니아.",              en: "California.",                            cat: "Vocabulary" },
    { kr: "제.",                     en: "I / my (humble).",                       cat: "Vocabulary" },
    { kr: "내.",                     en: "I / my (informal).",                     cat: "Vocabulary" },
  ],
  "1.1 C": [
    { kr: "계급.",                   en: "A (military) rank.",                            cat: "Military" },
    { kr: "신병.",                   en: "Private (E-1); \"new soldier\".",               cat: "Military" },
    { kr: "이병.",                   en: "Private (E-2); \"second soldier\".",            cat: "Military" },
    { kr: "일병.",                   en: "Private (E-3); \"first soldier\".",             cat: "Military" },
    { kr: "상병.",                   en: "Specialist (E-4); \"top soldier\".",            cat: "Military" },
    { kr: "병장.",                   en: "Sergeant (E-5).",                              cat: "Military" },
    { kr: "하사.",                   en: "Staff Sergeant (E-6); \"bottom sergeant\".",    cat: "Military" },
    { kr: "중사.",                   en: "Sergeant First Class (E-7); \"middle sergeant\".", cat: "Military" },
    { kr: "상사.",                   en: "Master Sergeant (E-8); \"top sergeant\".",      cat: "Military" },
    { kr: "원사.",                   en: "Sergeant Major (E-9); \"foremost sergeant\".",  cat: "Military" },
    { kr: "군인.",                   en: "A serviceman.",                               cat: "Military" },
    { kr: "그리고.",                  en: "And (then).",                                 cat: "Vocabulary" },
    { kr: "무엇.",                   en: "What.",                                       cat: "Vocabulary" },
    { kr: "소개하겠습니다.",           en: "Let me introduce.",                           cat: "Expressions" },
    { kr: "직업.",                   en: "An occupation.",                              cat: "Vocabulary" },
    { kr: "친구.",                   en: "A friend.",                                   cat: "Vocabulary" },
    { kr: "학생.",                   en: "A student.",                                  cat: "Vocabulary" },
    { kr: "육군.",                   en: "Army; a soldier; \"land military\".",           cat: "Military" },
    { kr: "해군.",                   en: "Navy; a sailor; \"sea military\".",             cat: "Military" },
    { kr: "해병대.",                  en: "Marine Corps; a marine.",                     cat: "Military" },
    { kr: "회사원.",                  en: "A company employee; \"company member\".",       cat: "Vocabulary" },
  ],
  "1.2 A": [
    { kr: "가족.",    en: "A family.",                                    cat: "Family" },
    { kr: "~명.",    en: "A counter for people.",                        cat: "Grammar" },
    { kr: "~분.",    en: "A counter for people (honorific).",            cat: "Grammar" },
    { kr: "모두.",   en: "All; every.",                                  cat: "Vocabulary" },
    { kr: "다.",     en: "All; every.",                                  cat: "Vocabulary" },
    { kr: "부모.",   en: "Parents; father and mother.",                  cat: "Family" },
    { kr: "부모님.", en: "Parents; father and mother (honorific).",      cat: "Family" },
    { kr: "아버지.", en: "A father.",                                    cat: "Family" },
    { kr: "아빠.",   en: "Dad.",                                         cat: "Family" },
    { kr: "아버님.", en: "A father (honorific).",                        cat: "Family" },
    { kr: "어머니.", en: "A mother.",                                    cat: "Family" },
    { kr: "엄마.",   en: "Mom.",                                         cat: "Family" },
    { kr: "어머님.", en: "A mother (honorific).",                        cat: "Family" },
    { kr: "남동생.", en: "A younger brother; \"male younger sibling\".", cat: "Family" },
    { kr: "누나.",   en: "An older sister (to male).",                   cat: "Family" },
    { kr: "누님.",   en: "An older sister (to male) (honorific).",       cat: "Family" },
    { kr: "언니.",   en: "An older sister (to female).",                 cat: "Family" },
    { kr: "여동생.", en: "A younger sister; \"female younger sibling\".", cat: "Family" },
    { kr: "오빠.",   en: "An older brother (to female).",                cat: "Family" },
    { kr: "형.",     en: "An older brother (to male).",                  cat: "Family" },
    { kr: "형님.",   en: "An older brother (to male) (honorific).",      cat: "Family" },
    { kr: "형제.",   en: "(Male) siblings.",                             cat: "Family" },
    { kr: "저희.",   en: "We; my; our (humble).",                        cat: "Vocabulary" },
    { kr: "우리.",   en: "We; my; our.",                                 cat: "Vocabulary" },
    { kr: "~하고.",  en: "And (as a noun connector).",                   cat: "Grammar" },
  ],
  "1.2 B": [
    { kr: "결혼하다.",      en: "To be married; to get married (dictionary form).", cat: "Vocabulary" },
    { kr: "결혼했습니다.",  en: "To be married; to get married (past tense).",      cat: "Vocabulary" },
    { kr: "나이.",          en: "Age.",                                             cat: "Vocabulary" },
    { kr: "연세.",          en: "Age (honorific).",                                 cat: "Vocabulary" },
    { kr: "남편.",          en: "A husband.",                                       cat: "Family" },
    { kr: "부인.",          en: "A wife (honorific).",                              cat: "Family" },
    { kr: "아내.",          en: "A wife.",                                          cat: "Family" },
    { kr: "아이.",          en: "A child; a kid.",                                  cat: "Family" },
    { kr: "딸.",            en: "A daughter.",                                      cat: "Family" },
    { kr: "따님.",          en: "A daughter (honorific).",                          cat: "Family" },
    { kr: "아들.",          en: "A son.",                                           cat: "Family" },
    { kr: "아드님.",        en: "A son (honorific).",                               cat: "Family" },
    { kr: "몇~.",           en: "How many ~",                                       cat: "Grammar" },
    { kr: "무슨~.",         en: "What ~",                                           cat: "Grammar" },
    { kr: "~살.",           en: "A counter for age.",                               cat: "Grammar" },
    { kr: "~세.",           en: "A counter for age (Sino-Korean).",                 cat: "Grammar" },
    { kr: "안+.",           en: "Not + verb.",                                      cat: "Grammar" },
    { kr: "없다.",          en: "Not to exist; not to possess.",                    cat: "Vocabulary" },
    { kr: "안 계시다.",     en: "Not to exist; not to possess (honorific).",        cat: "Vocabulary" },
    { kr: "일.",            en: "Work (noun).",                                     cat: "Vocabulary" },
    { kr: "일하다.",        en: "To work (verb).",                                  cat: "Vocabulary" },
    { kr: "있다.",          en: "To exist; to possess.",                            cat: "Vocabulary" },
    { kr: "계시다.",        en: "To exist; to possess (honorific).",                cat: "Vocabulary" },
    { kr: "할머니.",        en: "A grandmother.",                                   cat: "Family" },
    { kr: "할머님.",        en: "A grandmother (honorific).",                       cat: "Family" },
    { kr: "할아버지.",      en: "A grandfather.",                                   cat: "Family" },
    { kr: "할아버님.",      en: "A grandfather (honorific).",                       cat: "Family" },
  ],
  "1.2 C": [
    { kr: "간호사.",   en: "A nurse; \"nursing professional\".",                    cat: "Occupations" },
    { kr: "경찰관.",   en: "A police officer.",                                    cat: "Occupations" },
    { kr: "공무원.",   en: "A public servant; a government employee.",             cat: "Occupations" },
    { kr: "변호사.",   en: "A lawyer; \"advocating professional\".",               cat: "Occupations" },
    { kr: "소방관.",   en: "A firefighter; \"firefighting officer\".",             cat: "Occupations" },
    { kr: "의사.",     en: "A doctor; \"medical professional\".",                  cat: "Occupations" },
    { kr: "주부.",     en: "A homemaker.",                                         cat: "Occupations" },
    { kr: "은행원.",   en: "A bank teller; a bank clerk.",                         cat: "Occupations" },
    { kr: "병원.",     en: "A hospital; a doctor's office; \"illness house\".",    cat: "Vocabulary" },
    { kr: "사진.",     en: "A photo.",                                             cat: "Vocabulary" },
    { kr: "살다.",     en: "To live; to reside.",                                  cat: "Vocabulary" },
    { kr: "누구.",     en: "Who; whom.",                                           cat: "Vocabulary" },
    { kr: "아주.",     en: "Very; extremely.",                                     cat: "Vocabulary" },
    { kr: "젊다.",     en: "To be young; to be youthful.",                         cat: "Vocabulary" },
    { kr: "~에서.",    en: "At; in; on (location marker).",                        cat: "Grammar" },
    { kr: "-고.",      en: "And (conjunctive ending).",                            cat: "Grammar" },
  ],
  "1.3 A": [
    { kr: "집.",       en: "A house; a home.",                                     cat: "Home" },
    { kr: "~층.",      en: "A floor; a story (of a building).",                    cat: "Home" },
    { kr: "방.",       en: "A room.",                                              cat: "Home" },
    { kr: "거실.",     en: "A living room.",                                       cat: "Home" },
    { kr: "침실.",     en: "A bedroom; \"sleeping room\".",                        cat: "Home" },
    { kr: "화장실.",   en: "A bathroom; a restroom; \"makeup room\".",             cat: "Home" },
    { kr: "부엌.",     en: "A kitchen.",                                           cat: "Home" },
    { kr: "차고.",     en: "A garage; a carport.",                                 cat: "Home" },
    { kr: "마당.",     en: "A yard.",                                              cat: "Home" },
    { kr: "많다.",     en: "To be many.",                                          cat: "Descriptors" },
    { kr: "크다.",     en: "To be large; to be big; to be tall.",                  cat: "Descriptors" },
    { kr: "작다.",     en: "To be small.",                                         cat: "Descriptors" },
    { kr: "좋다.",     en: "To be good.",                                          cat: "Descriptors" },
    { kr: "넓다.",     en: "To be large; to be spacious.",                         cat: "Descriptors" },
    { kr: "근처.",     en: "Near; in the neighborhood.",                           cat: "Vocabulary" },
    { kr: "바다.",     en: "An ocean.",                                            cat: "Vocabulary" },
    { kr: "강.",       en: "A river.",                                             cat: "Vocabulary" },
    { kr: "나무.",     en: "A tree.",                                              cat: "Vocabulary" },
    { kr: "산.",       en: "A mountain; a hill.",                                  cat: "Vocabulary" },
    { kr: "~도.",      en: "Also; as well.",                                       cat: "Grammar" },
    { kr: "그래서.",   en: "Therefore; and (thus); so.",                           cat: "Vocabulary" },
    { kr: "~ 개.",     en: "A counter for things.",                                cat: "Grammar" },
  ],
  "1.3 B": [
    { kr: "빨간색.",   en: "Red (color).",                     cat: "Colors" },
    { kr: "빨강.",     en: "Red (color).",                     cat: "Colors" },
    { kr: "노란색.",   en: "Yellow (color).",                  cat: "Colors" },
    { kr: "노랑.",     en: "Yellow (color).",                  cat: "Colors" },
    { kr: "파란색.",   en: "Blue (color).",                    cat: "Colors" },
    { kr: "파랑.",     en: "Blue (color).",                    cat: "Colors" },
    { kr: "하얀색.",   en: "White (color).",                   cat: "Colors" },
    { kr: "하양.",     en: "White (color).",                   cat: "Colors" },
    { kr: "검은색.",   en: "Black (color).",                   cat: "Colors" },
    { kr: "검정.",     en: "Black (color).",                   cat: "Colors" },
    { kr: "앞.",       en: "Front.",                           cat: "Directions" },
    { kr: "뒤.",       en: "Behind; back.",                    cat: "Directions" },
    { kr: "위.",       en: "On; above.",                       cat: "Directions" },
    { kr: "밑.",       en: "Under; underneath; below.",        cat: "Directions" },
    { kr: "아래.",     en: "Under; underneath; below.",        cat: "Directions" },
    { kr: "안.",       en: "Inside.",                          cat: "Directions" },
    { kr: "밖.",       en: "Outside.",                         cat: "Directions" },
    { kr: "옆.",       en: "(Be)side.",                        cat: "Directions" },
    { kr: "오른쪽.",   en: "Right (side).",                    cat: "Directions" },
    { kr: "왼쪽.",     en: "Left (side).",                     cat: "Directions" },
    { kr: "볼펜.",     en: "A pen; a ballpoint pen.",          cat: "Objects" },
    { kr: "소파.",     en: "A sofa; a couch.",                 cat: "Objects" },
    { kr: "아파트.",   en: "An apartment.",                    cat: "Objects" },
    { kr: "옷.",       en: "Clothes.",                         cat: "Objects" },
    { kr: "옷장.",     en: "A closet; \"clothes cabinet\".",   cat: "Objects" },
    { kr: "의자.",     en: "A chair.",                         cat: "Objects" },
    { kr: "창문.",     en: "A window.",                        cat: "Objects" },
    { kr: "책.",       en: "A book.",                          cat: "Objects" },
    { kr: "책상.",     en: "A desk; \"book table\".",          cat: "Objects" },
    { kr: "책장.",     en: "A bookcase; \"book cabinet\".",    cat: "Objects" },
    { kr: "침대.",     en: "A bed; \"sleeping board\".",       cat: "Objects" },
    { kr: "카메라.",   en: "A camera.",                        cat: "Objects" },
    { kr: "커튼.",     en: "A curtain.",                       cat: "Objects" },
    { kr: "컴퓨터.",   en: "A computer.",                      cat: "Objects" },
    { kr: "테이블.",   en: "A table.",                         cat: "Objects" },
    { kr: "텔레비전.", en: "A television.",                    cat: "Objects" },
    { kr: "프린터.",   en: "A printer.",                       cat: "Objects" },
    { kr: "~에.",      en: "At; in; on (location marker).",   cat: "Grammar" },
    { kr: "이~.",      en: "This (indicator).",                cat: "Grammar" },
    { kr: "그 ~.",     en: "That (indicator).",                cat: "Grammar" },
    { kr: "저 ~.",     en: "That over there (indicator).",     cat: "Grammar" },
  ],
  "1.3 C": [
    { kr: "공부.",       en: "Studies (noun).",                          cat: "Vocabulary" },
    { kr: "공부하다.",   en: "To study (verb).",                         cat: "Vocabulary" },
    { kr: "듣다.",       en: "To listen; to hear.",                      cat: "Vocabulary" },
    { kr: "먹다.",       en: "To eat.",                                  cat: "Vocabulary" },
    { kr: "드시다.",     en: "To eat (honorific).",                      cat: "Vocabulary" },
    { kr: "잡수시다.",   en: "To eat (honorific).",                      cat: "Vocabulary" },
    { kr: "보다.",       en: "To see; to watch; to read.",               cat: "Vocabulary" },
    { kr: "빨래.",       en: "Laundry.",                                 cat: "Vocabulary" },
    { kr: "빨래하다.",   en: "To do laundry.",                           cat: "Vocabulary" },
    { kr: "설거지하다.", en: "To do the dishes.",                        cat: "Vocabulary" },
    { kr: "숙제.",       en: "Homework.",                                cat: "Vocabulary" },
    { kr: "숙제하다.",   en: "To do homework.",                          cat: "Vocabulary" },
    { kr: "쉬다.",       en: "To rest; to take a break.",                cat: "Vocabulary" },
    { kr: "식사.",       en: "A meal.",                                  cat: "Vocabulary" },
    { kr: "식사하다.",   en: "To have a meal; \"to do dining\".",        cat: "Vocabulary" },
    { kr: "요리.",       en: "Cooking.",                                 cat: "Vocabulary" },
    { kr: "요리하다.",   en: "To cook; \"to do cooking\".",              cat: "Vocabulary" },
    { kr: "운동.",       en: "Exercise.",                                cat: "Vocabulary" },
    { kr: "운동하다.",   en: "To do exercise; to work out.",             cat: "Vocabulary" },
    { kr: "읽다.",       en: "To read.",                                 cat: "Vocabulary" },
    { kr: "자다.",       en: "To sleep.",                                cat: "Vocabulary" },
    { kr: "주무시다.",   en: "To sleep (honorific).",                    cat: "Vocabulary" },
    { kr: "청소.",       en: "Cleaning.",                                cat: "Vocabulary" },
    { kr: "청소하다.",   en: "To clean; \"to do cleaning\".",            cat: "Vocabulary" },
    { kr: "~을.",        en: "Object marker (after consonant).",         cat: "Grammar" },
    { kr: "~를.",        en: "Object marker (after vowel).",             cat: "Grammar" },
    { kr: "음악.",       en: "Music; \"delightful sound\".",             cat: "Vocabulary" },
    { kr: "주말.",       en: "Weekend.",                                 cat: "Vocabulary" },
    { kr: "밥.",         en: "A meal; (cooked) rice.",                   cat: "Vocabulary" },
    { kr: "신문.",       en: "Newspapers.",                              cat: "Vocabulary" },
  ],
  "1.4 A": [
    { kr: "가깝다.",     en: "To be near; to be close.",                 cat: "Descriptors" },
    { kr: "멀다.",       en: "To be far.",                               cat: "Descriptors" },
    { kr: "건너편.",     en: "The opposite side; the other side; across from.", cat: "Vocabulary" },
    { kr: "맞은편.",     en: "The opposite side; the other side; across from.", cat: "Vocabulary" },
    { kr: "건물.",       en: "A building.",                              cat: "Vocabulary" },
    { kr: "기숙사.",     en: "A dormitory.",                             cat: "Vocabulary" },
    { kr: "도서관.",     en: "A library; \"house of pictures and books\".", cat: "Vocabulary" },
    { kr: "말.",         en: "A language; a speech; utterance.",         cat: "Vocabulary" },
    { kr: "독일.",       en: "Germany.",                                 cat: "Countries" },
    { kr: "러시아.",     en: "Russia.",                                  cat: "Countries" },
    { kr: "미국.",       en: "America; \"beautiful country\".",          cat: "Countries" },
    { kr: "영국.",       en: "Great Britain; the UK.",                   cat: "Countries" },
    { kr: "스페인.",     en: "Spain.",                                   cat: "Countries" },
    { kr: "사람.",       en: "A person; people.",                        cat: "Vocabulary" },
    { kr: "학교.",       en: "A school.",                                cat: "Vocabulary" },
    { kr: "본부.",       en: "A headquarters.",                          cat: "Vocabulary" },
    { kr: "~에서.",      en: "From.",                                    cat: "Grammar" },
    { kr: "조금.",       en: "A little; a bit.",                         cat: "Vocabulary" },
    { kr: "좀.",         en: "A little; a bit.",                         cat: "Vocabulary" },
    { kr: "주차장.",     en: "A parking lot.",                           cat: "Vocabulary" },
    { kr: "우체국.",     en: "A post office.",                           cat: "Vocabulary" },
    { kr: "운동장.",     en: "A track and field; a playground; \"exercising place\".", cat: "Vocabulary" },
    { kr: "일본.",       en: "Japan.",                                   cat: "Countries" },
    { kr: "일본어.",     en: "Japanese (language).",                     cat: "Countries" },
    { kr: "아랍어.",     en: "Arabic.",                                  cat: "Countries" },
    { kr: "중국.",       en: "China.",                                   cat: "Countries" },
    { kr: "중국어.",     en: "Chinese (language).",                      cat: "Countries" },
    { kr: "프랑스.",     en: "France.",                                  cat: "Countries" },
    { kr: "프랑스어.",   en: "French (language).",                       cat: "Countries" },
    { kr: "불어.",       en: "French (language).",                       cat: "Countries" },
    { kr: "영어.",       en: "English (language).",                      cat: "Countries" },
    { kr: "한국.",       en: "Korea.",                                   cat: "Countries" },
    { kr: "한국어.",     en: "Korean (language).",                       cat: "Countries" },
  ],
  "1.4 B": [
    { kr: "공책.",       en: "A notebook; \"empty book\".",              cat: "Vocabulary" },
    { kr: "교실.",       en: "A classroom.",                             cat: "Vocabulary" },
    { kr: "단어.",       en: "A (vocabulary) word.",                     cat: "Vocabulary" },
    { kr: "문.",         en: "A door; a gate.",                          cat: "Vocabulary" },
    { kr: "사무실.",     en: "An office.",                               cat: "Vocabulary" },
    { kr: "사전.",       en: "A dictionary.",                            cat: "Vocabulary" },
    { kr: "수업.",       en: "A class; a lesson.",                       cat: "Vocabulary" },
    { kr: "쓰다.",       en: "To use; to write.",                        cat: "Vocabulary" },
    { kr: "쓰레기통.",   en: "A trash can; \"trash container\".",        cat: "Vocabulary" },
    { kr: "자리.",       en: "A seat; one's place.",                     cat: "Vocabulary" },
    { kr: "일어서다.",   en: "To stand up.",                             cat: "Vocabulary" },
    { kr: "앉다.",       en: "To sit down.",                             cat: "Vocabulary" },
    { kr: "열다.",       en: "To open (a door / window).",               cat: "Vocabulary" },
    { kr: "닫다.",       en: "To close (a door / window).",              cat: "Vocabulary" },
    { kr: "칠판.",       en: "A blackboard; a whiteboard; \"painting board\".", cat: "Vocabulary" },
    { kr: "지우다.",     en: "To erase.",                                cat: "Vocabulary" },
    { kr: "켜다.",       en: "To turn on; to switch on.",                cat: "Vocabulary" },
    { kr: "끄다.",       en: "To turn off; to switch off; to put out.",  cat: "Vocabulary" },
    { kr: "펴다.",       en: "To open (a book); to unfold.",             cat: "Vocabulary" },
    { kr: "덮다.",       en: "To close (a book); to cover; to fold.",    cat: "Vocabulary" },
    { kr: "떠들다.",     en: "To make noises.",                          cat: "Vocabulary" },
    { kr: "졸다.",       en: "To doze off.",                             cat: "Vocabulary" },
    { kr: "-세요.",      en: "Please do. (으 added after a consonant-ending stem.)", cat: "Grammar" },
    { kr: "-지 마세요.", en: "Please don't.",                            cat: "Grammar" },
  ],
  "1.4 C": [
    { kr: "가다.",       en: "To go.",                                   cat: "Vocabulary" },
    { kr: "길.",         en: "A road; a street.",                        cat: "Vocabulary" },
    { kr: "나오다.",     en: "To appear; to come out; to come into view.", cat: "Vocabulary" },
    { kr: "동쪽.",       en: "East.",                                    cat: "Directions" },
    { kr: "서쪽.",       en: "West.",                                    cat: "Directions" },
    { kr: "남쪽.",       en: "South.",                                   cat: "Directions" },
    { kr: "북쪽.",       en: "North.",                                   cat: "Directions" },
    { kr: "첫 번째.",    en: "The first.",                                cat: "Vocabulary" },
    { kr: "첫째.",       en: "The first.",                                cat: "Vocabulary" },
    { kr: "두 번째.",    en: "The second.",                               cat: "Vocabulary" },
    { kr: "둘째.",       en: "The second.",                               cat: "Vocabulary" },
    { kr: "세 번째.",    en: "The third.",                                cat: "Vocabulary" },
    { kr: "셋째.",       en: "The third.",                                cat: "Vocabulary" },
    { kr: "네 번째.",    en: "The fourth.",                               cat: "Vocabulary" },
    { kr: "넷째.",       en: "The fourth.",                               cat: "Vocabulary" },
    { kr: "돌다.",       en: "To turn; to spin.",                        cat: "Vocabulary" },
    { kr: "바로.",       en: "Just; right.",                             cat: "Vocabulary" },
    { kr: "사거리.",     en: "An intersection; \"four ways\".",          cat: "Vocabulary" },
    { kr: "정문.",       en: "A main gate; \"right door\".",             cat: "Vocabulary" },
    { kr: "체육관.",     en: "A gymnasium; \"physical exercise place\".", cat: "Vocabulary" },
    { kr: "식당.",       en: "A restaurant; a cafeteria; a dining room.", cat: "Vocabulary" },
    { kr: "알다.",       en: "To know.",                                 cat: "Vocabulary" },
    { kr: "어떻게.",     en: "How.",                                     cat: "Vocabulary" },
    { kr: "오다.",       en: "To come.",                                 cat: "Vocabulary" },
    { kr: "좌회전.",     en: "Left turn.",                                cat: "Vocabulary" },
    { kr: "좌회전하다.", en: "To turn left.",                            cat: "Vocabulary" },
    { kr: "우회전.",     en: "Right turn.",                               cat: "Vocabulary" },
    { kr: "우회전하다.", en: "To turn right.",                           cat: "Vocabulary" },
    { kr: "~로.",        en: "To; toward; in the direction of. (으 added after a consonant-ending stem.)", cat: "Grammar" },
    { kr: "-은.",        en: "Attributive form for stative verbs (consonant-ending stem; ㄴ alone after a vowel-ending stem).", cat: "Grammar" },
    { kr: "~쪽.",        en: "A direction; a side.",                     cat: "Grammar" },
    { kr: "쭉.",         en: "Straight.",                                cat: "Vocabulary" },
    { kr: "곧장.",       en: "Straight.",                                cat: "Vocabulary" },
  ],
  "2.5 A": [
    { kr: "매일.",       en: "Every day.",                               cat: "Time" },
    { kr: "자주.",       en: "Often; frequently.",                       cat: "Time" },
    { kr: "가끔.",       en: "Sometimes; occasionally.",                 cat: "Time" },
    { kr: "-부터.",      en: "From (time/place).",                       cat: "Grammar" },
    { kr: "-까지.",      en: "Till; until; by.",                         cat: "Grammar" },
    { kr: "시작.",       en: "Beginning (noun).",                        cat: "Time" },
    { kr: "시작하다.",   en: "To start; to begin.",                      cat: "Time" },
    { kr: "끝나다.",     en: "To end; to be over.",                      cat: "Time" },
    { kr: "보내다.",     en: "To spend (time).",                         cat: "Time" },
    { kr: "시간.",       en: "An hour (duration); time.",                cat: "Time" },
    { kr: "-시.",        en: "Hour; o'clock.",                           cat: "Grammar" },
    { kr: "-분.",        en: "Minute.",                                  cat: "Grammar" },
    { kr: "-주.",        en: "A week.",                                  cat: "Grammar" },
    { kr: "-주일.",      en: "A week.",                                  cat: "Grammar" },
    { kr: "반.",         en: "A half; half of.",                         cat: "Time" },
    { kr: "하루.",       en: "A day.",                                   cat: "Time" },
    { kr: "-에.",        en: "Per.",                                     cat: "Grammar" },
    { kr: "-번.",        en: "A time/times; number.",                    cat: "Grammar" },
    { kr: "언제.",       en: "When.",                                    cat: "Vocabulary" },
    { kr: "얼마나.",     en: "How much/many (quantity or duration).",    cat: "Vocabulary" },
    { kr: "오전.",       en: "a.m.; morning.",                           cat: "Time" },
    { kr: "오후.",       en: "p.m.; afternoon.",                         cat: "Time" },
    { kr: "점심.",       en: "Lunch; lunch time.",                       cat: "Time" },
    { kr: "저녁.",       en: "Evening; dinner.",                         cat: "Time" },
    { kr: "-쯤.",        en: "About; approximately.",                    cat: "Grammar" },
  ],
  "2.5 B": [
    { kr: "군복.",           en: "A military uniform.",                     cat: "Vocabulary" },
    { kr: "입다.",           en: "To wear (clothes).",                      cat: "Vocabulary" },
    { kr: "돌아가다.",       en: "To return; \"to return and go\".",        cat: "Vocabulary" },
    { kr: "돌아오다.",       en: "To return; \"to return and come\".",      cat: "Vocabulary" },
    { kr: "마시다.",         en: "To drink.",                               cat: "Vocabulary" },
    { kr: "샤워.",           en: "A shower.",                               cat: "Vocabulary" },
    { kr: "샤워하다.",       en: "To take a shower.",                       cat: "Vocabulary" },
    { kr: "세수.",           en: "Washing one's face.",                     cat: "Vocabulary" },
    { kr: "세수하다.",       en: "To wash one's face.",                     cat: "Vocabulary" },
    { kr: "수업을 듣다.",    en: "To take a class.",                        cat: "Vocabulary" },
    { kr: "씻다.",           en: "To wash; to rinse.",                      cat: "Vocabulary" },
    { kr: "이를 닦다.",      en: "To brush one's teeth.",                   cat: "Vocabulary" },
    { kr: "일어나다.",       en: "To get up; to wake up; to rise.",         cat: "Vocabulary" },
    { kr: "연습.",           en: "Practicing (noun).",                      cat: "Vocabulary" },
    { kr: "연습하다.",       en: "To practice.",                            cat: "Vocabulary" },
    { kr: "말하기.",         en: "Speaking.",                               cat: "Vocabulary" },
    { kr: "아침.",           en: "a.m.; morning; breakfast.",               cat: "Time" },
    { kr: "-서.",        en: "And thus; and then; so; because. (Full form: -아서 / -어서)", cat: "Grammar", prefix: "-아/어" },
    { kr: "늘.",             en: "Always; all the time.",                   cat: "Vocabulary" },
    { kr: "정도.",           en: "About; approximately.",                   cat: "Vocabulary" },
    { kr: "다음.",           en: "Next; the following.",                    cat: "Vocabulary" },
  ],
  "2.5 C": [
    { kr: "가르치다.",       en: "To teach.",                                   cat: "Vocabulary" },
    { kr: "등산.",           en: "Hiking; mountain climbing.",                  cat: "Vocabulary" },
    { kr: "등산하다.",       en: "To hike; to go mountain climbing.",           cat: "Vocabulary" },
    { kr: "수영.",           en: "Swimming.",                                   cat: "Vocabulary" },
    { kr: "수영하다.",       en: "To swim.",                                    cat: "Vocabulary" },
    { kr: "지내다.",         en: "To spend; to pass one's time.",               cat: "Vocabulary" },
    { kr: "매주.",           en: "Every week.",                                 cat: "Time" },
    { kr: "-년.",            en: "Year.",                                       cat: "Grammar" },
    { kr: "-월.",            en: "A month.",                                    cat: "Grammar" },
    { kr: "-일.",            en: "A day.",                                      cat: "Grammar" },
    { kr: "밤.",             en: "The nighttime; night.",                       cat: "Time" },
    { kr: "너.",             en: "You (informal).",                             cat: "Vocabulary" },
    { kr: "-에게.",          en: "To (a person); formal/written.",              cat: "Grammar" },
    { kr: "-한테.",          en: "To (a person); colloquial/spoken.",           cat: "Grammar" },
    { kr: "-께.",            en: "To (a person); honorific.",                   cat: "Grammar" },
    { kr: "월요일.",         en: "Monday.",                                     cat: "Days" },
    { kr: "화요일.",         en: "Tuesday.",                                    cat: "Days" },
    { kr: "수요일.",         en: "Wednesday.",                                  cat: "Days" },
    { kr: "목요일.",         en: "Thursday.",                                   cat: "Days" },
    { kr: "금요일.",         en: "Friday.",                                     cat: "Days" },
    { kr: "토요일.",         en: "Saturday.",                                   cat: "Days" },
    { kr: "일요일.",         en: "Sunday.",                                     cat: "Days" },
    { kr: "일찍.",           en: "Early.",                                      cat: "Vocabulary" },
    { kr: "지난.",           en: "Last; the past.",                             cat: "Vocabulary" },
    { kr: "천.",             en: "A thousand.",                                 cat: "Vocabulary" },
    { kr: "함께.",           en: "Together.",                                   cat: "Vocabulary" },
    { kr: "-전에.",          en: "Before; prior to (after a noun).",            cat: "Grammar" },
    { kr: "-기 전에.",       en: "Before; prior to (after a verb stem).",       cat: "Grammar" },
    { kr: "-은 후에.",       en: "After (consonant-ending verb stem; -ㄴ 후에 after vowel-ending stem).", cat: "Grammar" },
    { kr: "-후에.",          en: "After (after a noun).",                       cat: "Grammar" },
  ],
  "2.6 A": [
    { kr: "봄.",         en: "Spring.",                                  cat: "Seasons" },
    { kr: "여름.",       en: "Summer.",                                  cat: "Seasons" },
    { kr: "가을.",       en: "Fall; autumn.",                            cat: "Seasons" },
    { kr: "겨울.",       en: "Winter.",                                  cat: "Seasons" },
    { kr: "계절.",       en: "A season.",                                cat: "Seasons" },
    { kr: "구름.",       en: "Cloud.",                                   cat: "Weather" },
    { kr: "끼다.",       en: "To form (fog, cloud, dust, etc.).",        cat: "Weather" },
    { kr: "날씨.",       en: "Weather.",                                 cat: "Weather" },
    { kr: "눈.",         en: "Snow.",                                    cat: "Weather" },
    { kr: "내리다.",     en: "To fall; to come down (rain, snow, etc.).", cat: "Weather" },
    { kr: "높다.",       en: "To be high.",                              cat: "Descriptors" },
    { kr: "따뜻하다.",   en: "To be warm.",                              cat: "Descriptors" },
    { kr: "쌀쌀하다.",   en: "To be chilly.",                            cat: "Descriptors" },
    { kr: "덥다.",       en: "To be hot.",                               cat: "Descriptors" },
    { kr: "춥다.",       en: "To be cold (weather).",                    cat: "Descriptors" },
    { kr: "맑다.",       en: "To be clear.",                             cat: "Descriptors" },
    { kr: "바람.",       en: "Wind.",                                    cat: "Weather" },
    { kr: "불다.",       en: "To blow.",                                 cat: "Weather" },
    { kr: "많이.",       en: "A lot; much.",                             cat: "Vocabulary" },
    { kr: "비.",         en: "Rain.",                                    cat: "Weather" },
    { kr: "안개.",       en: "Fog.",                                     cat: "Weather" },
    { kr: "하늘.",       en: "The sky.",                                 cat: "Weather" },
    { kr: "흐리다.",     en: "To be cloudy.",                            cat: "Descriptors" },
  ],
  "2.6 B": [
    { kr: "-고 싶다.",     en: "Want to; wish to (first person — one's own desire).",   cat: "Grammar" },
    { kr: "-고 싶어하다.", en: "Want to; wish to (third person — someone else's desire).", cat: "Grammar" },
    { kr: "못.",           en: "Unable to; cannot (used before a verb).",               cat: "Grammar", prefix: "못+" },
    { kr: "취미.",         en: "A hobby.",                                              cat: "Vocabulary" },
    { kr: "골프.",         en: "Golf.",                                                 cat: "Sports" },
    { kr: "농구.",         en: "Basketball.",                                           cat: "Sports" },
    { kr: "배구.",         en: "Volleyball.",                                           cat: "Sports" },
    { kr: "야구.",         en: "Baseball.",                                             cat: "Sports" },
    { kr: "축구.",         en: "Soccer; football.",                                     cat: "Sports" },
    { kr: "탁구.",         en: "Table tennis.",                                         cat: "Sports" },
    { kr: "볼링.",         en: "Bowling.",                                              cat: "Sports" },
    { kr: "독서.",         en: "Reading (books).",                                      cat: "Vocabulary" },
    { kr: "테니스.",       en: "Tennis.",                                               cat: "Sports" },
    { kr: "스노보드.",     en: "Snowboard.",                                            cat: "Sports" },
    { kr: "스케이트.",     en: "Skate.",                                                cat: "Sports" },
    { kr: "스키.",         en: "Ski.",                                                  cat: "Sports" },
    { kr: "쉽다.",         en: "To be easy.",                                           cat: "Descriptors" },
    { kr: "어렵다.",       en: "To be difficult.",                                      cat: "Descriptors" },
    { kr: "여행.",         en: "Travel; travels (noun).",                               cat: "Vocabulary" },
    { kr: "여행하다.",     en: "To travel.",                                            cat: "Vocabulary" },
    { kr: "재미있다.",     en: "To be fun; to be enjoyable.",                           cat: "Descriptors" },
    { kr: "좋아하다.",     en: "To like.",                                              cat: "Vocabulary" },
    { kr: "배우다.",       en: "To learn.",                                             cat: "Vocabulary" },
    { kr: "치다.",         en: "To hit; to play (tennis, ping-pong, etc.).",            cat: "Vocabulary" },
    { kr: "타다.",         en: "To ride; to get on (road or highway).",                 cat: "Vocabulary" },
    { kr: "어떤-.",        en: "Which; what kind (modifies a following noun).",         cat: "Vocabulary" },
    { kr: "하지만.",       en: "But; however.",                                         cat: "Vocabulary" },
  ],
  "2.6 C": [
    { kr: "같이.",         en: "(Along) with; together.",                               cat: "Vocabulary" },
    { kr: "계획.",         en: "A plan.",                                               cat: "Vocabulary" },
    { kr: "계획하다.",     en: "To plan.",                                              cat: "Vocabulary" },
    { kr: "구경.",         en: "Watching; sightseeing.",                                cat: "Vocabulary" },
    { kr: "구경하다.",     en: "To watch; to sightsee.",                                cat: "Vocabulary" },
    { kr: "만나다.",       en: "To meet; to see.",                                      cat: "Vocabulary" },
    { kr: "산책.",         en: "A walk; a stroll.",                                     cat: "Vocabulary" },
    { kr: "산책하다.",     en: "To take a walk.",                                       cat: "Vocabulary" },
    { kr: "야영.",         en: "A camp; camping.",                                      cat: "Vocabulary" },
    { kr: "야영하다.",     en: "To camp.",                                              cat: "Vocabulary" },
    { kr: "전화.",         en: "A telephone; a phone call.",                            cat: "Vocabulary" },
    { kr: "전화하다.",     en: "To make a phone call.",                                 cat: "Vocabulary" },
    { kr: "골프장.",       en: "A golf course.",                                        cat: "Vocabulary" },
    { kr: "공원.",         en: "A park.",                                               cat: "Vocabulary" },
    { kr: "달리기.",       en: "Running; jogging.",                                     cat: "Vocabulary" },
    { kr: "바닷가.",       en: "The beach; the coast; the seaside.",                    cat: "Vocabulary" },
    { kr: "영화.",         en: "A movie.",                                              cat: "Vocabulary" },
    { kr: "자전거.",       en: "A bicycle.",                                            cat: "Vocabulary" },
    { kr: "-러 가다.",     en: "To go (in order) to— (으 added after consonant-ending stem).", cat: "Grammar", prefix: "-으/" },
    { kr: "-러 오다.",     en: "To come (in order) to— (으 added after consonant-ending stem).", cat: "Grammar", prefix: "-으/" },
    { kr: "-읍시다.",      en: "Let's— (consonant-ending stem; vowel-ending stem drops 으 → -ㅂ시다).", cat: "Grammar" },
    { kr: "이번.",         en: "This (time).",                                          cat: "Vocabulary" },
  ],
  "2.7 A": [
    { kr: "시내.",         en: "Downtown; \"in(side) city\".",                          cat: "Vocabulary" },
    { kr: "가게.",         en: "A store; a shop.",                                      cat: "Vocabulary" },
    { kr: "서점.",         en: "A bookstore.",                                          cat: "Vocabulary" },
    { kr: "제과점.",       en: "A bakery.",                                             cat: "Vocabulary" },
    { kr: "빵집.",         en: "A bakery.",                                             cat: "Vocabulary" },
    { kr: "약국.",         en: "A pharmacy; a drug store.",                             cat: "Vocabulary" },
    { kr: "은행.",         en: "A bank.",                                               cat: "Vocabulary" },
    { kr: "영화관.",       en: "A movie theater.",                                      cat: "Vocabulary" },
    { kr: "주유소.",       en: "A gas station; \"fueling place\".",                     cat: "Vocabulary" },
    { kr: "꽃집.",         en: "A flower shop; \"flower house\".",                      cat: "Vocabulary" },
    { kr: "백화점.",       en: "A department store.",                                   cat: "Vocabulary" },
    { kr: "세탁소.",       en: "A dry cleaner; a Laundromat; \"cleaning place\".",      cat: "Vocabulary" },
    { kr: "걸리다.",       en: "To take (time).",                                       cat: "Vocabulary" },
    { kr: "찾다.",         en: "To withdraw (money); to find.",                         cat: "Vocabulary" },
    { kr: "이야기.",       en: "A chat; a talk; a story.",                              cat: "Vocabulary" },
    { kr: "이야기하다.",   en: "To have a chat; to have a talk.",                       cat: "Vocabulary" },
    { kr: "얘기.",         en: "A chat; a talk; a story (colloquial).",                 cat: "Vocabulary" },
    { kr: "얘기하다.",     en: "To have a chat; to have a talk (colloquial).",          cat: "Vocabulary" },
    { kr: "돈.",           en: "Money.",                                                cat: "Vocabulary" },
    { kr: "맡기다.",       en: "To entrust; to deposit; to leave in someone's care.",   cat: "Vocabulary" },
    { kr: "기름.",         en: "Oil; gasoline.",                                        cat: "Vocabulary" },
    { kr: "넣다.",         en: "To insert; to put in.",                                 cat: "Vocabulary" },
    { kr: "물건.",         en: "A thing; stuff; goods.",                                cat: "Vocabulary" },
    { kr: "생일.",         en: "A birthday.",                                           cat: "Vocabulary" },
    { kr: "생신.",         en: "A birthday (honorific).",                               cat: "Vocabulary" },
    { kr: "약.",           en: "Medicine; drug; medication.",                           cat: "Vocabulary" },
    { kr: "사다.",         en: "To buy; to purchase.",                                  cat: "Vocabulary" },
    { kr: "~등.",          en: "And so on; et cetera.",                                 cat: "Grammar" },
  ],
  "2.7 B": [
    { kr: "갖다 주다.",    en: "To bring; to take.",                                    cat: "Vocabulary" },
    { kr: "계산서.",       en: "A bill; a check; \"calculation paper\".",               cat: "Vocabulary" },
    { kr: "김밥.",         en: "Korean sushi; \"seaweed rice\".",                       cat: "Food" },
    { kr: "김치.",         en: "Kimchi.",                                               cat: "Food" },
    { kr: "김치찌개.",     en: "Kimchi stew.",                                          cat: "Food" },
    { kr: "만두.",         en: "Dumpling; pot stickers.",                               cat: "Food" },
    { kr: "불고기.",       en: "Bulgogi; Korean style barbecued beef.",                  cat: "Food" },
    { kr: "비빔밥.",       en: "Bibimbap; rice mixed with vegetables and meat; \"mixed rice\".", cat: "Food" },
    { kr: "반찬.",         en: "A side dish.",                                          cat: "Food" },
    { kr: "음료수.",       en: "Beverages; drinks; \"drinking water\".",                cat: "Food" },
    { kr: "맥주.",         en: "Beer.",                                                 cat: "Food" },
    { kr: "물.",           en: "Water.",                                                cat: "Food" },
    { kr: "음식.",         en: "Food.",                                                 cat: "Food" },
    { kr: "콜라.",         en: "Cola; Coke.",                                           cat: "Food" },
    { kr: "맵다.",         en: "To be spicy; to be hot.",                               cat: "Descriptors" },
    { kr: "맛있다.",       en: "To be tasty; to be delicious; \"taste exists\".",       cat: "Descriptors" },
    { kr: "받다.",         en: "To receive; to take.",                                  cat: "Vocabulary" },
    { kr: "배가 고프다.",  en: "To be hungry; \"the stomach is hungry\".",              cat: "Vocabulary" },
    { kr: "시키다.",       en: "To order (food or drinks).",                            cat: "Vocabulary" },
    { kr: "주문하다.",     en: "To order (food or drinks).",                            cat: "Vocabulary" },
    { kr: "주다.",         en: "To give.",                                              cat: "Vocabulary" },
    { kr: "드리다.",       en: "To give; to offer (honorific).",                        cat: "Vocabulary" },
    { kr: "신용카드.",     en: "A credit card.",                                        cat: "Vocabulary" },
    { kr: "~병.",          en: "A counter for bottles.",                                cat: "Grammar" },
    { kr: "~인분.",        en: "A serving; \"person's portion\".",                      cat: "Grammar" },
    { kr: "~잔.",          en: "A counter for a glass or cup.",                         cat: "Grammar" },
  ],
  "2.7 C": [
    { kr: "손님.",         en: "A customer; a client.",                                 cat: "Vocabulary" },
    { kr: "남자.",         en: "A man; a male.",                                        cat: "Vocabulary" },
    { kr: "여자.",         en: "A woman; a female.",                                    cat: "Vocabulary" },
    { kr: "짧다.",         en: "To be short. (adverb form: 짧게)",                     cat: "Descriptors" },
    { kr: "길다.",         en: "To be long. (adverb form: 길게)",                      cat: "Descriptors" },
    { kr: "마음에 들어요.",en: "To be likable; \"come to the mind\".",                  cat: "Expressions" },
    { kr: "어울리다.",     en: "To fit; to go well with; to suit.",                     cat: "Vocabulary" },
    { kr: "머리.",         en: "The head; hair.",                                       cat: "Vocabulary" },
    { kr: "미용실.",       en: "Beauty salon.",                                         cat: "Vocabulary" },
    { kr: "머리방.",       en: "Beauty salon.",                                         cat: "Vocabulary" },
    { kr: "샴푸.",         en: "A shampoo.",                                            cat: "Vocabulary" },
    { kr: "샴푸하다.",     en: "To shampoo.",                                           cat: "Vocabulary" },
    { kr: "염색.",         en: "Dyeing.",                                               cat: "Vocabulary" },
    { kr: "염색하다.",     en: "To dye.",                                               cat: "Vocabulary" },
    { kr: "커트.",         en: "Cut (hair).",                                           cat: "Vocabulary" },
    { kr: "파마.",         en: "Perm.",                                                 cat: "Vocabulary" },
    { kr: "파마하다.",     en: "To have a perm.",                                       cat: "Vocabulary" },
    { kr: "자르다.",       en: "To cut.",                                               cat: "Vocabulary" },
    { kr: "어서 오세요.",  en: "Welcome; come on in.",                                  cat: "Expressions" },
    { kr: "유행.",         en: "A fashion; popularity; a trend.",                       cat: "Vocabulary" },
    { kr: "예쁘다.",       en: "To be pretty.",                                         cat: "Descriptors" },
    { kr: "예약.",         en: "An appointment; a reservation.",                        cat: "Vocabulary" },
    { kr: "예약하다.",     en: "To make an appointment; to make a reservation.",        cat: "Vocabulary" },
    { kr: "짙다.",         en: "To be dark; to be dense; to be rich (color/flavor).",  cat: "Descriptors" },
    { kr: "갈색.",         en: "Brown (color).",                                        cat: "Vocabulary" },
    { kr: "주다.",         en: "To provide a service (auxiliary verb).",                cat: "Grammar", prefix: "-아/어" },
    { kr: "드리다.",       en: "To provide a service (auxiliary verb, honorific).",     cat: "Grammar", prefix: "-아/어" },
    { kr: "-을 까요?",     en: "Shall we~? (consonant-ending stem; vowel-ending drops 으 → -ㄹ까요?)", cat: "Grammar" },
    { kr: "-게.",          en: "Adverb marker.",                                        cat: "Grammar" },
    { kr: "그런데.",       en: "By the way; however; but; and.",                        cat: "Vocabulary" },
    { kr: "~얼마예요?",    en: "How much is ~?",                                        cat: "Expressions" },
  ],
  "2.8 A": [
    { kr: "거기.",         en: "There.",                                                cat: "Vocabulary" },
    { kr: "떠나다.",       en: "To leave (a place).",                                   cat: "Vocabulary" },
    { kr: "출발.",         en: "Departure.",                                            cat: "Vocabulary" },
    { kr: "출발하다.",     en: "To depart; to set out.",                                cat: "Vocabulary" },
    { kr: "도착.",         en: "Arrival.",                                              cat: "Vocabulary" },
    { kr: "도착하다.",     en: "To arrive.",                                            cat: "Vocabulary" },
    { kr: "놀다.",         en: "To have fun; to play.",                                 cat: "Vocabulary" },
    { kr: "돌아다니다.",   en: "To go around; to walk around; to roam.",                cat: "Vocabulary" },
    { kr: "운전.",         en: "Driving.",                                              cat: "Vocabulary" },
    { kr: "운전하다.",     en: "To drive; to operate.",                                 cat: "Vocabulary" },
    { kr: "막히다.",       en: "To be blocked; to be congested.",                       cat: "Vocabulary" },
    { kr: "피곤하다.",     en: "To be tired.",                                          cat: "Descriptors" },
    { kr: "춤추다.",       en: "To dance.",                                             cat: "Vocabulary" },
    { kr: "한복.",         en: "Traditional Korean dress (Hanbok).",                    cat: "Vocabulary" },
    { kr: "박물관.",       en: "A museum.",                                             cat: "Vocabulary" },
    { kr: "선물.",         en: "A gift; a present.",                                    cat: "Vocabulary" },
    { kr: "아시아.",       en: "Asia.",                                                 cat: "Vocabulary" },
    { kr: "오래.",         en: "For a long time.",                                      cat: "Vocabulary" },
    { kr: "늦게.",         en: "Late (as in arrive late).",                             cat: "Vocabulary" },
    { kr: "먼저.",         en: "First of all; to begin with.",                          cat: "Vocabulary" },
  ],
  "2.8 B": [
    { kr: "날.",           en: "Day.",                                                  cat: "Vocabulary" },
    { kr: "장교.",         en: "An officer.",                                           cat: "Military" },
    { kr: "준위.",         en: "A warrant officer; \"deputy officer\".",                cat: "Military" },
    { kr: "소위.",         en: "A second lieutenant; \"young officer\".",               cat: "Military" },
    { kr: "중위.",         en: "A first lieutenant; \"middle officer\".",               cat: "Military" },
    { kr: "대위.",         en: "A captain; \"big officer\".",                           cat: "Military" },
    { kr: "소령.",         en: "A major; \"young governor\".",                          cat: "Military" },
    { kr: "중령.",         en: "A lieutenant colonel; \"middle governor\".",            cat: "Military" },
    { kr: "대령.",         en: "A colonel; \"big governor\".",                          cat: "Military" },
    { kr: "장군.",         en: "A general; \"commander of military\".",                 cat: "Military" },
    { kr: "준장.",         en: "A brigadier general; \"deputy commander\".",            cat: "Military" },
    { kr: "소장.",         en: "A major general; \"young commander\".",                 cat: "Military" },
    { kr: "중장.",         en: "A lieutenant general; \"middle commander\".",           cat: "Military" },
    { kr: "대장.",         en: "A general; a four-star general; \"big commander\".",    cat: "Military" },
    { kr: "또.",           en: "Also.",                                                 cat: "Vocabulary" },
    { kr: "만들다.",       en: "To make.",                                              cat: "Vocabulary" },
    { kr: "모이다.",       en: "To get together.",                                      cat: "Vocabulary" },
    { kr: "즐겁다.",       en: "To be pleasant; to be delightful.",                     cat: "Descriptors" },
    { kr: "진급.",         en: "Promotion.",                                            cat: "Vocabulary" },
    { kr: "진급하다.",     en: "To get promoted.",                                      cat: "Vocabulary" },
    { kr: "처음.",         en: "The first time.",                                       cat: "Vocabulary" },
    { kr: "처음으로.",     en: "For the first time.",                                   cat: "Vocabulary" },
    { kr: "축하.",         en: "Congratulations.",                                      cat: "Expressions" },
    { kr: "축하하다.",     en: "To congratulate.",                                      cat: "Expressions" },
    { kr: "파티.",         en: "Party.",                                                cat: "Vocabulary" },
    { kr: "파티하다.",     en: "To have a party.",                                      cat: "Vocabulary" },
    { kr: "술.",           en: "Alcohol; liquor.",                                      cat: "Vocabulary" },
    { kr: "보다.",         en: "To try (doing).",                                       cat: "Grammar", prefix: "-아/어" },
    { kr: "어제.",         en: "Yesterday.",                                            cat: "Vocabulary" },
  ],
  "2.8 C": [
    { kr: "그냥.",         en: "Just; without a particular reason.",                    cat: "Vocabulary" },
    { kr: "그래?",         en: "Really?; Is it so?",                                   cat: "Expressions" },
    { kr: "기념품.",       en: "A souvenir; \"commemoration item\".",                   cat: "Vocabulary" },
    { kr: "레스토랑.",     en: "A (fancy) restaurant.",                                 cat: "Vocabulary" },
    { kr: "수족관.",       en: "An aquarium.",                                          cat: "Vocabulary" },
    { kr: "바쁘다.",       en: "To be busy.",                                           cat: "Descriptors" },
    { kr: "시험.",         en: "Test; exam.",                                           cat: "Vocabulary" },
    { kr: "시험 보다.",    en: "To take a test.",                                       cat: "Vocabulary" },
    { kr: "심심하다.",     en: "To be bored.",                                          cat: "Descriptors" },
    { kr: "집안일.",       en: "Housework; chores.",                                    cat: "Vocabulary" },
    { kr: "집안일하다.",   en: "To do housework.",                                      cat: "Vocabulary" },
    { kr: "연휴.",         en: "A long weekend; consecutive holidays.",                 cat: "Vocabulary" },
    { kr: "~동안.",        en: "During.",                                               cat: "Grammar" },
    { kr: "~만.",          en: "Only.",                                                 cat: "Grammar" },
    { kr: "하다.",         en: "Must; have to.",                                        cat: "Grammar", prefix: "-아/어야" },
    { kr: "되다.",         en: "Must; have to (alternate form).",                       cat: "Grammar", prefix: "-아/어야" },
    { kr: "여기.",         en: "Here.",                                                 cat: "Vocabulary" },
    { kr: "저기.",         en: "Over there.",                                           cat: "Vocabulary" },
    { kr: "왜.",           en: "Why.",                                                  cat: "Vocabulary" },
    { kr: "응.",           en: "Yes (blunt form).",                                     cat: "Expressions" },
  ],
  "3.9 A": [
    { kr: "여보세요.",     en: "Hello (when answering the phone).",                    cat: "Expressions" },
    { kr: "국.",           en: "The first three digits of a phone number.",            cat: "Vocabulary" },
    { kr: "기다리다.",     en: "To wait.",                                             cat: "Vocabulary" },
    { kr: "남기다.",       en: "To leave (a message, food, etc.).",                   cat: "Vocabulary" },
    { kr: "바꾸다.",       en: "To put someone on the phone; to exchange; to replace.", cat: "Vocabulary" },
    { kr: "보내다.",       en: "To send.",                                             cat: "Vocabulary" },
    { kr: "부탁.",         en: "A favor.",                                             cat: "Vocabulary" },
    { kr: "부탁하다.",     en: "To ask a favor; to ask to put someone on the phone.", cat: "Vocabulary" },
    { kr: "부탁 드리다.",  en: "To ask a favor; to ask to put someone on the phone (honorific).", cat: "Vocabulary" },
    { kr: "전화를 걸다.",  en: "To make a phone call; \"to dial a phone\".",          cat: "Expressions" },
    { kr: "다시.",         en: "Again.",                                               cat: "Vocabulary" },
    { kr: "문자.",         en: "Text.",                                                cat: "Vocabulary" },
    { kr: "음성.",         en: "Voice.",                                               cat: "Vocabulary" },
    { kr: "메시지.",       en: "A message.",                                           cat: "Vocabulary" },
    { kr: "통화 중.",      en: "On the phone; \"in the middle of a phone conversation\".", cat: "Expressions" },
    { kr: "핸드폰.",       en: "A cell phone; a mobile phone; \"hand phone\".",       cat: "Vocabulary" },
    { kr: "휴대폰.",       en: "A cell phone; a mobile phone.",                       cat: "Vocabulary" },
    { kr: "중학교.",       en: "A middle school.",                                    cat: "Vocabulary" },
    { kr: "잠시만.",       en: "For a moment; for a second.",                         cat: "Vocabulary" },
    { kr: "잠깐만.",       en: "For a moment; for a second.",                         cat: "Vocabulary" },
    { kr: "~의.",          en: "Possessive marker; 's.",                              cat: "Grammar" },
  ],
  "3.9 B": [
    { kr: "이틀.",         en: "Two days.",                                            cat: "Time" },
    { kr: "사흘.",         en: "Three days.",                                          cat: "Time" },
    { kr: "나흘.",         en: "Four days.",                                           cat: "Time" },
    { kr: "특별하다.",     en: "To be special.",                                       cat: "Descriptors" },
    { kr: "편하다.",       en: "To be convenient; to be comfortable.",                 cat: "Descriptors" },
    { kr: "힘들다.",       en: "To be difficult; to be challenging.",                  cat: "Descriptors" },
    { kr: "비행기.",       en: "An airplane.",                                         cat: "Vocabulary" },
    { kr: "표.",           en: "A ticket.",                                            cat: "Vocabulary" },
    { kr: "아직.",         en: "(Not) yet; still.",                                    cat: "Vocabulary" },
    { kr: "방학.",         en: "A school break.",                                      cat: "Vocabulary" },
    { kr: "#박#일.",       en: "~ nights ~ days (counter pattern, e.g. 3박 4일 = three nights four days).", cat: "Grammar" },
    { kr: "~때.",          en: "Time; when.",                                          cat: "Grammar" },
    { kr: "-을 거다.",     en: "To be going to— (consonant-ending stem; vowel-ending drops 으 → -ㄹ 거다).", cat: "Grammar" },
    { kr: "-을 수 있다.",  en: "Can; to be able to.",                                 cat: "Grammar" },
    { kr: "-을 수 없다.",  en: "Cannot; to be unable to.",                            cat: "Grammar" },
    { kr: "한~.",          en: "About; approximately (used before a quantity).",       cat: "Grammar" },
    { kr: "~로.",          en: "By means of.",                                        cat: "Grammar" },
  ],
  "3.9 C": [
    { kr: "~것.",          en: "A thing; an object.",                                  cat: "Grammar" },
    { kr: "~과.",          en: "And (noun connector, after consonant).",               cat: "Grammar" },
    { kr: "~와.",          en: "And (noun connector, after vowel).",                   cat: "Grammar" },
    { kr: "노래.",         en: "A song.",                                              cat: "Vocabulary" },
    { kr: "노래하다.",     en: "To sing.",                                             cat: "Vocabulary" },
    { kr: "노래 부르다.",  en: "To sing a song (more natural spoken form).",           cat: "Vocabulary" },
    { kr: "머무르다.",     en: "To stay; to lodge.",                                   cat: "Vocabulary" },
    { kr: "머물다.",       en: "To stay; to lodge.",                                   cat: "Vocabulary" },
    { kr: "초대.",         en: "An invitation.",                                       cat: "Vocabulary" },
    { kr: "초대하다.",     en: "To invite.",                                           cat: "Vocabulary" },
    { kr: "잔치.",         en: "A feast; a party; a banquet.",                         cat: "Vocabulary" },
    { kr: "친척.",         en: "A relative.",                                          cat: "Vocabulary" },
    { kr: "환갑.",         en: "The 60th birthday (還甲).",                           cat: "Vocabulary" },
    { kr: "회갑.",         en: "The 60th birthday (回甲).",                           cat: "Vocabulary" },
    { kr: "아마.",         en: "Probably; maybe; perhaps.",                            cat: "Vocabulary" },
    { kr: "-으면.",        en: "If; in case; when (consonant-ending stem; vowel-ending drops 으 → -면).", cat: "Grammar" },
    { kr: "~이랑.",        en: "With; and (after consonant).",                        cat: "Grammar" },
    { kr: "~랑.",          en: "With; and (after vowel).",                            cat: "Grammar" },
  ],
};

// Tag every phrase with its source list name (__list) so any flattened
// view of the data (Quiz, Review, etc.) can trace a word back to which
// list it came from — needed for per-list Review eligibility.
Object.entries(LISTS).forEach(([listName, phrases]) => {
  phrases.forEach(phrase => { phrase.__list = listName; });
});

// ── STORIES DATA ──────────────────────────────────────────────────────────
const STORIES = {
  "1.4C — Directions to the Dormitory": {
    list: "1.4 C",
    sentences: [
      { speaker: "남", kr: "학교 정문에서 육군 기숙사에 어떻게 가요?" },
      { speaker: "여", kr: "체육관을 아세요?" },
      { speaker: "남", kr: "아니요, 잘 모릅니다." },
      { speaker: "여", kr: "그럼, 저 길에서 오른쪽으로 도세요." },
      { speaker: "남", kr: "아, 저 사거리에서 우회전해요?" },
      { speaker: "여", kr: "네. 그리고 쭉 가세요." },
      { speaker: "여", kr: "그럼 왼쪽에 큰 노란색 건물이 나와요." },
      { speaker: "여", kr: "그 건물이 학생 식당이에요." },
      { speaker: "남", kr: "네." },
      { speaker: "여", kr: "학생 식당 바로 뒤에 기숙사가 있어요." },
      { speaker: "여", kr: "첫 번째 건물이 육군 기숙사입니다." },
      { speaker: "남", kr: "감사합니다." },
    ]
  },
  "2.6A — The Four Seasons of Korea": {
    list: "2.6 A",
    sentences: [
      { speaker: null, kr: "한국은 봄, 여름, 가을, 겨울, 사계절이 있습니다." },
      { speaker: null, kr: "봄에는 날씨가 따뜻합니다." },
      { speaker: null, kr: "가끔 바람이 불고 비도 조금 내립니다." },
      { speaker: null, kr: "여름은 아주 덥습니다." },
      { speaker: null, kr: "구름이 많이 끼고 비가 자주 옵니다." },
      { speaker: null, kr: "가을에는 조금 쌀쌀합니다." },
      { speaker: null, kr: "하늘이 높고 맑습니다." },
      { speaker: null, kr: "겨울은 춥고 눈이 자주 옵니다." },
    ]
  }
};

// ── GLOBAL STATE ──────────────────────────────────────────────────────────
let mode         = 'study';
let selectedLists = new Set();

let quizQueue        = [];
let quizIndex        = 0;
let quizScore        = 0;
let quizResults      = [];
let phraseStatus     = new Map();
let currentQuizPhrases = [];
let currentRoundKeys = new Set(); // kr keys in the current round — used to detect true first attempts

let lockedSyls    = [];
let currentPhrase = null;
let composingChar = '';
let submittedWrong   = false;
let submissionLocked = false; // true during the advance delay after a correct answer

// ── STORIES STATE ─────────────────────────────────────────────────────────
let currentStoryKey  = null;   // key into STORIES
let storySubMode     = 'read'; // 'read' | 'drill' | 'mask'
let storySentenceIdx = 0;      // current sentence index during drill/mask
let storyResults     = [];     // { idx, correct } per sentence this pass
let maskPassNumber   = 1;      // which progressive mask pass we're on
let maskPassWords    = new Map(); // sentence-index -> Set of word indices masked this pass

// Chapter selector state — tracks which chapter groups are expanded
let openChapters = new Set(); // e.g. Set { "1.1", "2.7" }

// ── PRONOUNCE MODE STATE ─────────────────────────────────────────────────
const PRONOUNCE_PROXY_URL = "https://korean-pronunciation-proxy.cgmn9jdtsh.workers.dev";
let pronouncePhrases  = [];
let pronounceIndex    = 0;
let pronouncePhrase   = null;
let pronounceQueue    = [];           // shuffled indices into pronouncePhrases
let pronounceQueuePos = 0;            // position within pronounceQueue
let pronounceScores   = new Map();    // kr -> best score seen this round
let allPronouncePhrases = [];         // full original set, preserved across retry rounds
let mediaRecorder     = null;
let recordedChunks    = [];
let isRecording       = false;

// ── HELPERS ───────────────────────────────────────────────────────────────
const PUNCT = new Set(['.', '?', '!', ',', '·', '~', '+', '-', '/', '#']);

function isHangulSyllable(ch) {
  const c = ch.codePointAt(0);
  return c >= 0xAC00 && c <= 0xD7A3;
}

function getSyllables(str) {
  return [...str].filter(isHangulSyllable);
}

// Tokenize a Korean phrase into {type, char} tokens for rendering.
// Types: 'syl' | 'space' | 'punct'
function tokenize(kr) {
  const tokens = [];
  for (const ch of kr) {
    if (isHangulSyllable(ch))   tokens.push({ type: 'syl',   char: ch });
    else if (ch === ' ')        tokens.push({ type: 'space', char: ch });
    else if (PUNCT.has(ch))     tokens.push({ type: 'punct', char: ch });
    // ignore anything else (e.g. stray chars)
  }
  return tokens;
}

function getActivePhrases() {
  return Object.entries(LISTS)
    .filter(([k]) => selectedLists.has(k))
    .flatMap(([, v]) => v);
}

function shuffle(arr) {
  const a = [...arr];
  for (let i = a.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1));
    [a[i], a[j]] = [a[j], a[i]];
  }
  return a;
}

function resetInputState(phrase) {
  currentPhrase    = phrase;
  composingChar    = '';
  committedSyls    = [];
  isComposing      = false;
  submissionLocked = false;
  lockedSyls       = getSyllables(phrase.kr).map(() => null);
}

// ── LIST SELECTOR ─────────────────────────────────────────────────────────
function renderListSelector() {
  const container = document.getElementById('listSelector');
  const label     = document.getElementById('selectorLabel');
  if (!container) return;
  label.textContent = mode === 'study' ? 'Lists' : mode === 'quiz' ? 'Quiz list' : 'Practice list';

  const chapters = {};
  Object.keys(LISTS).forEach(name => {
    const chapter = name.slice(0, 3);
    if (!chapters[chapter]) chapters[chapter] = [];
    chapters[chapter].push(name);
  });

  const chapterKeys   = Object.keys(chapters);
  const activeChapter = [...openChapters].find(c => chapters[c]) || null;

  // Each chapter gets its own row: [tab] [A] [B] [C] (chips only on active row)
  container.innerHTML = '<div class="chapter-tab-strip">' +
    chapterKeys.map(chapter => {
      const anySelected = chapters[chapter].some(n => selectedLists.has(n));
      const isActive    = chapter === activeChapter;
      const dot         = anySelected ? '<span class="tab-dot"></span>' : '';
      const tabBtn      = '<button class="chapter-tab ' + (isActive ? 'active' : '') +
        '" onclick="toggleChapter(\'' + chapter + '\')">' + chapter + dot + '</button>';
      const chips       = isActive
        ? '<div class="chapter-chip-picker">' +
          chapters[chapter].map(name => {
            const isSel  = selectedLists.has(name);
            const letter = name.slice(4);
            return '<button class="list-chip ' + (isSel ? 'selected' : '') +
              '" onclick="toggleList(\'' + name + '\')">' + letter + '</button>';
          }).join('') + '</div>'
        : '';
      return '<div class="chapter-row">' + tabBtn + chips + '</div>';
    }).join('') +
  '</div>';

  renderSelectedChipsBar();
}

function renderSelectedChipsBar() {
  const bar = document.getElementById('selectedChipsBar');
  if (!bar) return;
  const selected = [...selectedLists];
  if (selected.length === 0) { bar.style.display = 'none'; return; }
  bar.style.display = 'flex';
  bar.innerHTML = selected.map(name =>
    `<span class="selected-chip">${name}</span>`
  ).join('') + `<button class="clear-lists-btn" onclick="clearAllLists()" title="Clear all">✕</button>`;
}

function toggleChapter(chapter) {
  // Tapping the active chapter collapses it; tapping another opens it
  if (openChapters.has(chapter)) {
    openChapters.clear();
  } else {
    openChapters.clear();
    openChapters.add(chapter);
  }
  renderListSelector();
}

function toggleList(name) {
  if (selectedLists.has(name)) selectedLists.delete(name);
  else selectedLists.add(name);
  renderListSelector();
  if (mode === 'study') renderStudy();
  else if (mode === 'quiz') startQuiz();
  else startPronounce();
}

function clearAllLists() {
  selectedLists.clear();
  openChapters.clear();
  renderListSelector();
  if (mode === 'study') renderStudy();
  else if (mode === 'quiz') startQuiz();
  else if (mode === 'pronounce') startPronounce();
}

// ── MODE SWITCH ───────────────────────────────────────────────────────────
function switchMode(m) {
  mode = m;
  document.getElementById('tabStudy').classList.toggle('active', m === 'study');
  document.getElementById('tabQuiz').classList.toggle('active',  m === 'quiz');
  document.getElementById('tabPronounce').classList.toggle('active', m === 'pronounce');
  document.getElementById('tabReview').classList.toggle('active', m === 'review');
  document.getElementById('tabStories').classList.toggle('active', m === 'stories');
  document.getElementById('scoreDisplay').textContent = '';

  // Review and Stories hide the sidebar since they operate globally
  const sidebar = document.getElementById('sidebar');
  if (sidebar) sidebar.style.display = (m === 'review' || m === 'stories') ? 'none' : '';

  if (m === 'study')    { renderListSelector(); renderStudy(); }
  else if (m === 'quiz')     { renderListSelector(); startQuiz(); }
  else if (m === 'pronounce') { renderListSelector(); startPronounce(); }
  else if (m === 'review')   { startReview(); }
  else                       { renderStorySelector(); }
}

// ── STUDY MODE ────────────────────────────────────────────────────────────
function renderStudy() {
  const area      = document.getElementById('mainArea');
  const listNames = Object.keys(LISTS).filter(k => selectedLists.has(k));
  if (listNames.length === 0) {
    area.innerHTML = `<div style="text-align:center;padding:60px 0;color:#bbb;font-size:15px;letter-spacing:1px;">Select a set to begin.</div>`;
    return;
  }
  area.innerHTML = listNames.map(listName => {
    const phrases = LISTS[listName];
    return `
      <div class="study-section">
        <div class="study-section-title">${listName}</div>
        ${phrases.map(p => `
          <div class="study-row">
            <span class="study-kr">${p.kr}</span>
            <span class="study-en">${p.en}</span>
          </div>
        `).join('')}
      </div>
    `;
  }).join('');
}

// ── BLOCK HTML BUILDER ────────────────────────────────────────────────────
// Builds the static HTML for the blocks row from a phrase string.
// Returns { html, sylCount } — html has syl-block, block-space, block-punct elements.
// syl-blocks are indexed by their syllable order (id="block-N").
function buildBlocksHTML(kr) {
  const tokens = tokenize(kr);
  let sylIdx = 0;
  const parts = tokens.map(tok => {
    if (tok.type === 'syl') {
      return `<div class="syl-block empty" id="block-${sylIdx++}"></div>`;
    } else if (tok.type === 'space') {
      return `<div class="block-space"></div>`;
    } else {
      return `<span class="block-punct">${tok.char}</span>`;
    }
  });
  return { html: parts.join(''), sylCount: sylIdx };
}

// ── BLOCK UPDATER ─────────────────────────────────────────────────────────
// typedSyls: array of hangul syllable chars from input.value (unlocked only).
// state: 'typing' | 'correct' | 'wrong'
function updateBlocks(typedSyls, state) {
  const syllables = getSyllables(currentPhrase.kr);
  let typedCursor = 0;

  // Find composing block index (absolute syl index of next empty unlocked slot)
  let composingBlockIdx = -1;
  if (state === 'typing' && composingChar) {
    let unlockedSeen = 0;
    for (let i = 0; i < syllables.length; i++) {
      if (lockedSyls[i] !== null) continue;
      if (unlockedSeen === typedSyls.length) { composingBlockIdx = i; break; }
      unlockedSeen++;
    }
  }

  syllables.forEach((targetCh, i) => {
    const block = document.getElementById('block-' + i);
    if (!block) return;

    // Locked always wins
    if (lockedSyls[i] !== null) {
      block.textContent = lockedSyls[i];
      block.className   = 'syl-block locked';
      return;
    }

    if (state === 'correct') {
      block.textContent = targetCh;
      block.className   = 'syl-block correct pop';
      return;
    }

    const typedChar = typedSyls[typedCursor];
    typedCursor++;

    if (state === 'wrong') {
      block.textContent = typedChar || '';
      block.className   = typedChar ? 'syl-block wrong' : 'syl-block empty wrong';
      return;
    }

    // Typing state
    if (i === composingBlockIdx) {
      block.textContent = composingChar;
      block.className   = 'syl-block active-block';
    } else if (typedChar) {
      const isActive    = (typedCursor - 1 === typedSyls.length - 1) && !composingChar;
      block.textContent = typedChar;
      block.className   = isActive ? 'syl-block active-block' : 'syl-block filled';
    } else {
      block.textContent = '';
      block.className   = 'syl-block empty';
    }
  });
}

// ── INPUT HANDLING ────────────────────────────────────────────────────────
// We never read input.value during an active composition session on macOS,
// because the value is unreliable mid-composition (syllables may appear
// reordered or partially re-opened). Instead we maintain committedSyls
// ourselves, only syncing from input.value on compositionend and plain input.
let committedSyls = []; // syllables confirmed committed (not mid-composition)
let isComposing   = false;

function attachInput() {
  const input = document.getElementById('hiddenInput');
  if (!input) return;
  input.value   = '';
  committedSyls = [];
  isComposing   = false;

  input.addEventListener('compositionstart', () => {
    isComposing   = true;
    composingChar = '';
  });

  input.addEventListener('compositionupdate', e => {
    composingChar = e.data || '';
    // Display committed syls + live composing char preview
    updateBlocks(committedSyls, 'typing');
  });

  input.addEventListener('compositionend', e => {
    isComposing   = false;
    composingChar = '';
    // On compositionend, input.value is now reliable — sync committedSyls from it
    committedSyls = getSyllables(input.value);
    updateBlocks(committedSyls, 'typing');
    clearWrongFeedback();
  });

  input.addEventListener('input', () => {
    if (isComposing) return; // handled by compositionupdate/end
    composingChar = '';
    committedSyls = getSyllables(input.value);
    updateBlocks(committedSyls, 'typing');
    clearWrongFeedback();
  });

  input.addEventListener('keydown', e => {
    if (e.key === 'Enter') { e.preventDefault(); submitAnswer(); }
  });

  const area = document.getElementById('blocksArea');
  if (area) area.addEventListener('click', () => input.focus({ preventScroll: true }));

  const hint = document.getElementById('tapHint');
  if (hint) {
    input.addEventListener('focus', () => hint.style.opacity = '0');
    input.addEventListener('blur',  () => hint.style.opacity = '1');
  }
  input.focus({ preventScroll: true });
}

function clearWrongFeedback() {
  const fb = document.getElementById('feedbackLine');
  if (fb && fb.dataset.state === 'wrong') { fb.textContent = ''; fb.className = 'feedback'; fb.dataset.state = ''; }
}

// ── SUBMIT ────────────────────────────────────────────────────────────────
function submitAnswer() {
  if (submissionLocked) return; // guard: ignore during advance delay after correct answer
  const input = document.getElementById('hiddenInput');
  if (!input) return;
  if (!input) return;
  const typedSyls = committedSyls.slice(); // use our tracked committed state
  if (typedSyls.length === 0) return;

  const phrase     = currentPhrase;
  const targetSyls = getSyllables(phrase.kr);

  // Merge locked + typed into full answer
  let cur = 0;
  const fullSyls = targetSyls.map((_, i) => {
    if (lockedSyls[i] !== null) return lockedSyls[i];
    return typedSyls[cur++] || '';
  });
  const isCorrect = fullSyls.join('') === targetSyls.join('');

  if (isCorrect) {
    updateBlocks(typedSyls, 'correct');
    const fb = document.getElementById('feedbackLine');
    if (fb) { fb.textContent = '✓ 맞았어요!'; fb.className = 'feedback correct'; fb.dataset.state = 'correct'; }
    quizScore++;
    const isFirst = currentRoundKeys.has(phrase.kr);
    phraseStatus.set(phrase.kr, { phrase, status: isFirst ? 'first' : 'corrected' });
    currentRoundKeys.delete(phrase.kr);
    if (isFirst) recordSrsAttempt(phrase.kr, true);
    document.getElementById('scoreDisplay').textContent = `★ ${quizScore}`;

    // Lock out further submissions during the advance delay without
    // touching input.disabled (which causes iOS Safari to dismiss the keyboard).
    submissionLocked = true;
    setTimeout(() => advanceQuiz(), 1000);
  } else {
    submittedWrong = true;
    const newLocked = [...lockedSyls];
    let j = 0;
    targetSyls.forEach((targetCh, i) => {
      if (newLocked[i] !== null) return;
      if (typedSyls[j] === targetCh) newLocked[i] = targetCh;
      j++;
    });
    lockedSyls = newLocked;
    updateBlocks(typedSyls, 'wrong');
    const fb = document.getElementById('feedbackLine');
    if (fb) { fb.textContent = '✗ 다시 해 보세요.'; fb.className = 'feedback wrong'; fb.dataset.state = 'wrong'; }
    // Mark as missed and remove from currentRoundKeys so a subsequent correct answer knows it was attempted this round
    const wasFirstAttempt = currentRoundKeys.has(phrase.kr);
    currentRoundKeys.delete(phrase.kr);
    phraseStatus.set(phrase.kr, { phrase, status: 'missed' });
    // Only the FIRST wrong attempt on this question counts toward SRS —
    // subsequent wrong retries on the same question don't further penalize.
    if (wasFirstAttempt) recordSrsAttempt(phrase.kr, false);
    setTimeout(() => {
      input.value   = '';
      committedSyls = [];
      composingChar = '';
      updateBlocks([], 'typing');
      input.focus({ preventScroll: true });
    }, 500);
  }
}

// ── QUIZ MODE ─────────────────────────────────────────────────────────────
function startQuiz() {
  const phrases = getActivePhrases();
  if (phrases.length === 0) {
    document.getElementById('scoreDisplay').textContent = '';
    document.getElementById('mainArea').innerHTML = `<div style="text-align:center;padding:60px 0;color:#bbb;font-size:15px;letter-spacing:1px;">Select a set to begin.</div>`;
    return;
  }
  currentQuizPhrases = phrases;
  currentRoundKeys   = new Set(phrases.map(p => p.kr));
  quizQueue     = shuffle(phrases.map((_, i) => i));
  quizIndex     = 0;
  quizScore     = 0;
  quizResults   = []; // phraseStatus entries added in order of encounter
  phraseStatus  = new Map();
  document.getElementById('scoreDisplay').textContent = `★ 0`;
  renderQuiz(phrases);
}

// ── REVIEW (SPACED REPETITION) MODE ──────────────────────────────────────
// Pulls due words from ACROSS ALL lists (ignoring the list selector, since
// spaced repetition is a global concept) and runs them through the same
// Quiz UI/scoring machinery. Falls back to a friendly empty state if
// nothing is due right now.
async function startReview() {
  const allPhrases = Object.values(LISTS).flat();
  document.getElementById('mainArea').innerHTML = `<div style="text-align:center;padding:60px 0;color:var(--muted-2);font-size:15px;letter-spacing:1px;">Checking what's due…</div>`;

  const duePhrases = await getDuePhrases(allPhrases);

  if (duePhrases.length === 0) {
    document.getElementById('scoreDisplay').textContent = '';
    document.getElementById('mainArea').innerHTML = `
      <div style="text-align:center;padding:60px 0;">
        <div style="font-size:40px;margin-bottom:12px">🎉</div>
        <div style="color:var(--muted);font-size:15px;letter-spacing:0.5px;">Nothing due for review right now.<br>Come back later, or practice a list directly.</div>
      </div>`;
    return;
  }

  currentQuizPhrases = duePhrases;
  currentRoundKeys   = new Set(duePhrases.map(p => p.kr));
  quizQueue     = shuffle(duePhrases.map((_, i) => i));
  quizIndex     = 0;
  quizScore     = 0;
  quizResults   = [];
  phraseStatus  = new Map();
  document.getElementById('scoreDisplay').textContent = `★ 0`;
  renderQuiz(duePhrases);
}

// Updates the small due-count badge on the Review tab. Called on init and
// after any Quiz/Review session completes, so the count stays current.
async function refreshReviewBadge() {
  const badge = document.getElementById('reviewDueBadge');
  if (!badge) return;
  const allPhrases = Object.values(LISTS).flat();
  const due = await getDuePhrases(allPhrases);
  if (due.length > 0) {
    badge.textContent = due.length;
    badge.style.display = 'inline-flex';
  } else {
    badge.style.display = 'none';
  }
}

function speak(text) {
  if (!window.speechSynthesis) return;
  // Strip punctuation chars before speaking
  const clean = [...text].filter(ch => {
    const c = ch.codePointAt(0);
    return (c >= 0xAC00 && c <= 0xD7A3) || ch === ' ';
  }).join('');
  if (!clean) return;
  window.speechSynthesis.cancel();
  const utt  = new SpeechSynthesisUtterance(clean);
  utt.lang   = 'ko-KR';
  utt.rate   = 0.9;
  // Prefer a Korean voice if available
  const voices = window.speechSynthesis.getVoices();
  const krVoice = voices.find(v => v.lang.startsWith('ko'));
  if (krVoice) utt.voice = krVoice;
  window.speechSynthesis.speak(utt);
}

function renderQuiz(phrases) {
  phrases = phrases || currentQuizPhrases;
  if (quizIndex >= quizQueue.length) { renderEndScreen(); return; }

  const phrase       = phrases[quizQueue[quizIndex]];
  resetInputState(phrase);
  submittedWrong     = false;
  const pct          = Math.round((quizIndex / quizQueue.length) * 100);
  const { html: blocksHTML } = buildBlocksHTML(phrase.kr);

  document.getElementById('mainArea').innerHTML = `
    <div class="progress-wrap">
      <div class="progress-meta">
        <span>${quizIndex} / ${quizQueue.length}</span>
        <span>★ ${quizScore}</span>
      </div>
      <div class="progress-track"><div class="progress-fill" style="width:${pct}%"></div></div>
    </div>

    <div class="category-label">${phrase.cat}</div>

    <div class="prompt-card">
      <div class="meaning-text">${phrase.en}</div>
    </div>

    <div class="blocks-area" id="blocksArea">
      <input class="hidden-input" id="hiddenInput" type="text"
        autocomplete="off" autocorrect="off" autocapitalize="off" spellcheck="false">
      ${phrase.prefix ? `<div style="font-family:'Gowun Dodum',sans-serif;font-size:18px;color:var(--muted);margin-bottom:6px">${phrase.prefix}</div>` : ''}
      <div class="blocks-row">${blocksHTML}</div>
      <div class="tap-hint" id="tapHint">tap to type</div>
    </div>

    <div class="submit-row">
      <button class="submit-btn" onclick="submitAnswer()">확인 ↵</button>
    </div>

    <div class="feedback" id="feedbackLine"></div>

    <div class="action-row">
      <button class="ghost-btn" onclick="skipQuiz()">Skip →</button>
      <button class="ghost-btn" onclick="speak(currentPhrase.kr)">🔊 Listen</button>
    </div>
  `;
  attachInput();
}

function advanceQuiz() { quizIndex++; renderQuiz(); }

function skipQuiz() {
  const phrase = currentPhrase;
  if (!phraseStatus.has(phrase.kr)) phraseStatus.set(phrase.kr, { phrase, status: 'missed' });
  advanceQuiz();
}

// ── END SCREEN ────────────────────────────────────────────────────────────
function renderEndScreen() {
  const total     = quizQueue.length;
  const pct       = Math.round((quizScore / total) * 100);
  const stampText = pct === 100 ? '만점!' : pct >= 70 ? '잘했어요' : '다시 해봐요';

  const first     = [];
  const corrected = [];
  const missed    = [];

  // Preserve encounter order using quizQueue
  const phrases = currentQuizPhrases;
  quizQueue.forEach(i => {
    const phrase = phrases[i];
    const entry  = phraseStatus.get(phrase.kr);
    if (!entry || entry.status === 'missed') missed.push(phrase);
    else if (entry.status === 'corrected')   corrected.push(phrase);
    else                                     first.push(phrase);
  });

  const retryPhrases = [...corrected, ...missed];
  const allMastered  = retryPhrases.length === 0;

  // A completed Quiz round (not Review) marks every list represented in
  // this round as eligible for Review going forward. Review-mode
  // completions don't re-trigger this, since that would be circular.
  if (mode === 'quiz') {
    const listsInRound = new Set(phrases.map(p => p.__list).filter(Boolean));
    listsInRound.forEach(listName => markListAutoStarted(listName));
  }

  function bucket(label, items, color) {
    if (items.length === 0) return '';
    return `
      <div style="margin-bottom:20px">
        <div style="font-size:11px;font-weight:700;letter-spacing:1.5px;text-transform:uppercase;color:${color};margin-bottom:6px">${label}</div>
        ${items.map(p => `
          <div class="breakdown-row">
            <span class="kr">${p.kr}</span>
            <span class="en">${p.en}</span>
          </div>
        `).join('')}
      </div>
    `;
  }

  document.getElementById('mainArea').innerHTML = `
    <div class="end-screen">
      <div class="stamp">${stampText}</div>
      <div class="end-score">${quizScore} / ${total}</div>
      <div class="end-label">${pct}% correct</div>
      <div class="end-breakdown">
        ${bucket('✓ First attempt', first, 'var(--correct)')}
        ${bucket('◎ Corrected', corrected, 'var(--gold)')}
        ${bucket('✗ Missed', missed, 'var(--red-stamp)')}
      </div>
      <div style="display:flex;gap:10px;justify-content:center;flex-wrap:wrap">
        ${!allMastered ? `<button class="submit-btn" onclick="startRetry()">Practice missed &amp; corrected →</button>` : ''}
        <button class="restart-btn" onclick="${mode === 'review' ? 'startReview()' : 'startQuiz()'}">다시 하기 — Full restart</button>
      </div>
    </div>
  `;
  document.getElementById('scoreDisplay').textContent = '';
  refreshReviewBadge();
}

function startRetry() {
  const phrases  = currentQuizPhrases;
  const retrySet = [];

  quizQueue.forEach(i => {
    const phrase = phrases[i];
    const entry  = phraseStatus.get(phrase.kr);
    if (!entry || entry.status === 'missed' || entry.status === 'corrected') {
      retrySet.push(phrase);
    }
  });

  if (retrySet.length === 0) return;

  // Reset scores but keep phraseStatus — mastered words stay mastered.
  // Downgrade 'corrected' back to 'missed' so they must be first-attempt this round.
  retrySet.forEach(p => {
    phraseStatus.set(p.kr, { phrase: p, status: 'missed' });
  });

  currentQuizPhrases = retrySet;
  currentRoundKeys   = new Set(retrySet.map(p => p.kr));
  quizQueue  = shuffle(retrySet.map((_, i) => i));
  quizIndex  = 0;
  quizScore  = 0;
  document.getElementById('scoreDisplay').textContent = `★ 0`;
  renderQuiz(retrySet);
}

// ── SPACED REPETITION STATS (IndexedDB) ──────────────────────────────────
// Tracks per-word Quiz (spelling) performance across sessions to power a
// lightweight Leitner-box style spaced repetition system. Separate database
// from the recordings store, so a schema change to one never affects the
// other. Keyed by the Korean phrase text.
//
// Box levels and their "due again after" gaps (in days). Box 0 is brand new
// / just got it wrong — due immediately. Higher boxes mean better-known
// words that surface less often. Box 5 is treated as "mastered" and is
// reviewed only occasionally as a long-term check.
const SRS_DB_NAME    = 'korean-game-srs';
const SRS_DB_VERSION = 2; // bumped to add the list-eligibility store
const SRS_STORE      = 'wordStats';
const LIST_ELIGIBILITY_STORE = 'listEligibility'; // key = list name, value = { autoStarted: bool, manualOverride: 'on'|'off'|null }
const SRS_BOX_GAPS_DAYS = [0, 1, 3, 7, 14, 30]; // index = box level
const SRS_MAX_BOX = SRS_BOX_GAPS_DAYS.length - 1;

let srsDbPromise = null;

function openSrsDb() {
  if (srsDbPromise) return srsDbPromise;
  srsDbPromise = new Promise((resolve) => {
    if (!window.indexedDB) { resolve(null); return; }
    const req = indexedDB.open(SRS_DB_NAME, SRS_DB_VERSION);
    req.onupgradeneeded = () => {
      const db = req.result;
      if (!db.objectStoreNames.contains(SRS_STORE)) {
        db.createObjectStore(SRS_STORE); // key = phrase.kr
      }
      if (!db.objectStoreNames.contains(LIST_ELIGIBILITY_STORE)) {
        db.createObjectStore(LIST_ELIGIBILITY_STORE); // key = list name
      }
    };
    req.onsuccess = () => resolve(req.result);
    req.onerror   = () => resolve(null); // fail soft — SRS simply won't persist
  });
  return srsDbPromise;
}

function defaultSrsRecord() {
  return {
    box: 0,
    timesCorrect: 0,
    timesWrong: 0,
    lastSeen: null,     // ISO date string
    nextDue: null,       // ISO date string, null = due now (never reviewed)
  };
}

async function getSrsRecord(kr) {
  const db = await openSrsDb();
  if (!db) return defaultSrsRecord();
  return new Promise((resolve) => {
    const tx = db.transaction(SRS_STORE, 'readonly');
    const req = tx.objectStore(SRS_STORE).get(kr);
    req.onsuccess = () => resolve(req.result || defaultSrsRecord());
    req.onerror   = () => resolve(defaultSrsRecord());
  });
}

async function getAllSrsRecords() {
  const db = await openSrsDb();
  if (!db) return new Map();
  return new Promise((resolve) => {
    const tx = db.transaction(SRS_STORE, 'readonly');
    const store = tx.objectStore(SRS_STORE);
    const result = new Map();
    const cursorReq = store.openCursor();
    cursorReq.onsuccess = (e) => {
      const cursor = e.target.result;
      if (cursor) {
        result.set(cursor.key, cursor.value);
        cursor.continue();
      } else {
        resolve(result);
      }
    };
    cursorReq.onerror = () => resolve(result);
  });
}

async function recordSrsAttempt(kr, wasCorrect) {
  const db = await openSrsDb();
  if (!db) return;
  const existing = await getSrsRecord(kr);

  const record = { ...existing };
  if (wasCorrect) {
    record.timesCorrect = (record.timesCorrect || 0) + 1;
    record.box = Math.min(SRS_MAX_BOX, (record.box || 0) + 1);
  } else {
    record.timesWrong = (record.timesWrong || 0) + 1;
    record.box = 0;
  }
  const now = new Date();
  record.lastSeen = now.toISOString();
  const gapDays = SRS_BOX_GAPS_DAYS[record.box];
  const due = new Date(now.getTime() + gapDays * 24 * 60 * 60 * 1000);
  record.nextDue = due.toISOString();

  return new Promise((resolve) => {
    const tx = db.transaction(SRS_STORE, 'readwrite');
    tx.objectStore(SRS_STORE).put(record, kr);
    tx.oncomplete = () => resolve(record);
    tx.onerror    = () => resolve(record);
  });
}

// Returns the list of phrases (from the given full phrase pool) that are
// currently due for review: either never attempted, or whose nextDue date
// has passed. Only considers phrases from lists currently eligible for
// Review (see list-eligibility functions below).
async function getDuePhrases(allPhrases) {
  const records = await getAllSrsRecords();
  const eligibleListNames = await getEligibleListNames();
  const now = new Date();
  return allPhrases.filter(phrase => {
    if (!eligibleListNames.has(phrase.__list)) return false;
    const rec = records.get(phrase.kr);
    if (!rec || !rec.nextDue) return true; // never attempted — due now
    return new Date(rec.nextDue) <= now;
  });
}

// ── LIST ELIGIBILITY FOR REVIEW MODE ─────────────────────────────────────
// A list only feeds into Review mode once it's "eligible" — by default,
// that means you've completed at least one full Quiz round on it (reaching
// the end screen, not just answering a question). This is auto-detected
// and stored per list, but can be manually overridden either direction
// from the settings panel, in case the automatic signal is wrong (e.g. an
// accidental quiz completion, or wanting to opt a list in/out manually).
async function getListEligibilityRecord(listName) {
  const db = await openSrsDb();
  if (!db) return { autoStarted: false, manualOverride: null };
  return new Promise((resolve) => {
    const tx = db.transaction(LIST_ELIGIBILITY_STORE, 'readonly');
    const req = tx.objectStore(LIST_ELIGIBILITY_STORE).get(listName);
    req.onsuccess = () => resolve(req.result || { autoStarted: false, manualOverride: null });
    req.onerror   = () => resolve({ autoStarted: false, manualOverride: null });
  });
}

async function getAllListEligibilityRecords() {
  const db = await openSrsDb();
  if (!db) return new Map();
  return new Promise((resolve) => {
    const tx = db.transaction(LIST_ELIGIBILITY_STORE, 'readonly');
    const store = tx.objectStore(LIST_ELIGIBILITY_STORE);
    const result = new Map();
    const cursorReq = store.openCursor();
    cursorReq.onsuccess = (e) => {
      const cursor = e.target.result;
      if (cursor) { result.set(cursor.key, cursor.value); cursor.continue(); }
      else resolve(result);
    };
    cursorReq.onerror = () => resolve(result);
  });
}

// Marks a list as "auto-started" — called when a full Quiz round completes
// for phrases belonging to that list. Does not override an existing manual
// setting; it only sets the underlying automatic flag.
async function markListAutoStarted(listName) {
  const db = await openSrsDb();
  if (!db) return;
  const existing = await getListEligibilityRecord(listName);
  if (existing.autoStarted) return; // already set, nothing to do
  const updated = { ...existing, autoStarted: true };
  return new Promise((resolve) => {
    const tx = db.transaction(LIST_ELIGIBILITY_STORE, 'readwrite');
    tx.objectStore(LIST_ELIGIBILITY_STORE).put(updated, listName);
    tx.oncomplete = () => resolve();
    tx.onerror    = () => resolve();
  });
}

// Sets a manual override for a list: 'on' forces it eligible, 'off' forces
// it ineligible, null clears the override and falls back to the automatic
// flag.
async function setListManualOverride(listName, value) {
  const db = await openSrsDb();
  if (!db) return;
  const existing = await getListEligibilityRecord(listName);
  const updated = { ...existing, manualOverride: value };
  return new Promise((resolve) => {
    const tx = db.transaction(LIST_ELIGIBILITY_STORE, 'readwrite');
    tx.objectStore(LIST_ELIGIBILITY_STORE).put(updated, listName);
    tx.oncomplete = () => resolve();
    tx.onerror    = () => resolve();
  });
}

// Returns a Set of list names currently eligible for Review: manual
// override wins if set, otherwise falls back to the auto-started flag.
async function getEligibleListNames() {
  const allNames = Object.keys(LISTS);
  const records = await getAllListEligibilityRecords();
  const eligible = new Set();
  allNames.forEach(name => {
    const rec = records.get(name);
    if (!rec) return; // never touched — not eligible by default
    const isEligible = rec.manualOverride === 'on' ? true
      : rec.manualOverride === 'off' ? false
      : !!rec.autoStarted;
    if (isEligible) eligible.add(name);
  });
  return eligible;
}


// Persists the user's best (90+) pronunciation recording per phrase across
// browser sessions, so they can play back their own voice later from the
// Pronounce summary screen. Keyed by the Korean phrase text.
const RECORDING_DB_NAME    = 'korean-game-recordings';
const RECORDING_DB_VERSION = 1;
const RECORDING_STORE      = 'bestRecordings';
let recordingDbPromise = null;

function openRecordingDb() {
  if (recordingDbPromise) return recordingDbPromise;
  recordingDbPromise = new Promise((resolve, reject) => {
    if (!window.indexedDB) { resolve(null); return; }
    const req = indexedDB.open(RECORDING_DB_NAME, RECORDING_DB_VERSION);
    req.onupgradeneeded = () => {
      const db = req.result;
      if (!db.objectStoreNames.contains(RECORDING_STORE)) {
        db.createObjectStore(RECORDING_STORE); // key = phrase.kr, value = { blob, score }
      }
    };
    req.onsuccess = () => resolve(req.result);
    req.onerror   = () => resolve(null); // fail soft — playback simply won't be available
  });
  return recordingDbPromise;
}

async function saveBestRecording(kr, score, blob) {
  const db = await openRecordingDb();
  if (!db) return;
  return new Promise((resolve) => {
    const tx = db.transaction(RECORDING_STORE, 'readwrite');
    tx.objectStore(RECORDING_STORE).put({ score, blob }, kr);
    tx.oncomplete = () => resolve();
    tx.onerror    = () => resolve();
  });
}

async function getBestRecording(kr) {
  const db = await openRecordingDb();
  if (!db) return null;
  return new Promise((resolve) => {
    const tx = db.transaction(RECORDING_STORE, 'readonly');
    const req = tx.objectStore(RECORDING_STORE).get(kr);
    req.onsuccess = () => resolve(req.result || null);
    req.onerror   = () => resolve(null);
  });
}

async function playSavedRecording(kr) {
  const entry = await getBestRecording(kr);
  if (!entry || !entry.blob) return;
  const url = URL.createObjectURL(entry.blob);
  const audio = new Audio(url);
  audio.play();
  audio.addEventListener('ended', () => URL.revokeObjectURL(url));
}


function startPronounce() {
  const phrases = getActivePhrases();
  if (phrases.length === 0) {
    document.getElementById('scoreDisplay').textContent = '';
    document.getElementById('mainArea').innerHTML = `<div style="text-align:center;padding:60px 0;color:var(--muted-2);font-size:15px;letter-spacing:1px;">Select a set to begin.</div>`;
    return;
  }
  pronouncePhrases  = phrases;
  allPronouncePhrases = phrases;
  pronounceQueue    = shuffle(phrases.map((_, i) => i));
  pronounceQueuePos = 0;
  pronounceScores   = new Map();
  pronouncePhrase   = phrases[pronounceQueue[0]];
  document.getElementById('scoreDisplay').textContent = '';
  renderPronounce();
}

function nextPronounceWord() {
  pronounceQueuePos++;
  if (pronounceQueuePos >= pronounceQueue.length) {
    renderPronounceSummary();
    return;
  }
  pronouncePhrase = pronouncePhrases[pronounceQueue[pronounceQueuePos]];
  renderPronounce();
}

function renderPronounce() {
  const phrase = pronouncePhrase;
  const { html: blocksHTML } = buildBlocksHTML(phrase.kr);
  const pct = Math.round((pronounceQueuePos / pronounceQueue.length) * 100);

  document.getElementById('mainArea').innerHTML = `
    <div class="progress-wrap">
      <div class="progress-meta">
        <span>${pronounceQueuePos} / ${pronounceQueue.length}</span>
      </div>
      <div class="progress-track"><div class="progress-fill" style="width:${pct}%"></div></div>
    </div>

    <div class="category-label">${phrase.cat}</div>

    <div class="prompt-card">
      <div class="meaning-text">${phrase.en}</div>
    </div>

    <div class="pronounce-card">
      ${phrase.prefix ? `<div style="font-family:'Gowun Dodum',sans-serif;font-size:18px;color:var(--muted);margin-bottom:6px;text-align:center">${phrase.prefix}</div>` : ''}
      <div class="blocks-row" id="pronounceBlocks" style="justify-content:center;margin-bottom:24px">${blocksHTML}</div>

      <div class="record-label" id="recordLabel">Tap to record</div>
      <button class="record-btn" id="recordBtn" onclick="toggleRecording()">🎙️</button>

      <div class="pronounce-score" id="pronounceScoreNum" style="display:none"></div>
      <div class="pronounce-score-label" id="pronounceScoreLabel" style="display:none">Overall score</div>

      <div class="pronounce-status" id="pronounceStatus"></div>

      <div class="action-row" style="justify-content:center;margin-top:10px">
        <button class="ghost-btn" onclick="speak(currentKrTarget())">🔊 Listen</button>
        <button class="ghost-btn" onclick="retryPronounceWord()">↻ Retry</button>
        <button class="submit-btn" onclick="nextPronounceWord()">${pronounceQueuePos === pronounceQueue.length - 1 ? 'Finish' : 'Next word'} →</button>
      </div>
    </div>
  `;
}

function retryPronounceWord() {
  // Re-render the same word fresh, so the user can record again
  renderPronounce();
}

function currentKrTarget() {
  return pronouncePhrase ? pronouncePhrase.kr : '';
}

const PRONOUNCE_MASTERY_THRESHOLD = 90;

function renderPronounceSummary() {
  const total = allPronouncePhrases.length;
  const results = allPronouncePhrases.map(phrase => {
    const score = pronounceScores.get(phrase.kr) ?? null; // null = never attempted
    return { phrase, score };
  });

  const mastered = results.filter(r => r.score !== null && r.score >= PRONOUNCE_MASTERY_THRESHOLD);
  const needsPractice = results.filter(r => r.score === null || r.score < PRONOUNCE_MASTERY_THRESHOLD);

  const attempted = results.filter(r => r.score !== null);
  const avgScore = attempted.length > 0
    ? Math.round(attempted.reduce((sum, r) => sum + r.score, 0) / attempted.length)
    : 0;

  const stampText = mastered.length === total ? '만점!' : mastered.length / total >= 0.7 ? '잘했어요' : '다시 해봐요';

  function bucket(label, items, color, withPlayButton) {
    if (items.length === 0) return '';
    return `
      <div style="margin-bottom:20px">
        <div style="font-size:11px;font-weight:700;letter-spacing:1.5px;text-transform:uppercase;color:${color};margin-bottom:6px">${label}</div>
        ${items.map(r => `
          <div class="breakdown-row">
            <span class="kr">${r.phrase.kr}</span>
            <span class="en">${r.phrase.en}</span>
            ${withPlayButton ? `<button class="play-recording-btn" onclick="playSavedRecording('${r.phrase.kr.replace(/'/g, "\\'")}')" title="Play your recording">▶️</button>` : ''}
            <span class="mark" style="font-weight:700;color:${r.score === null ? 'var(--muted-2)' : scoreToColor(r.score)}">${r.score === null ? '—' : r.score}</span>
          </div>
        `).join('')}
      </div>
    `;
  }

  const retryPhrases = needsPractice.map(r => r.phrase);
  const allMastered  = retryPhrases.length === 0;

  document.getElementById('mainArea').innerHTML = `
    <div class="end-screen">
      <div class="stamp">${stampText}</div>
      <div class="end-score">${mastered.length} / ${total}</div>
      <div class="end-label">words mastered (90+) · avg score ${avgScore}</div>
      <div class="end-breakdown">
        ${bucket('✓ Mastered (90+)', mastered, 'var(--correct)', true)}
        ${bucket('◎ Needs practice', needsPractice, 'var(--red-stamp)', false)}
      </div>
      <div style="display:flex;gap:10px;justify-content:center;flex-wrap:wrap">
        ${!allMastered ? `<button class="submit-btn" onclick="retryNeedsPracticePronounce()">Practice needs-practice words →</button>` : ''}
        <button class="restart-btn" onclick="startPronounce()">다시 하기 — Full restart</button>
      </div>
    </div>
  `;
  document.getElementById('scoreDisplay').textContent = '';
}

function retryNeedsPracticePronounce() {
  const needsPractice = allPronouncePhrases.filter(phrase => {
    const score = pronounceScores.get(phrase.kr) ?? null;
    return score === null || score < PRONOUNCE_MASTERY_THRESHOLD;
  });

  if (needsPractice.length === 0) return;

  // Re-queue only the not-yet-mastered phrases. pronounceScores is preserved
  // (not reset) so already-mastered words stay mastered, and any score
  // improvement this round still uses the best-of-attempts rule.
  // allPronouncePhrases is NOT touched, so the summary screen always reflects
  // the full original word set across any number of retry rounds.
  pronouncePhrases  = needsPractice;
  pronounceQueue    = shuffle(needsPractice.map((_, i) => i));
  pronounceQueuePos = 0;
  pronouncePhrase   = pronouncePhrases[pronounceQueue[0]];
  document.getElementById('scoreDisplay').textContent = '';
  renderPronounce();
}

// Map a 0-100 score to a red→yellow→green color
function scoreToColor(score) {
  // Clamp
  const s = Math.max(0, Math.min(100, score));
  // 0 = red (0,  74%, 47%) ... 50 = yellow (45, 90%, 55%) ... 100 = green (145, 55%, 40%)
  let hue;
  if (s <= 50) {
    hue = (s / 50) * 45; // 0 -> 45 (red to yellow)
  } else {
    hue = 45 + ((s - 50) / 50) * 100; // 45 -> 145 (yellow to green)
  }
  return `hsl(${hue}, 70%, 45%)`;
}

function scoreToBgColor(score) {
  const s = Math.max(0, Math.min(100, score));
  let hue;
  if (s <= 50) hue = (s / 50) * 45;
  else hue = 45 + ((s - 50) / 50) * 100;
  return `hsl(${hue}, 70%, 94%)`;
}

async function toggleRecording() {
  if (isRecording) {
    stopRecording();
  } else {
    await startRecording();
  }
}

async function startRecording() {
  const statusEl = document.getElementById('pronounceStatus');
  try {
    const stream = await navigator.mediaDevices.getUserMedia({ audio: true });
    recordedChunks = [];
    mediaRecorder = new MediaRecorder(stream, { mimeType: pickSupportedMimeType() });
    mediaRecorder.addEventListener('dataavailable', e => {
      if (e.data.size > 0) recordedChunks.push(e.data);
    });
    mediaRecorder.addEventListener('stop', () => {
      stream.getTracks().forEach(t => t.stop());
      handleRecordingComplete();
    });
    mediaRecorder.start();
    isRecording = true;
    document.getElementById('recordBtn').classList.add('recording');
    document.getElementById('recordLabel').textContent = 'Recording… tap to stop';
    if (statusEl) statusEl.textContent = '';
  } catch (err) {
    if (statusEl) statusEl.textContent = 'Microphone access denied or unavailable.';
  }
}

function stopRecording() {
  if (mediaRecorder && isRecording) {
    mediaRecorder.stop();
    isRecording = false;
    document.getElementById('recordBtn').classList.remove('recording');
    document.getElementById('recordLabel').textContent = 'Processing…';
  }
}

function pickSupportedMimeType() {
  const candidates = ['audio/webm;codecs=opus', 'audio/webm', 'audio/ogg;codecs=opus', 'audio/mp4'];
  for (const type of candidates) {
    if (window.MediaRecorder && MediaRecorder.isTypeSupported(type)) return type;
  }
  return '';
}

async function handleRecordingComplete() {
  const statusEl = document.getElementById('pronounceStatus');
  const recordLabel = document.getElementById('recordLabel');
  if (recordedChunks.length === 0) {
    if (statusEl) statusEl.textContent = 'No audio captured — try again.';
    if (recordLabel) recordLabel.textContent = 'Tap to record';
    return;
  }

  const blob = new Blob(recordedChunks, { type: recordedChunks[0].type || 'audio/webm' });

  try {
    if (statusEl) statusEl.textContent = 'Converting audio…';
    const wavBlob = await convertBlobToWav16kMono(blob);

    if (statusEl) statusEl.textContent = 'Scoring your pronunciation…';
    const audioBase64 = await blobToBase64(wavBlob);
    const referenceText = pronouncePhrase.kr.replace(/[~+\-.?!,]/g, '').trim();

    const response = await fetch(PRONOUNCE_PROXY_URL, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ referenceText, audioBase64 }),
    });

    const result = await response.json();
    applyPronunciationResult(result, wavBlob);
  } catch (err) {
    if (statusEl) statusEl.textContent = 'Error scoring pronunciation: ' + err.message;
  } finally {
    if (recordLabel) recordLabel.textContent = 'Tap to record';
  }
}

// Decode any browser-recorded audio blob (WebM/Opus, OGG, MP4, etc.) into
// raw PCM, resample to 16kHz mono, and re-encode as a proper WAV file.
// Azure's pronunciation assessment endpoint requires real 16kHz mono PCM WAV —
// sending compressed audio mislabeled as WAV silently breaks scoring even
// though plain transcription may still partially succeed.
async function convertBlobToWav16kMono(blob) {
  const arrayBuffer = await blob.arrayBuffer();

  // Decode the compressed audio into raw PCM using the Web Audio API.
  // Safari requires webkitAudioContext fallback on older versions.
  const AudioCtx = window.AudioContext || window.webkitAudioContext;
  const decodeCtx = new AudioCtx();
  const decodedBuffer = await decodeCtx.decodeAudioData(arrayBuffer.slice(0));
  decodeCtx.close && decodeCtx.close();

  const targetSampleRate = 16000;

  // Resample to 16kHz mono using an OfflineAudioContext, which can render
  // at an arbitrary target sample rate directly.
  const durationSeconds = decodedBuffer.duration;
  const offlineCtx = new OfflineAudioContext(
    1, // mono output
    Math.ceil(durationSeconds * targetSampleRate),
    targetSampleRate
  );

  const source = offlineCtx.createBufferSource();
  source.buffer = decodedBuffer;

  // If the source has multiple channels, downmix to mono via a gain merge.
  // Connecting directly to a 1-channel destination context handles downmixing
  // automatically per the Web Audio spec's channel interpretation rules.
  source.connect(offlineCtx.destination);
  source.start(0);

  const renderedBuffer = await offlineCtx.startRendering();
  const pcmSamples = renderedBuffer.getChannelData(0); // Float32Array, mono, 16kHz

  const wavArrayBuffer = encodeWavPCM16(pcmSamples, targetSampleRate);
  return new Blob([wavArrayBuffer], { type: 'audio/wav' });
}

// Encode Float32 PCM samples (range -1.0 to 1.0) into a 16-bit PCM WAV file,
// including a standard 44-byte RIFF/WAVE header.
function encodeWavPCM16(samples, sampleRate) {
  const numChannels = 1;
  const bytesPerSample = 2; // 16-bit
  const blockAlign = numChannels * bytesPerSample;
  const byteRate = sampleRate * blockAlign;
  const dataSize = samples.length * bytesPerSample;
  const bufferSize = 44 + dataSize;

  const buffer = new ArrayBuffer(bufferSize);
  const view = new DataView(buffer);

  function writeString(offset, str) {
    for (let i = 0; i < str.length; i++) view.setUint8(offset + i, str.charCodeAt(i));
  }

  // RIFF header
  writeString(0, 'RIFF');
  view.setUint32(4, 36 + dataSize, true);
  writeString(8, 'WAVE');

  // fmt subchunk
  writeString(12, 'fmt ');
  view.setUint32(16, 16, true);          // Subchunk1Size (16 for PCM)
  view.setUint16(20, 1, true);           // AudioFormat (1 = PCM)
  view.setUint16(22, numChannels, true);
  view.setUint32(24, sampleRate, true);
  view.setUint32(28, byteRate, true);
  view.setUint16(32, blockAlign, true);
  view.setUint16(34, bytesPerSample * 8, true); // BitsPerSample

  // data subchunk
  writeString(36, 'data');
  view.setUint32(40, dataSize, true);

  // Write PCM samples, converting Float32 [-1, 1] to Int16
  let offset = 44;
  for (let i = 0; i < samples.length; i++) {
    let s = Math.max(-1, Math.min(1, samples[i]));
    s = s < 0 ? s * 0x8000 : s * 0x7FFF;
    view.setInt16(offset, s, true);
    offset += 2;
  }

  return buffer;
}

function blobToBase64(blob) {
  return new Promise((resolve, reject) => {
    const reader = new FileReader();
    reader.onloadend = () => {
      // reader.result is a data URL like "data:audio/wav;base64,XXXX"
      const base64 = reader.result.split(',')[1];
      resolve(base64);
    };
    reader.onerror = reject;
    reader.readAsDataURL(blob);
  });
}


function applyPronunciationResult(result, wavBlob) {
  const statusEl = document.getElementById('pronounceStatus');
  const scoreNumEl = document.getElementById('pronounceScoreNum');
  const scoreLabelEl = document.getElementById('pronounceScoreLabel');

  const nbest = result?.NBest?.[0];
  if (!nbest) {
    if (statusEl) statusEl.textContent = 'No result returned — check your recording and try again.';
    return;
  }

  // Overall score location varies by API response shape — try the nested
  // PronunciationAssessment object first, then fall back to a flat field
  // directly on nbest, then derive it by averaging word-level scores.
  let overall = nbest.PronunciationAssessment?.PronScore
    ?? nbest.PronScore
    ?? null;

  if (overall == null) {
    const wordScores = (nbest.Words || [])
      .map(w => w.AccuracyScore ?? w.PronunciationAssessment?.AccuracyScore)
      .filter(s => typeof s === 'number');
    if (wordScores.length > 0) {
      overall = wordScores.reduce((a, b) => a + b, 0) / wordScores.length;
    } else {
      overall = 0;
    }
  }
  overall = Math.round(overall);

  if (scoreNumEl) {
    scoreNumEl.textContent = overall;
    scoreNumEl.style.display = 'block';
    scoreNumEl.style.color = scoreToColor(overall);
  }
  if (scoreLabelEl) scoreLabelEl.style.display = 'block';
  if (statusEl) {
    statusEl.style.textAlign = 'center';
    statusEl.style.whiteSpace = 'normal';
    statusEl.style.fontSize = '13px';
    statusEl.textContent = '';
  }

  // Track the best (highest) score seen for this phrase across retries this round
  const key = pronouncePhrase.kr;
  const prevBest = pronounceScores.get(key) ?? -1;
  if (overall > prevBest) pronounceScores.set(key, overall);

  // Save the recording if this attempt is a new best AND crosses the mastery
  // threshold — only one (the best) 90+ recording is kept per phrase, and it
  // persists across browser sessions via IndexedDB for later playback.
  if (overall > prevBest && overall >= PRONOUNCE_MASTERY_THRESHOLD && wavBlob) {
    saveBestRecording(key, overall, wavBlob);
  }

  // Map per-syllable scores onto syllable blocks.
  // Azure returns a Syllables[] array within each word with real per-syllable
  // AccuracyScore values (the Syllable name field itself is often blank for
  // Korean, but the scores and ordering are reliable) — use those directly
  // rather than repeating one word-level score across all its syllables.
  const words = nbest.Words || [];
  const syllables = getSyllables(pronouncePhrase.kr);
  const perSylScores = [];

  words.forEach(w => {
    if (Array.isArray(w.Syllables) && w.Syllables.length > 0) {
      w.Syllables.forEach(syl => perSylScores.push(syl.AccuracyScore ?? 0));
    } else {
      // Fallback: no syllable breakdown for this word — repeat its word score
      const wordSyls = getSyllables(w.Word || '');
      const score = w.AccuracyScore ?? w.PronunciationAssessment?.AccuracyScore ?? 0;
      wordSyls.forEach(() => perSylScores.push(score));
    }
  });

  // Fallback: if word/syllable counts don't line up, just spread overall score
  const finalScores = (perSylScores.length === syllables.length)
    ? perSylScores
    : syllables.map(() => overall);

  syllables.forEach((ch, i) => {
    const block = document.getElementById('block-' + i);
    if (!block) return;
    const score = finalScores[i];
    block.className = 'syl-block score-grad';
    block.style.borderColor = scoreToColor(score);
    block.style.background  = scoreToBgColor(score);
    block.style.color       = scoreToColor(score);
    block.style.position    = 'relative';

    // Set the character via a dedicated text node so appending the score
    // label afterward doesn't get wiped out by textContent reassignment.
    block.innerHTML = '';
    const charSpan = document.createElement('span');
    charSpan.textContent = ch;
    block.appendChild(charSpan);

    const label = document.createElement('span');
    label.className = 'syl-score-label';
    label.textContent = Math.round(score);
    label.style.color = scoreToColor(score);
    block.appendChild(label);
  });
}

// ── THEME ─────────────────────────────────────────────────────────────────
let isDarkMode = false;

function applyTheme() {
  document.documentElement.setAttribute('data-theme', isDarkMode ? 'dark' : 'light');
  const btn = document.getElementById('themeToggle');
  if (btn) btn.textContent = '🔆';
}

function toggleTheme() {
  isDarkMode = !isDarkMode;
  applyTheme();
}

// ── SETTINGS PANEL (Review eligibility overrides) ────────────────────────
async function openSettingsPanel() {
  const overlay = document.getElementById('settingsOverlay');
  if (!overlay) return;
  await renderSettingsListRows();
  overlay.style.display = 'flex';
}

function closeSettingsPanel(event) {
  // If called from the overlay click, only close when clicking the
  // backdrop itself, not the panel content (handled via stopPropagation
  // on the panel, so this only fires for genuine backdrop clicks or the
  // explicit Close button with no event).
  const overlay = document.getElementById('settingsOverlay');
  if (overlay) overlay.style.display = 'none';
}

async function renderSettingsListRows() {
  const container = document.getElementById('settingsListRows');
  if (!container) return;
  const records = await getAllListEligibilityRecords();

  container.innerHTML = Object.keys(LISTS).map(listName => {
    const rec = records.get(listName) || { autoStarted: false, manualOverride: null };
    const effectiveOn = rec.manualOverride === 'on' ? true
      : rec.manualOverride === 'off' ? false
      : !!rec.autoStarted;

    const statusText = rec.manualOverride === 'on'  ? 'Manually included'
      : rec.manualOverride === 'off' ? 'Manually excluded'
      : rec.autoStarted ? 'Included (completed a quiz round)'
      : 'Not yet started';

    return `
      <div class="settings-list-row">
        <div>
          <div class="settings-list-name">${listName}</div>
          <div class="settings-list-status">${statusText}</div>
        </div>
        <div class="settings-toggle-group">
          <button class="settings-toggle-btn ${rec.manualOverride === null ? 'active-auto' : ''}"
            onclick="setListOverrideAndRefresh('${listName}', null)">Auto</button>
          <button class="settings-toggle-btn ${rec.manualOverride === 'on' ? 'active-on' : ''}"
            onclick="setListOverrideAndRefresh('${listName}', 'on')">On</button>
          <button class="settings-toggle-btn ${rec.manualOverride === 'off' ? 'active-off' : ''}"
            onclick="setListOverrideAndRefresh('${listName}', 'off')">Off</button>
        </div>
      </div>
    `;
  }).join('');
}

async function setListOverrideAndRefresh(listName, value) {
  await setListManualOverride(listName, value);
  await renderSettingsListRows();
  refreshReviewBadge();
}

// ── STORIES MODE ─────────────────────────────────────────────────────────

// Story selector screen — shown when Stories tab is first opened
function renderStorySelector() {
  const area = document.getElementById('mainArea');
  const keys = Object.keys(STORIES);

  area.innerHTML = `
    <div class="category-label">Stories</div>
    <div class="story-selector-list">
      ${keys.map(key => {
        const story = STORIES[key];
        const count = story.sentences.length;
        return `
          <div class="story-card" onclick="openStory('${key.replace(/'/g,"\\'")}')">
            <div class="story-card-title">${key}</div>
            <div class="story-card-meta">${count} lines · ${story.list}</div>
          </div>
        `;
      }).join('')}
    </div>
  `;
}

function openStory(key) {
  currentStoryKey  = key;
  storySubMode     = 'read';
  storySentenceIdx = 0;
  storyResults     = [];
  maskPassNumber   = 1;
  maskPassWords    = new Map();
  renderStoryView();
}

// Main story view — renders sub-mode tabs + current sub-mode content
function renderStoryView() {
  const story = STORIES[currentStoryKey];
  const area  = document.getElementById('mainArea');

  area.innerHTML = `
    <button class="story-back-btn" onclick="renderStorySelector()">← All stories</button>
    <div class="category-label" style="margin-bottom:12px">${currentStoryKey}</div>

    <div class="story-sub-tabs">
      <button class="story-sub-tab ${storySubMode === 'read'  ? 'active' : ''}" onclick="setStorySubMode('read')">Read</button>
      <button class="story-sub-tab ${storySubMode === 'drill' ? 'active' : ''}" onclick="setStorySubMode('drill')">Drill</button>
      <button class="story-sub-tab ${storySubMode === 'mask'  ? 'active' : ''}" onclick="setStorySubMode('mask')">Mask</button>
    </div>

    <div id="storyContent"></div>
  `;

  renderStoryContent();
}

function setStorySubMode(sub) {
  storySubMode     = sub;
  storySentenceIdx = 0;
  storyResults     = [];
  if (sub === 'mask') {
    maskPassNumber = 1;
    buildMaskPass();
  }
  // Re-render just the sub-tabs active state + content
  document.querySelectorAll('.story-sub-tab').forEach((btn, i) => {
    const modes = ['read','drill','mask'];
    btn.classList.toggle('active', modes[i] === sub);
  });
  renderStoryContent();
}

function renderStoryContent() {
  if (storySubMode === 'read')  renderStoryRead();
  else if (storySubMode === 'drill') renderStoryDrillSentence();
  else                               renderStoryMaskSentence();
}

// ── READ MODE ─────────────────────────────────────────────────────────────
function renderStoryRead() {
  const story   = STORIES[currentStoryKey];
  const content = document.getElementById('storyContent');
  if (!content) return;

  content.innerHTML = `
    <div style="margin-bottom:8px">
      ${story.sentences.map((s, i) => `
        <div class="story-read-line">
          <span class="story-speaker">${s.speaker ?? ""}</span>
          <span class="story-line-kr">${s.kr}</span>
          <button class="story-line-listen" onclick="speak('${s.kr.replace(/'/g,"\\'")}')">🔊</button>
        </div>
      `).join('')}
    </div>
    <div class="action-row" style="margin-top:16px">
      <button class="submit-btn" onclick="setStorySubMode('drill')">Start drilling →</button>
    </div>
  `;
}

// ── DRILL MODE ────────────────────────────────────────────────────────────
// Sentence by sentence in order. User sees previous sentence as context,
// then types the current sentence from memory using block input.
function renderStoryDrillSentence() {
  const story   = STORIES[currentStoryKey];
  const content = document.getElementById('storyContent');
  if (!content) return;

  if (storySentenceIdx >= story.sentences.length) {
    renderStoryDrillSummary();
    return;
  }

  const sentence = story.sentences[storySentenceIdx];
  const prev     = storySentenceIdx > 0 ? story.sentences[storySentenceIdx - 1] : null;
  const pct      = Math.round((storySentenceIdx / story.sentences.length) * 100);

  // Build block display for the current sentence using existing machinery
  // We treat the whole sentence as a single "phrase" for the block system
  currentPhrase = { kr: sentence.kr, en: '', cat: '' };
  lockedSyls    = getSyllables(sentence.kr).map(() => null);
  composingChar = '';
  submittedWrong = false;

  const { html: blocksHTML } = buildBlocksHTML(sentence.kr);

  content.innerHTML = `
    <div class="progress-wrap">
      <div class="progress-meta">
        <span>${storySentenceIdx + 1} / ${story.sentences.length}</span>
      </div>
      <div class="progress-track"><div class="progress-fill" style="width:${pct}%"></div></div>
    </div>

    ${prev ? `
      <div class="story-context-line">
        <span class="story-context-speaker">${prev.speaker ? prev.speaker : ""}</span>${prev.kr}
      </div>
    ` : '<div style="height:20px"></div>'}

    ${sentence.speaker ? `<div class="story-target-speaker">${sentence.speaker}</div>` : ""}

    <div class="blocks-area" id="blocksArea">
      <input class="hidden-input" id="hiddenInput" type="text"
        autocomplete="off" autocorrect="off" autocapitalize="off" spellcheck="false">
      <div class="blocks-row">${blocksHTML}</div>
      <div class="tap-hint" id="tapHint">tap to type</div>
    </div>

    <div class="submit-row">
      <button class="submit-btn" onclick="submitStoryDrill()">확인 ↵</button>
    </div>

    <div class="feedback" id="feedbackLine"></div>

    <div class="action-row">
      <button class="ghost-btn" onclick="speak('${sentence.kr.replace(/'/g,"\\'")}')">🔊 Listen</button>
      <button class="ghost-btn" onclick="skipStoryDrill()">Skip →</button>
    </div>
  `;

  attachInput();
}

function submitStoryDrill() {
  const input = document.getElementById('hiddenInput');
  if (!input) return;
  const typedSyls  = committedSyls.slice();
  if (typedSyls.length === 0) return;

  const targetSyls = getSyllables(currentPhrase.kr);
  let cur = 0;
  const fullSyls = targetSyls.map((_, i) => {
    if (lockedSyls[i] !== null) return lockedSyls[i];
    return typedSyls[cur++] || '';
  });
  const isCorrect = fullSyls.join('') === targetSyls.join('');

  if (isCorrect) {
    updateBlocks(typedSyls, 'correct');
    const fb = document.getElementById('feedbackLine');
    if (fb) { fb.textContent = '✓'; fb.className = 'feedback correct'; }
    // Only record first-attempt result
    if (!submittedWrong) {
      storyResults.push({ idx: storySentenceIdx, correct: true });
    }
    setTimeout(() => {
      storySentenceIdx++;
      renderStoryDrillSentence();
    }, 800);
  } else {
    submittedWrong = true;
    // Lock correct syllables, shake wrong
    const newLocked = [...lockedSyls];
    let j = 0;
    targetSyls.forEach((targetCh, i) => {
      if (newLocked[i] !== null) return;
      if (typedSyls[j] === targetCh) newLocked[i] = targetCh;
      j++;
    });
    lockedSyls = newLocked;
    updateBlocks(typedSyls, 'wrong');
    const fb = document.getElementById('feedbackLine');
    if (fb) { fb.textContent = '✗ 다시 해 보세요.'; fb.className = 'feedback wrong'; fb.dataset.state = 'wrong'; }
    storyResults.push({ idx: storySentenceIdx, correct: false });
    setTimeout(() => {
      input.value = '';
      committedSyls = [];
      composingChar = '';
      updateBlocks([], 'typing');
      input.focus({ preventScroll: true });
    }, 500);
  }
}

function skipStoryDrill() {
  storyResults.push({ idx: storySentenceIdx, correct: false });
  storySentenceIdx++;
  renderStoryDrillSentence();
}

function renderStoryDrillSummary() {
  const story   = STORIES[currentStoryKey];
  const content = document.getElementById('storyContent');
  if (!content) return;

  // Deduplicate: first result per sentence index counts
  const seen = new Map();
  storyResults.forEach(r => {
    if (!seen.has(r.idx)) seen.set(r.idx, r.correct);
  });

  const total   = story.sentences.length;
  const correct = [...seen.values()].filter(Boolean).length;
  const pct     = Math.round((correct / total) * 100);
  const stamp   = pct === 100 ? '만점!' : pct >= 70 ? '잘했어요' : '다시 해봐요';

  content.innerHTML = `
    <div class="end-screen">
      <div class="stamp">${stamp}</div>
      <div class="end-score">${correct} / ${total}</div>
      <div class="end-label">${pct}% first-attempt correct</div>
      <div class="end-breakdown">
        ${story.sentences.map((s, i) => {
          const res = seen.get(i);
          const mark = res === undefined ? '—'
            : res ? '<span style="color:var(--correct)">✓</span>'
            : '<span style="color:var(--red-stamp)">✗</span>';
          return `
            <div class="breakdown-row">
              <span style="font-size:11px;color:var(--red-stamp);font-weight:700;text-transform:uppercase;flex-shrink:0;width:22px">${s.speaker ?? ""}</span>
              <span class="kr" style="font-size:14px">${s.kr}</span>
              <span class="mark">${mark}</span>
            </div>
          `;
        }).join('')}
      </div>
      <div style="display:flex;gap:10px;justify-content:center;flex-wrap:wrap">
        <button class="submit-btn" onclick="setStorySubMode('drill')">다시 하기</button>
        <button class="restart-btn" onclick="setStorySubMode('mask')">Try Mask mode →</button>
      </div>
    </div>
  `;
}

// ── MASK (PROGRESSIVE MASKING) MODE ──────────────────────────────────────
// Each pass, words that were recalled correctly get masked (blanked out).
// Words still being learned stay visible. The story gets progressively
// harder as more words disappear into memory.
// We track masking at the word level: a "word" here is a space-delimited
// token within each sentence. Punctuation stays visible always.

function tokenizeSentenceWords(kr) {
  // Returns array of { text, isMaskable } tokens.
  // Space-delimited segments that contain Hangul are maskable.
  return kr.split(' ').map(word => ({
    text: word,
    isMaskable: getSyllables(word).length > 0,
  }));
}

function buildMaskPass() {
  // On pass 1: mask nothing (start fully visible, then let the user
  // choose words to recall). Actually: pass 1 shows the full text and
  // the user taps words they want to test. Passes 2+ progressively mask
  // words they got right in the previous pass.
  // Simplified implementation: each pass randomly selects ~30% more words
  // to mask, ensuring words that were correct last pass are always masked.
  // This gives a natural progressive feel without complex state tracking.
  const story = STORIES[currentStoryKey];
  maskPassWords = new Map();

  story.sentences.forEach((s, si) => {
    const words = tokenizeSentenceWords(s.kr);
    const maskable = words.map((w, wi) => w.isMaskable ? wi : -1).filter(i => i >= 0);
    // How many to mask: increases per pass, caps at all maskable words
    const maskCount = Math.min(
      maskable.length,
      Math.ceil(maskable.length * Math.min(1, maskPassNumber * 0.35))
    );
    // Pick deterministically but varied: shuffle maskable indices by pass seed
    const seeded = [...maskable].sort((a, b) =>
      ((a * 37 + maskPassNumber * 17) % 97) - ((b * 37 + maskPassNumber * 17) % 97)
    );
    maskPassWords.set(si, new Set(seeded.slice(0, maskCount)));
  });
}

function renderStoryMaskSentence() {
  const story   = STORIES[currentStoryKey];
  const content = document.getElementById('storyContent');
  if (!content) return;

  if (storySentenceIdx >= story.sentences.length) {
    renderStoryMaskSummary();
    return;
  }

  const sentence  = story.sentences[storySentenceIdx];
  const prev      = storySentenceIdx > 0 ? story.sentences[storySentenceIdx - 1] : null;
  const maskedSet = maskPassWords.get(storySentenceIdx) || new Set();
  const words     = tokenizeSentenceWords(sentence.kr);
  const pct       = Math.round((storySentenceIdx / story.sentences.length) * 100);

  // Build the inline masked sentence HTML
  const maskedHTML = words.map((w, wi) => {
    if (maskedSet.has(wi)) {
      // Blank — user needs to recall this word
      const placeholder = '　'.repeat(Math.max(1, w.text.length)); // ideographic space for width
      return `<span class="mask-blank" id="mask-${storySentenceIdx}-${wi}" data-word="${w.text.replace(/"/g,'&quot;')}">${placeholder}</span>`;
    }
    return `<span class="mask-word">${w.text}</span>`;
  }).join(' ');

  const totalBlanks = [...maskedSet].length;

  content.innerHTML = `
    <div class="progress-wrap">
      <div class="progress-meta">
        <span>${storySentenceIdx + 1} / ${story.sentences.length} · Pass ${maskPassNumber}</span>
      </div>
      <div class="progress-track"><div class="progress-fill" style="width:${pct}%"></div></div>
    </div>

    ${prev ? `
      <div class="story-context-line">
        <span class="story-context-speaker">${prev.speaker ? prev.speaker : ""}</span>${prev.kr}
      </div>
    ` : '<div style="height:20px"></div>'}

    ${sentence.speaker ? `<div class="story-target-speaker">${sentence.speaker}</div>` : ""}

    <div class="mask-sentence" id="maskSentence">${maskedHTML}</div>

    <div class="feedback" id="maskFeedback" style="height:22px;margin-bottom:12px"></div>

    <div class="action-row" style="flex-wrap:wrap;gap:8px">
      ${totalBlanks > 0 ? `<button class="submit-btn" onclick="revealMaskSentence()">Reveal →</button>` : ''}
      <button class="ghost-btn" onclick="speak('${sentence.kr.replace(/'/g,"\\'")}')">🔊 Listen</button>
      <button class="ghost-btn" onclick="advanceMaskSentence(true)">Got it ✓</button>
      <button class="ghost-btn" onclick="advanceMaskSentence(false)">Missed ✗</button>
    </div>
    <div style="font-size:11px;color:var(--muted);margin-top:10px">
      ${totalBlanks === 0 ? 'No blanks this pass — tap Got it or Missed.' : `${totalBlanks} word${totalBlanks !== 1 ? 's' : ''} blanked. Listen, recall, then reveal to check.`}
    </div>
  `;
}

function revealMaskSentence() {
  const sentence  = STORIES[currentStoryKey].sentences[storySentenceIdx];
  const maskedSet = maskPassWords.get(storySentenceIdx) || new Set();
  const words     = tokenizeSentenceWords(sentence.kr);
  maskedSet.forEach(wi => {
    const el = document.getElementById(`mask-${storySentenceIdx}-${wi}`);
    if (el) {
      el.textContent = words[wi].text;
      el.classList.add('revealed');
    }
  });
  const fb = document.getElementById('maskFeedback');
  if (fb) fb.textContent = 'Did you recall all the blanked words?';
}

function advanceMaskSentence(gotIt) {
  storyResults.push({ idx: storySentenceIdx, correct: gotIt });
  storySentenceIdx++;
  renderStoryMaskSentence();
}

function renderStoryMaskSummary() {
  const story   = STORIES[currentStoryKey];
  const content = document.getElementById('storyContent');
  if (!content) return;

  const total   = story.sentences.length;
  const correct = storyResults.filter(r => r.correct).length;
  const pct     = Math.round((correct / total) * 100);
  const allGood = pct === 100;
  const stamp   = allGood ? '만점!' : pct >= 70 ? '잘했어요' : '다시 해봐요';

  content.innerHTML = `
    <div class="end-screen">
      <div class="stamp">${stamp}</div>
      <div class="end-score">${correct} / ${total}</div>
      <div class="end-label">Pass ${maskPassNumber} — ${pct}% recalled</div>
      <div class="end-breakdown">
        ${story.sentences.map((s, i) => {
          const r = storyResults.find(r => r.idx === i);
          const mark = !r ? '—'
            : r.correct ? '<span style="color:var(--correct)">✓</span>'
            : '<span style="color:var(--red-stamp)">✗</span>';
          return `
            <div class="breakdown-row">
              <span style="font-size:11px;color:var(--red-stamp);font-weight:700;text-transform:uppercase;flex-shrink:0;width:22px">${s.speaker ?? ""}</span>
              <span class="kr" style="font-size:14px">${s.kr}</span>
              <span class="mark">${mark}</span>
            </div>
          `;
        }).join('')}
      </div>
      <div style="display:flex;gap:10px;justify-content:center;flex-wrap:wrap">
        ${!allGood ? `<button class="submit-btn" onclick="nextMaskPass()">Next pass (harder) →</button>` : ''}
        <button class="ghost-btn" onclick="setStorySubMode('mask')">Restart mask</button>
        <button class="restart-btn" onclick="setStorySubMode('read')">Back to Read</button>
      </div>
    </div>
  `;
}

function nextMaskPass() {
  maskPassNumber++;
  storySentenceIdx = 0;
  storyResults     = [];
  buildMaskPass();
  renderStoryMaskSentence();
}


document.addEventListener('DOMContentLoaded', () => {
  // Default to system preference if available
  if (window.matchMedia && window.matchMedia('(prefers-color-scheme: dark)').matches) {
    isDarkMode = true;
  }
  applyTheme();
  renderListSelector();
  renderStudy();
  refreshReviewBadge();

  // Collapse the chip picker when user taps/clicks anywhere in the content column
  document.getElementById('contentCol')?.addEventListener('click', () => {
    if (openChapters.size > 0) {
      openChapters.clear();
      renderListSelector();
    }
  });
});
</script>
</body>
</html>

