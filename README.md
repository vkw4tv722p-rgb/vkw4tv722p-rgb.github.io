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
    padding: 0 20px 20px;
    max-height: var(--app-max-height, none);
    overflow-y: auto;
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

  /* ── LIST SELECTOR ── */
  .selector-wrap { margin-bottom: 20px; }
  .selector-label { font-size: 11px; font-weight: 700; letter-spacing: 1.5px; text-transform: uppercase; color: var(--muted-2); margin-bottom: 8px; }
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
    <span class="score-badge" id="scoreDisplay" style="display:none"></span>
    <button class="theme-toggle" id="themeToggle" onclick="toggleTheme()" aria-label="Toggle dark mode">🔆</button>
  </header>

  <div class="mode-tabs">
    <button class="mode-tab active" id="tabStudy" onclick="switchMode('study')">Study</button>
    <button class="mode-tab"        id="tabQuiz"  onclick="switchMode('quiz')">Quiz</button>
    <button class="mode-tab"        id="tabPronounce" onclick="switchMode('pronounce')">Pronounce</button>
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
let submittedWrong = false;

// ── PRONOUNCE MODE STATE ─────────────────────────────────────────────────
const PRONOUNCE_PROXY_URL = "https://korean-pronunciation-proxy.cgmn9jdtsh.workers.dev";
let pronouncePhrases  = [];
let pronounceIndex    = 0;
let pronouncePhrase   = null;
let mediaRecorder     = null;
let recordedChunks    = [];
let isRecording       = false;

// ── HELPERS ───────────────────────────────────────────────────────────────
const PUNCT = new Set(['.', '?', '!', ',', '·', '~', '+', '-']);

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
  committedSyls = [];
  isComposing   = false;
  lockedSyls    = getSyllables(phrase.kr).map(() => null);
}

// ── LIST SELECTOR ─────────────────────────────────────────────────────────
function renderListSelector() {
  const container = document.getElementById('listSelector');
  const label     = document.getElementById('selectorLabel');
  if (!container) return;
  label.textContent = mode === 'study' ? 'Lists' : mode === 'quiz' ? 'Quiz list' : 'Practice list';
  container.innerHTML = Object.keys(LISTS).map(name => {
    const isSel = selectedLists.has(name);
    return `<button class="list-chip ${isSel ? 'selected' : ''}"
      onclick="toggleList('${name}')">${name}</button>`;
  }).join('');
}

function toggleList(name) {
  if (selectedLists.has(name)) selectedLists.delete(name);
  else selectedLists.add(name);
  renderListSelector();
  if (mode === 'study') renderStudy();
  else if (mode === 'quiz') startQuiz();
  else startPronounce();
}

