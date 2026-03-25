<!DOCTYPE html>
<html lang="ca">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Bank de Qüestionaris</title>
  <link href="https://fonts.googleapis.com/css2?family=Syne:wght@400;600;700;800&family=DM+Sans:ital,wght@0,300;0,400;0,500;1,300&family=Outfit:wght@400;600;700;800&display=swap" rel="stylesheet"/>
  <style>
    :root {
      --bg:        #0d0f14;
      --surface:   #151820;
      --card:      #1c2030;
      --border:    #2a2f42;
      --accent:    #5b8dee;
      --accent2:   #7c6df7;
      --green:     #3ecf8e;
      --red:       #ff5e5e;
      --text:      #e8eaf0;
      --muted:     #6b7280;
      --radius:    14px;
    }

    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

    body {
      background: var(--bg);
      color: var(--text);
      font-family: 'DM Sans', sans-serif;
      min-height: 100vh;
      overflow-x: hidden;
    }

    /* ── NOISE OVERLAY ── */
    body::before {
      content: '';
      position: fixed; inset: 0;
      background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 200 200' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)' opacity='0.04'/%3E%3C/svg%3E");
      pointer-events: none; z-index: 0;
    }

    /* ── GLOW BLOBS ── */
    .blob {
      position: fixed;
      border-radius: 50%;
      filter: blur(120px);
      opacity: 0.12;
      pointer-events: none;
      z-index: 0;
    }
    .blob-1 { width: 600px; height: 600px; background: var(--accent2); top: -200px; right: -150px; }
    .blob-2 { width: 400px; height: 400px; background: var(--accent);  bottom: -100px; left: -100px; }

    /* ── SCREENS ── */
    .screen { position: relative; z-index: 1; display: none; }
    .screen.active { display: block; }

    /* ════════════════════════════════════
       SCREEN 1 — HOME
    ════════════════════════════════════ */
    #home {
      padding: 60px 24px 100px;
      max-width: 960px;
      margin: 0 auto;
    }

    .site-header {
      display: flex;
      align-items: center;
      gap: 16px;
      margin-bottom: 64px;
    }
    .site-logo {
      width: 42px; height: 42px;
      background: linear-gradient(135deg, var(--accent), var(--accent2));
      border-radius: 10px;
      display: grid; place-items: center;
      font-size: 20px;
    }
    .site-title {
      font-family: 'Syne', sans-serif;
      font-size: 1.1rem;
      font-weight: 600;
      color: var(--muted);
      letter-spacing: 0.04em;
      text-transform: uppercase;
    }

    .hero-label {
      display: inline-block;
      font-size: 0.72rem;
      font-weight: 500;
      letter-spacing: 0.15em;
      text-transform: uppercase;
      color: var(--accent);
      background: rgba(91,141,238,0.1);
      border: 1px solid rgba(91,141,238,0.25);
      padding: 5px 12px;
      border-radius: 99px;
      margin-bottom: 20px;
    }

    .hero-title {
      font-family: 'Syne', sans-serif;
      font-size: clamp(2.6rem, 6vw, 4.2rem);
      font-weight: 800;
      line-height: 1.05;
      margin-bottom: 16px;
      background: linear-gradient(135deg, #e8eaf0 30%, var(--accent));
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
    }

    .hero-sub {
      color: var(--muted);
      font-size: 1.05rem;
      max-width: 520px;
      line-height: 1.7;
      margin-bottom: 56px;
    }

    /* ── DEGREE SECTIONS ── */
    .degree-section { margin-bottom: 56px; }

    .degree-label {
      font-family: 'Syne', sans-serif;
      font-size: 0.68rem;
      font-weight: 700;
      letter-spacing: 0.18em;
      text-transform: uppercase;
      color: var(--muted);
      margin-bottom: 18px;
      display: flex;
      align-items: center;
      gap: 10px;
    }
    .degree-label::after {
      content: '';
      flex: 1;
      height: 1px;
      background: var(--border);
    }

    .degree-name {
      font-family: 'Syne', sans-serif;
      font-size: 1.55rem;
      font-weight: 700;
      margin-bottom: 24px;
      color: var(--text);
    }
    .degree-name span {
      background: linear-gradient(90deg, var(--accent), var(--accent2));
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
    }

    .subjects-grid {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(260px, 1fr));
      gap: 16px;
    }

    .subject-card {
      background: var(--card);
      border: 1px solid var(--border);
      border-radius: var(--radius);
      padding: 24px 22px;
      cursor: pointer;
      transition: transform 0.2s, border-color 0.2s, box-shadow 0.2s;
      position: relative;
      overflow: hidden;
    }
    .subject-card::before {
      content: '';
      position: absolute;
      top: 0; left: 0; right: 0;
      height: 2px;
      background: linear-gradient(90deg, var(--accent), var(--accent2));
      opacity: 0;
      transition: opacity 0.2s;
    }
    .subject-card:hover {
      transform: translateY(-3px);
      border-color: rgba(91,141,238,0.4);
      box-shadow: 0 12px 40px rgba(0,0,0,0.35);
    }
    .subject-card:hover::before { opacity: 1; }

    .subject-icon {
      width: 36px; height: 36px;
      border-radius: 9px;
      background: rgba(91,141,238,0.12);
      display: grid; place-items: center;
      font-size: 17px;
      margin-bottom: 14px;
    }
    .subject-name {
      font-family: 'Syne', sans-serif;
      font-weight: 700;
      font-size: 1.0rem;
      margin-bottom: 6px;
    }
    .subject-meta {
      font-size: 0.78rem;
      color: var(--muted);
    }
    .subject-arrow {
      position: absolute;
      right: 18px; top: 50%;
      transform: translateY(-50%);
      color: var(--muted);
      font-size: 1.1rem;
      transition: transform 0.2s, color 0.2s;
    }
    .subject-card:hover .subject-arrow { transform: translateY(-50%) translateX(4px); color: var(--accent); }

    /* ════════════════════════════════════
       SCREEN 2 — SUBJECT (quiz list)
    ════════════════════════════════════ */
    #subject-screen {
      padding: 48px 24px 100px;
      max-width: 860px;
      margin: 0 auto;
    }

    .back-btn {
      display: inline-flex;
      align-items: center;
      gap: 8px;
      color: var(--muted);
      font-size: 0.85rem;
      cursor: pointer;
      margin-bottom: 40px;
      transition: color 0.15s;
      background: none;
      border: none;
      font-family: inherit;
    }
    .back-btn:hover { color: var(--text); }

    .subject-header {
      margin-bottom: 40px;
    }
    .subject-header .tag {
      font-size: 0.7rem;
      letter-spacing: 0.14em;
      text-transform: uppercase;
      color: var(--accent);
      margin-bottom: 10px;
    }
    .subject-header h2 {
      font-family: 'Syne', sans-serif;
      font-size: 2rem;
      font-weight: 800;
    }

    .quiz-list { display: flex; flex-direction: column; gap: 14px; }

    .quiz-item {
      background: var(--card);
      border: 1px solid var(--border);
      border-radius: var(--radius);
      padding: 20px 24px;
      display: flex;
      align-items: center;
      justify-content: space-between;
      cursor: pointer;
      transition: border-color 0.2s, transform 0.15s, box-shadow 0.2s;
    }
    .quiz-item:hover {
      border-color: rgba(91,141,238,0.4);
      transform: translateX(4px);
      box-shadow: 0 8px 30px rgba(0,0,0,0.3);
    }
    .quiz-item-left { display: flex; align-items: center; gap: 16px; }
    .quiz-num {
      width: 38px; height: 38px;
      border-radius: 9px;
      background: rgba(91,141,238,0.1);
      border: 1px solid rgba(91,141,238,0.2);
      display: grid; place-items: center;
      font-family: 'Syne', sans-serif;
      font-weight: 700;
      font-size: 0.85rem;
      color: var(--accent);
    }
    .quiz-title { font-weight: 500; font-size: 0.98rem; }
    .quiz-count { font-size: 0.78rem; color: var(--muted); margin-top: 2px; }
    .quiz-start-btn {
      background: rgba(91,141,238,0.1);
      border: 1px solid rgba(91,141,238,0.25);
      color: var(--accent);
      padding: 8px 18px;
      border-radius: 8px;
      font-size: 0.82rem;
      font-family: inherit;
      cursor: pointer;
      transition: background 0.2s, border-color 0.2s;
      white-space: nowrap;
    }
    .quiz-start-btn:hover { background: rgba(91,141,238,0.2); border-color: var(--accent); }

    /* ════════════════════════════════════
       SCREEN 3 — QUIZ
    ════════════════════════════════════ */
    #quiz-screen {
      padding: 48px 24px 100px;
      max-width: 720px;
      margin: 0 auto;
    }

    .quiz-top {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 36px;
    }
    .quiz-progress-wrap { flex: 1; margin: 0 24px; }
    .quiz-progress-label {
      font-size: 0.75rem;
      color: var(--muted);
      margin-bottom: 6px;
      text-align: center;
    }
    .quiz-progress-bar {
      height: 4px;
      background: var(--border);
      border-radius: 99px;
      overflow: hidden;
    }
    .quiz-progress-fill {
      height: 100%;
      background: linear-gradient(90deg, var(--accent), var(--accent2));
      border-radius: 99px;
      transition: width 0.4s ease;
    }

    .question-card {
      background: var(--card);
      border: 1px solid var(--border);
      border-radius: 18px;
      padding: 36px 32px;
      margin-bottom: 20px;
      animation: fadeUp 0.3s ease;
    }
    @keyframes fadeUp {
      from { opacity: 0; transform: translateY(12px); }
      to   { opacity: 1; transform: translateY(0); }
    }

    .question-number {
      font-size: 0.72rem;
      letter-spacing: 0.12em;
      text-transform: uppercase;
      color: var(--accent);
      margin-bottom: 14px;
    }
    .question-text {
      font-family: 'Syne', sans-serif;
      font-size: 1.18rem;
      font-weight: 600;
      line-height: 1.5;
      margin-bottom: 28px;
    }

    .options { display: flex; flex-direction: column; gap: 10px; }

    .option {
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: 10px;
      padding: 14px 18px;
      cursor: pointer;
      display: flex;
      align-items: center;
      gap: 14px;
      transition: border-color 0.15s, background 0.15s;
      font-size: 0.95rem;
    }
    .option:hover:not(.disabled) { border-color: rgba(91,141,238,0.5); background: rgba(91,141,238,0.06); }
    .option.correct  { border-color: var(--green); background: rgba(62,207,142,0.08); }
    .option.wrong    { border-color: var(--red);   background: rgba(255,94,94,0.08); }
    .option.disabled { cursor: default; }

    .option-letter {
      width: 28px; height: 28px;
      border-radius: 7px;
      background: var(--border);
      display: grid; place-items: center;
      font-family: 'Syne', sans-serif;
      font-size: 0.8rem;
      font-weight: 700;
      flex-shrink: 0;
      transition: background 0.15s, color 0.15s;
    }
    .option.correct .option-letter  { background: var(--green); color: #0d0f14; }
    .option.wrong   .option-letter  { background: var(--red);   color: #0d0f14; }

    .feedback-msg {
      font-size: 0.88rem;
      padding: 10px 16px;
      border-radius: 9px;
      margin-bottom: 18px;
      display: none;
    }
    .feedback-msg.show { display: block; }
    .feedback-msg.correct { background: rgba(62,207,142,0.1); color: var(--green); border: 1px solid rgba(62,207,142,0.25); }
    .feedback-msg.wrong   { background: rgba(255,94,94,0.1);  color: var(--red);   border: 1px solid rgba(255,94,94,0.25); }

    .next-btn {
      width: 100%;
      padding: 15px;
      background: linear-gradient(135deg, var(--accent), var(--accent2));
      border: none;
      border-radius: 10px;
      color: #fff;
      font-family: 'Syne', sans-serif;
      font-size: 0.95rem;
      font-weight: 700;
      cursor: pointer;
      display: none;
      transition: opacity 0.2s;
    }
    .next-btn.show { display: block; }
    .next-btn:hover { opacity: 0.88; }

    /* ── RESULTS ── */
    .results-card {
      background: var(--card);
      border: 1px solid var(--border);
      border-radius: 18px;
      padding: 48px 32px;
      text-align: center;
      animation: fadeUp 0.3s ease;
    }
    .results-emoji { font-size: 3rem; margin-bottom: 20px; }
    .results-title {
      font-family: 'Syne', sans-serif;
      font-size: 1.8rem;
      font-weight: 800;
      margin-bottom: 8px;
    }
    .results-score {
      font-size: 3.5rem;
      font-family: 'Syne', sans-serif;
      font-weight: 800;
      background: linear-gradient(135deg, var(--accent), var(--accent2));
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      margin: 16px 0;
    }
    .results-sub { color: var(--muted); font-size: 0.95rem; margin-bottom: 32px; }
    .results-actions { display: flex; gap: 12px; justify-content: center; flex-wrap: wrap; }
    .btn-secondary {
      padding: 12px 24px;
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: 10px;
      color: var(--text);
      font-family: inherit;
      font-size: 0.9rem;
      cursor: pointer;
      transition: border-color 0.2s;
    }
    .btn-secondary:hover { border-color: var(--accent); }
    .btn-primary {
      padding: 12px 24px;
      background: linear-gradient(135deg, var(--accent), var(--accent2));
      border: none;
      border-radius: 10px;
      color: #fff;
      font-family: 'Syne', sans-serif;
      font-weight: 700;
      font-size: 0.9rem;
      cursor: pointer;
      transition: opacity 0.2s;
    }
    .btn-primary:hover { opacity: 0.88; }
  </style>
</head>
<body>

<div class="blob blob-1"></div>
<div class="blob blob-2"></div>

<!-- ════ SCREEN 1: HOME ════ -->
<div id="home" class="screen active">
  <div class="site-header">
    <div class="site-logo">📚</div>
    <span class="site-title">Bank de Qüestionaris</span>
  </div>

  <h1 class="hero-title" style="font-family: 'Outfit', sans-serif;">DG Fisioteràpia<br/>i INEF UdL<br/><span style="font-size:0.55em; -webkit-text-fill-color: var(--muted); background:none;">Àlex Sánchez</span></h1>
  <p class="hero-sub">Selecciona el grau i l'assignatura per accedir als qüestionaris i posar a prova els teus coneixements.</p>

  <!-- GRAU: FISIOTERÀPIA -->
  <div class="degree-section">
    <div class="degree-label">Grau universitari</div>
    <div class="degree-name"><span>Fisioteràpia</span></div>
    <div class="subjects-grid" id="grid-fisio"></div>
  </div>

  <!-- GRAU: INEF -->
  <div class="degree-section">
    <div class="degree-label">Grau universitari</div>
    <div class="degree-name"><span>INEF</span></div>
    <div class="subjects-grid" id="grid-inef"></div>
  </div>
</div>

<!-- ════ SCREEN 2: SUBJECT ════ -->
<div id="subject-screen" class="screen">
  <button class="back-btn" onclick="goHome()">← Tornar</button>
  <div class="subject-header">
    <div class="tag" id="sub-degree-tag"></div>
    <h2 id="sub-name"></h2>
  </div>
  <div class="quiz-list" id="quiz-list"></div>
</div>

<!-- ════ SCREEN 3: QUIZ ════ -->
<div id="quiz-screen" class="screen">
  <div class="quiz-top">
    <button class="back-btn" onclick="goSubject()" style="margin:0">← Tornar</button>
    <div class="quiz-progress-wrap">
      <div class="quiz-progress-label" id="progress-label">Pregunta 1 de 10</div>
      <div class="quiz-progress-bar"><div class="quiz-progress-fill" id="progress-fill" style="width:0%"></div></div>
    </div>
    <div style="width:80px"></div>
  </div>

  <div id="quiz-content"></div>
</div>

<script>
// ════════════════════════════════════════
//  DATA
// ════════════════════════════════════════
const data = {
  fisio: {
    label: "Fisioteràpia",
    subjects: [
      {
        id: "fisiopato2",
        name: "Fisiopatologia 2",
        icon: "🫀",
        quizzes: [
          {
            title: "Qüestionari d'exemple",
            questions: [
              {
                q: "Qüestionari pendent d'afegir.",
                options: ["Opció A", "Opció B", "Opció C", "Opció D"],
                answer: 0
              }
            ]
          }
        ]
      },
      {
        id: "proced2",
        name: "Procediments 2",
        icon: "🩺",
        quizzes: [
          {
            title: "Qüestionari d'exemple",
            questions: [
              {
                q: "Qüestionari pendent d'afegir.",
                options: ["Opció A", "Opció B", "Opció C", "Opció D"],
                answer: 0
              }
            ]
          }
        ]
      }
    ]
  },
  inef: {
    label: "INEF",
    subjects: [
      {
        id: "teoria2",
        name: "Teoria de l'Entrenament 2",
        icon: "🏋️",
        quizzes: [
          {
            title: "Test 2 — Nivell Avançat i Llegendari (50 preguntes)",
            questions: [
              { q: "Un esportista realitza 6 repeticions amb una càrrega amb la qual podria fer 10. Quin és el seu RIR i com es relaciona amb el caràcter de l'esforç (CE)?", options: ["RIR 4, CE baix: fa menys de la meitat de les possibles, poca pèrdua de velocitat i poca fatiga generada", "RIR 6, CE alt: supera la meitat de les possibles i genera fatiga neuromuscular elevada", "RIR 4, CE alt: superar el 50% de les possibles implica CE alt per definició", "RIR 4, CE mig: exactament la meitat de les repeticions possibles s'han realitzat"], answer: 0 },
              { q: "Dos atletes A i B fan el mateix exercici amb la mateixa intensitat relativa (75% 1RM). A fa 8 repeticions màximes i B en fa 14. Quin dels dos ha generat més fatiga i per quin motiu?", options: ["A, perquè treballa a un percentatge de la RM més elevat en valors absoluts", "Ambdós han generat la mateixa fatiga perquè treballen al mateix %1RM", "A, perquè amb menys repeticions possibles té una RFD màxima superior", "B, perquè ha fet més repeticions amb la mateixa intensitat relativa, cosa que implica major pèrdua de velocitat i major estrès metabòlic i mecànic"], answer: 3 },
              { q: "Segons el model de sistemes dinàmics, quin rol assumeix el preparador físic respecte a l'entrenament de força?", options: ["Expert en biomecànica que prescriu exercicis analítics per corregir els punts febles de l'esportista", "Supervisor de càrregues progressives basat en el model analític de sobrecàrrega", "Dissenyador d'experiències que contextualitza els estímuls perquè l'esportista s'autoorganitzi", "Tècnic que suma les parts individuals de l'esportista per obtenir el rendiment total"], answer: 2 },
              { q: "Relaciona: una esportista de voleibol necessita maximitzar el salt vertical. Sabent que el CEA ràpid és <250 ms, quin tipus de tensió muscular i manifestació de força predominen en el seu salt?", options: ["Tensió fàsica i manifestació activa explosiva (SJ sense contramoviment)", "Tensió reactiva-balística-explosiva i manifestació reflex elàstic-explosiva (CEA ràpid, DJ)", "Tensió tònica-explosiva i manifestació estàtica màxima (PIMA)", "Tensió fàsica-tònica i manifestació dinàmica màxima relativa"], answer: 1 },
              { q: "Un subjecte entrena squat durant 8 setmanes i la seva 1RM passa de 80 kg a 100 kg. Quin paràmetre confirma que ha millorat la força aplicada i no simplement ha après la tècnica?", options: ["Que la velocitat d'execució amb 80 kg ha augmentat després de les 8 setmanes d'entrenament", "Que el nombre de repeticions amb 80 kg s'ha incrementat de 8 a 14", "Que la freqüència de sessions setmanals era de 3 o més", "Que l'RPE reportat per l'esportista ha disminuït amb la mateixa càrrega"], answer: 0 },
              { q: "Quin efecte té un gran dèficit de força sobre la transferència d'un increment de la 1RM al rendiment esportiu?", options: ["Augmenta la transferència perquè el potencial de millora és major", "No té cap efecte sobre la transferència, que depèn exclusivament de l'especificitat de l'exercici", "El dèficit de força és irrellevant en esports on la velocitat és el factor limitant", "Redueix la transferència: si el dèficit és gran, incrementar la força màxima no garanteix millora en el gest específic de competició"], answer: 3 },
              { q: "Per quin motiu la tensió muscular elevada no implica necessàriament una força aplicada elevada?", options: ["Perquè la tensió muscular és una mesura subjectiva que no es pot quantificar externament", "Perquè la tensió muscular i la força aplicada es mesuren en unitats diferents (fisiològica vs. mecànica)", "Perquè la força aplicada depèn de la resistència externa i el temps disponible; una tensió elevada contra una resistència immòbil genera poca força aplicada (no hi ha moviment)", "Perquè les fibres tipus I generen menys tensió que les tipus II però apliquen més força en moviments lents"], answer: 2 },
              { q: "Un llançador de javelina usa la tensió balística-explosiva. En quina categoria de Julio Tous s'emmarca el seu gest i quina característica CEA el defineix?", options: ["Manifestació reactiva elàstic-explosiva, CEA lent >250 ms, amb fase excèntrica d'estirament prolongat", "Manifestació reactiva reflex elàstic-explosiva, CEA ràpid <250 ms, amb fort reflex miotàtic", "Manifestació activa explosiva màxima (SJ), sense contramoviment ni estirament previ", "Manifestació estàtica màxima (PIMA), amb contracció isomètrica contra resistència insalvable"], answer: 0 },
              { q: "Quina relació lògica existeix entre la rigidesa músculo-tendinosa i el rendiment en exercicis de CEA?", options: ["Una rigidesa elevada redueix el rendiment en CEA perquè limita el rang de moviment i l'estirament previ", "Una rigidesa adequada millora l'emmagatzematge i alliberament d'energia elàstica en el CEA, augmentant l'eficiència mecànica i la transferència de força", "La rigidesa músculo-tendinosa és irrellevant en el CEA perquè l'energia elàstica prové exclusivament del reflex miotàtic", "Una rigidesa elevada perjudica la coordinació intramuscular però millora la coordinació intermuscular"], answer: 1 },
              { q: "Relaciona el concepte de 'freqüència d'impuls neuronal' amb la millora de la força màxima: quin mecanisme neuronal predomina quan treballem amb càrregues superiors al 85% 1RM?", options: ["La coordinació intermuscular, ja que sincronitza els músculs antagonistes per reduir la co-activació", "El reflex miotàtic, que s'activa especialment davant de càrregues supramàximes", "La reducció dels òrgans de Golgi, que permet una major activació sense inhibició protectora", "La freqüència d'impuls i la sincronització intramuscular, ja que càrregues altes requereixen activar les unitats motores d'alta llindar a freqüències màximes"], answer: 3 },
              { q: "Un preparador física vol millorar la força en nens de 10 anys. Quin criteri ha de prioritzar per adaptar la càrrega, i per quin motiu?", options: ["L'edat cronològica, ja que és el referent estàndard en pediatria per calcular la dosi d'exercici", "El training age exclusivament, ja que les adaptacions depenen de l'experiència i no de la maduració", "L'edat biològica (PHV), perquè dos nens de 10 anys cronològics poden tenir madurations físiques molt diferents", "El pes corporal, perquè és el paràmetre més objectiu per escalar la càrrega en entrenament pediàtric"], answer: 2 },
              { q: "Quina és la implicació pràctica del fet que el rang de repeticions possibles davant d'una mateixa intensitat relativa sigui molt ampli (p.ex. 50-85% 1RM)?", options: ["Que programar el màxim de repeticions per a tots els subjectes farà que cadascun entreni a una intensitat relativa diferent, generant graus de fatiga dispars", "Que el %1RM és un indicador fiable de la càrrega perquè permet predir el nombre exacte de repeticions per a cada esportista", "Que la velocitat d'execució és irrellevant si es controla el percentatge de la 1RM", "Que el caràcter d'esforç és idèntic per a tots els subjectes que fan la mateixa sèrie"], answer: 0 },
              { q: "En el model de Paco Seirul·lo, quin nivell d'aproximació (0-5) és el més adequat per a un entrenament on l'esportista fa exercicis tècnics amb oposició real i presa de decisions específiques del joc?", options: ["Nivell 0-1: exercicis generals que treballen la musculatura principal de forma inespecífica", "Nivell 2: exercicis que imiten el gest tècnic amb una petita sobrecàrrega sense oposició", "Nivell 3: cooperació-oposició sense presa de decisions complexes", "Nivell 4-5: exercicis de naturalesa i organització similars a la competició amb oposició i alt component cognitiu"], answer: 3 },
              { q: "Quina és la diferència funcional entre hipertròfia sarcomèrica i sarcoplasmàtica en termes de pes de competició i rendiment?", options: ["La sarcomèrica augmenta el pes corporal més que la sarcoplasmàtica i per tant perjudica la força relativa", "La sarcomèrica augmenta els elements contràctils (força real), mentre la sarcoplasmàtica augmenta fluids i glucogen sense guany proporcional de força, perjudicant la força relativa en esports de pes per categoria", "Ambdues milloren la força màxima de manera idèntica però per mecanismes contràctils diferents", "La sarcoplasmàtica és superior per a esports de potència i la sarcomèrica per a esports d'endurança"], answer: 1 },
              { q: "Per quin motiu els entrenaments amb alta pèrdua de velocitat potencien les fibres tipus I en detriment de les IIx, tot i que l'entrenament comenci activant les fibres d'alta freqüència?", options: ["Perquè les fibres IIx es converteixen progressivament en fibres IIa i tipus I quan s'entrena amb alta fatiga", "Perquè la pèrdua de velocitat inhibeix el reclutament de les UM d'alta llindar per mecanisme reflex", "Perquè les IIx s'esgoten a les primeres repeticions i, en acumular fatiga, s'activen les tipus I per mantenir la producció de força, sent les que reben l'estímul d'entrenament principal", "Perquè a alta fatiga, la freqüència d'impuls neuronal disminueix i només les fibres tipus I responen a freqüències baixes"], answer: 2 },
              { q: "Compara l'efecte de l'entrenament concèntric i l'excèntric sobre el dany muscular i la capacitat de recuperació: quina combinació és més recomanable 3 dies abans d'una competició?", options: ["Concèntric o excèntric de baixa intensitat i volum, perquè generen menys DOMS i fatiga neuromuscular i permeten arribar fresc a la competició", "Excèntric d'alta intensitat, perquè genera el major estímul d'adaptació estructural i prepara el múscul per a l'esforç competitiu", "Isomètric PIMA d'alta intensitat, perquè millora la RFD sense generar DOMS", "Cap tipus d'entrenament de força, ja que qualsevol estímul 3 dies abans compromet el rendiment"], answer: 0 },
              { q: "En la corba F-T, per quin motiu la RFD màxima es produeix a la fase isomètrica ABANS del moviment i no al pic de força màxima?", options: ["Perquè la fase isomètrica permet reclutar totes les UM simultàniament sense el fre de la resistència externa", "Perquè durant el moviment les fibres IIx ja estan parcialment activades i no poden incrementar la velocitat d'activació", "Perquè el reflex miotàtic augmenta temporalment la pendent de la corba en la fase isomètrica inicial", "Perquè la pendent de la corba (ΔF/Δt) és màxima en l'inici de l'aplicació de força quan el múscul és al 30% FIM, moment de major acceleració de la corba, que s'aplana a mesura que s'aproxima al pic"], answer: 3 },
              { q: "Un esportista de powerlifting té una 1RM de 200 kg al squat però tarda 1,2 segons a assolir-la. Un velocista té una 1RM de 120 kg però arriba al 80% en 0,15 s. Quin dels dos té millor força útil per al seu esport i per quin motiu?", options: ["El powerlifter, perquè la força absoluta és el factor determinant en qualsevol manifestació de força", "El velocista, perquè en el seu esport el temps disponible per aplicar força és inferior als 0,2 s i necessita expressar un alt percentatge de la seva força en molt poc temps (RFD elevada)", "El powerlifter, perquè amb major força màxima sempre tindrà major força útil independentment del temps", "El velocista, perquè l'entrenament de velocitat millora la coordinació intramuscular fins a superar la força absoluta del powerlifter"], answer: 1 },
              { q: "Quina diferència hi ha entre el tonatge setmanal i el nombre de sèries efectives com a indicadors de volum, i quan és preferible usar cadascun?", options: ["El tonatge és sempre superior com a indicador perquè inclou la càrrega absoluta; les sèries efectives s'usen només en fase de hipertròfia", "Ambdós indicadors mesuren exactament el mateix i es poden usar de forma intercanviable", "El tonatge (sèries × reps × kg) mesura el volum total però no el grau d'esforç; les sèries efectives (Beardsley) consideren el RIR, prioritzant la proximitat al fallo com a indicador de qualitat de l'estímul", "El tonatge s'usa en esports de força i les sèries efectives en esports de resistència"], answer: 2 },
              { q: "Si la pèrdua de velocitat entre la primera i l'última repetició d'una sèrie és un indicador de fatiga, quin tipus d'entrenament generarà major augment de la concentració d'amoni intramuscular?", options: ["Entrenament amb 4 sèries de 12 repeticions al 70% 1RM fins a la fallada, on es fan més d'1-2 reps sobre la meitat de les possibles", "Entrenament amb 3 sèries de 3 repeticions al 90% 1RM amb pèrdua de velocitat <10%", "Entrenament isomètric HIMA amb temps sota tensió de 30 segons per sèrie", "Entrenament pliomètric amb Drop Jumps a <250 ms de temps de contacte"], answer: 0 },
              { q: "Quin és el mecanisme pel qual l'entrenament de força té un efecte analgèsic en el dolor menstrual?", options: ["Perquè el treball isomètric redueix directament la contractilitat uterina per inhibició neuronal segmentàri", "Per alliberament d'endorfines i millora del flux sanguini pelvià durant l'exercici, que redueixen la percepció del dolor", "Perquè l'augment de testosterona derivat de l'entrenament suprimeix la producció de prostaglandines", "Perquè la càrrega mecànica sobre el cos vertebral redueix la sensibilitat als canvis hormonals del cicle"], answer: 1 },
              { q: "Relaciona: l'ús del trineu com a material d'entrenament i el seu efecte sobre la tècnica de sprint. Quin és el llindar de càrrega i per quin motiu?", options: ["Màxim 10% del pes corporal per a qualsevol objectiu, perquè per sobre d'aquest valor la tècnica es degrada sempre", "El trineu no té un llindar establert perquè la càrrega òptima depèn exclusivament del vector de força horitzontal aplicat", "Mínim 30% del pes corporal per garantir un estímul suficient sobre la fase d'acceleració de l'esprint", "Per replicar la tècnica de sprint: no superar el 20% del pes corporal; per millorar la força màxima: es poden usar càrregues superiors al 20% en esportistes entrenats, però amb contraversia"], answer: 3 },
              { q: "Per quin motiu la força relativa (força/pes corporal) és un millor predictor del rendiment que la força absoluta en la majoria d'esports?", options: ["Perquè en la majoria d'accions esportives l'esportista ha de desplaçar i accelerar el seu propi cos, i qui pot expressar més força en relació al seu pes tindrà avantatge en salts, sprints i canvis de direcció", "Perquè la força relativa s'incrementa sempre de manera lineal amb l'entrenament, independentment del pes corporal", "Perquè la força relativa inclou la força útil i la força absoluta en un únic indicador", "Perquè la força absoluta és irrellevant en esports que no requereixen aixecar objectes externs"], answer: 0 },
              { q: "Com afecta la variable 'eficiència energètica' a la decisió d'induir hipertròfia en un fondista?", options: ["Positivament: més massa muscular implica major reserva de glucogen i millor rendiment en distàncies llargues", "Neutralment: l'eficiència energètica no depèn de la massa muscular sinó de la tècnica de carrera", "Negativament: un múscul més hipertrofiat és menys eficient energèticament (requereix més oxigen per acció), cosa que penalitza l'economia de moviment del fondista", "Positivament: la hipertròfia sarcoplasmàtica augmenta el glucogen intramuscular sense augmentar el cost metabòlic"], answer: 2 },
              { q: "Quina relació existeix entre l'índex de dificultat i el caràcter d'esforç, i en quin tipus d'exercici s'utilitza cadascun?", options: ["Ambdós mesuren la mateixa variable; s'usa el caràcter d'esforç en entrenament de força i l'índex de dificultat en resistència", "El caràcter d'esforç (reps fetes/reps possibles) s'usa en exercicis dinàmics amb repeticions comptables; l'índex de dificultat (escala 0-1 × temps) s'usa en exercicis sense repeticions com la planxa isomètrica", "L'índex de dificultat és la versió numèrica del RPE i es pot usar intercanviablement amb el caràcter d'esforç", "El caràcter d'esforç s'aplica exclusivament a exercicis multiarticulars i l'índex de dificultat a exercicis monoarticulars"], answer: 1 },
              { q: "En l'entrenament de força durant l'embaràs, per quin motiu s'ha d'evitar el decúbit supí a partir de la setmana 16-20 i quina és la implicació mecànica?", options: ["Perquè en decúbit supí la pressió intraabdominal augmenta i pot provocar diastàsi de rectes", "Perquè en decúbit supí la laxitud provocada per la relaxina fa inestables les articulacions sacroilíaques", "Perquè la posició horitzontal augmenta la freqüència cardíaca materna fins a valors que comprometen el fetus", "Perquè el pes de l'úter comprimit sobre la vena cava pot reduir el retorn venós i la perfusió placentària, afectant el fetus"], answer: 3 },
              { q: "Quina és la diferència entre l'estirada neuromuscular en nois i l'absència d'aquest fenomen en noies durant l'adolescència, i quina implicació té per a la programació?", options: ["En nois hi ha un augment brusc de força i coordinació vinculat a la testosterona i la maduració; en noies, el guany és més lineal. Per tant, les noies necessiten treball de força estructurat ABANS del PHV per no quedar-se sense una base neuromuscular sòlida", "Les noies presenten una estirada neuromuscular equivalent però més tardana (als 16 anys), cosa que retarda la introducció del treball de força", "La diferència és purament hormonal i no afecta la programació de l'entrenament de força", "Les noies presenten una estirada cognitiva superior que compensa la manca d'estirada neuromuscular"], answer: 0 },
              { q: "En el context de la sarcopènia i l'entrenament en gent gran, quin és l'efecte de la progressió d'augment de repeticions ABANS d'augmentar sèries, i per quin motiu?", options: ["Perquè augmentar sèries és menys agressiu que augmentar repeticions en gent gran, ja que cada sèrie es fa amb menys fatiga acumulada", "Perquè en gent gran les adaptacions neurals requereixen primer un augment de les repeticions per consolidar el patró motor", "Perquè augmentar sèries suposa un increment molt major del volum total (totes les repeticions de la nova sèrie d'un cop), mentre que augmentar repeticions és un increment gradual i més controlable", "Perquè el principi de reversibilitat fa que les sèries es perdin abans que les repeticions si es deixa d'entrenar"], answer: 2 },
              { q: "Relaciona la definició mecànica i fisiològica de força: quin escenari trenca la suposada coherència entre ambdues definicions?", options: ["Un exercici concèntric ràpid, on la velocitat és alta però la tensió muscular és baixa", "Un exercici isomètric màxim: la tensió muscular és màxima (fisiològic) però la força aplicada és zero o molt baixa (mecànic) perquè no hi ha desplaçament de la resistència externa", "Un exercici excèntric lent, on la tensió és màxima i la velocitat és mínima però sí hi ha força aplicada", "Un sprint, on la força aplicada és elevada però la tensió muscular és baixa perquè les fibres es contrauen a alta velocitat"], answer: 1 },
              { q: "Quin impacte té la reducció de l'activitat dels òrgans de Golgi amb l'entrenament de força, i com es relaciona amb la millora de la coordinació intramuscular?", options: ["Els òrgans de Golgi s'eliminen progressivament amb l'entrenament, permetent contraccions sense límit de seguretat", "L'entrenament activa els òrgans de Golgi més d'hora, augmentant la sensibilitat propioceptiva i millorant la coordinació", "La reducció dels òrgans de Golgi millora exclusivament la coordinació intermuscular, no la intramuscular", "L'entrenament fa que els òrgans de Golgi s'activin més tard (major llindar d'inhibició), permetent al múscul generar tensions més elevades abans de l'inhibició protectora, millorant la coordinació entre agonista i sinergista"], answer: 3 },
              { q: "Com afecta l'ús de material d'inestabilitat (BOSU, discos propioceptius) a la capacitat de millorar la força màxima, i quan és recomanable el seu ús?", options: ["Millora la força màxima de manera superior als exercicis estables perquè activa un major nombre d'unitats motores totals", "No té cap efecte sobre la força màxima i és equivalent als exercicis en superfície estable per a la hipertròfia", "Disminueix la producció de força màxima perquè augmenta la funció d'estabilització i redueix la força disponible per al moviment; és útil en salut, reeducació funcional i quan es busca millorar l'estabilitat lleu-moderada", "És recomanable en fase de hipertròfia perquè la instabilitat augmenta el temps sota tensió i l'activació dels músculs estabilitzadors"], answer: 2 },
              { q: "Explica la paradoxa: dos subjectes amb la mateixa força relativa (% de la seva 1RM) NO tenen la mateixa força aplicada davant la mateixa càrrega absoluta.", options: ["Perquè la força relativa no inclou la component gravitacional i per tant no és comparable entre subjectes de pesos corporals diferents", "Perquè la mateixa força relativa implica 1RM absolutes diferents. Si A té 1RM=100 kg i B té 1RM=150 kg, el 60% suposa 60 vs 90 kg, forces aplicades radicalment diferents", "Perquè la força aplicada depèn de la velocitat d'execució, que varia entre subjectes fins i tot amb la mateixa intensitat relativa", "Perquè la força relativa és un indicador estadístic poblacional que no es pot aplicar a comparacions individuals"], answer: 1 },
              { q: "Quin és el risc principal d'usar el XRM promig (p.ex. 10RM) com a mètode de quantificació de la intensitat en l'entrenament diari?", options: ["Que obliga a fer el màxim de repeticions en cada sèrie per mantenir la precisió, cosa que genera excés de fatiga, augmenta el risc de lesió i redueix la velocitat d'execució de totes les sèries", "Que és un mètode massa complex per usar en sessions pràctiques amb grups nombrosos", "Que sobreestima la intensitat en esportistes avançats perquè la seva 1RM és molt variable d'un dia a l'altre", "Que no permet comparar entre exercicis perquè cada exercici té un XRM diferent per la mateixa intensitat relativa"], answer: 0 },
              { q: "Per quin motiu la CK (creatina quinasa) és un marcador indirecte del dany muscular induït per l'exercici i no un indicador de rendiment?", options: ["Perquè la CK és una enzima que mesura el metabolisme aeròbic i no la potència anaeròbica pròpia de la força", "Perquè la CK té una vida mitja de 24 hores i per tant no reflecteix l'estat de fatiga aguda de la sessió", "Perquè la CK és un marcador cardiovascular i no muscular, i per tant no es relaciona amb l'entrenament de força", "Perquè la CK s'allibera del múscul quan hi ha dany a les membranes cel·lulars; mesura l'estrès i la destrucció tissular, no la capacitat contràctil ni el rendiment directe de l'esportista"], answer: 3 },
              { q: "Quin és el fonament fisiològic que justifica que la distribució del volum en petites dosis (alta freqüència) millori el rendiment en força respecte a sessions de volum alt poc freqüents?", options: ["Perquè la freqüència alta redueix la percepció de l'esforç (RPE) en cada sessió", "Perquè la distribució del volum en petites dosis redueix la concentració d'amoni i permet treballar sempre a velocitat màxima", "Perquè la síntesi proteica muscular es manté elevada durant més hores si els estímuls s'escampen al llarg de la setmana, maximitzant les adaptacions estructurals", "Perquè l'augment de freqüència compensa automàticament la reducció de la intensitat"], answer: 2 },
              { q: "En la llegenda de Miló de Cotorna, quin principi de l'entrenament modern il·lustra i com es relaciona amb el concepte actual de progressió?", options: ["Il·lustra el principi de sobrecàrrega progressiva: incrementar gradualment la càrrega (bou creixent) per generar adaptació contínua, base de la progressió moderna en entrenament de força", "Il·lustra el principi d'especificitat: Miló entrenava sempre amb el mateix bou per adaptar-se específicament a les seves característiques", "Il·lustra el principi de variabilitat: canviar d'estímul cada dia per evitar l'estancament adaptatiu", "Il·lustra el principi de reversibilitat: si Miló deixa d'entrenar, perd la força guanyada"], answer: 0 },
              { q: "Com es relacionen el PHV, els canvis biomecànics de les noies i la major incidència de lesions de LCA en l'adolescència femenina?", options: ["Les noies amb PHV tardà tenen més risc perquè el seu sistema musculoesquelètic no ha madurat completament", "L'os creix més ràpid que el múscul durant el PHV; els canvis biomecànics (major valg de genoll, menor activació isquiosural) es combinen amb el desequilibri músculo-ossi per augmentar el moment d'adducció de genoll, factor de risc principal del LCA", "El major greix corporal post-PHV en noies augmenta la càrrega sobre el genoll, que és el factor principal de risc de LCA", "El risc de LCA en noies ve exclusivament de la laxitud articular causada pels estrògens, sense relació amb el control neuromuscular"], answer: 1 },
              { q: "Un preparador físic programa 20 sèries setmanals de quàdriceps per a un esportista entrenat. Tenint en compte la proposta de Baz-Valle et al. (2022), estaria dins del rang òptim?", options: ["Sí, 20 sèries és exactament el límit superior del rang òptim (12-20) i per tant és el màxim recomanat", "No, 20 sèries supera clarament el rang òptim i s'associa sempre amb sobreentrenament", "Sí, perquè per a esportistes entrenats el rang òptim és 20-30 sèries setmanals", "Sí però al límit: 12-20 sèries és el rang òptim; 20 sèries ja comença la transició cap al rang alt (>20), on els guanys poden ser majors però també el risc de sobrecarregar"], answer: 3 },
              { q: "Quin és el raonament que explica per quin motiu les adaptacions inicials a l'entrenament de força en una persona sedentària són NEURONALS i no estructurals?", options: ["Perquè el sistema nerviós és més sensible als estímuls externs i per tant es modifica primer, seguint un ordre de maduresa tisular", "Perquè les cèl·lules musculars no poden dividir-se ni créixer fins que el sistema nerviós envia els senyals adequats de proliferació cel·lular", "Perquè l'organisme primer optimitza com utilitza els recursos musculars existents (reclutament, sincronització, coordinació) abans d'invertir en estructures noves (hipertròfia), que requereix més temps i energia", "Perquè la proteïna muscular disponible és insuficient al principi i per tant l'únic mecanisme possible és el neuronal"], answer: 2 },
              { q: "Quina és la relació entre el temps de descans entre sèries, el caràcter d'esforç i el tipus d'adaptació buscada?", options: ["Un descans curt (30-60 s) amb CE alt genera major estrès metabòlic (millor hipertròfia sarcoplasmàtica); un descans llarg (3-5 min) amb CE moderat permet mantenir la velocitat i qualitat, millor per a força màxima i hipertròfia sarcomèrica", "A major temps de descans, major acumulació de metabòlits i per tant millor adaptació per a hipertròfia sarcoplasmàtica", "El temps de descans no afecta el tipus d'adaptació, només la percepció subjectiva de l'esforç (RPE)", "Un descans de 2 minuts és universalment òptim per a totes les manifestacions de força i tipus d'hipertròfia"], answer: 0 },
              { q: "Quin problema metodològic fonamental impedeix extreure conclusions universals sobre l'impacte del cicle menstrual en el rendiment de força?", options: ["Que les esportistes d'elit no participen en estudis per motius de confidencialitat competitiva", "Que el rendiment de força és massa variable per mesurar-se amb precisió suficient en context de laboratori", "Que l'efecte hormonal és tan petit que queda emmascarat per les diferències individuals en tècnica i motivació", "Que la majoria d'estudis usen mostres petites, no fan confirmació hormonal objectiva del cicle i exclouen cicles irregulars, limitant la validesa externa de les conclusions"], answer: 3 },
              { q: "Un esportista vol saber si la seva força ha millorat en 8 setmanes d'entrenament sense fer un test de 1RM. Quin mètode és el més vàlid i per quin principi es fonamenta?", options: ["Mesurar si ha augmentat el nombre de repeticions màximes amb la mateixa càrrega absoluta", "Fer una avaluació subjectiva de la percepció de l'esforç (RPE) amb la mateixa càrrega", "Mesurar la velocitat d'execució de la primera repetició amb la mateixa càrrega absoluta: si la velocitat ha augmentat, la força aplicada ha millorat, sense necessitat de test de 1RM", "Comparar el tonatge setmanal actual amb el d'inici del mesocicle"], answer: 2 },
              { q: "En quin context és contraindicat usar material d'inestabilitat i per quin mecanisme fisiològic?", options: ["En tots els contextos, perquè la inestabilitat sempre redueix la força màxima i per tant és inferior als exercicis estables", "En gent gran amb risc de caigudes, perquè la destabilització pot generar lesions agudes i la millora de la força és menor", "Exclusivament en nens, perquè el seu sistema propioceptiu no està prou madur per processar la informació de la inestabilitat", "En esportistes d'alt rendiment, perquè la inestabilitat redueix la especificitat del gest esportiu"], answer: 1 },
              { q: "Relaciona la capacitat intrínseca (Intrinsic Capacity) de la OMS amb l'entrenament de força en gent gran: quins dominis millora directament l'entrenament de força?", options: ["La mobilitat (menys caigudes i millor desplaçament), la vitalitat (contra la sarcopènia), la funció cognitiva (estimulació neural), i parcialment la salut emocional (autoestima i benestar), abordant 4 dels 5 dominis", "Exclusivament la mobilitat i la vitalitat, que són els dominis físics més directament relacionats amb la massa muscular", "Cap domini de la capacitat intrínseca, perquè aquesta es mesura exclusivament per paràmetres clínics i no funcionals", "Únicament la funció sensorial, perquè l'entrenament de força millora la propioceptivitat i la resposta als estímuls exteriors"], answer: 0 },
              { q: "Quin error conceptual es comet quan s'afirma que 'fer 10 repeticions amb el 70% 1RM és equivalent per a tots els subjectes'?", options: ["Que el 70% 1RM implica masses absolutes molt diferents entre subjectes, cosa que fa que la fatiga muscular sigui diferent", "Que el 70% 1RM no és una intensitat vàlida per a la millora de la força màxima i per tant la comparació és irrellevant", "Que les 10 repeticions al 70% representen sempre un caràcter d'esforç màxim per definició", "Que el nombre de repeticions màximes davant el 70% 1RM varia molt entre individus (alguns poden fer 8, d'altres 16); dir que tots fan '10 reps al 70%' oculta que cadascun té un caràcter d'esforç completament diferent"], answer: 3 },
              { q: "Quina és la implicació de la fórmula F = m·(a+g) per entendre qui és 'més fort': un haltèrofil que aixeca 200 kg lentament o un esportista explosiu que fa 50 kg a màxima acceleració?", options: ["L'haltèrofil, perquè la massa (m) és la variable principal de la fórmula i 200 > 50", "Depèn del context i l'objectiu: l'haltèrofil genera més força total (m alta, a baixa) però l'esportista explosiu genera una acceleració molt major. Per a un gest esportiu d'alta velocitat, el segon pot ser 'més fort en context'", "L'esportista explosiu, perquè l'acceleració (a) és sempre més important que la massa (m) en qualsevol context esportiu", "Ambdós generen la mateixa força perquè F=m·a és una llei física invariable que s'equilibra sempre"], answer: 1 },
              { q: "Quin dels tres factors de la hipertròfia (tensió mecànica, estrès metabòlic, dany muscular) és el predominant en entrenaments de poca pèrdua de velocitat i alta intensitat relativa?", options: ["La tensió mecànica, ja que càrregues altes impliquen una gran força per fibra activada, que és l'estímul principal per a la hipertròfia sarcomèrica i les adaptacions de força", "L'estrès metabòlic, ja que les càrregues altes generen major acumulació de metabòlits com el lactat", "El dany muscular, perquè les càrregues altes provoquen microlesions estructurals superiors a les de càrregues baixes", "Tots tres factors contribueixen per igual independentment de la intensitat relativa"], answer: 0 },
              { q: "Per quin motiu González Badillo no considera la força-resistència com una manifestació de força pròpiament dita, a diferència de Joan Solé que sí la inclou?", options: ["Perquè González Badillo considera que la resistència és una capacitat coordinativa i no condicional", "Perquè la força-resistència implica sempre la component aeròbica que escapa a la definició mecànica de la força", "Perquè per a González Badillo la força-resistència no és una forma d'expressar la força sinó la capacitat de mantenir una expressió de força al llarg del temps; és una relació entre força i temps, no una manifestació nova de la força", "Perquè González Badillo basa la seva classificació exclusivament en les corbes F-T i F-V, on la força-resistència no té representació gràfica"], answer: 2 },
              { q: "Un Vivifrail amb nivell funcional baix (alt risc de caigudes) fa el test de la cadira en 30 s i obté 5 repeticions. Quin programa seria adequat i quin principi es prioritza?", options: ["Programa d'alta intensitat (80% 1RM) per compensar ràpidament la pèrdua de força, prioritzant el principi de sobrecàrrega", "Programa exclusivament aeròbic (bicicleta) fins que assoleixi les 8 repeticions en el test de la cadira", "Cap programa d'exercici fins obtenir autorització mèdica, perquè <8 repeticions és una contraindicació absoluta", "Programa adaptant la seguretat, posicions estables, exercicis funcionals simples (assentar-se i aixecar-se) amb progressió molt gradual, prioritzant la reversibilitat i l'adherència per sobre de la intensitat"], answer: 3 },
              { q: "Quina variable de la càrrega és 'dominant' en esportistes experts i per quin motiu es considera el paràmetre que genera millores en força?", options: ["El volum, perquè els esportistes experts necessiten major estimulació total per superar el llindar d'adaptació", "La intensitat, perquè en esportistes experts les adaptacions additionals requereixen estímuls d'alta exigència que el volum per si sol no pot proporcionar", "La freqüència, perquè més sessions setmanals garanteixen major síntesi proteica setmanal", "La densitat (relació treball/descans), perquè determina l'acumulació de fatiga i el caràcter de l'esforç"], answer: 1 },
              { q: "Relaciona l'efecte creuat i el dèficit bilateral amb la programació per a jugadors de tennis o esports unilaterals dominants.", options: ["El dèficit bilateral obliga a entrenar sempre bilateralment per compensar el desequilibri entre extremitats", "El dèficit bilateral no existeix en esports unilaterals perquè la dominant sempre compensa la no dominant", "En esports unilaterals, el dèficit bilateral és difícil de compensar bilateralment; cal entrenar unilateralment (manuelles) per mantenir-lo, i usar l'efecte creuat (l'aprenentatge d'un hemisferi ajuda l'altre) per optimitzar les adaptacions de l'extremitat no dominant", "L'efecte creuat és exclusiu d'exercicis d'endurança i no té relació amb l'entrenament de força"], answer: 2 },
              { q: "Quin és el fonament per prioritzar el treball de sòl pelvià i extremitats inferiors durant l'entrenament de força en l'embaràs?", options: ["Perquè el sòl pelvià ha de suportar el pes creixent de l'úter i és fonamental per al part; les extremitats inferiors mantenen la mobilitat i funcionalitat diàries, reduint dolors lumbars i preparant el cos per al part", "Perquè les extremitats inferiors no es veuen afectades per la relaxina i per tant poden entrenar a major intensitat", "Perquè el treball de tren superior és contraindicat durant l'embaràs per la pressió que genera sobre el fetus", "Perquè els canvis en el centre de gravetat fan que el tren superior sigui biomecànicament inaccessible durant el segon i tercer trimestre"], answer: 0 },
              { q: "Si un entrenament genera respostes agudes d'augment de fibres IIa i disminució de fibres IIx, quin tipus d'entrenament (pèrdua de velocitat alta o baixa) ha predominat i per quin mecanisme?", options: ["Pèrdua de velocitat baixa: les fibres IIx es converteixen en IIa quan el volum és insuficient per activar-les completament", "Pèrdua de velocitat baixa: treballa exclusivament les IIx perquè la velocitat alta requereix fibres ràpides", "La transició IIx → IIa és independent de la pèrdua de velocitat i depèn únicament del volum total d'entrenament", "Pèrdua de velocitat alta: els entrenaments amb moltes reps i alta fatiga fan que les IIx (molt demandades però que s'esgoten) tendeixin a convertir-se en les més resistents a la fatiga (IIa), mentre que amb poca pèrdua de velocitat les IIx es mantenen o augmenten"], answer: 3 },
              { q: "Quina és la diferència entre 'freqüència d'estímuls' i 'freqüència de sessions' i per quin motiu és una distinció important en la programació?", options: ["La freqüència de sessions sempre és igual o superior a la freqüència d'estímuls per definició", "La freqüència d'estímuls (suma de sèries i reps d'un patró o grup muscular) pot ser diferent de la freqüència de sessions: es pot entrenar el mateix grup muscular 3 cops per setmana en 2 sessions totals, distribuint l'estímul entre sessions", "Ambdós termes són sinònims; la distinció és merament terminològica sense implicació pràctica", "La freqüència d'estímuls es refereix als estímuls neurals i la de sessions als estímuls estructurals (hipertròfia)"], answer: 1 }
            ]
          }
        ]
      }
    ]
  }
};

// ════════════════════════════════════════
//  STATE
// ════════════════════════════════════════
let currentDegreeKey = null;
let currentSubjectIndex = null;
let currentQuizIndex = null;
let currentQuestionIndex = 0;
let score = 0;
let answered = false;

// ════════════════════════════════════════
//  RENDER HOME
// ════════════════════════════════════════
function renderHome() {
  renderSubjectsGrid('grid-fisio', 'fisio');
  renderSubjectsGrid('grid-inef', 'inef');
}

function renderSubjectsGrid(containerId, degreeKey) {
  const container = document.getElementById(containerId);
  const subjects = data[degreeKey].subjects;
  container.innerHTML = subjects.map((s, i) => `
    <div class="subject-card" onclick="goSubject2('${degreeKey}', ${i})">
      <div class="subject-icon">${s.icon}</div>
      <div class="subject-name">${s.name}</div>
      <div class="subject-meta">${s.quizzes.length} qüestionari${s.quizzes.length !== 1 ? 's' : ''}</div>
      <span class="subject-arrow">→</span>
    </div>
  `).join('');
}

// ════════════════════════════════════════
//  NAVIGATION
// ════════════════════════════════════════
function showScreen(id) {
  document.querySelectorAll('.screen').forEach(s => s.classList.remove('active'));
  document.getElementById(id).classList.add('active');
  window.scrollTo(0,0);
}

function goHome() { showScreen('home'); }

function goSubject2(degreeKey, subjectIndex) {
  currentDegreeKey = degreeKey;
  currentSubjectIndex = subjectIndex;
  const degree = data[degreeKey];
  const subject = degree.subjects[subjectIndex];

  document.getElementById('sub-degree-tag').textContent = degree.label;
  document.getElementById('sub-name').textContent = subject.name;

  const list = document.getElementById('quiz-list');
  list.innerHTML = subject.quizzes.map((quiz, qi) => `
    <div class="quiz-item" onclick="startQuiz(${qi})">
      <div class="quiz-item-left">
        <div class="quiz-num">${String(qi+1).padStart(2,'0')}</div>
        <div>
          <div class="quiz-title">${quiz.title}</div>
          <div class="quiz-count">${quiz.questions.length} preguntes</div>
        </div>
      </div>
      <button class="quiz-start-btn">Iniciar →</button>
    </div>
  `).join('');

  showScreen('subject-screen');
}

function goSubject() {
  goSubject2(currentDegreeKey, currentSubjectIndex);
}

// ════════════════════════════════════════
//  QUIZ LOGIC
// ════════════════════════════════════════
function startQuiz(quizIndex) {
  currentQuizIndex = quizIndex;
  currentQuestionIndex = 0;
  score = 0;
  showScreen('quiz-screen');
  renderQuestion();
}

function renderQuestion() {
  const quiz = data[currentDegreeKey].subjects[currentSubjectIndex].quizzes[currentQuizIndex];
  const total = quiz.questions.length;
  const q = quiz.questions[currentQuestionIndex];
  answered = false;

  // progress
  const pct = (currentQuestionIndex / total) * 100;
  document.getElementById('progress-fill').style.width = pct + '%';
  document.getElementById('progress-label').textContent = `Pregunta ${currentQuestionIndex+1} de ${total}`;

  const letters = ['A','B','C','D','E'];
  const html = `
    <div class="question-card">
      <div class="question-number">Pregunta ${currentQuestionIndex+1}</div>
      <div class="question-text">${q.q}</div>
      <div class="options">
        ${q.options.map((opt, i) => `
          <div class="option" id="opt-${i}" onclick="selectOption(${i})">
            <span class="option-letter">${letters[i]}</span>
            ${opt}
          </div>
        `).join('')}
      </div>
    </div>
    <div class="feedback-msg" id="feedback"></div>
    <button class="next-btn" id="next-btn" onclick="nextQuestion()">
      ${currentQuestionIndex + 1 < total ? 'Següent pregunta →' : 'Veure resultats →'}
    </button>
  `;
  document.getElementById('quiz-content').innerHTML = html;
}

function selectOption(index) {
  if (answered) return;
  answered = true;

  const quiz = data[currentDegreeKey].subjects[currentSubjectIndex].quizzes[currentQuizIndex];
  const q = quiz.questions[currentQuestionIndex];
  const correct = q.answer === index;

  document.querySelectorAll('.option').forEach(o => o.classList.add('disabled'));

  const selected = document.getElementById('opt-' + index);
  selected.classList.add(correct ? 'correct' : 'wrong');

  if (!correct) {
    document.getElementById('opt-' + q.answer).classList.add('correct');
  }

  if (correct) score++;

  const fb = document.getElementById('feedback');
  fb.className = 'feedback-msg show ' + (correct ? 'correct' : 'wrong');
  fb.textContent = correct ? '✓ Correcte!' : `✗ Incorrecte. La resposta correcta era: ${q.options[q.answer]}`;

  document.getElementById('next-btn').classList.add('show');
}

function nextQuestion() {
  const quiz = data[currentDegreeKey].subjects[currentSubjectIndex].quizzes[currentQuizIndex];
  currentQuestionIndex++;

  if (currentQuestionIndex >= quiz.questions.length) {
    showResults(quiz.questions.length);
  } else {
    renderQuestion();
  }
}

function showResults(total) {
  document.getElementById('progress-fill').style.width = '100%';
  document.getElementById('progress-label').textContent = 'Completat!';

  const pct = Math.round((score / total) * 100);
  let emoji = pct >= 80 ? '🏆' : pct >= 60 ? '👍' : '📖';
  let msg = pct >= 80 ? 'Excel·lent!' : pct >= 60 ? 'Bon resultat!' : 'Segueix practicant!';

  document.getElementById('quiz-content').innerHTML = `
    <div class="results-card">
      <div class="results-emoji">${emoji}</div>
      <div class="results-title">${msg}</div>
      <div class="results-score">${score}/${total}</div>
      <div class="results-sub">${pct}% de respostes correctes</div>
      <div class="results-actions">
        <button class="btn-secondary" onclick="startQuiz(${currentQuizIndex})">🔁 Repetir</button>
        <button class="btn-primary" onclick="goSubject()">← Tornar a l'assignatura</button>
      </div>
    </div>
  `;
}

// ════════════════════════════════════════
//  INIT
// ════════════════════════════════════════
renderHome();
</script>
</body>
</html>
