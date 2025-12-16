<!doctype html>
<html lang="fr">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Flashcards Cybersécurité</title>
  <style>
    :root { --max: 860px; --r: 16px; }
    body { font-family: system-ui, -apple-system, Segoe UI, Roboto, Arial, sans-serif; margin: 0; background: #0b1220; color: #e9eefb; }
    .wrap { max-width: var(--max); margin: 0 auto; padding: 18px; }
    .card { background: rgba(255,255,255,.06); border: 1px solid rgba(255,255,255,.12); border-radius: var(--r); padding: 16px; box-shadow: 0 10px 30px rgba(0,0,0,.25); }
    h1 { font-size: 18px; margin: 0 0 10px; }
    .meta { display:flex; gap:10px; flex-wrap:wrap; opacity:.9; font-size: 13px; }
    .pill { padding: 6px 10px; border-radius: 999px; background: rgba(255,255,255,.08); border: 1px solid rgba(255,255,255,.12); }
    .situation { font-size: 16px; line-height: 1.45; margin: 14px 0 12px; }
    .missing { display:inline-block; min-width: 140px; border-bottom: 2px dashed rgba(233,238,251,.6); padding: 0 6px; }
    .row { display:flex; gap:10px; flex-wrap:wrap; align-items: center; margin: 10px 0; }
    input[type="text"]{ flex: 1; min-width: 220px; padding: 10px 12px; border-radius: 12px; border: 1px solid rgba(255,255,255,.18); background: rgba(0,0,0,.25); color: #e9eefb; }
    button { padding: 10px 12px; border-radius: 12px; border: 1px solid rgba(255,255,255,.18); background: rgba(255,255,255,.08); color: #e9eefb; cursor: pointer; }
    button:hover { background: rgba(255,255,255,.12); }
    .hint { margin-top: 10px; padding: 10px 12px; border-radius: 12px; background: rgba(255,255,255,.07); border: 1px solid rgba(255,255,255,.12); display:none; }
    .feedback { margin-top: 10px; padding: 10px 12px; border-radius: 12px; border: 1px solid transparent; display:none; }
    .ok { background: rgba(46, 204, 113, .12); border-color: rgba(46, 204, 113, .35); }
    .no { background: rgba(231, 76, 60, .12); border-color: rgba(231, 76, 60, .35); }
    .answerBox { margin-top: 10px; padding: 12px; border-radius: 12px; background: rgba(255,255,255,.07); border: 1px solid rgba(255,255,255,.12); display:none; }
    .answerBox b { font-size: 16px; }
    .nav { display:flex; justify-content: space-between; gap: 10px; margin-top: 14px; }
    .small { font-size: 13px; opacity: .9; }
    .footer { margin-top: 10px; font-size: 12px; opacity: .8; }
    a { color: #b9d2ff; }
  </style>
</head>
<body>
  <div class="wrap">
    <div class="card">
      <h1>Jeu de flash cards — Cybersécurité</h1>
      <div class="meta">
        <span class="pill" id="progress">Carte 1 / 7</span>
        <span class="pill small">Consigne : complète le mot manquant, puis vérifie. Utilise les indices si besoin.</span>
      </div>

      <div class="situation" id="situation"></div>

      <div class="row">
        <input id="answer" type="text" placeholder="Écris le mot ici (ex. phishing)..." autocomplete="off" />
        <button id="checkBtn">Vérifier</button>
        <button id="revealBtn">Voir la réponse</button>
      </div>

      <div class="row">
        <button id="hint1Btn">Indice 1</button>
        <button id="hint2Btn">Indice 2</button>
        <button id="resetBtn">Réinitialiser</button>
      </div>

      <div class="hint" id="hintBox"></div>
      <div class="feedback" id="feedback"></div>

      <div class="answerBox" id="answerBox"></div>

      <div class="nav">
        <button id="prevBtn">← Précédent</button>
        <button id="nextBtn">Suivant →</button>
      </div>

      <div class="footer">
        Astuce Rise 360 : utilise un bloc <i>Embed</i> et colle un iframe pointant vers cette page hébergée.
      </div>
    </div>
  </div>

  <script>
    const cards = [
      {
        text: "Un courriel frauduleux incitant l’utilisateur à fournir ses identifiants personnels relève d’une attaque de type <span class='missing'>__________</span>.",
        answers: ["phishing", "hameçonnage"],
        hint1: "Technique d’ingénierie sociale.",
        hint2: "Repose sur l’usurpation de confiance (mail, SMS, faux site).",
        definition: "Attaque visant à tromper l’utilisateur pour obtenir des informations sensibles."
      },
      {
        text: "Un logiciel malveillant qui chiffre les données d’un système et exige une rançon pour leur restitution est appelé <span class='missing'>__________</span>.",
        answers: ["ransomware"],
        hint1: "Menace portant atteinte à la disponibilité des données.",
        hint2: "Le nom contient l’idée de “rançon”.",
        definition: "Malware chiffrant des données et exigeant un paiement pour la clé ou la restitution."
      },
      {
        text: "Le fait de combiner un mot de passe avec un code temporaire reçu sur un autre support correspond à une <span class='missing'>__________</span> à deux facteurs.",
        answers: ["authentification"],
        hint1: "Renforce la sécurité des accès.",
        hint2: "Deux preuves d’identité (connaissance + possession, par ex.).",
        definition: "Procédure de vérification d’identité basée sur plusieurs facteurs."
      },
      {
        text: "Une faille exploitée par un attaquant avant qu’un correctif ne soit disponible est appelée vulnérabilité <span class='missing'>__________</span>.",
        answers: ["zero-day", "0-day", "zeroday"],
        hint1: "Inconnue/non corrigée au moment de l’attaque.",
        hint2: "Évoque un délai de réaction nul.",
        definition: "Vulnérabilité exploitée sans patch disponible, souvent très critique."
      },
      {
        text: "N’accorder que les droits strictement nécessaires à un utilisateur relève du principe du <span class='missing'>__________</span> privilège.",
        answers: ["moindre"],
        hint1: "Principe fondamental en sécurité des systèmes d’information.",
        hint2: "Limite l’impact d’une compromission.",
        definition: "Réduction des permissions au strict nécessaire pour minimiser les risques."
      },
      {
        text: "Rendre un service indisponible en saturant ses ressources correspond à une attaque par <span class='missing'>__________</span> de service.",
        answers: ["déni", "deni"],
        hint1: "Cible la disponibilité.",
        hint2: "Souvent abrégée DDoS lorsqu’elle est distribuée.",
        definition: "Attaque visant à interrompre un service en surchargeant son réseau/serveur."
      },
      {
        text: "Le chiffrement des données vise notamment à garantir leur <span class='missing'>__________</span> face à un accès non autorisé.",
        answers: ["confidentialité", "confidentialite"],
        hint1: "Composante du triptyque CIA.",
        hint2: "Empêche la lecture des données par des tiers non autorisés.",
        definition: "Propriété assurant que seules les entités autorisées accèdent aux informations."
      }
    ];

    let i = 0;
    let hintLevel = 0;

    const elSituation = document.getElementById("situation");
    const elProgress = document.getElementById("progress");
    const elAnswer = document.getElementById("answer");
    const elHintBox = document.getElementById("hintBox");
    const elFeedback = document.getElementById("feedback");
    const elAnswerBox = document.getElementById("answerBox");

    const normalize = (s) => (s || "")
      .toLowerCase()
      .trim()
      .normalize("NFD")
      .replace(/[\u0300-\u036f]/g, "");

    function render() {
      const c = cards[i];
      elSituation.innerHTML = c.text;
      elProgress.textContent = `Carte ${i+1} / ${cards.length}`;
      elAnswer.value = "";
      hintLevel = 0;
      elHintBox.style.display = "none";
      elFeedback.style.display = "none";
      elAnswerBox.style.display = "none";
    }

    function showHint(level) {
      const c = cards[i];
      hintLevel = Math.max(hintLevel, level);
      let txt = "";
      if (hintLevel >= 1) txt += `<b>Indice 1 :</b> ${c.hint1}<br>`;
      if (hintLevel >= 2) txt += `<b>Indice 2 :</b> ${c.hint2}`;
      elHintBox.innerHTML = txt;
      elHintBox.style.display = "block";
    }

    function check() {
      const c = cards[i];
      const user = normalize(elAnswer.value);
      const ok = c.answers.map(normalize).includes(user);

      elFeedback.className = "feedback " + (ok ? "ok" : "no");
      elFeedback.style.display = "block";
      elFeedback.innerHTML = ok
        ? "✅ Réponse correcte."
        : "❌ Réponse incorrecte. Essaie encore ou utilise un indice.";
      if (ok) reveal(true);
    }

    function reveal(fromCheck=false) {
      const c = cards[i];
      elAnswerBox.style.display = "block";
      elAnswerBox.innerHTML =
        `<b>Réponse :</b> ${c.answers[0]}<br><span class="small">${c.definition}</span>` +
        (fromCheck ? "" : `<div class="small" style="margin-top:8px;opacity:.9;">(Tu peux passer à la carte suivante.)</div>`);
    }

    document.getElementById("hint1Btn").addEventListener("click", () => showHint(1));
    document.getElementById("hint2Btn").addEventListener("click", () => showHint(2));
    document.getElementById("checkBtn").addEventListener("click", check);
    document.getElementById("revealBtn").addEventListener("click", () => reveal(false));
    document.getElementById("resetBtn").addEventListener("click", render);

    document.getElementById("prevBtn").addEventListener("click", () => { i = (i - 1 + cards.length) % cards.length; render(); });
    document.getElementById("nextBtn").addEventListener("click", () => { i = (i + 1) % cards.length; render(); });

    elAnswer.addEventListener("keydown", (e) => { if (e.key === "Enter") check(); });

    render();
  </script>
</body>
</html>
