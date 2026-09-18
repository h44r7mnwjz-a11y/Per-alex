
<!DOCTYPE html>
<html lang="it">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <meta name="theme-color" content="#ffc9dc">
  <title>Per Alex ❤️</title>

  <style>
    * {
      box-sizing: border-box;
    }

    body {
      margin: 0;
      min-height: 100vh;
      min-height: 100svh;
      padding: 24px 16px;
      display: flex;
      justify-content: center;
      align-items: center;
      overflow-x: hidden;
      font-family: Georgia, "Times New Roman", serif;
      color: #6d1837;
      background:
        radial-gradient(circle at 20% 20%, #fff, transparent 35%),
        linear-gradient(135deg, #ffb6cf, #ffe4ed, #ffc9dc);
    }

    #decorations {
      position: fixed;
      inset: 0;
      overflow: hidden;
      pointer-events: none;
    }

    .card {
      position: relative;
      z-index: 2;
      width: min(100%, 560px);
      padding: 40px 24px;
      text-align: center;
      background: rgba(255, 255, 255, 0.9);
      border: 2px solid white;
      border-radius: 35px;
      box-shadow: 0 20px 60px #7d194133;
    }

    h1 {
      margin: 16px 0;
      color: #c9184a;
      font-size: clamp(2.2rem, 8vw, 3.5rem);
    }

    .message {
      margin: 24px 0;
      font-size: 1.15rem;
      line-height: 1.75;
    }

    .small-heart {
      display: inline-block;
      font-size: 2.5rem;
      animation: beat 1.2s infinite;
    }

    .buttons {
      display: flex;
      justify-content: center;
      align-items: center;
      gap: 16px;
      flex-wrap: wrap;
      min-height: 60px;
    }

    button,
    #yes {
      display: inline-block;
      padding: 15px 24px;
      border: 0;
      border-radius: 50px;
      font: bold 1.1rem Georgia, serif;
      text-decoration: none;
      cursor: pointer;
      touch-action: manipulation;
      -webkit-tap-highlight-color: transparent;
    }

    #yes {
      color: white;
      background: linear-gradient(135deg, #e6396f, #c9184a);
      box-shadow: 0 8px 20px #c9184a55;
    }

    #no {
      color: #666;
      background: #eee;
      box-shadow: 0 5px 15px #00000022;
      white-space: nowrap;
      z-index: 100;
      user-select: none;
      -webkit-user-select: none;
    }

    #no.escaping {
      position: fixed;
      margin: 0;
    }

    #no-slot {
      display: inline-block;
    }

    .signature {
      margin-top: 26px;
      color: #9d365a;
      line-height: 1.6;
    }

    #success {
      display: none;
    }

    #success:target {
      display: block;
    }

    #success:target ~ #question,
    #success:target ~ #no {
      display: none;
    }

    .big-heart {
      display: inline-block;
      font-size: 5rem;
      animation: beat 1s infinite;
    }

    .heart {
      position: absolute;
      bottom: -60px;
      animation: floatHeart linear forwards;
    }

    @keyframes beat {
      50% { transform: scale(1.2); }
    }

    @keyframes floatHeart {
      0% {
        transform: translateY(0) rotate(0);
        opacity: 0;
      }
      10% { opacity: 0.9; }
      100% {
        transform: translateY(-120vh) rotate(300deg);
        opacity: 0;
      }
    }

    @media (prefers-reduced-motion: reduce) {
      * { animation: none !important; }
      #decorations { display: none; }
    }
  </style>
</head>

