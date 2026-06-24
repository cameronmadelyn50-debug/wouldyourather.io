# wouldyourather.io
game
[index.html](https://github.com/user-attachments/files/29272595/index.html)
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
<title>Would You Rather?</title>
<style>
  @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap');

  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; -webkit-tap-highlight-color: transparent; }

  :root {
    --purple: #6C5CE7;
    --purple-light: #a29bfe;
    --purple-dark: #4834d4;
    --teal: #00b894;
    --teal-dark: #00856d;
    --bg: #0f0f1a;
    --surface: #1a1a2e;
    --surface2: #22223a;
    --border: rgba(255,255,255,0.08);
    --text: #f1f0ff;
    --muted: rgba(241,240,255,0.5);
    --faint: rgba(241,240,255,0.12);
  }

  html, body {
    height: 100%;
    background: var(--bg);
    color: var(--text);
    font-family: 'Inter', -apple-system, sans-serif;
    overflow: hidden;
  }

  #app {
    height: 100dvh;
    display: flex;
    flex-direction: column;
    padding: env(safe-area-inset-top, 16px) 0 env(safe-area-inset-bottom, 16px);
  }

  /* Header */
  .header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 12px 20px 0;
    flex-shrink: 0;
  }
  .logo {
    font-size: 15px;
    font-weight: 700;
    letter-spacing: -0.3px;
    background: linear-gradient(135deg, var(--purple-light), var(--teal));
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
  }
  .score-badge {
    font-size: 12px;
    font-weight: 600;
    background: var(--surface2);
    border: 1px solid var(--border);
    border-radius: 99px;
    padding: 4px 12px;
    color: var(--muted);
  }
  .score-badge span { color: var(--text); }

  /* Progress */
  .progress-wrap { padding: 12px 20px 0; flex-shrink: 0; }
  .progress-track {
    height: 3px;
    background: var(--faint);
    border-radius: 99px;
    overflow: hidden;
  }
  .progress-fill {
    height: 100%;
    background: linear-gradient(90deg, var(--purple), var(--teal));
    border-radius: 99px;
    transition: width 0.4s cubic-bezier(.4,0,.2,1);
  }
  .progress-label {
    display: flex;
    justify-content: space-between;
    margin-top: 6px;
    font-size: 11px;
    color: var(--muted);
  }

  /* Filters */
  .filter-scroll {
    display: flex;
    gap: 6px;
    overflow-x: auto;
    padding: 12px 20px 0;
    scrollbar-width: none;
    flex-shrink: 0;
  }
  .filter-scroll::-webkit-scrollbar { display: none; }
  .chip {
    flex-shrink: 0;
    font-size: 12px;
    font-weight: 500;
    padding: 6px 14px;
    border-radius: 99px;
    border: 1px solid var(--border);
    background: var(--surface);
    color: var(--muted);
    cursor: pointer;
    transition: all 0.15s;
    white-space: nowrap;
    font-family: inherit;
  }
  .chip.active {
    background: var(--purple);
    border-color: var(--purple);
    color: #fff;
  }

  /* Game area */
  .game-area {
    flex: 1;
    display: flex;
    flex-direction: column;
    justify-content: center;
    padding: 16px 20px;
    overflow: hidden;
  }

  .category-label {
    text-align: center;
    font-size: 11px;
    font-weight: 600;
    color: var(--muted);
    letter-spacing: 0.08em;
    text-transform: uppercase;
    margin-bottom: 12px;
  }

  .wyr-label {
    text-align: center;
    font-size: 13px;
    color: var(--muted);
    margin-bottom: 14px;
    font-weight: 500;
  }

  /* Options */
  .options {
    display: flex;
    flex-direction: column;
    gap: 10px;
  }

  .option {
    background: var(--surface);
    border: 1.5px solid var(--border);
    border-radius: 18px;
    padding: 18px 20px;
    font-size: 16px;
    font-weight: 500;
    color: var(--text);
    line-height: 1.45;
    cursor: pointer;
    text-align: center;
    transition: all 0.18s cubic-bezier(.4,0,.2,1);
    font-family: inherit;
    -webkit-appearance: none;
    width: 100%;
    position: relative;
    overflow: hidden;
  }
  .option:active { transform: scale(0.97); }

  .option.chosen-a {
    background: rgba(108,92,231,0.18);
    border-color: var(--purple);
    color: #fff;
  }
  .option.chosen-b {
    background: rgba(0,184,148,0.15);
    border-color: var(--teal);
    color: #fff;
  }
  .option.unchosen {
    opacity: 0.35;
  }

  .or-badge {
    text-align: center;
    font-size: 11px;
    font-weight: 700;
    letter-spacing: 0.15em;
    color: var(--muted);
    padding: 2px 0;
  }

  /* Vote bars */
  .vote-bars {
    margin-top: 14px;
    display: none;
    gap: 6px;
    flex-direction: column;
  }
  .vote-bars.show { display: flex; }
  .vote-row {
    display: flex;
    align-items: center;
    gap: 8px;
    font-size: 11px;
  }
  .vote-letter {
    font-weight: 700;
    min-width: 14px;
    color: var(--muted);
  }
  .vote-track {
    flex: 1;
    height: 5px;
    background: var(--faint);
    border-radius: 99px;
    overflow: hidden;
  }
  .vote-fill {
    height: 100%;
    border-radius: 99px;
    width: 0%;
    transition: width 0.6s cubic-bezier(.4,0,.2,1);
  }
  .vote-fill.a { background: var(--purple-light); }
  .vote-fill.b { background: var(--teal); }
  .vote-pct {
    font-size: 11px;
    font-weight: 600;
    min-width: 28px;
    text-align: right;
    color: var(--muted);
  }

  /* Nav */
  .nav-area {
    padding: 0 20px 8px;
    display: flex;
    flex-direction: column;
    gap: 8px;
    flex-shrink: 0;
  }
  .btn-row { display: flex; gap: 10px; }

  .btn {
    flex: 1;
    padding: 14px;
    border-radius: 14px;
    font-size: 15px;
    font-weight: 600;
    cursor: pointer;
    font-family: inherit;
    border: none;
    transition: all 0.15s;
  }
  .btn:active { transform: scale(0.97); }
  .btn-ghost {
    background: var(--surface2);
    color: var(--muted);
    border: 1px solid var(--border);
  }
  .btn-primary {
    background: linear-gradient(135deg, var(--purple), #8e79f5);
    color: #fff;
  }
  .btn-skip {
    background: none;
    border: none;
    color: var(--muted);
    font-size: 13px;
    padding: 8px;
    cursor: pointer;
    font-family: inherit;
    text-align: center;
    width: 100%;
    font-weight: 500;
  }

  /* Summary */
  #summary-view {
    display: none;
    flex: 1;
    flex-direction: column;
    overflow-y: auto;
    padding: 20px;
  }
  #summary-view.show { display: flex; }

  .summary-title {
    font-size: 26px;
    font-weight: 700;
    text-align: center;
    margin-bottom: 4px;
    background: linear-gradient(135deg, var(--purple-light), var(--teal));
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
  }
  .summary-sub {
    text-align: center;
    font-size: 14px;
    color: var(--muted);
    margin-bottom: 24px;
  }
  .stat-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 10px;
    margin-bottom: 24px;
  }
  .stat-card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 16px;
    padding: 16px;
    text-align: center;
  }
  .stat-num { font-size: 28px; font-weight: 700; color: var(--text); }
  .stat-label { font-size: 11px; color: var(--muted); margin-top: 2px; font-weight: 500; }

  .cat-section h3 {
    font-size: 12px;
    font-weight: 700;
    letter-spacing: 0.1em;
    text-transform: uppercase;
    color: var(--muted);
    margin-bottom: 10px;
  }
  .cat-row {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 10px 0;
    border-bottom: 1px solid var(--border);
    font-size: 13px;
  }
  .cat-row:last-child { border-bottom: none; }
  .cat-name { color: var(--text); }
  .cat-nums { color: var(--muted); font-size: 12px; }

  .btn-restart {
    margin-top: 24px;
    width: 100%;
    padding: 16px;
    border-radius: 16px;
    font-size: 16px;
    font-weight: 700;
    background: linear-gradient(135deg, var(--purple), #8e79f5);
    color: #fff;
    border: none;
    cursor: pointer;
    font-family: inherit;
  }

  /* Transitions */
  .slide-in { animation: slideIn 0.25s cubic-bezier(.4,0,.2,1); }
  @keyframes slideIn {
    from { opacity: 0; transform: translateX(30px); }
    to { opacity: 1; transform: translateX(0); }
  }
</style>
</head>
<body>
<div id="app">

  <!-- GAME VIEW -->
  <div id="game-view" style="display:flex;flex-direction:column;flex:1;overflow:hidden;">
    <div class="header">
      <div class="logo">Would You Rather?</div>
      <div class="score-badge">Q <span id="q-num">1</span> / <span id="q-total">0</span></div>
    </div>

    <div class="progress-wrap">
      <div class="progress-track"><div class="progress-fill" id="prog" style="width:0%"></div></div>
      <div class="progress-label">
        <span id="cat-label"></span>
        <span id="prog-label">0%</span>
      </div>
    </div>

    <div class="filter-scroll" id="filter-row"></div>

    <div class="game-area">
      <div class="wyr-label">Would you rather…</div>
      <div class="options" id="options-wrap">
        <button class="option" id="btn-a" onclick="choose('a')"></button>
        <div class="or-badge">— OR —</div>
        <button class="option" id="btn-b" onclick="choose('b')"></button>
      </div>
      <div class="vote-bars" id="vote-bars">
        <div class="vote-row">
          <span class="vote-letter">A</span>
          <div class="vote-track"><div class="vote-fill a" id="vf-a"></div></div>
          <span class="vote-pct" id="vp-a"></span>
        </div>
        <div class="vote-row">
          <span class="vote-letter">B</span>
          <div class="vote-track"><div class="vote-fill b" id="vf-b"></div></div>
          <span class="vote-pct" id="vp-b"></span>
        </div>
      </div>
    </div>

    <div class="nav-area">
      <div class="btn-row">
        <button class="btn btn-ghost" onclick="prevQ()">← Back</button>
        <button class="btn btn-primary" id="next-btn" onclick="nextQ()">Next →</button>
      </div>
      <button class="btn-skip" onclick="skipQ()">Skip this one</button>
    </div>
  </div>

  <!-- SUMMARY VIEW -->
  <div id="summary-view">
    <div class="summary-title">You finished! 🎉</div>
    <div class="summary-sub">Here's how your session went</div>
    <div class="stat-grid">
      <div class="stat-card"><div class="stat-num" id="s-ans">0</div><div class="stat-label">Answered</div></div>
      <div class="stat-card"><div class="stat-num" id="s-skip">0</div><div class="stat-label">Skipped</div></div>
      <div class="stat-card"><div class="stat-num" id="s-apct">—</div><div class="stat-label">Picked A</div></div>
      <div class="stat-card"><div class="stat-num" id="s-bpct">—</div><div class="stat-label">Picked B</div></div>
    </div>
    <div class="cat-section">
      <h3>By category</h3>
      <div id="cat-bd"></div>
    </div>
    <button class="btn-restart" onclick="restart()">Play Again ↺</button>
  </div>

</div>

<script>
const Q = [
  {c:"🌍 Travel",a:"Explore every country in the world",b:"Live in your dream city forever"},
  {c:"🌍 Travel",a:"Travel first class everywhere free",b:"Stay in 5-star hotels free forever"},
  {c:"🌍 Travel",a:"Speak every language fluently",b:"Teleport anywhere instantly"},
  {c:"🌍 Travel",a:"Have a private jet",b:"Have a private yacht"},
  {c:"🌍 Travel",a:"Visit the past as a tourist",b:"Visit the future as a tourist"},
  {c:"🌍 Travel",a:"Live in a different country every year",b:"Own a mansion in one place"},
  {c:"🌍 Travel",a:"See the Northern Lights every night",b:"Live on a tropical beach"},
  {c:"🌍 Travel",a:"Never have jet lag again",b:"Never lose luggage again"},
  {c:"🌍 Travel",a:"Travel to space for a week",b:"Explore the deep ocean floor"},
  {c:"🌍 Travel",a:"Road trip across the US with friends",b:"Solo backpack through Europe"},
  {c:"🌍 Travel",a:"Have a vacation home in Paris",b:"Have a vacation home in Tokyo"},
  {c:"🌍 Travel",a:"Spend a month in the Amazon jungle",b:"Spend a month in Antarctica"},
  {c:"💰 Money",a:"Win $10M in the lottery",b:"Earn $500K/year doing your dream job"},
  {c:"💰 Money",a:"Never pay for food again",b:"Never pay for housing again"},
  {c:"💰 Money",a:"Get $1,000 every day",b:"Get $30,000 every month"},
  {c:"💰 Money",a:"Have zero debt ever",b:"Have $500K in savings right now"},
  {c:"💰 Money",a:"Make money while you sleep",b:"Love what you do so work feels easy"},
  {c:"💰 Money",a:"Be a millionaire but work 60hr weeks",b:"Be comfortable and work 30hr weeks"},
  {c:"💰 Money",a:"Have free healthcare for life",b:"Have free college education for life"},
  {c:"💰 Money",a:"Get a $100K raise at your current job",b:"Start a business that hits $1M/year"},
  {c:"💰 Money",a:"Pay off all your student loans today",b:"Never pay rent or mortgage again"},
  {c:"💰 Money",a:"Find $500 on the street every Monday",b:"Get a $25K bonus at work every year"},
  {c:"💰 Money",a:"Never worry about money again",b:"Have the exact job of your dreams"},
  {c:"🦸 Powers",a:"Be able to fly",b:"Be invisible whenever you want"},
  {c:"🦸 Powers",a:"Read minds",b:"See 10 minutes into the future"},
  {c:"🦸 Powers",a:"Have super strength",b:"Have super speed"},
  {c:"🦸 Powers",a:"Never need sleep",b:"Never need food or water"},
  {c:"🦸 Powers",a:"Pause time whenever you want",b:"Rewind time up to 10 minutes"},
  {c:"🦸 Powers",a:"Heal any injury instantly",b:"Never get sick"},
  {c:"🦸 Powers",a:"Control the weather",b:"Talk to animals"},
  {c:"🦸 Powers",a:"Have photographic memory",b:"Learn any skill in one day"},
  {c:"🦸 Powers",a:"Breathe underwater",b:"Survive in any temperature"},
  {c:"🦸 Powers",a:"Always know when someone is lying",b:"Always find what you're looking for"},
  {c:"🦸 Powers",a:"Teleport but only solo",b:"Fly but others can see you"},
  {c:"🦸 Powers",a:"Control fire",b:"Control water"},
  {c:"🦸 Powers",a:"Never age past 30",b:"Age normally but live to 150 healthily"},
  {c:"🎮 Fun & Games",a:"Be the best gamer in the world",b:"Be the best athlete in the world"},
  {c:"🎮 Fun & Games",a:"Live inside a video game for a year",b:"Live inside a movie world for a year"},
  {c:"🎮 Fun & Games",a:"Have Netflix free for life",b:"Have Spotify free for life"},
  {c:"🎮 Fun & Games",a:"Be a pro gamer",b:"Be a pro athlete"},
  {c:"🎮 Fun & Games",a:"Win an Olympic gold medal",b:"Win a championship ring in your fav sport"},
  {c:"🎮 Fun & Games",a:"Free theme park access for life",b:"Front row seats at any concert for life"},
  {c:"🎮 Fun & Games",a:"Own every video game ever made",b:"Own every board game ever made"},
  {c:"🎮 Fun & Games",a:"Never lose at poker",b:"Always win at trivia nights"},
  {c:"🎮 Fun & Games",a:"Speedrun your favorite game perfectly",b:"Win a major esports tournament"},
  {c:"🍕 Food",a:"Eat your favorite meal every day forever",b:"Try a new incredible meal every day"},
  {c:"🍕 Food",a:"Never feel full after eating junk food",b:"Actually enjoy eating healthy food"},
  {c:"🍕 Food",a:"Be a world-class chef",b:"Have a personal chef cook for you daily"},
  {c:"🍕 Food",a:"Only eat sweet foods for a year",b:"Only eat savory foods for a year"},
  {c:"🍕 Food",a:"Have free pizza for life",b:"Have free sushi for life"},
  {c:"🍕 Food",a:"Taste food 10x more intensely",b:"Smell food 10x more intensely"},
  {c:"🍕 Food",a:"Never gain weight from eating",b:"Digest everything perfectly with no issues"},
  {c:"🍕 Food",a:"Win a competitive eating contest",b:"Win a competition like MasterChef"},
  {c:"🍕 Food",a:"Eat only spicy food for a year",b:"Eat only bland food for a year"},
  {c:"🍕 Food",a:"Unlimited ice cream but no chocolate",b:"Unlimited chocolate but no ice cream"},
  {c:"🍕 Food",a:"Brunch every Saturday with friends",b:"Fancy dinner date every Friday night"},
  {c:"🌿 Nature",a:"Live in a treehouse in the forest",b:"Live on a houseboat on the ocean"},
  {c:"🌿 Nature",a:"Have a pet lion",b:"Have a pet dolphin"},
  {c:"🌿 Nature",a:"Swim with sharks",b:"Jump out of a plane"},
  {c:"🌿 Nature",a:"Explore a volcano",b:"Explore a glacier"},
  {c:"🌿 Nature",a:"See a total solar eclipse",b:"See a meteor shower from a remote mountain"},
  {c:"🌿 Nature",a:"Have a huge garden",b:"Have a rooftop pool"},
  {c:"🌿 Nature",a:"See the Great Barrier Reef in its prime",b:"Hike the entire Appalachian Trail"},
  {c:"🌿 Nature",a:"Camp under the stars for a month",b:"Sail across the Atlantic"},
  {c:"🌿 Nature",a:"Witness the wildebeest migration",b:"See blue whales up close in the wild"},
  {c:"🧠 Mind",a:"Be brilliant but socially awkward",b:"Be socially gifted but average intelligence"},
  {c:"🧠 Mind",a:"Know the answer to any question",b:"Solve any problem brilliantly"},
  {c:"🧠 Mind",a:"Forget all bad memories",b:"Remember every good memory perfectly"},
  {c:"🧠 Mind",a:"Never feel boredom",b:"Never feel anxiety"},
  {c:"🧠 Mind",a:"Be extremely wise",b:"Be extremely creative"},
  {c:"🧠 Mind",a:"Understand any language instantly",b:"Master any instrument in seconds"},
  {c:"🧠 Mind",a:"Have total focus on demand",b:"Always sleep perfectly"},
  {c:"🧠 Mind",a:"Know your true life purpose",b:"Have zero regrets about your past"},
  {c:"🧠 Mind",a:"Read 1,000 words per minute",b:"Retain 100% of everything you read"},
  {c:"🧠 Mind",a:"Be a genius at math",b:"Be a genius at writing"},
  {c:"❤️ Relationships",a:"Have one amazing best friend",b:"Have 20 good friends"},
  {c:"❤️ Relationships",a:"Know your soulmate exists but never meet them",b:"Meet your soulmate but they live far away"},
  {c:"❤️ Relationships",a:"Be deeply loved by one person",b:"Be admired by millions"},
  {c:"❤️ Relationships",a:"Have a partner who is hilarious",b:"Have a partner who is thoughtful"},
  {c:"❤️ Relationships",a:"Always tell the truth even if it hurts",b:"Tell white lies to protect feelings"},
  {c:"❤️ Relationships",a:"Have a huge loving family",b:"Have a small tight-knit friend circle"},
  {c:"❤️ Relationships",a:"Be the most popular person at work",b:"Have just a few people who truly get you"},
  {c:"❤️ Relationships",a:"Have a partner who is adventurous",b:"Have a partner who makes you feel safe"},
  {c:"❤️ Relationships",a:"Fall in love but lose them after 5 years",b:"Never fall deeply in love but be content"},
  {c:"❤️ Relationships",a:"Have your crush like you back",b:"Get closure from your biggest heartbreak"},
  {c:"🌟 Fame",a:"Be famous for your art",b:"Be famous for changing the world"},
  {c:"🌟 Fame",a:"Be a movie star",b:"Be a rock star"},
  {c:"🌟 Fame",a:"Have 10M followers on social media",b:"Win a Nobel Prize"},
  {c:"🌟 Fame",a:"Be famous for 5 years then go private",b:"Be quietly influential forever"},
  {c:"🌟 Fame",a:"Have your book be a bestseller",b:"Have your film win an Oscar"},
  {c:"🌟 Fame",a:"Be interviewed by your favorite journalist",b:"Be on the cover of your favorite magazine"},
  {c:"🌟 Fame",a:"Be on a popular podcast",b:"Host your own TV show"},
  {c:"🌟 Fame",a:"Have a Wikipedia page about you",b:"Have a documentary made about your life"},
  {c:"🌟 Fame",a:"Be famous only in your city",b:"Be unknown but impact millions globally"},
  {c:"😂 Silly",a:"Only speak in rhymes for a year",b:"Only speak in questions for a year"},
  {c:"😂 Silly",a:"Have hiccups for a month",b:"Have the sneezes for a month"},
  {c:"😂 Silly",a:"Wear a chicken suit for a year",b:"Talk like a pirate for a year"},
  {c:"😂 Silly",a:"Laugh at everything for a day",b:"Cry at everything for a day"},
  {c:"😂 Silly",a:"Have pizza-flavored water",b:"Have water-flavored pizza"},
  {c:"😂 Silly",a:"Only walk sideways like a crab",b:"Only skip everywhere you go"},
  {c:"😂 Silly",a:"Have ears on your chin",b:"Have a nose on your forehead"},
  {c:"😂 Silly",a:"Sneeze confetti",b:"Sweat lemonade"},
  {c:"😂 Silly",a:"Have a parrot that only repeats bad advice",b:"Have a dog that judges every decision you make"},
  {c:"😂 Silly",a:"Never be able to whisper",b:"Never be able to shout"},
  {c:"😂 Silly",a:"Cluck like a chicken once an hour",b:"Bark like a dog once an hour"},
  {c:"😂 Silly",a:"Have spaghetti for hair",b:"Have gummy bears for teeth"},
  {c:"💻 Tech",a:"Live without your phone for a year",b:"Live without the internet for a year"},
  {c:"💻 Tech",a:"Have a robot that cleans everything",b:"Have an AI that handles all your email"},
  {c:"💻 Tech",a:"Have an exoskeleton suit",b:"Have a brain-computer interface"},
  {c:"💻 Tech",a:"Live in the metaverse for a year",b:"Live completely off-grid for a year"},
  {c:"💻 Tech",a:"Have a self-driving car",b:"Have a drone deliver everything you order"},
  {c:"💻 Tech",a:"3D print anything you want",b:"Have a replicator like Star Trek"},
  {c:"💻 Tech",a:"Upload your mind to a computer",b:"Download skills directly to your brain"},
  {c:"💻 Tech",a:"Time travel via machine",b:"Teleport via app"},
  {c:"💻 Tech",a:"Have a robot best friend",b:"Have an AI that truly understands you"},
  {c:"💻 Tech",a:"Own the first iPhone ever made",b:"Own the latest phone 5 years early"},
  {c:"🎨 Creative",a:"Write a bestselling novel",b:"Paint a masterpiece in the Louvre"},
  {c:"🎨 Creative",a:"Play every instrument perfectly",b:"Sing with a perfect voice"},
  {c:"🎨 Creative",a:"Design your dream home",b:"Design the most iconic building ever built"},
  {c:"🎨 Creative",a:"Star in a movie you write",b:"Direct a movie someone else wrote"},
  {c:"🎨 Creative",a:"Have your art shown in galleries worldwide",b:"Have your music played at major festivals"},
  {c:"🎨 Creative",a:"Write a song that defines a generation",b:"Write a book that changes the world"},
  {c:"🎨 Creative",a:"Be a fashion designer",b:"Be an interior designer"},
  {c:"🎨 Creative",a:"Have your own clothing line",b:"Have your own fragrance line"},
  {c:"🎨 Creative",a:"Be a tattoo artist",b:"Be a muralist"},
  {c:"🎨 Creative",a:"Create the most viral video ever",b:"Create the most shared photo ever"},
  {c:"🏠 Home Life",a:"Have a tiny house with zero clutter",b:"Have a huge house that's always messy"},
  {c:"🏠 Home Life",a:"Always have clean laundry magically",b:"Always have clean dishes magically"},
  {c:"🏠 Home Life",a:"Have a hot tub at home",b:"Have a home movie theater"},
  {c:"🏠 Home Life",a:"Have a home gym",b:"Have a home library"},
  {c:"🏠 Home Life",a:"Have a sauna",b:"Have an indoor pool"},
  {c:"🏠 Home Life",a:"Live in a penthouse in NYC",b:"Live in a beach house in Malibu"},
  {c:"🏠 Home Life",a:"Have a smart home that manages itself",b:"Have a chef's kitchen with every tool"},
  {c:"🏠 Home Life",a:"Never do dishes again",b:"Never fold laundry again"},
  {c:"🏠 Home Life",a:"Have the perfect commute",b:"Work from home forever"},
  {c:"🏠 Home Life",a:"Have a wildflower garden",b:"Have an indoor jungle"},
  {c:"🎓 Knowledge",a:"Know every historical fact",b:"Know the answer to every science question"},
  {c:"🎓 Knowledge",a:"Have an Ivy League degree for free",b:"Have 10 years of real-world experience"},
  {c:"🎓 Knowledge",a:"Learn anything in one hour",b:"Never forget anything you've learned"},
  {c:"🎓 Knowledge",a:"Be the smartest person in any room",b:"Be the most emotionally intelligent"},
  {c:"🎓 Knowledge",a:"Know every secret in history",b:"Know what happens in your own future"},
  {c:"🎓 Knowledge",a:"Have 3 PhDs",b:"Have mastered 3 totally different careers"},
  {c:"🎓 Knowledge",a:"Know what everyone really thinks of you",b:"Know what you'll think of yourself in 10 years"},
  {c:"🎓 Knowledge",a:"Always know what to say",b:"Always know what to do"},
  {c:"🏃 Health",a:"Have unlimited energy",b:"Need only 3 hours of sleep"},
  {c:"🏃 Health",a:"Never get a headache again",b:"Never get a cold again"},
  {c:"🏃 Health",a:"Have a perfect metabolism",b:"Have a perfect immune system"},
  {c:"🏃 Health",a:"Run a marathon easily",b:"Swim 5 miles easily"},
  {c:"🏃 Health",a:"Have the flexibility of a gymnast",b:"Have the strength of a powerlifter"},
  {c:"🏃 Health",a:"Never feel sore after working out",b:"Always feel motivated to work out"},
  {c:"🏃 Health",a:"Have perfect eyesight forever",b:"Have perfect hearing forever"},
  {c:"🏃 Health",a:"Age 10 years younger than you are",b:"Have the energy of a 20-year-old forever"},
  {c:"🏃 Health",a:"Recover from any injury in one day",b:"Never get injured during exercise"},
  {c:"🏃 Health",a:"Always wake up refreshed",b:"Always fall asleep in under 5 minutes"},
  {c:"🌙 Life Choices",a:"Know the day you'll die",b:"Know how you'll die"},
  {c:"🌙 Life Choices",a:"Restart life at 18 with all your memories",b:"Fast-forward 10 years from now"},
  {c:"🌙 Life Choices",a:"Change one decision in your past",b:"Get 3 extra years added to your life"},
  {c:"🌙 Life Choices",a:"Be remembered for 500 years",b:"Live an extra 50 years happily"},
  {c:"🌙 Life Choices",a:"Know your true purpose in life",b:"Have the courage to pursue it no matter what"},
  {c:"🌙 Life Choices",a:"Live 100 great years",b:"Live 200 average years"},
  {c:"🌙 Life Choices",a:"Be fearless",b:"Be endlessly kind"},
  {c:"🌙 Life Choices",a:"Have certainty about your choices",b:"Always trust your gut"},
  {c:"🌙 Life Choices",a:"Change a big regret",b:"Add a meaningful experience you never had"},
  {c:"🌙 Life Choices",a:"Choose exactly when to retire",b:"Live anywhere with no barriers"},
  {c:"🐾 Animals",a:"Have a talking dog",b:"Have a telepathic cat"},
  {c:"🐾 Animals",a:"Ride a horse to work",b:"Ride a dolphin to work"},
  {c:"🐾 Animals",a:"Be able to talk to any animal",b:"Understand what every animal is thinking"},
  {c:"🐾 Animals",a:"Have a pet dragon",b:"Have a pet unicorn"},
  {c:"🐾 Animals",a:"Have the speed of a cheetah",b:"Have the strength of a gorilla"},
  {c:"🐾 Animals",a:"Swim like a dolphin",b:"Fly like an eagle"},
  {c:"🐾 Animals",a:"Wake up as a bear for a week",b:"Wake up as an eagle for a week"},
  {c:"🐾 Animals",a:"Have 10 dogs",b:"Have 10 cats"},
  {c:"🐾 Animals",a:"Be a professional wildlife photographer",b:"Be a marine biologist"},
  {c:"🐾 Animals",a:"Have a wolf as a companion",b:"Have a raven as a companion"},
  {c:"🌀 Weird",a:"Have to sing everything you say",b:"Have to dance every time you hear music"},
  {c:"🌀 Weird",a:"Everything you touch turns to jello",b:"Everything you say echoes 3 times"},
  {c:"🌀 Weird",a:"Sneeze every time you lie",b:"Hiccup every time you're nervous"},
  {c:"🌀 Weird",a:"Only dream in movie trailers",b:"Only dream in video game cutscenes"},
  {c:"🌀 Weird",a:"Always smell like fresh cookies",b:"Always sound like you're underwater"},
  {c:"🌀 Weird",a:"See people as cartoon versions of themselves",b:"Hear everyone's voice as a famous character"},
  {c:"🌀 Weird",a:"Have a theme song that plays when you walk in",b:"Have a laugh track wherever you go"},
  {c:"🌀 Weird",a:"Glow faintly in the dark",b:"Leave a glitter trail wherever you walk"},
  {c:"🌀 Weird",a:"Age backwards from 80 to 0",b:"Age twice as fast but stop at 40 forever"},
  {c:"🌀 Weird",a:"See in black and white but hear in surround sound",b:"See in full HD color but be tone deaf"},
  {c:"🌀 Weird",a:"Speak in a different accent each day",b:"Walk at twice normal speed always"},
  {c:"🌀 Weird",a:"Have your thoughts shown as subtitles above you",b:"Have your mood shown as weather around you"},
  {c:"🌊 Adventure",a:"Surf giant waves in Hawaii",b:"Snowboard in the Alps"},
  {c:"🌊 Adventure",a:"Bungee jump off a bridge",b:"Zip-line through a rainforest"},
  {c:"🌊 Adventure",a:"Climb Everest with full support",b:"Solo hike Patagonia for a month"},
  {c:"🌊 Adventure",a:"Free dive to 100 feet",b:"Skydive from 30,000 feet"},
  {c:"🌊 Adventure",a:"Race a motorcycle on a track",b:"Pilot a small plane"},
  {c:"🌊 Adventure",a:"Do a polar plunge in Norway",b:"Go sandboarding in the Sahara"},
  {c:"🌊 Adventure",a:"Explore a massive cave system",b:"Navigate a jungle river by kayak"},
  {c:"🌊 Adventure",a:"Ride a hot air balloon at sunrise",b:"Watch the sunrise from a mountaintop"},
  {c:"🌊 Adventure",a:"Do a 10-day silent meditation retreat",b:"Go on a 10-day party cruise"},
  {c:"🌊 Adventure",a:"Live off the land for a month",b:"Sail around the world in a year"},
  {c:"🎬 Pop Culture",a:"Live in the world of Harry Potter",b:"Live in the world of Star Wars"},
  {c:"🎬 Pop Culture",a:"Be a character in your favorite book",b:"Be a character in your favorite movie"},
  {c:"🎬 Pop Culture",a:"Have dinner with any historical figure",b:"Have dinner with your fav fictional character"},
  {c:"🎬 Pop Culture",a:"Only watch 5 movies for the rest of your life",b:"Only listen to 5 albums for the rest of your life"},
  {c:"🎬 Pop Culture",a:"Have a cameo in a Marvel movie",b:"Guest star in your favorite TV show"},
  {c:"🎬 Pop Culture",a:"Meet your favorite musician",b:"Meet your favorite athlete"},
  {c:"🎬 Pop Culture",a:"Attend the Met Gala",b:"Attend the Oscars ceremony"},
  {c:"🎬 Pop Culture",a:"Have a song written about you",b:"Have a Netflix series made about your life"},
  {c:"🎬 Pop Culture",a:"Be a contestant on your fav game show",b:"Host your own talk show for one season"},
  {c:"🎬 Pop Culture",a:"Be in the final season of your fav show",b:"Write the ending of your favorite book series"},
  {c:"🏙️ Society",a:"Fix climate change completely",b:"End world poverty completely"},
  {c:"🏙️ Society",a:"Have world peace for 20 years",b:"Have a cure for cancer"},
  {c:"🏙️ Society",a:"Eliminate all misinformation online",b:"Give everyone access to clean water"},
  {c:"🏙️ Society",a:"Have universal healthcare globally",b:"Have universal education globally"},
  {c:"🏙️ Society",a:"End animal extinction",b:"Restore all coral reefs completely"},
  {c:"🏙️ Society",a:"Give the world a global empathy boost",b:"Give the world a global compassion boost"},
  {c:"🏙️ Society",a:"Live in a world with no crime",b:"Live in a world with no illness"},
  {c:"🏙️ Society",a:"Give everyone a guaranteed basic income",b:"Give everyone free mental health support"},
  {c:"🏙️ Society",a:"Give everyone a safe home",b:"Give everyone a meaningful job"},
  {c:"🏙️ Society",a:"Reverse ocean plastic pollution",b:"Replant every deforested area on Earth"},
];

const CATS = ["All", ...new Set(Q.map(q=>q.c))];
let filter = "All", shuffled = [], idx = 0, answers = {}, simVotes = {};

function shuffle(arr) {
  let a=[...arr];
  for(let i=a.length-1;i>0;i--){let j=Math.floor(Math.random()*(i+1));[a[i],a[j]]=[a[j],a[i]];}
  return a;
}
function simV(i) {
  if(!simVotes[i]){let a=33+Math.floor(Math.random()*34);simVotes[i]={a,b:100-a};}
  return simVotes[i];
}
function filteredQ() {
  return filter==="All"?Q:Q.filter(q=>q.c===filter);
}
function init() {
  shuffled=shuffle(filteredQ());
  idx=0;answers={};simVotes={};
  renderFilters();showQ();
  document.getElementById('game-view').style.display='flex';
  document.getElementById('summary-view').classList.remove('show');
}
function renderFilters() {
  document.getElementById('filter-row').innerHTML=CATS.map(c=>`<button class="chip${c===filter?' active':''}" onclick="setFilter('${c}')">${c}</button>`).join('');
}
function setFilter(cat){filter=cat;init();}

function showQ() {
  if(idx>=shuffled.length){showSummary();return;}
  const q=shuffled[idx];
  document.getElementById('cat-label').textContent=q.c;
  document.getElementById('q-num').textContent=idx+1;
  document.getElementById('q-total').textContent=shuffled.length;
  const pct=Math.round((idx/shuffled.length)*100);
  document.getElementById('prog').style.width=pct+'%';
  document.getElementById('prog-label').textContent=pct+'%';
  const btnA=document.getElementById('btn-a');
  const btnB=document.getElementById('btn-b');
  btnA.textContent=q.a;
  btnB.textContent=q.b;
  const ans=answers[idx];
  const bars=document.getElementById('vote-bars');
  btnA.className='option';btnB.className='option';
  bars.classList.remove('show');
  if(ans&&ans!=='skip'){
    btnA.className='option '+(ans==='a'?'chosen-a':'unchosen');
    btnB.className='option '+(ans==='b'?'chosen-b':'unchosen');
    bars.classList.add('show');
    const v=simV(idx);
    setTimeout(()=>{
      document.getElementById('vf-a').style.width=v.a+'%';
      document.getElementById('vf-b').style.width=v.b+'%';
      document.getElementById('vp-a').textContent=v.a+'%';
      document.getElementById('vp-b').textContent=v.b+'%';
    },60);
  }
  document.getElementById('next-btn').textContent=idx===shuffled.length-1?'Finish ✓':'Next →';
  document.getElementById('options-wrap').classList.remove('slide-in');
  void document.getElementById('options-wrap').offsetWidth;
  document.getElementById('options-wrap').classList.add('slide-in');
}

function choose(pick){answers[idx]=pick;showQ();}
function nextQ(){if(idx<shuffled.length-1){idx++;showQ();}else showSummary();}
function prevQ(){if(idx>0){idx--;showQ();}}
function skipQ(){answers[idx]='skip';if(idx<shuffled.length-1){idx++;showQ();}else showSummary();}

function showSummary(){
  document.getElementById('game-view').style.display='none';
  document.getElementById('summary-view').classList.add('show');
  const vals=Object.values(answers);
  const ans=vals.filter(v=>v!=='skip');
  const aC=ans.filter(v=>v==='a').length;
  const bC=ans.filter(v=>v==='b').length;
  document.getElementById('s-ans').textContent=ans.length;
  document.getElementById('s-skip').textContent=vals.filter(v=>v==='skip').length;
  document.getElementById('s-apct').textContent=ans.length?Math.round(aC/ans.length*100)+'%':'—';
  document.getElementById('s-bpct').textContent=ans.length?Math.round(bC/ans.length*100)+'%':'—';
  const map={};
  shuffled.forEach((q,i)=>{
    const v=answers[i];if(!v||v==='skip')return;
    if(!map[q.c])map[q.c]={a:0,b:0};
    map[q.c][v]++;
  });
  document.getElementById('cat-bd').innerHTML=Object.entries(map).map(([c,v])=>`<div class="cat-row"><span class="cat-name">${c}</span><span class="cat-nums">A: ${v.a} · B: ${v.b}</span></div>`).join('');
}
function restart(){
  document.getElementById('summary-view').classList.remove('show');
  init();
}

init();
</script>
</body>
</html>
