<!DOCTYPE html>
<html lang="ja">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>あなたのオタク属性×沼深度診断</title>

<style>
* {
  box-sizing: border-box;
}

body {
  margin: 0;
  font-family:
    -apple-system,
    BlinkMacSystemFont,
    "Helvetica Neue",
    "Hiragino Kaku Gothic ProN",
    "Yu Gothic",
    sans-serif;
  background: #fafafa;
  color: #222;
}

.container {
  width: min(92%, 480px);
  margin: auto;
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
}

.card {
  width: 100%;
  background: white;
  border-radius: 28px;
  padding: 30px 24px;
  box-shadow: 0 8px 30px rgba(0,0,0,.08);
  text-align: center;
}

h1 {
  font-size: 25px;
  line-height: 1.45;
  margin: 0 0 12px;
}

.subtitle {
  color: #888;
  font-size: 13px;
  margin-bottom: 28px;
}

.start-button,
.answer,
.action-button {
  width: 100%;
  border: none;
  border-radius: 16px;
  padding: 16px;
  font-size: 15px;
  font-weight: bold;
  cursor: pointer;
}

.start-button {
  background: #222;
  color: white;
}

.answer {
  background: #f5f5f5;
  margin-top: 12px;
  text-align: left;
  transition: .2s;
}

.answer:hover {
  background: #eaeaea;
  transform: translateY(-2px);
}

.progress {
  height: 6px;
  background: #eee;
  border-radius: 10px;
  overflow: hidden;
  margin-bottom: 25px;
}

.progress-bar {
  height: 100%;
  background: #222;
  width: 0%;
  transition: .3s;
}

.question-number {
  color: #999;
  font-size: 13px;
  margin-bottom: 12px;
}

.question {
  font-size: 20px;
  font-weight: bold;
  line-height: 1.5;
  margin-bottom: 20px;
}

.result-title {
  font-size: 14px;
  color: #999;
}

.type {
  font-size: 29px;
  font-weight: 900;
  margin: 8px 0 20px;
}

.level {
  font-size: 48px;
  font-weight: 900;
  margin: 5px 0;
}

.level-label {
  font-size: 13px;
  color: #999;
}

.meter {
  height: 12px;
  background: #eee;
  border-radius: 20px;
  overflow: hidden;
  margin: 15px 0 25px;
}

.meter-inner {
  height: 100%;
  background: #222;
  border-radius: 20px;
  transition: width 1s;
}

.description {
  background: #f7f7f7;
  border-radius: 18px;
  padding: 18px;
  line-height: 1.8;
  font-size: 14px;
  text-align: left;
  margin-bottom: 20px;
}

.stats {
  text-align: left;
  margin-bottom: 20px;
}

.stat {
  margin: 10px 0;
}

.stat-name {
  font-size: 12px;
  color: #777;
  margin-bottom: 4px;
}

.stat-bar {
  height: 7px;
  background: #eee;
  border-radius: 10px;
  overflow: hidden;
}

.stat-inner {
  height: 100%;
  background: #333;
}

.action-button {
  background: #222;
  color: white;
  margin-top: 10px;
}

.secondary {
  background: #eee;
  color: #222;
}

.hidden {
  display: none;
}

.small {
  font-size: 11px;
  color: #aaa;
  margin-top: 15px;
}
</style>
</head>

<body>

<div class="container">

  <!-- START -->
  <div class="card" id="startScreen">
    <div style="font-size:48px;">🫠</div>

    <h1>
      あなたの<br>
      オタク属性×沼深度診断
    </h1>

    <div class="subtitle">
      あなたは何に弱いオタク？<br>
      そして、どれくらい沼ってる？
    </div>

    <button class="start-button" onclick="startQuiz()">
      診断する
    </button>

    <div class="small">
      全20問・約2分
    </div>
  </div>


  <!-- QUIZ -->
  <div class="card hidden" id="quizScreen">

    <div class="progress">
      <div class="progress-bar" id="progressBar"></div>
    </div>

    <div class="question-number" id="questionNumber"></div>

    <div class="question" id="question"></div>

    <div id="answers"></div>

  </div>


  <!-- RESULT -->
  <div class="card hidden" id="resultScreen">

    <div class="result-title">
      あなたのオタク属性は……
    </div>

    <div class="type" id="resultType"></div>

    <div class="level-label">
      沼深度
    </div>

    <div class="level">
      <span id="resultLevel"></span>
      <span style="font-size:20px;"> / 100</span>
    </div>

    <div class="meter">
      <div class="meter-inner" id="meter"></div>
    </div>

    <div class="description" id="description"></div>

    <div class="stats" id="stats"></div>

    <button class="action-button" onclick="shareX()">
      𝕏 結果をXに投稿
    </button>

    <button class="action-button secondary" onclick="restart()">
      もう一度診断する
    </button>

    <div class="small">
      #オタク属性診断
    </div>

  </div>

