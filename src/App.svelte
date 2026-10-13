
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Simon Says Challenge</title>

  <style>
    * {
      box-sizing: border-box;
    }

    body {
      margin: 0;
      min-height: 100vh;
      padding: 24px 16px;
      display: flex;
      justify-content: center;
      align-items: center;
      background: #101321;
      color: white;
      font-family: Arial, sans-serif;
    }

    .game {
      width: 100%;
      max-width: 500px;
      text-align: center;
    }

    h1 {
      font-size: 2.5rem;
      margin-bottom: 8px;
    }

    .subtitle {
      color: #a5acc7;
      margin-bottom: 24px;
    }

    .stats {
      display: flex;
      gap: 10px;
      margin-bottom: 20px;
    }

    .stat {
      flex: 1;
      padding: 12px 6px;
      border-radius: 12px;
      background: #20253b;
    }

    .stat p {
      color: #a5acc7;
      font-size: 11px;
      margin: 0 0 8px;
    }

    .stat h2 {
      margin: 0;
      font-size: 23px;
    }

    .instruction {
      min-height: 120px;
      padding: 20px 12px;
      margin-bottom: 18px;
      border: 2px solid #454d70;
      border-radius: 18px;
      background: #20253b;
      display: flex;
      flex-direction: column;
      justify-content: center;
      gap: 10px;
    }

    .instruction small {
      color: #a5acc7;
      text-transform: uppercase;
      letter-spacing: 2px;
    }

    #message {
      font-size: 1.2rem;
      line-height: 1.5;
      font-weight: bold;
    }

    .timer-container {
      height: 12px;
      background: #303650;
      border-radius: 20px;
      overflow: hidden;
      margin-bottom: 8px;
    }

    #timerBar {
      width: 100%;
      height: 100%;
      background: #22c55e;
      transition: width 0.1s linear, background 0.2s;
    }

    #timerText {
      margin-bottom: 18px;
      color: #c4c9e0;
      font-weight: bold;
    }

    .board {
      display: grid;
      grid-template-columns: repeat(2, 1fr);
      gap: 14px;
      justify-items: center;
      margin-bottom: 18px;
    }

    .color {
      width: 100%;
      max-width: 180px;
      height: 120px;
      border: 3px solid #ffffff35;
      color: white;
      font-size: 1rem;
      font-weight: bold;
      cursor: pointer;
      transition:
        border-radius 0.3s,
        clip-path 0.3s,
        background 0.3s,
        transform 0.15s,
        filter 0.15s;
    }

    .color:hover:not(:disabled) {
      filter: brightness(1.2);
      transform: scale(1.04);
    }

    .color:active:not(:disabled) {
      transform: scale(0.94);
    }

    .color:disabled,
    .car:disabled,
    .main-btn:disabled {
      cursor: not-allowed;
      opacity: 0.65;
    }

    /* Different shapes */

    .square {
      border-radius: 12px;
      clip-path: none;
    }

    .circle {
      border-radius: 50%;
      clip-path: none;
    }

    .pill {
      border-radius: 60px;
      clip-path: none;
    }

    .diamond {
      border-radius: 0;
      clip-path: polygon(
        50% 0%, 100% 50%,
        50% 100%, 0% 50%
      );
    }

    .hexagon {
      border-radius: 0;
      clip-path: polygon(
        25% 0%, 75% 0%,
        100% 50%, 75% 100%,
        25% 100%, 0% 50%
      );
    }

    .car {
      width: 100%;
      padding: 14px;
      margin-bottom: 14px;
      border: 2px solid #454d70;
      border-radius: 16px;
      background: #252b43;
      color: white;
      font-size: 1rem;
      cursor: pointer;
    }

    .car span {
      display: block;
      font-size: 2rem;
      margin-bottom: 5px;
    }

    .main-btn {
      width: 100%;
      padding: 15px;
      margin-top: 5px;
      border: none;
      border-radius: 12px;
      background: #8b5cf6;
      color: white;
      font-size: 1rem;
      font-weight: bold;
      cursor: pointer;
    }

    .main-btn:hover:not(:disabled) {
      background: #7c3aed;
    }

    .hint {
      color: #858eaf;
      font-size: 0.85rem;
      line-height: 1.5;
      margin-top: 18px;
    }

    @media (max-width: 360px) {
      h1 {
        font-size: 2rem;
      }

      .color {
        height: 105px;
        font-size: 0.85rem;
      }
    }
  </style>
