<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>Chaos Quest</title>
  <style>
    body {
      margin: 0;
      font-family: Arial, sans-serif;
      background: linear-gradient(135deg, #0f0f1a, #1b1b3a);
      color: white;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
    }
    #game {
      width: 360px;
      background: #111;
      border-radius: 16px;
      padding: 20px;
      box-shadow: 0 0 20px rgba(0,0,0,0.6);
    }
    h1 {
      text-align: center;
      margin-top: 0;
    }
    .stats {
      display: flex;
      justify-content: space-between;
      margin-bottom: 10px;
      font-size: 14px;
    }
    #text {
      min-height: 100px;
      margin-bottom: 15px;
    }
    button {
      width: 100%;
      padding: 10px;
      margin: 6px 0;
      font-size: 16px;
      border: none;
      border-radius: 10px;
      cursor: pointer;
      background: #5a5aff;
      color: white;
    }
    button:hover {
      background: #7777ff;
    }
  </style>
</head>
<body>
  <div id="game">
    <h1>⚡ Chaos Quest ⚡</h1>
    <div class="stats">
      <div id="health">❤️ 100</div>
      <div id="energy">⚡ 50</div>
      <div id="score">💎 0</div>
    </div>
    <div id="text"></div>
    <div id="choices"></div>
  </div>

  <script>
    let health = 100;
    let energy = 50;
    let score = 0;
    let level = 0;

    const text = document.getElementById('text');
    const choicesDiv = document.getElementById('choices');

    function updateStats() {
      document.getElementById('health').innerText = `❤️ ${health}`;
      document.getElementById('energy').innerText = `⚡ ${energy}`;
      document.getElementById('score').innerText = `💎 ${score}`;
    }

    function setScene(sceneText, choices) {
      text.innerText = sceneText;
      choicesDiv.innerHTML = '';
      choices.forEach(choice => {
        const btn = document.createElement('button');
        btn.innerText = choice.text;
        btn.onclick = choice.action;
        choicesDiv.appendChild(btn);
      });
      updateStats();
    }

    function startGame() {
      level = 1;
      setScene(
        'You wake up in a floating city. Alarms echo through the sky.',
        [
          { text: 'Grab glowing sword', action: () => { energy += 20; score += 10; level2(); } },
          { text: 'Hack nearby terminal', action: () => { score += 30; level2(); } },
          { text: 'Run into the chaos', action: () => { health += 10; score += 15; level2(); } }
        ]
      );
    }

    function level2() {
      level = 2;
      setScene(
        'Attack drones swarm above you.',
        [
          { text: 'Fight them', action: () => { health -= 20; score += 40; level3(); } },
          { text: 'Hide and recharge', action: () => { energy += 25; level3(); } },
          { text: 'Parkour rooftops', action: () => { score += 30; level3(); } }
        ]
      );
    }

    function level3() {
      level = 3;
      setScene(
        'A cyber-dragon descends from the clouds!',
        [
          { text: 'Attack head-on', action: () => { health -= 30; score += 60; endGame(); } },
          { text: 'Dodge and counter', action: () => { health -= 10; score += 50; endGame(); } },
          { text: 'Use environment', action: () => { energy -= 5; score += 70; endGame(); } }
        ]
      );
    }

    function endGame() {
      let rank = 'Survivor';
      if (score >= 200) rank = 'CHAOS LEGEND';
      else if (score >= 100) rank = 'Elite';

      setScene(
        `Game Over!\nFinal Score: ${score}\nRank: ${rank}`,
        [
          { text: 'Play Again', action: () => { health = 100; energy = 50; score = 0; startGame(); } }
        ]
      );
    }

    startGame();
  </script>
</body>
</html>