<body>
  <div id="decorations" aria-hidden="true"></div>

  <main class="card" id="success">
    <div class="big-heart">💖</div>

    <h1>Sapevo che avresti detto sì!</h1>

    <div class="message">
      Alex, hai appena reso Giovanni
      la persona più felice del mondo. 🥹❤️

      <br><br>

      Allora è ufficiale:

      <br>

      <strong>APPUNTAMENTO CONFERMATO! 💕</strong>

      <br><br>

      Non vedo l'ora di passare
      un po' di tempo insieme. 🌹

      <br><br>

      ❤️ Ti aspetto ❤️
    </div>
  </main>

  <main class="card" id="question">
    <div class="small-heart">❤️</div>

    <h1>Alex...</h1>

    <div class="message">
      Ho una cosa importante da chiederti. 💕

      <br><br>

      Ci sono persone che rendono le nostre giornate
      un po' più belle semplicemente essendoci...

      <br><br>

      E tu sei una di quelle persone. ❤️

      <br><br>

      Quindi, Alex...

      <br>

      <strong>vuoi uscire con me? 🥰</strong>
    </div>

    <div class="buttons">
      <a id="yes" href="#success">Sì, certo! ❤️</a>

      <span id="no-slot">
        <button id="no" type="button">No 🙈</button>
      </span>
    </div>

    <noscript>
      <p>Apri il sito con JavaScript attivo per far scappare il “No”.</p>
    </noscript>

    <div class="signature">
      Con affetto,<br>
      <strong>Giovanni ❤️</strong>
    </div>
  </main>

  <script>
    const noButton = document.getElementById("no");
    const yesButton = document.getElementById("yes");
    const noSlot = document.getElementById("no-slot");
    const decorations = document.getElementById("decorations");

    const emojis = ["😑", "😔", "🥲", "🥹", "😭"];

    let touches = 0;
    let lastTouch = -Infinity;
    let accepted = location.hash === "#success";

    const reducedMotion = window.matchMedia
      ? window.matchMedia("(prefers-reduced-motion: reduce)")
      : { matches: false };

    function moveNoButton() {
      if (accepted) return;

      const previous = noButton.getBoundingClientRect();

      if (!noButton.classList.contains("escaping")) {
        noSlot.style.width = previous.width + "px";
        noSlot.style.height = previous.height + "px";

        // Fuori dalla scheda, può muoversi su tutto lo schermo.
        document.body.appendChild(noButton);
        noButton.classList.add("escaping");
      }

      const width = noButton.offsetWidth;
      const height = noButton.offsetHeight;
      const viewport = window.visualViewport;
      const screenWidth = viewport
        ? viewport.width
        : document.documentElement.clientWidth;
      const screenHeight = viewport
        ? viewport.height
        : window.innerHeight;

      const margin = 12;
      const minX = (viewport ? viewport.offsetLeft : 0) + margin;
      const minY = (viewport ? viewport.offsetTop : 0) + margin;
      const maxX = Math.max(minX, minX + screenWidth - width - margin * 2);
      const maxY = Math.max(minY, minY + screenHeight - height - margin * 2);
      const yesRect = yesButton.getBoundingClientRect();

      let destination = null;
      let bestDistance = -1;

      for (let i = 0; i < 80; i++) {
        const x = minX + Math.random() * (maxX - minX);
        const y = minY + Math.random() * (maxY - minY);

        const coversYes =
          x < yesRect.right + margin &&
          x + width > yesRect.left - margin &&
          y < yesRect.bottom + margin &&
          y + height > yesRect.top - margin;

        if (coversYes) continue;

        const distance = Math.hypot(
          x - previous.left,
          y - previous.top
        );

        if (distance > bestDistance) {
          bestDistance = distance;
          destination = { x, y };
        }

        if (distance >= Math.min(180, screenWidth * 0.45)) {
          break;
        }
      }

      if (!destination) {
        destination = { x: minX, y: minY };
      }

      noButton.style.left = destination.x + "px";
      noButton.style.top = destination.y + "px";
    }

    function escape() {
      if (accepted) return;

      noButton.textContent =
        "No " + emojis[Math.min(touches, emojis.length - 1)];

      touches++;
      moveNoButton();
    }

    // Telefono: scappa appena il dito tocca il pulsante.
    noButton.addEventListener("touchstart", function(event) {
      event.preventDefault();
      lastTouch = Date.now();
      escape();
    }, { passive: false });

    // Computer e tastiera.
    noButton.addEventListener("click", function(event) {
      event.preventDefault();

      // Evita di contare due volte lo stesso tocco.
      if (event.detail !== 0 && Date.now() - lastTouch < 700) {
        return;
      }

      escape();
    });

    window.addEventListener("resize", function() {
      if (noButton.classList.contains("escaping")) {
        moveNoButton();
      }
    });

    window.addEventListener("hashchange", function() {
      accepted = location.hash === "#success";
      noButton.hidden = accepted;
    });

    yesButton.addEventListener("click", function() {
      accepted = true;
      noButton.hidden = true;

      if (!reducedMotion.matches) {
        for (let i = 0; i < 60; i++) {
          setTimeout(createHeart, i * 60);
        }
      }
    });

    function createHeart() {
      if (document.hidden || reducedMotion.matches) return;

      const heart = document.createElement("div");
      const hearts = ["❤️", "💕", "💖", "💗", "💓", "💘"];

      heart.className = "heart";
      heart.textContent = hearts[Math.floor(Math.random() * hearts.length)];
      heart.style.left = Math.random() * 100 + "%";
      heart.style.fontSize = 18 + Math.random() * 25 + "px";
      heart.style.animationDuration = 4 + Math.random() * 5 + "s";

      decorations.appendChild(heart);
      setTimeout(function() { heart.remove(); }, 10000);
    }

    setInterval(createHeart, 350);
  </script>
</body>
</html>