</div>


<script>

const questions = [

  {
    q: "新しい写真が公開された！まず見るのは？",
    answers: [
      { text:"顔・ビジュ", type:"visual", level:5 },
      { text:"衣装", type:"visual", level:3 },
      { text:"メンバー同士の絡み", type:"relationship", level:4 },
      { text:"投稿された日時や情報", type:"info", level:5 }
    ]
  },

  {
    q: "自担の新曲が出たら？",
    answers: [
      { text:"とにかくMVを見る", type:"performance", level:4 },
      { text:"歌詞を読む", type:"words", level:4 },
      { text:"推しのパートを探す", type:"voice", level:5 },
      { text:"売上やランキングも気になる", type:"info", level:6 }
    ]
  },

  {
    q: "雑誌に自担が載っていたら？",
    answers: [
      { text:"1冊買う", type:"visual", level:3 },
      { text:"表紙なら絶対買う", type:"visual", level:5 },
      { text:"複数冊買う", type:"info", level:8 },
      { text:"過去の掲載号も探す", type:"info", level:10 }
    ]
  },

  {
    q: "ライブ映像で一番見てしまうのは？",
    answers: [
      { text:"顔", type:"visual", level:4 },
      { text:"ダンス", type:"performance", level:5 },
      { text:"歌声", type:"voice", level:5 },
      { text:"メンバーとの絡み", type:"relationship", level:5 }
    ]
  },

  {
    q: "自担が誰かと仲良くしている。あなたは？",
    answers: [
      { text:"かわいい〜！", type:"relationship", level:4 },
      { text:"その絡みを何度も見る", type:"relationship", level:7 },
      { text:"コンビ名を調べる", type:"info", level:8 },
      { text:"過去の絡みまで掘る", type:"relationship", level:10 }
    ]
  },

  {
    q: "推しの髪型が変わった！",
    answers: [
      { text:"似合ってるか確認", type:"visual", level:3 },
      { text:"昔の髪型と比較する", type:"visual", level:7 },
      { text:"過去写真を大量に探す", type:"visual", level:9 },
      { text:"どの時期が一番好きか語れる", type:"visual", level:10 }
    ]
  },

  {
    q: "自担のブログが更新された！",
    answers: [
      { text:"あとで読む", type:"words", level:2 },
      { text:"すぐ読む", type:"words", level:5 },
      { text:"全文スクショする", type:"words", level:8 },
      { text:"文章から色々考察する", type:"words", level:10 }
    ]
  },

  {
    q: "自担の声についてどう思う？",
    answers: [
      { text:"好き", type:"voice", level:4 },
      { text:"かなり好き", type:"voice", level:6 },
      { text:"声だけで誰かわかる", type:"voice", level:9 },
      { text:"話し声・歌声・笑い声全部好き", type:"voice", level:10 }
    ]
  },

  {
    q: "推しがテレビ出演！",
    answers: [
      { text:"リアタイする", type:"info", level:4 },
      { text:"録画する", type:"info", level:6 },
      { text:"録画＋見逃し配信も確認", type:"info", level:8 },
      { text:"出演シーンを何度も見る", type:"info", level:10 }
    ]
  },

  {
    q: "ファンサを選ぶなら？",
    answers: [
      { text:"指差し", type:"fanservice", level:5 },
      { text:"投げキッス", type:"fanservice", level:6 },
      { text:"確定ファンサ", type:"fanservice", level:8 },
      { text:"目が合うだけでいい", type:"fanservice", level:7 }
    ]
  },

  {
    q: "過去映像を見つけた！",
    answers: [
      { text:"ちょっと見る", type:"info", level:3 },
      { text:"最後まで見る", type:"info", level:5 },
      { text:"関連動画も全部見る", type:"info", level:9 },
      { text:"その時期の映像を全部掘る", type:"info", level:10 }
    ]
  },

  {
    q: "自担の昔の写真を見つけたら？",
    answers: [
      { text:"かわいい", type:"visual", level:4 },
      { text:"保存", type:"visual", level:6 },
      { text:"年代順に集める", type:"info", level:9 },
      { text:"出典まで探す", type:"info", level:10 }
    ]
  },

  {
    q: "ライブ遠征について",
    answers: [
      { text:"近場だけ", type:"live", level:2 },
      { text:"予定が合えば遠征", type:"live", level:5 },
      { text:"遠征する", type:"live", level:8 },
      { text:"全国どこでも行く", type:"live", level:10 }
    ]
  },

  {
    q: "グッズはどうする？",
    answers: [
      { text:"欲しいものだけ", type:"live", level:3 },
      { text:"推しのものは買う", type:"live", level:5 },
      { text:"基本全部買う", type:"live", level:8 },
      { text:"保存用も買う", type:"live", level:10 }
    ]
  },

  {
    q: "自担の誕生日が近づくと？",
    answers: [
      { text:"お祝いする", type:"fanservice", level:4 },
      { text:"ケーキを買う", type:"fanservice", level:5 },
      { text:"祭壇を作る", type:"fanservice", level:8 },
      { text:"SNSを誕生日仕様にする", type:"fanservice", level:9 }
    ]
  },

  {
    q: "推しの発言で気になることがあったら？",
    answers: [
      { text:"そのまま受け取る", type:"words", level:2 },
      { text:"もう一度聞く", type:"words", level:4 },
      { text:"過去発言を探す", type:"words", level:8 },
      { text:"関連する発言を全部調べる", type:"info", level:10 }
    ]
  },

  {
    q: "新しい衣装が出た！",
    answers: [
      { text:"似合ってる〜", type:"visual", level:4 },
      { text:"細部まで見る", type:"visual", level:7 },
      { text:"過去衣装と比較する", type:"visual", level:9 },
      { text:"衣装のブランドまで調べる", type:"info", level:10 }
    ]
  },

  {
    q: "自担の出演情報を見つけたら？",
    answers: [
      { text:"覚えておく", type:"info", level:3 },
      { text:"カレンダーに入れる", type:"info", level:6 },
      { text:"全部録画予約", type:"info", level:8 },
      { text:"出演情報を常に追っている", type:"info", level:10 }
    ]
  },

  {
    q: "自担のコンビに新しい供給が来た！",
    answers: [
      { text:"かわいい！", type:"relationship", level:4 },
      { text:"保存する", type:"relationship", level:6 },
      { text:"何度も見返す", type:"relationship", level:8 },
      { text:"過去の供給まで見返す", type:"relationship", level:10 }
    ]
  },

  {
    q: "最後の質問。自担についてどれくらい語れる？",
    answers: [
      { text:"好きなところなら語れる", type:"visual", level:4 },
      { text:"かなり語れる", type:"words", level:6 },
      { text:"年代別に語れる", type:"info", level:9 },
      { text:"本人より詳しい気がする", type:"info", level:10 }
    ]
  }

];


