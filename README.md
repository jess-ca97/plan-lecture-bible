# plan-lecture-bible
Plan de lecture de la Bible
<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Plan Biblique Chronologique</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      margin: 2rem;
      background: #f2f2f2;
      color: #333;
    }
    .container {
      background: white;
      padding: 2rem;
      border-radius: 10px;
      max-width: 600px;
      margin: auto;
      box-shadow: 0 2px 10px rgba(0,0,0,0.1);
    }
    h1 {
      text-align: center;
      color: #005b96;
    }
    .reading {
      margin-top: 1.5rem;
    }
    .done {
      margin-top: 1rem;
    }
    button {
      padding: 0.6rem 1rem;
      font-size: 1rem;
      border: none;
      background: #005b96;
      color: white;
      border-radius: 5px;
      cursor: pointer;
    }
    button:disabled {
      background: #aaa;
    }
    a {
      color: #005b96;
      text-decoration: none;
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>Lecture Biblique Chronologique</h1>
    <div class="reading" id="readingBlock">
      <p><strong>Jour :</strong> <span id="dayNumber"></span> / 365</p>
      <p><strong>Lecture (Français) :</strong> <span id="readingText"></span></p>
      <p><a href="#" target="_blank" id="linkFR">Lire sur YouVersion (FR)</a></p>
      <p><a href="#" target="_blank" id="linkPT">Lire sur YouVersion (PT)</a></p>
    </div>
    <div class="done">
      <button id="markDoneBtn">Marquer comme lu</button>
      <p id="statusText" style="margin-top: 0.5rem;"></p>
    </div>
    <p id="messageTooEarly" style="display:none; text-align:center; color:gray;">
      Le plan commence le <strong>13 mai 2025</strong>. Revenez demain !
    </p>
  </div>

  <script>
    const plan = [
      "Genèse 1-3", "Genèse 4-7", "Genèse 8-11", "Job 1-5", "Job 6-9",
      "Job 10-13", "Job 14-17", "Job 18-21", "Job 22-25", "Job 26-29"
    ];
    while (plan.length < 365) {
      plan.push(...plan.slice(0, 365 - plan.length));
    }

    const startDate = new Date("2025-05-13");
    const today = new Date();
    const diff = Math.floor((today - startDate) / (1000 * 60 * 60 * 24));
    const dayNumber = diff + 1;

    const readingBlock = document.getElementById("readingBlock");
    const statusText = document.getElementById("statusText");
    const btn = document.getElementById("markDoneBtn");

    if (dayNumber < 1) {
      readingBlock.style.display = "none";
      btn.style.display = "none";
      document.getElementById("messageTooEarly").style.display = "block";
    } else if (dayNumber > 365) {
      readingBlock.innerHTML = "<p>🎉 Vous avez terminé le plan de lecture !</p>";
      btn.style.display = "none";
    } else {
      document.getElementById("dayNumber").textContent = dayNumber;
      const reading = plan[dayNumber - 1];
      document.getElementById("readingText").textContent = reading;

      const youVersionFR = "https://www.bible.com/fr/bible/93/GEN.1.LSG";
      const youVersionPT = "https://www.bible.com/pt/bible/212/GEN.1.ARA";
      document.getElementById("linkFR").href = youVersionFR;
      document.getElementById("linkPT").href = youVersionPT;

      const key = `bible-read-day-${dayNumber}`;
      if (localStorage.getItem(key) === "done") {
        btn.disabled = true;
        statusText.textContent = "Déjà marqué comme lu.";
      }

      btn.addEventListener("click", () => {
        localStorage.setItem(key, "done");
        statusText.textContent = "Marqué comme lu.";
        btn.disabled = true;
      });
    }
  </script>
</body>
</html>