// ── MODE SWITCH ───────────────────────────────────────────────────────────
function switchMode(m) {
  mode = m;
  document.getElementById('tabStudy').classList.toggle('active', m === 'study');
  document.getElementById('tabQuiz').classList.toggle('active',  m === 'quiz');
  document.getElementById('tabPronounce').classList.toggle('active', m === 'pronounce');
  document.getElementById('scoreDisplay').textContent = '';
  renderListSelector();
  if (m === 'study') renderStudy();
  else if (m === 'quiz') startQuiz();
  else startPronounce();
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
  const input = document.getElementById('hiddenInput');
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
    // If key still in currentRoundKeys, no wrong attempt happened this round → first
    // If key already removed (wrong attempt occurred), → corrected
    const isFirst = currentRoundKeys.has(phrase.kr);
    phraseStatus.set(phrase.kr, { phrase, status: isFirst ? 'first' : 'corrected' });
    currentRoundKeys.delete(phrase.kr);
    document.getElementById('scoreDisplay').textContent = `★ ${quizScore}`;
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
    currentRoundKeys.delete(phrase.kr);
    phraseStatus.set(phrase.kr, { phrase, status: 'missed' });
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
        <button class="restart-btn" onclick="startQuiz()">다시 하기 — Full restart</button>
      </div>
    </div>
  `;
  document.getElementById('scoreDisplay').textContent = '';
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

// ── PRONOUNCE MODE ────────────────────────────────────────────────────────
function startPronounce() {
  const phrases = getActivePhrases();
  if (phrases.length === 0) {
    document.getElementById('scoreDisplay').textContent = '';
    document.getElementById('mainArea').innerHTML = `<div style="text-align:center;padding:60px 0;color:var(--muted-2);font-size:15px;letter-spacing:1px;">Select a set to begin.</div>`;
    return;
  }
  pronouncePhrases = phrases;
  pronounceIndex    = Math.floor(Math.random() * phrases.length);
  pronouncePhrase   = phrases[pronounceIndex];
  document.getElementById('scoreDisplay').textContent = '';
  renderPronounce();
}

function nextPronounceWord() {
  if (pronouncePhrases.length === 0) return;
  // Avoid repeating the same word twice in a row if list has more than 1 entry
  let next = pronouncePhrase;
  if (pronouncePhrases.length > 1) {
    while (next === pronouncePhrase) {
      next = pronouncePhrases[Math.floor(Math.random() * pronouncePhrases.length)];
    }
  }
  pronouncePhrase = next;
  renderPronounce();
}

function renderPronounce() {
  const phrase = pronouncePhrase;
  const { html: blocksHTML } = buildBlocksHTML(phrase.kr);

  document.getElementById('mainArea').innerHTML = `
    <div class="category-label">${phrase.cat}</div>

    <div class="prompt-card">
      <div class="meaning-text">${phrase.en}</div>
    </div>

    <div class="pronounce-card">
      <div class="blocks-row" id="pronounceBlocks" style="justify-content:center">${blocksHTML}</div>

      <div class="record-label" id="recordLabel">Tap to record</div>
      <button class="record-btn" id="recordBtn" onclick="toggleRecording()">🎙️</button>

      <div class="pronounce-score" id="pronounceScoreNum" style="display:none"></div>
      <div class="pronounce-score-label" id="pronounceScoreLabel" style="display:none">Overall score</div>

      <div class="pronounce-status" id="pronounceStatus"></div>

      <div class="action-row" style="justify-content:center;margin-top:10px">
        <button class="ghost-btn" onclick="nextPronounceWord()">Next word →</button>
        <button class="ghost-btn" onclick="speak(currentKrTarget())">🔊 Listen</button>
      </div>
    </div>
  `;
}

function currentKrTarget() {
  return pronouncePhrase ? pronouncePhrase.kr : '';
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
    if (statusEl) statusEl.textContent = 'Scoring your pronunciation…';
    const audioBase64 = await blobToBase64(blob);
    const referenceText = pronouncePhrase.kr.replace(/[~+\-.?!,]/g, '').trim();

    const response = await fetch(PRONOUNCE_PROXY_URL, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ referenceText, audioBase64 }),
    });

    const result = await response.json();
    applyPronunciationResult(result);
  } catch (err) {
    if (statusEl) statusEl.textContent = 'Error scoring pronunciation. Try again.';
  } finally {
    if (recordLabel) recordLabel.textContent = 'Tap to record';
  }
}

function blobToBase64(blob) {
  return new Promise((resolve, reject) => {
    const reader = new FileReader();
    reader.onloadend = () => {
      // reader.result is a data URL like "data:audio/webm;base64,XXXX"
      const base64 = reader.result.split(',')[1];
      resolve(base64);
    };
    reader.onerror = reject;
    reader.readAsDataURL(blob);
  });
}

function applyPronunciationResult(result) {
  const statusEl = document.getElementById('pronounceStatus');
  const scoreNumEl = document.getElementById('pronounceScoreNum');
  const scoreLabelEl = document.getElementById('pronounceScoreLabel');

  // Azure's response shape: NBest[0].PronunciationAssessment.PronScore, and
  // NBest[0].Words[].PronunciationAssessment.AccuracyScore for per-word scores
  const nbest = result?.NBest?.[0];
  if (!nbest) {
    if (statusEl) statusEl.textContent = 'No result returned — check your recording and try again.';
    return;
  }

  const overall = Math.round(nbest.PronunciationAssessment?.PronScore ?? 0);
  if (scoreNumEl) {
    scoreNumEl.textContent = overall;
    scoreNumEl.style.display = 'block';
    scoreNumEl.style.color = scoreToColor(overall);
  }
  if (scoreLabelEl) scoreLabelEl.style.display = 'block';
  if (statusEl) statusEl.textContent = '';

  // Map word-level scores onto syllable blocks.
  // Azure gives per-word scores; each "word" in Korean without spaces may map
  // to multiple syllable blocks, so we distribute each word's score across
  // the syllables it covers, in order.
  const words = nbest.Words || [];
  const syllables = getSyllables(pronouncePhrase.kr);
  const perSylScores = [];

  words.forEach(w => {
    const wordSyls = getSyllables(w.Word || '');
    const score = w.PronunciationAssessment?.AccuracyScore ?? 0;
    wordSyls.forEach(() => perSylScores.push(score));
  });

  // Fallback: if word/syllable counts don't line up, just spread overall score
  const finalScores = (perSylScores.length === syllables.length)
    ? perSylScores
    : syllables.map(() => overall);

  syllables.forEach((ch, i) => {
    const block = document.getElementById('block-' + i);
    if (!block) return;
    const score = finalScores[i];
    block.textContent = ch;
    block.className = 'syl-block score-grad';
    block.style.borderColor = scoreToColor(score);
    block.style.background  = scoreToBgColor(score);
    block.style.color       = scoreToColor(score);
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

// ── VIEWPORT (keyboard-aware sizing) ────────────────────────────────────────
// On mobile, opening the OS keyboard shrinks the visual viewport. We track
// that height and cap the app container to it so content compresses to fit
// instead of the page auto-scrolling to keep the focused input visible.
function syncViewportHeight() {
  const vv = window.visualViewport;
  if (!vv) return;
  document.documentElement.style.setProperty('--app-max-height', vv.height + 'px');
}

if (window.visualViewport) {
  window.visualViewport.addEventListener('resize', syncViewportHeight);
  window.visualViewport.addEventListener('scroll', syncViewportHeight);
}

// ── INIT ──────────────────────────────────────────────────────────────────
document.addEventListener('DOMContentLoaded', () => {
  syncViewportHeight();
  // Default to system preference if available
  if (window.matchMedia && window.matchMedia('(prefers-color-scheme: dark)').matches) {
    isDarkMode = true;
  }
  applyTheme();
  renderListSelector();
  renderStudy();
});
</script>
</body>
</html>