</head>

<body>
  <main class="game">
    <h1>🎮 Simon Says</h1>
    <p class="subtitle">
      Follow the rules. Avoid the tricks!
    </p>

    <section class="stats">
      <div class="stat">
        <p>ROUND</p>
        <h2 id="round">0</h2>
      </div>

      <div class="stat">
        <p>SCORE</p>
        <h2 id="score">0</h2>
      </div>

      <div class="stat">
        <p>BEST</p>
        <h2 id="best">0</h2>
      </div>
    </section>

    <section class="instruction">
      <small id="label">Your challenge</small>
      <div id="message">
        Press Start Game to begin!
      </div>
    </section>

    <div class="timer-container">
      <div id="timerBar"></div>
    </div>

    <p id="timerText">Time: 5 seconds</p>

    <section class="board">
      <button class="color square" data-index="0" disabled>
        GREEN
      </button>

      <button class="color square" data-index="1" disabled>
        RED
      </button>

      <button class="color square" data-index="2" disabled>
        YELLOW
      </button>

      <button class="color square" data-index="3" disabled>
        BLUE
      </button>
    </section>

    <button class="car" id="car" disabled>
      <span>🚗</span>
      Car
    </button>

    <button class="main-btn" id="done" disabled>
      Done — I followed the rule!
    </button>

    <button class="main-btn" id="start">
      Start Game
    </button>

    <p class="hint">
      You have 5 seconds for each challenge.
      Every 5 correct answers, the colors and shapes change!
    </p>
  </main>

  <script>
    // HTML elements
    const message = document.getElementById("message");
    const label = document.getElementById("label");
    const roundDisplay = document.getElementById("round");
    const scoreDisplay = document.getElementById("score");
    const bestDisplay = document.getElementById("best");
    const timerBar = document.getElementById("timerBar");
    const timerText = document.getElementById("timerText");

    const colorButtons = document.querySelectorAll(".color");
    const carButton = document.getElementById("car");
    const doneButton = document.getElementById("done");
    const startButton = document.getElementById("start");

    // Game settings
    const TIME_LIMIT = 5;

    let round = 0;
    let score = 0;
    let best =
      Number(localStorage.getItem("simonChallengeBest")) || 0;

    let currentTask = null;
    let acceptingInput = false;
    let finished = true;
    let clickCount = 0;

    let timerInterval = null;
    let timeLeft = TIME_LIMIT;
    let gameId = 0;

    bestDisplay.textContent = best;

    // Color and shape themes.
    // A new theme is selected every 5 correct answers.

    const themes = [
      {
        colors: [
          { name: "green", hex: "#16a34a" },
          { name: "red", hex: "#dc2626" },
          { name: "yellow", hex: "#eab308" },
          { name: "blue", hex: "#2563eb" }
        ],
        shapes: ["square", "square", "square", "square"]
      },

      {
        colors: [
          { name: "purple", hex: "#9333ea" },
          { name: "orange", hex: "#ea580c" },
          { name: "cyan", hex: "#0891b2" },
          { name: "pink", hex: "#db2777" }
        ],
        shapes: ["circle", "circle", "circle", "circle"]
      },

      {
        colors: [
          { name: "lime", hex: "#65a30d" },
          { name: "coral", hex: "#f05252" },
          { name: "violet", hex: "#7c3aed" },
          { name: "gold", hex: "#ca8a04" }
        ],
        shapes: ["diamond", "diamond", "diamond", "diamond"]
      },

      {
        colors: [
          { name: "teal", hex: "#0d9488" },
          { name: "magenta", hex: "#c026d3" },
          { name: "indigo", hex: "#4f46e5" },
          { name: "salmon", hex: "#f97373" }
        ],
        shapes: ["hexagon", "pill", "hexagon", "pill"]
      }
    ];

    let currentTheme = 0;

    // Get the current theme
    function getTheme() {
      return themes[currentTheme % themes.length];
    }

    // Change button colors and shapes
    function updateTheme() {
      const theme = getTheme();

      colorButtons.forEach((button, index) => {
        const color = theme.colors[index];

        button.style.backgroundColor = color.hex;
        button.textContent = color.name.toUpperCase();

        button.classList.remove(
          "square", "circle", "pill", "diamond", "hexagon"
        );

        button.classList.add(theme.shapes[index]);
      });

      label.textContent =
        `Level ${currentTheme + 1} — Round ${round}`;
    }

    function randomColorIndex() {
      return Math.floor(Math.random() * 4);
    }

    // Create a random Simon Says instruction
    function createTask() {
      const type = Math.floor(Math.random() * 5);
      const says = Math.random() < 0.65;
      const theme = getTheme();

      if (type === 0) {
        const index = randomColorIndex();
        const count = Math.floor(Math.random() * 3) + 2;
        const color = theme.colors[index].name;

        return {
          says,
          target: "color",
          index,
          count,
          text: `${says ? "Simon says: " : ""}Press the ${color} button ${count} times!`
        };
      }

      if (type === 1) {
        const index = randomColorIndex();
        const color = theme.colors[index].name;

        return {
          says,
          target: "color",
          index,
          count: 1,
          text: `${says ? "Simon says: " : ""}Press the ${color} button!`
        };
      }

      if (type === 2) {
        return {
          says,
          target: "car",
          text: `${says ? "Simon says: " : ""}Don't press the car!`
        };
      }

      if (type === 3) {
        const index = randomColorIndex();
        const color = theme.colors[index].name;

        return {
          says,
          target: "avoidColor",
          index,
          text: `${says ? "Simon says: " : ""}Don't press the ${color} button!`
        };
      }

      return {
        says,
        target: "wait",
        text: `${says ? "Simon says: " : ""}Don't press anything!`
      };
    }

    // Stop the timer
    function stopTimer() {
      clearInterval(timerInterval);
      timerInterval = null;
    }

    // Start a fresh 5-second timer
    function startTimer(id) {
      stopTimer();

      const startedAt = Date.now();
      timeLeft = TIME_LIMIT;

      timerBar.style.width = "100%";
      timerBar.style.backgroundColor = "#22c55e";
      timerText.textContent = "Time: 5 seconds";

      timerInterval = setInterval(() => {
        if (id !== gameId || !acceptingInput) {
          stopTimer();
          return;
        }

        const elapsed = (Date.now() - startedAt) / 1000;
        timeLeft = Math.max(0, TIME_LIMIT - elapsed);

        timerBar.style.width =
          `${(timeLeft / TIME_LIMIT) * 100}%`;

        timerText.textContent =
          `Time: ${timeLeft.toFixed(1)} seconds`;

        if (timeLeft <= 2) {
          timerBar.style.backgroundColor = "#ef4444";
        }

        if (timeLeft <= 0) {
          stopTimer();
          endGame("⏰ Time's up! Try again.");
        }
      }, 50);
    }

    // Start or restart the game
    function startGame() {
      gameId++;
      stopTimer();

      round = 0;
      score = 0;
      currentTheme = 0;
      clickCount = 0;
      finished = false;
      acceptingInput = false;

      roundDisplay.textContent = round;
      scoreDisplay.textContent = score;

      startButton.textContent = "Restart Game";

      updateTheme();
      nextRound();
    }

    // Start the next challenge
    function nextRound() {
      if (finished) return;

      stopTimer();

      round++;
      clickCount = 0;

      roundDisplay.textContent = round;

      // Change theme after every 5 correct answers
      currentTheme = Math.floor(score / 5) % themes.length;

      updateTheme();

      currentTask = createTask();
      acceptingInput = true;

      message.textContent = currentTask.text;

      if (!currentTask.says) {
        label.textContent = "⚠️ Trick instruction!";
      } else {
        label.textContent = `Level ${currentTheme + 1}`;
      }

      colorButtons.forEach(button => button.disabled = false);
      carButton.disabled = false;
      doneButton.disabled = false;

      startTimer(gameId);
    }

    // Handle color button clicks
    function handleColorClick(index) {
      if (!acceptingInput || finished) return;

      // A trick instruction must never be followed
      if (!currentTask.says) {
        endGame("❌ Simon didn't say it! Game over.");
        return;
      }

      if (currentTask.target === "avoidColor") {
        if (currentTask.index === index) {
          endGame("❌ You pressed the forbidden color!");
        } else {
          endGame("❌ That wasn't the correct action!");
        }
        return;
      }

      if (currentTask.target !== "color") {
        endGame("❌ Wrong action! Read the instruction.");
        return;
      }

      if (currentTask.index !== index) {
        endGame("❌ Wrong color! Game over.");
        return;
      }

      clickCount++;

      message.textContent =
        `${getTheme().colors[index].name.toUpperCase()}! ` +
        `${clickCount} / ${currentTask.count}`;

      if (clickCount === currentTask.count) {
        completeRound();
      }
    }

    // Handle car clicks
    function handleCarClick() {
      if (!acceptingInput || finished) return;

      if (!currentTask.says) {
        endGame("❌ Simon didn't say it! Game over.");
        return;
      }

      if (currentTask.target === "car") {
        endGame("🚗 You pressed the car! Game over.");
      } else {
        endGame("❌ Wrong action! Game over.");
      }
    }

    // Confirm that the player followed a DON'T instruction
    function handleDone() {
      if (!acceptingInput || finished) return;

      if (!currentTask.says) {
        // Do nothing for trick instructions.
        completeRound();
        return;
      }

      if (currentTask.target === "color") {
        endGame("❌ You didn't press the required color!");
        return;
      }

      if (
        currentTask.target === "avoidColor" ||
        currentTask.target === "car" ||
        currentTask.target === "wait"
      ) {
        completeRound();
      }
    }

    // Correct answer
    function completeRound() {
      if (!acceptingInput || finished) return;

      acceptingInput = false;
      stopTimer();

      score++;
      scoreDisplay.textContent = score;

      if (score > best) {
        best = score;
        bestDisplay.textContent = best;

        localStorage.setItem("simonChallengeBest", best);
      }

      colorButtons.forEach(button => button.disabled = true);
      carButton.disabled = true;
      doneButton.disabled = true;

      if (score % 5 === 0) {
        message.textContent =
          "🎉 Level completed! New colors and shapes incoming!";
      } else {
        message.textContent =
          "✅ Correct! Get ready for the next challenge.";
      }

      const thisGame = gameId;

      setTimeout(() => {
        if (thisGame === gameId && !finished) {
          nextRound();
        }
      }, 1200);
    }

    // Game over
    function endGame(text) {
      if (finished) return;

      stopTimer();

      acceptingInput = false;
      finished = true;

      colorButtons.forEach(button => button.disabled = true);
      carButton.disabled = true;
      doneButton.disabled = true;

      message.textContent = text;
      label.textContent = "Game over";
      timerText.textContent = "Time's up!";
      timerBar.style.width = "0%";

      startButton.textContent = "Play Again";
    }

    // Connect the buttons
    colorButtons.forEach(button => {
      button.addEventListener("click", () => {
        handleColorClick(Number(button.dataset.index));
      });
    });

    carButton.addEventListener("click", handleCarClick);
    doneButton.addEventListener("click", handleDone);
    startButton.addEventListener("click", startGame);
  </script>
</body>
</html>
