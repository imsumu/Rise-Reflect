<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Balance - Deen & Dunya Motivation</title>
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet">
  <style>
    body {
      background: linear-gradient(to right, #e0f7fa, #fff3e0);
      min-height: 100vh;
      display: flex;
      align-items: center;
      justify-content: center;
      font-family: 'Segoe UI', sans-serif;
    }
    .bismillah {
      font-family: 'Amiri', serif;
      font-size: 1.5rem;
      color: #7b1fa2;
    }
    .quote-box {
      background-color: #ffffffcc;
      border-radius: 15px;
      padding: 30px;
      box-shadow: 0 4px 15px rgba(0,0,0,0.1);
      text-align: center;
    }
    #quoteContainer {
      padding: 20px;
      border-radius: 10px;
      transition: background-color 0.6s ease;
    }
    #quoteText {
      font-size: 1.3rem;
      font-style: italic;
    }
    #quoteAuthor {
      margin-top: 10px;
      font-weight: bold;
      color: #00796b;
    }
    .marquee-container {
      overflow: hidden;
      white-space: nowrap;
    }
    .marquee {
      display: inline-block;
      padding-left: 100%;
      animation: scroll-left 12s linear infinite, color-change 6s linear infinite;
      font-size: 1.5rem;
      font-weight: bold;
    }
    @keyframes scroll-left {
      0%   { transform: translateX(0%); }
      100% { transform: translateX(-100%); }
    }
    @keyframes color-change {
      0%   { color: red; }
      25%  { color: blue; }
      50%  { color: black; }
      75%  { color: #2e7d32; }
      100% { color: #00796b; }
    }
  </style>
</head>
<body>

<div class="container">
  <div class="row justify-content-center">
    <div class="col-md-8 text-center quote-box">
      <div class="marquee-container mb-4">
        <div class="marquee">Student Focus & Inner Peace 🌿</div>
      </div>

      <div class="bismillah mb-3">﷽</div>

      <div class="mb-3">
        <select id="categorySelect" class="form-select w-auto mx-auto">
          <option value="all">All Quotes</option>
          <option value="islamic">Spiritual Only</option>
          <option value="scientific">Modern Thinker</option>
        </select>
      </div>

      <div id="quoteContainer" class="mb-2">
        <div id="quoteText">"Loading wisdom..."</div>
        <div id="quoteAuthor"></div>
      </div>

      <div class="mt-4">
        <button onclick="newQuote()" class="btn btn-primary me-2">🔁 New Quote</button>
        <button onclick="startTimer()" class="btn btn-outline-success">🧘 Reflect Options</button>
      </div>
    </div>
  </div>
</div>

<div class="modal fade" id="reflectionModal" tabindex="-1" aria-labelledby="reflectionModalLabel" aria-hidden="true">
  <div class="modal-dialog modal-dialog-centered">
    <div class="modal-content">
      <div class="modal-header">
        <h5 class="modal-title d-flex justify-content-between w-100" id="reflectionModalLabel">
          <span>Reflection Started</span>
          <span id="modalTimerLabel" class="ms-2 text-muted">--</span>
        </h5>
        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
      </div>
      <div class="modal-body text-center">
        <div id="modalOptions">
          <p>Select your focus:</p>
          <button class="btn btn-outline-primary me-2" onclick="beginSession(30, 'Breathe')">🫁 Breathe (30s)</button>
          <button class="btn btn-outline-success" onclick="beginSession(60, 'Reflect')">🧘 Reflect (60s)</button>
        </div>
        <div id="sessionDisplay" style="display: none;">
          <div id="timer" class="mb-2">🕯</div>
          <p id="sessionMessage">May your thoughts be clear and your heart at peace. 🌿</p>
        </div>
      </div>
      <div class="modal-footer">
        <button type="button" class="btn btn-success" data-bs-dismiss="modal">Close</button>
      </div>
    </div>
  </div>
</div>

<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js"></script>

<script>
  const quotes = [
{ text: "So verily, with hardship, there is relief.", author: "Surah Ash-Sharh (94:6)", category: "islamic" },
{ text: "Indeed, Allah is with the patient.", author: "Surah Al-Baqarah (2:153)", category: "islamic" },
{ text: "The heart will never find peace until it turns to Allah.", author: "Ibn Qayyim Al-Jawziyya", category: "islamic" },
{ text: "Look deep into nature, and then you will understand everything better.", author: "Albert Einstein", category: "scientific" },
{ text: "Nothing in life is to be feared, it is only to be understood.", author: "Marie Curie", category: "scientific" },
{ text: "Success is not final, failure is not fatal: It is the courage to continue that counts.", author: "Winston Churchill", category: "scientific" },
{ text: "Seek knowledge from the cradle to the grave.", author: "Prophet Muhammad ﷺ", category: "islamic" },
{ text: "Whoever follows a path in pursuit of knowledge, Allah will make a path to Paradise easy for him.", author: "Prophet Muhammad ﷺ (Sahih Muslim)", category: "islamic" },
{ text: "Read! In the Name of your Lord who created.", author: "Surah Al-‘Alaq (96:1)", category: "islamic" },
{ text: "Allah does not burden a soul beyond that it can bear.", author: "Surah Al-Baqarah (2:286)", category: "islamic" },
{ text: "When you seek knowledge with sincerity, Allah opens the path of understanding for you.", author: "Imam Al-Shafi'i", category: "islamic" },
{ text: "Acquire knowledge and impart it to the people.", author: "Prophet Muhammad ﷺ (Tirmidhi)", category: "islamic" },
{ text: "Education is the kindling of a flame, not the filling of a vessel.", author: "Socrates", category: "spiritual" },
{ text: "Your work is to discover your world and then with all your heart give yourself to it.", author: "Buddha", category: "spiritual" },
{ text: "The more you know yourself, the more clarity there is. Self-knowledge has no end.", author: "Jiddu Krishnamurti", category: "spiritual" },
{ text: "Don't limit a child to your own learning, for he was born in another time.", author: "Rabindranath Tagore", category: "spiritual" },
{ text: "Faith is taking the first step even when you don’t see the whole staircase.", author: "Martin Luther King Jr.", category: "spiritual" },

    { text: "Knowledge enlivens the soul.", author: "Imam Ali (RA)", category: "islamic" }
  ];

  const quoteText = document.getElementById("quoteText");
  const quoteAuthor = document.getElementById("quoteAuthor");
  const quoteContainer = document.getElementById("quoteContainer");
  const categorySelect = document.getElementById("categorySelect");

  function newQuote() {
    const category = categorySelect.value;
    const filtered = category === "all" ? quotes : quotes.filter(q => q.category === category);
    const random = filtered[Math.floor(Math.random() * filtered.length)];

    quoteText.innerText = `"${random.text}"`;
    quoteAuthor.innerText = `— ${random.author}`;

    const colors = ["#ffe0b2", "#dcedc8", "#f8bbd0", "#b3e5fc"]


    quoteContainer.style.backgroundColor = colors[Math.floor(Math.random() * colors.length)];
  }

  function startTimer() {
    const modalElement = document.getElementById('reflectionModal');
    const reflectionModal = new bootstrap.Modal(modalElement);
    reflectionModal.show();

    document.getElementById('modalOptions').style.display = 'block';
    document.getElementById('sessionDisplay').style.display = 'none';
    document.getElementById('modalTimerLabel').innerText = '--';
    document.getElementById('timer').innerText = '';
  }

  function beginSession(duration, type) {
    const timerDisplay = document.getElementById("timer");
    const modalTimerLabel = document.getElementById("modalTimerLabel");
    const modalElement = document.getElementById('reflectionModal');

    document.getElementById('modalOptions').style.display = 'none';
    document.getElementById('sessionDisplay').style.display = 'block';
    document.getElementById('sessionMessage').innerText = type === "Breathe"
      ? "Inhale deeply… Exhale slowly 🌬️"
      : "Let your thoughts settle and reflect 🌿";

    let time = duration;
    timerDisplay.innerText = `🕯 ${type}: ${time}s`;
    modalTimerLabel.innerText = `${time}s`;

    const interval = setInterval(() => {
      time--;
      timerDisplay.innerText = `🕯 ${type}: ${time}s`;
      modalTimerLabel.innerText = `${time}s`;

      if (time <= 0) {
        clearInterval(interval);
        timerDisplay.innerText = "✅ Time’s up. May your heart be light ✨";
        modalTimerLabel.innerText = "✅";

        setTimeout(() => {
          const modalInstance = bootstrap.Modal.getInstance(modalElement);
          if (modalInstance) modalInstance.hide();
        }, 1500);
      }
    }, 1000);
  }

  window.onload = newQuote;
</script>

</body>
</html>