const typeNames = {
  visual: "💘 ビジュアル型",
  voice: "🎤 声・歌型",
  performance: "🕺 パフォーマンス型",
  words: "📝 言葉・ブログ型",
  relationship: "🫂 関係性型",
  fanservice: "💌 ファンサ型",
  info: "🔎 情報収集型",
  live: "🎫 現場型"
};


const descriptions = {

  visual:
    "あなたは「顔が好き」から始まり、髪型・衣装・表情まで全部見てしまうタイプ。『この時期のビジュが一番好き』という話になると急に饒舌になります。",

  voice:
    "あなたは声に弱いタイプ。歌声はもちろん、話し声や笑い声まで好きになりがち。イヤホンから聞こえる自担の声だけで幸せになれます。",

  performance:
    "あなたはステージ上の自担に弱いタイプ。ダンス、歌、表情、立ち姿……ライブ映像を何度も見返してしまいます。",

  words:
    "あなたは言葉に弱いタイプ。ブログ、インタビュー、MCなど、自担が何を考えているのかを知るのが大好き。何気ない一言をずっと覚えています。",

  relationship:
    "あなたは関係性に弱いタイプ。本人単体も好きだけど、メンバーと一緒にいるとさらに好き。供給が来るたびに過去の絡みまで掘り返します。",

  fanservice:
    "あなたはファンサに弱いタイプ。たった数秒の出来事を何年でも擦れます。『あれ絶対私にだった』が一生の宝物。",

  info:
    "あなたは情報収集型。出演情報、雑誌、過去映像、ブログ、インタビューまで気になったら調べずにはいられません。知らない情報を見つけると嬉しくなるタイプ。",

  live:
    "あなたは現場型。画面越しもいいけど、やっぱり現場が好き。ライブ、舞台、イベントの思い出がどんどん増えていくタイプです。"
};


