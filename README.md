<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
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
  }

  body {
    background: var(--paper);
    color: var(--ink);
    font-family: 'Quicksand', sans-serif;
    min-height: 100vh;
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  .app {
    position: relative;
    width: 100%;
    max-width: 580px;
    padding: 0 20px 60px;
  }

  /* ── HEADER ── */
  header { padding: 28px 0 16px; display: flex; align-items: baseline; gap: 12px; }
  .title-kr { font-family: 'Nanum Myeongjo', serif; font-size: 26px; font-weight: 700; color: var(--ink); letter-spacing: 2px; }
  .title-en { font-size: 12px; color: #888; letter-spacing: 1px; text-transform: uppercase; }
  .score-badge { margin-left: auto; font-size: 13px; font-weight: 700; color: var(--gold); letter-spacing: 1px; }

  /* ── MODE TABS ── */
  .mode-tabs { display: flex; margin-bottom: 16px; border-bottom: 2px solid var(--ruled); }
  .mode-tab {
    padding: 8px 20px;
    font-family: 'Quicksand', sans-serif; font-size: 13px; font-weight: 700;
    letter-spacing: 1px; text-transform: uppercase;
    background: none; border: none; border-bottom: 3px solid transparent; margin-bottom: -2px;
    cursor: pointer; color: #aaa; transition: color 0.2s, border-color 0.2s;
  }
  .mode-tab.active { color: var(--blue-ink); border-bottom-color: var(--blue-ink); }

  /* ── LIST SELECTOR ── */
  .selector-wrap { margin-bottom: 20px; }
  .selector-label { font-size: 11px; font-weight: 700; letter-spacing: 1.5px; text-transform: uppercase; color: #bbb; margin-bottom: 8px; }
  .list-chips { display: flex; gap: 8px; flex-wrap: wrap; }
  .list-chip {
    padding: 6px 14px; border-radius: 20px;
    border: 1.5px solid var(--blue-ink); background: var(--blue-light);
    font-family: 'Quicksand', sans-serif; font-size: 12px; font-weight: 700;
    color: var(--blue-ink); cursor: pointer; letter-spacing: 0.5px;
    transition: opacity 0.15s; user-select: none;
  }
  .list-chip.selected { opacity: 1; }
  .list-chip:not(.selected) { opacity: 0.3; }
  .list-chip:hover:not(.selected):not(.only-one) { opacity: 0.5; }
  .list-chip.only-one { cursor: not-allowed; }

  /* ── CATEGORY LABEL ── */
  .category-label {
    font-size: 11px; font-weight: 700; letter-spacing: 2px; text-transform: uppercase;
    color: var(--red-stamp); margin-bottom: 10px; display: flex; align-items: center; gap: 8px;
  }
  .category-label::after { content: ''; flex: 1; height: 1px; background: rgba(192,57,43,0.2); }

  /* ── PROMPT CARD ── */
  .prompt-card {
    background: #fff; border: 1px solid var(--ruled); border-radius: 4px;
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

  /* blocks-row uses flex with wrap; space and punct tokens sit inline */
  .blocks-row {
    display: flex; flex-wrap: wrap;
    gap: 6px; align-items: center; min-height: 68px;
  }

  /* A space between words */
  .block-space { width: 14px; flex-shrink: 0; }

  /* Punctuation shown between/after blocks */
  .block-punct {
    font-family: 'Gowun Dodum', sans-serif;
    font-size: 26px; color: #aaa;
    line-height: 58px; flex-shrink: 0;
    user-select: none; margin: 0 1px;
  }

  .syl-block {
    width: 58px; height: 58px; flex-shrink: 0;
    border: 2px solid var(--ruled); border-radius: 4px; background: #fff;
    display: flex; align-items: center; justify-content: center;
    font-family: 'Gowun Dodum', sans-serif; font-size: 28px; color: var(--blue-ink);
    box-shadow: 2px 2px 0 var(--ruled);
    transition: border-color 0.12s, background 0.12s, color 0.12s;
    user-select: none;
  }
  .syl-block.empty   { border-color: var(--ruled); background: #fff; color: transparent; }
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

  .tap-hint { font-size: 11px; color: #bbb; margin-top: 8px; letter-spacing: 0.3px; transition: opacity 0.2s; }

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
    color: #888; cursor: pointer; letter-spacing: 0.5px; transition: border-color 0.15s, color 0.15s;
  }
  .ghost-btn:hover { border-color: #aaa; color: var(--ink); }

  /* ── PROGRESS ── */
  .progress-wrap { margin-bottom: 18px; }
  .progress-meta { display: flex; justify-content: space-between; font-size: 11px; color: #aaa; font-weight: 600; letter-spacing: 1px; margin-bottom: 6px; }
  .progress-track { height: 4px; background: var(--ruled); border-radius: 2px; overflow: hidden; }
  .progress-fill { height: 100%; background: var(--blue-ink); border-radius: 2px; transition: width 0.4s ease; }

  /* ── STUDY LIST ── */
  .study-section { margin-bottom: 28px; }
  .study-section-title {
    font-size: 11px; font-weight: 700; letter-spacing: 2px; text-transform: uppercase;
    color: var(--red-stamp); padding-bottom: 8px; border-bottom: 1px solid rgba(192,57,43,0.2);
    margin-bottom: 0;
  }
  .study-row { display: flex; align-items: baseline; padding: 10px 4px; gap: 16px; border-bottom: 1px solid var(--ruled); }
  .study-kr { font-family: 'Gowun Dodum', sans-serif; font-size: 19px; color: var(--blue-ink); flex-shrink: 0; min-width: 160px; }
  .study-en { font-size: 13px; color: #888; flex: 1; }

  /* ── END SCREEN ── */
  .end-screen { text-align: center; padding: 40px 0; }
  .stamp {
    display: inline-block; border: 3px solid var(--red-stamp); border-radius: 4px;
    padding: 8px 20px; font-family: 'Nanum Myeongjo', serif; font-size: 36px; font-weight: 700;
    color: var(--red-stamp); letter-spacing: 6px; transform: rotate(-4deg); margin-bottom: 24px; opacity: 0.85;
  }
  .end-score { font-size: 48px; font-weight: 700; color: var(--ink); line-height: 1; margin-bottom: 6px; }
  .end-label { font-size: 13px; color: #999; letter-spacing: 1px; text-transform: uppercase; margin-bottom: 28px; }
  .end-breakdown {
    text-align: left; background: #fff; border: 1px solid var(--ruled);
    border-radius: 4px; padding: 16px 20px; margin-bottom: 24px; box-shadow: 2px 2px 0 var(--ruled);
  }
  .breakdown-row { display: flex; justify-content: space-between; align-items: center; font-size: 13px; padding: 5px 0; border-bottom: 1px solid var(--ruled); gap: 8px; }
  .breakdown-row:last-child { border-bottom: none; }
  .breakdown-row .kr { font-family: 'Gowun Dodum', sans-serif; font-size: 15px; color: var(--blue-ink); flex-shrink: 0; }
  .breakdown-row .en { color: #999; font-size: 12px; flex: 1; }
  .breakdown-row .mark { font-size: 14px; flex-shrink: 0; }
  .restart-btn {
    padding: 12px 32px; background: var(--red-stamp); color: #fff; border: none; border-radius: 4px;
    font-family: 'Quicksand', sans-serif; font-size: 14px; font-weight: 700; letter-spacing: 1px;
    cursor: pointer; transition: background 0.15s;
  }
  .restart-btn:hover { background: #a03020; }
</style>
</head>
<body>
<div class="app">
  <header>
    <span class="title-kr">한글 쓰기</span>
    <span class="title-en">Spelling Practice</span>
    <span class="score-badge" id="scoreDisplay"></span>
  </header>

  <div class="mode-tabs">
    <button class="mode-tab active" id="tabStudy" onclick="switchMode('study')">Study</button>
    <button class="mode-tab"        id="tabQuiz"  onclick="switchMode('quiz')">Quiz</button>
  </div>

  <div class="selector-wrap">
    <div class="selector-label" id="selectorLabel">Lists</div>
    <div class="list-chips" id="listSelector"></div>
  </div>

  <div id="mainArea"></div>
</div>

<script>
// ── DATA ──────────────────────────────────────────────────────────────────
const LISTS = {
  "Unit 1 TPA": [
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
  "Unit 1 TPB": [
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
  "Unit 1 TPC": [
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
};

// ── GLOBAL STATE ──────────────────────────────────────────────────────────
let mode         = 'study';
let selectedLists = new Set(Object.keys(LISTS));

let quizQueue   = [];
let quizIndex   = 0;
let quizScore   = 0;
let quizResults = [];

let lockedSyls    = [];
let currentPhrase = null;
let composingChar = '';
let submittedWrong = false;

// ── HELPERS ───────────────────────────────────────────────────────────────
const PUNCT = new Set(['.', '?', '!', ',', '·']);

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
  currentPhrase = phrase;
  composingChar = '';
  lockedSyls = getSyllables(phrase.kr).map(() => null);
}

// ── LIST SELECTOR ─────────────────────────────────────────────────────────
function renderListSelector() {
  const container = document.getElementById('listSelector');
  const label     = document.getElementById('selectorLabel');
  if (!container) return;
  label.textContent = mode === 'study' ? 'Lists' : 'Quiz list';
  container.innerHTML = Object.keys(LISTS).map(name => {
    const isSel  = selectedLists.has(name);
    const isOnly = isSel && selectedLists.size === 1;
    return `<button class="list-chip ${isSel ? 'selected' : ''} ${isOnly ? 'only-one' : ''}"
      onclick="toggleList('${name}')">${name}</button>`;
  }).join('');
}

function toggleList(name) {
  if (selectedLists.has(name) && selectedLists.size === 1) return;
  if (selectedLists.has(name)) selectedLists.delete(name);
  else selectedLists.add(name);
  renderListSelector();
  if (mode === 'study') renderStudy();
  else startQuiz();
}

// ── MODE SWITCH ───────────────────────────────────────────────────────────
function switchMode(m) {
  mode = m;
  document.getElementById('tabStudy').classList.toggle('active', m === 'study');
  document.getElementById('tabQuiz').classList.toggle('active',  m === 'quiz');
  document.getElementById('scoreDisplay').textContent = '';
  renderListSelector();
  if (m === 'study') renderStudy();
  else startQuiz();
}

// ── STUDY MODE ────────────────────────────────────────────────────────────
function renderStudy() {
  const area      = document.getElementById('mainArea');
  const listNames = Object.keys(LISTS).filter(k => selectedLists.has(k));
  area.innerHTML  = listNames.map(listName => {
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
function attachInput() {
  const input = document.getElementById('hiddenInput');
  if (!input) return;
  input.value = '';
  input.addEventListener('compositionstart',  () => { composingChar = ''; });
  input.addEventListener('compositionupdate', e  => {
    composingChar = e.data || '';
    updateBlocks(getSyllables(input.value), 'typing');
  });
  input.addEventListener('compositionend',    () => { composingChar = ''; });
  input.addEventListener('input', () => {
    composingChar = '';
    updateBlocks(getSyllables(input.value), 'typing');
    const fb = document.getElementById('feedbackLine');
    if (fb && fb.dataset.state === 'wrong') { fb.textContent = ''; fb.className = 'feedback'; fb.dataset.state = ''; }
  });
  input.addEventListener('keydown', e => { if (e.key === 'Enter') { e.preventDefault(); submitAnswer(); } });

  const area = document.getElementById('blocksArea');
  if (area) area.addEventListener('click', () => input.focus());

  const hint = document.getElementById('tapHint');
  if (hint) {
    input.addEventListener('focus', () => hint.style.opacity = '0');
    input.addEventListener('blur',  () => hint.style.opacity = '1');
  }
  input.focus();
}

// ── SUBMIT ────────────────────────────────────────────────────────────────
function submitAnswer() {
  const input = document.getElementById('hiddenInput');
  if (!input) return;
  const typedSyls  = getSyllables(input.value);
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
    quizResults.push({ phrase, correct: true });
    document.getElementById('scoreDisplay').textContent = `★ ${quizScore}`;
    setTimeout(() => advanceQuiz(), 1000);
  } else {
    submittedWrong = true;
    // Lock newly-correct positions
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
    quizResults.push({ phrase, correct: false });
    setTimeout(() => { input.value = ''; updateBlocks([], 'typing'); input.focus(); }, 500);
  }
}

// ── QUIZ MODE ─────────────────────────────────────────────────────────────
function startQuiz() {
  const phrases  = getActivePhrases();
  quizQueue      = shuffle(phrases.map((_, i) => i));
  quizIndex      = 0;
  quizScore      = 0;
  quizResults    = [];
  document.getElementById('scoreDisplay').textContent = `★ 0`;
  renderQuiz(phrases);
}

function renderQuiz(phrases) {
  phrases = phrases || getActivePhrases();
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
      <div class="blocks-row">${blocksHTML}</div>
      <div class="tap-hint" id="tapHint">tap to type</div>
    </div>

    <div class="submit-row">
      <button class="submit-btn" onclick="submitAnswer()">확인 ↵</button>
    </div>

    <div class="feedback" id="feedbackLine"></div>

    <div class="action-row">
      <button class="ghost-btn" onclick="skipQuiz()">Skip →</button>
    </div>
  `;
  attachInput();
}

function advanceQuiz() { quizIndex++; renderQuiz(); }
function skipQuiz()    { quizResults.push({ phrase: currentPhrase, correct: false }); advanceQuiz(); }

// ── END SCREEN ────────────────────────────────────────────────────────────
function renderEndScreen() {
  const total     = quizQueue.length;
  const pct       = Math.round((quizScore / total) * 100);
  const stampText = pct === 100 ? '만점!' : pct >= 70 ? '잘했어요' : '다시 해봐요';

  document.getElementById('mainArea').innerHTML = `
    <div class="end-screen">
      <div class="stamp">${stampText}</div>
      <div class="end-score">${quizScore} / ${total}</div>
      <div class="end-label">${pct}% correct</div>
      <div class="end-breakdown">
        <div class="breakdown-row" style="font-size:11px;letter-spacing:1px;text-transform:uppercase;color:#bbb;font-weight:700">
          <span>Phrase</span><span></span><span>Result</span>
        </div>
        ${quizResults.map(r => `
          <div class="breakdown-row">
            <span class="kr">${r.phrase.kr}</span>
            <span class="en">${r.phrase.en}</span>
            <span class="mark">${r.correct
              ? '<span style="color:var(--correct)">✓</span>'
              : '<span style="color:var(--red-stamp)">✗</span>'}</span>
          </div>
        `).join('')}
      </div>
      <button class="restart-btn" onclick="startQuiz()">다시 하기 — Play Again</button>
    </div>
  `;
  document.getElementById('scoreDisplay').textContent = '';
}

// ── INIT ──────────────────────────────────────────────────────────────────
document.addEventListener('DOMContentLoaded', () => {
  renderListSelector();
  renderStudy();
});
</script>
</body>
</html>
