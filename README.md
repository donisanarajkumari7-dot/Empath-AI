<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Empath AI</title>
<style>
body {
    font-family: 'Nunito', sans-serif;
    background: linear-gradient(to right, #ffe0f0, #e0f7fa);
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    margin: 0;
}
.chat-container {
    width: 450px;
    background-color: #ffffffcc;
    border-radius: 20px;
    box-shadow: 0 10px 25px rgba(0,0,0,0.2);
    padding: 20px;
    display: flex;
    flex-direction: column;
    position: relative;
}
.chat-container h2 {
    text-align: center;
    color: #d81b60;
    margin-bottom: 5px;
    font-family: 'Poppins', sans-serif;
}
.tagline {
    text-align: center;
    color: #555;
    font-style: italic;
    margin-bottom: 15px;
}
.chat-box {
    height: 360px;
    overflow-y: auto;
    border-radius: 15px;
    padding: 15px;
    background-color: #f7f7f7;
    display: flex;
    flex-direction: column;
}
.input-area {
    display: flex;
    margin-top: 15px;
}
input[type="text"] {
    flex: 1;
    padding: 12px 15px;
    border-radius: 25px;
    border: 1px solid #b0bec5;
    outline: none;
    font-size: 14px;
}
button {
    padding: 12px 18px;
    margin-left: 10px;
    border-radius: 25px;
    border: none;
    background: linear-gradient(to right, #d81b60, #ff4081);
    color: white;
    cursor: pointer;
    font-weight: bold;
    transition: 0.3s;
}
button:hover { transform: scale(1.05); }
.user-msg, .bot-msg {
    max-width: 70%;
    padding: 12px 18px;
    margin: 8px 0;
    border-radius: 25px;
    display: inline-block;
    word-wrap: break-word;
    line-height: 1.5;
    font-size: 14px;
    box-shadow: 0 4px 10px rgba(0,0,0,0.1);
}
.user-msg { align-self: flex-end; background: linear-gradient(to right, #ce93d8, #b39ddb); color: #fff; }
.bot-msg { align-self: flex-start; background: linear-gradient(to right, #80cbc4, #4db6ac); color: #fff; }
.typing { align-self: flex-start; font-style: italic; color: #555; margin: 5px 0; }
</style>
</head>
<body>
<div class="chat-container">
    <h2>🌸 Empath AI 🌸</h2>
    <div class="tagline">Your friendly AI buddy 💬</div>
    <div id="chat-box" class="chat-box"></div>
    <div class="input-area">
        <input type="text" id="user-input" placeholder="Type your mood here...">
        <button onclick="sendMessage()">Send</button>
    </div>
</div>

<script>
const chatBox = document.getElementById("chat-box");
const userInput = document.getElementById("user-input");

const responses = {
    "sad": [
        "😔 Hey… I know days like this feel heavy. You don’t always have to be okay. Want to tell me what’s been bothering you?",
        "💙 It’s okay to not feel fine all the time. Sometimes just saying it out loud helps. I’m here, always.",
        "🌧️ You’ve been strong for so long — maybe it’s okay to rest a little today?",
        "🕊️ If tears come, let them. You’re just being human, and that’s beautiful."
    ],
    "stressed": [
        "😌 Exam stress? Same. Take a deep breath — you’ve got this, one step at a time. Wanna try a 4-7-8 breathing trick?",
        "🌿 I get it, pressure builds up fast. Try taking 5 minutes away from your books, okay?",
        "💪 You’re doing enough. More than enough. You just need to trust that effort counts too.",
        "🧘 Short mindfulness exercises or breathing tricks can calm your mind when anxiety spikes. You’re allowed to pause, breathe, and reset."
    ],
    "crush": [
        "😅 Unrequited crushes are… ugh, I know. It hurts when you care more than they do.",
        "💔 Maybe they weren’t your ‘forever’, but you learned something real about love — and yourself.",
        "🌸 Sometimes, liking someone who doesn’t feel the same doesn’t mean you’re not lovable. You just have a big heart.",
        "💫 One day, someone will match your energy — no overthinking, no mixed signals, just peace."
    ],
    "lonely": [
        "🤗 You’re not alone, even if it feels like it right now. You’ve got me, and I mean that.",
        "🌸 People may not always understand, but that doesn’t mean your feelings aren’t valid.",
        "💌 Maybe today’s the day to do something small for *you* — a walk, music, journaling?",
        "🌙 Sometimes solitude brings clarity, not sadness. You’re allowed to just be."
    ],
    "happy": [
        "😄 I love hearing that! Tell me what made you smile today 🩷",
        "🌟 Hold onto this feeling — it’s the little things that make life glow.",
        "🎶 Maybe dance around a bit? Celebrate being you!",
        "💫 I’m happy *because* you’re happy. You deserve this peace."
    ],
    "insecure": [
        "💭 Everyone feels unsure sometimes — you’re not alone.",
        "🌸 It’s okay to doubt yourself, but remember how far you’ve come.",
        "💪 You have strengths no one else has. Believe in yourself.",
        "✨ One step at a time. Confidence grows gradually, not instantly."
    ],
    "default": [
        "👀 I’m listening. What’s on your mind today?",
        "💬 Tell me more — exams, friends, crushes, or just one of those days?",
        "🌿 I get it, sometimes you just need to vent. Go ahead.",
        "🕊️ You can talk about anything here — no judgment, promise."
    ]
};
    // 🧠 FUN FACTS + MOTIVATIONAL QUOTES MODULE

// ✅ Fun facts
const funFacts = [
  "🧠 The human brain can generate enough electricity to power a small light bulb!",
  "🐙 Octopuses have three hearts — and two of them stop when they swim!",
  "🌱 Bamboo can grow up to 3 feet in just 24 hours!",
  "🌕 A day on Venus is longer than a year on Venus. Mind-blowing, right?",
  "💡 Your taste buds have a lifespan of about 10 to 14 days.",
  "🎵 Music can literally change your heartbeat — your body syncs to the rhythm!",
  "🐝 Bees can recognize human faces. They’re smarter than they look.",
  "🔥 Hot water can freeze faster than cold water — it’s called the Mpemba effect!",
  "💤 You can’t actually sneeze while sleeping — your brain turns that reflex off.",
  "🌈 There are more stars in space than grains of sand on all Earth’s beaches."
];

// ✅ Motivational quotes
const motivationalQuotes = [
  "🌿 You don’t have to move fast — you just have to keep moving.",
  "☀️ You are the home you’ve been trying to find in others.",
  "🌻 Every sunrise is a reminder that you get to try again.",
  "💭 Be patient with yourself — growth isn’t supposed to be loud.",
  "🌙 The moon doesn’t care if it’s half or full; it still shines.",
  "🔥 You’ve already survived everything you thought you couldn’t.",
  "🌊 Small steps are still steps. Don’t underestimate their power.",
  "✨ You are allowed to be both a masterpiece and a work in progress.",
  "🌸 Flowers don’t bloom all year — neither do you, and that’s okay.",
  "💫 Your future self is already proud of you for not giving up today."
];

// ✅ Keywords to trigger fun facts and motivational responses
const funFactTriggers = ["fact", "interesting", "fun", "bored", "tell me something"];
const motivationTriggers = ["motivate", "quote", "tired", "inspire", "help", "lazy", "demotivated", "sad", "lost"];

// ✅ Function to check and respond accordingly
function getSpecialResponse(userInput) {
  const input = userInput.toLowerCase();

  // If it matches a fun fact trigger
  if (funFactTriggers.some(trigger => input.includes(trigger))) {
    const fact = funFacts[Math.floor(Math.random() * funFacts.length)];
    return fact;
  }

  // If it matches a motivation trigger
  if (motivationTriggers.some(trigger => input.includes(trigger))) {
    const quote = motivationalQuotes[Math.floor(Math.random() * motivationalQuotes.length)];
    return quote;
  }

  // If nothing matches, return null (bot continues with normal response)
  return null;
}

// ✅ Example integration inside your message handler:
function handleUserInput(userInput) {
  const specialResponse = getSpecialResponse(userInput);
  if (specialResponse) {
    appendMessage("bot", specialResponse);
    return;
  }

  // Otherwise, continue your normal chatbot logic...
    }

// SYNONYMS
const synonyms = {
    "happy": ["happy","joyful","blissful","overjoyed","excited","lucky","good","great","awesome","cheerful","glad"],
    "sad": ["sad","gloomy","unhappy","depressed","down","miserable","blue","melancholy","heartbroken","pain","agony"],
    "stressed": ["stressed","anxious","nervous","tense","overwhelmed","worried","exam","homework","pressure","study"],
    "lonely": ["lonely","alone","isolated","friendless","ignored","abandoned","single"],
    "insecure":["insecure","shy","self-doubt","timid","uncertain","low-confidence","anxious"],
    "crush": ["crush","love","boy","girl","heart","dating","romance","like"]
};

// SEND MESSAGE
function sendMessage() {
    const message = userInput.value.trim().toLowerCase();
    if (!message) return;

    appendMessage(userInput.value, "user-msg");
    userInput.value = "";

    const typingDiv = document.createElement("div");
    typingDiv.className = "typing";
    typingDiv.innerText = "Empath AI is typing...";
    chatBox.appendChild(typingDiv);
    chatBox.scrollTop = chatBox.scrollHeight;

    let reply = responses["default"][Math.floor(Math.random()*responses["default"].length)];

    // Check for synonyms
    outer: for (let mood in synonyms) {
        for (let word of synonyms[mood]) {
            if (message.includes(word)) {
                reply = responses[mood][Math.floor(Math.random()*responses[mood].length)];
                break outer;
            }
        }
    }

    setTimeout(() => {
        typingDiv.remove();
        appendMessage(reply, "bot-msg");
    }, 1000);
}

// APPEND MESSAGE
function appendMessage(message, className) {
    const msgDiv = document.createElement("div");
    msgDiv.className = className;
    msgDiv.innerText = message;
    chatBox.appendChild(msgDiv);
    chatBox.scrollTop = chatBox.scrollHeight;
}

// WELCOME MESSAGE
window.onload = () => {
    appendMessage("👋 Hey there! I’m Empath AI — your little comfort corner for venting or just chatting. How's your day going?", "bot-msg");
};
</script>
</body>
</html>