let current = 0;

let scores = {
  visual:0,
  voice:0,
  performance:0,
  words:0,
  relationship:0,
  fanservice:0,
  info:0,
  live:0
};

let levelScore = 0;


function startQuiz() {

  current = 0;

  scores = {
    visual:0,
    voice:0,
    performance:0,
    words:0,
    relationship:0,
    fanservice:0,
    info:0,
    live:0
  };

  levelScore = 0;

  document.getElementById("startScreen").classList.add("hidden");
  document.getElementById("resultScreen").classList.add("hidden");
  document.getElementById("quizScreen").classList.remove("hidden");

  showQuestion();
}


function showQuestion() {

  const q = questions[current];

  document.getElementById("questionNumber").textContent =
    `QUESTION ${current + 1} / ${questions.length}`;

  document.getElementById("question").textContent = q.q;

  document.getElementById("progressBar").style.width =
    `${(current / questions.length) * 100}%`;

  const answers = document.getElementById("answers");

  answers.innerHTML = "";

  q.answers.forEach(answer => {

    const button = document.createElement("button");

    button.className = "answer";
    button.textContent = answer.text;

    button.onclick = () => {

      scores[answer.type] += 1;
      levelScore += answer.level;

      current++;

      if(current >= questions.length) {
        showResult();
      } else {
        showQuestion();
      }

    };

    answers.appendChild(button);

  });
}


function showResult() {

  document.getElementById("quizScreen").classList.add("hidden");
  document.getElementById("resultScreen").classList.remove("hidden");

  let type = Object.keys(scores).reduce((a,b) =>
    scores[a] > scores[b] ? a : b
  );

  let level = Math.round(
    (levelScore / (questions.length * 10)) * 100
  );

  level = Math.min(100, level);

  document.getElementById("resultType").textContent =
    typeNames[type];

  document.getElementById("resultLevel").textContent =
    level;

  document.getElementById("meter").style.width =
    level + "%";

  document.getElementById("description").textContent =
    descriptions[type];

  const stats = document.getElementById("stats");

  stats.innerHTML = "";

  const sorted = Object.entries(scores)
    .sort((a,b) => b[1] - a[1]);

  sorted.slice(0,5).forEach(([key,value]) => {

    const percentage = Math.round(
      (value / 5) * 100
    );

    stats.innerHTML += `
      <div class="stat">
        <div class="stat-name">${typeNames[key]}</div>
        <div class="stat-bar">
          <div class="stat-inner"
               style="width:${Math.min(100,percentage)}%">
          </div>
        </div>
      </div>
    `;

  });

  window.resultText =
    `私のオタク属性は「${typeNames[type]}」でした！\n` +
    `沼深度 ${level}/100 🫠\n\n` +
    `#オタク属性診断`;

}


function shareX() {

  const url =
    "https://twitter.com/intent/tweet?text=" +
    encodeURIComponent(window.resultText);

  window.open(url, "_blank");

}


function restart() {

  document.getElementById("resultScreen").classList.add("hidden");
  document.getElementById("startScreen").classList.remove("hidden");

}

</script>

</body>
</html>
