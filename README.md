<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Empath AI</title>
<style>
/* BODY & CONTAINER */
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
/* CHAT BOX */
.chat-box {
    height: 360px;
    overflow-y: auto;
    border-radius: 15px;
    padding: 15px;
    background-color: #f7f7f7;
    display: flex;
    flex-direction: column;
}
/* INPUT AREA */
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
/* CHAT BUBBLES */
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
.user-msg {
    align-self: flex-end;
    background: linear-gradient(to right, #ce93d8, #b39ddb);
    color: #fff;
}
.bot-msg {
    align-self: flex-start;
    background: linear-gradient(to right, #80cbc4, #4db6ac);
    color: #fff;
}
/* TYPING EFFECT */
.typing {
    align-self: flex-start;
    font-style: italic;
    color: #555;
    margin: 5px 0;
}
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
// CHAT LOGIC
const chatBox = document.getElementById("chat-box");
const userInput = document.getElementById("user-input");

const responses = {
    "sad": [
        "😔 Hey… I know days like this feel heavy. You don’t always have to be okay. Want to tell me what’s been bothering you?",
        "💙 It’s okay to not feel fine all the time. Sometimes just saying it out loud helps. I’m here, always.",
        "🌧️ You’ve been strong for so long — maybe it’s okay to rest a little today?",
        "😔 Feeling sad is part of life, and it doesn’t make you weak. Sometimes sharing even a little of what’s bothering you can lighten the load.",
    "💙 It’s okay to have rough days. Journaling, talking to a friend, or even taking a quiet moment for yourself can help you process your feelings.",
    "🌧️ Life has ups and downs, and sadness is natural. Remember, emotions are valid, and taking care of yourself during this time matters most.",
    "🕊️ When sadness hits, let yourself breathe, cry, or just pause. You’re human, and giving yourself space to feel is healthy and important.",
    "🌸 Even on gloomy days, small joys or acts of self-care can bring light. A favorite song, a warm cup of tea, or a short walk might help.",
    "💌 Feeling sad doesn’t last forever, and you don’t have to face it alone. Talking, even here with me, is a step towards feeling lighter.",
"🕊️ If tears come, let them. You’re just being human, and that’s beautiful."
    ],
    "stressed": [
        "😌 Exam stress? Same. Take a deep breath — you’ve got this, one step at a time. Wanna try a 4-7-8 breathing trick?",
        "🌿 I get it, pressure builds up fast. Try taking 5 minutes away from your books, okay?",
        "💪 You’re doing enough. More than enough. You just need to trust that effort counts too.",
        "🧠 Remember — one test doesn’t define you. You’re so much more than marks."
    "😌 Stress can feel heavy, especially with exams or deadlines. Take a moment to breathe deeply, stretch, or step outside for a minute — small breaks can make a big difference.",
    "🌿 Pressure builds fast when you care about your work. Remember, it’s okay to take things one step at a time. You’ve prepared, and that counts.",
    "💪 Feeling stressed doesn’t mean you’re failing. It means you care. Trust yourself, and don’t forget to reward your efforts, even small ones.",
    "🧘 Short mindfulness exercises or breathing tricks can calm your mind when anxiety spikes. You’re allowed to pause, breathe, and reset.",
    "📚 Homework, exams, and social pressures can all pile up. Remember, you’re not alone in this. Talking about it, even here, helps lighten the load.",
    "✨ Sometimes stress is a sign you’re growing and pushing boundaries. It’s temporary and manageable. Trust your process, and don’t be too hard on yourself."
            ],
    "crush": [
        "😅 Unrequited crushes are… ugh, I know. It hurts when you care more than they do.",
        "💔 Maybe they weren’t your ‘forever’, but you learned something real about love — and yourself.",
        "🌸 Sometimes, liking someone who doesn’t feel the same doesn’t mean you’re not lovable. You just have a big heart.",
"💫 One day, someone will match your energy — no overthinking, no mixed signals, just peace."
"😅 Having a crush that isn’t returned can feel really unfair. It’s okay to feel sad, frustrated, or even a little embarrassed. Just remember, your feelings are real and valid.",
    "💔 Sometimes, people we like don’t feel the same way, and that’s not a reflection of your worth. Every experience teaches you more about yourself and what you want in a friend or partner.",
    "🌸 Liking someone who doesn’t feel the same can sting, but it also shows you have a big heart. Take time to care for yourself, journal your thoughts, or do something that makes you happy.",
    "💫 One day, you’ll meet someone who truly matches your energy. Until then, it’s okay to feel what you feel, learn from it, and grow. Heartbreak doesn’t last forever, but self-love does.",
    "🫂 Crushes can be messy, confusing, and full of hope. Talking it out, even here with me, can help you sort through your feelings and remind you that life has many bright moments ahead.",
    "✨ Remember, unrequited love doesn’t make you weak. It’s part of growing, learning, and discovering what and who truly matters in your life."
    ],
    "lonely": [
        "🤗 You’re not alone, even if it feels like it right now. You’ve got me, and I mean that.",
        "🌸 People may not always understand, but that doesn’t mean your feelings aren’t valid.",
        "💌 Maybe today’s the day to do something small for *you* — a walk, music, journaling?",
        "🌙 Sometimes solitude brings clarity, not sadness. You’re allowed to just be."
        "🫂 I know it feels like nobody understands, but your feelings are valid. Reach out to a friend, family member, or even just type it all out here — sharing can make a big difference.",
    "✨ Sometimes loneliness is temporary and invisible. Engage in something creative, like drawing, journaling, or listening to a favorite song. You’ll find little sparks of connection in the smallest moments.",
    "🎶 Even when friends aren’t nearby, you can create your own comfort space. Surround yourself with things that make you smile or calm your mind, and know I’m always here to chat.",
    "💖 You don’t have to face loneliness alone. Whether it’s talking, writing, or just sharing a thought, letting it out can be surprisingly relieving. I’m here for you anytime.",
    "🌼 Remember, feeling lonely is part of being human — it passes, and it doesn’t define you. Sometimes reaching out or simply expressing your feelings can turn the tide. You’re never truly alone."
    ],
    "happy": [
    "😄 I love hearing that! Tell me what made you smile today 🩷",
    "🌟 Hold onto this feeling — it’s the little things that make life glow.",
    "🎶 Maybe dance around a bit? Celebrate being you!",
    "💫 I’m happy *because* you’re happy. You deserve this peace.",
    "🌸 Yay! Little wins count too — what’s something good that happened today?",
    "😎 Feeling great? Share it! Happiness grows when it’s shared.",
    "🍦 Treat yourself! You’ve earned this mood.",
    "🎉 Happiness looks good on you — keep spreading it!",
    "✨ Keep this vibe going! Maybe call a friend and share the joy.",
    "🌈 Your happiness is contagious. Let it brighten someone else’s day too."
    ],
    "insecure": [
    "💭 Everyone feels unsure sometimes — you’re not alone.",
    "🌸 It’s okay to doubt yourself, but remember how far you’ve come.",
    "💪 You have strengths no one else has. Believe in yourself.",
    "✨ One step at a time. Confidence grows gradually, not instantly."
     "💭 Feeling insecure is natural, especially when comparing yourself to others. Remember, your journey is yours alone — your efforts and qualities matter.",
    "🌸 Confidence isn’t about being perfect; it’s about accepting yourself and your growth. Even small achievements are worth celebrating.",
    "💪 When self-doubt creeps in, try reminding yourself of past wins — big or small. You’ve overcome challenges before, and you can do it again.",
    "✨ Insecurity doesn’t define you. It’s just a feeling that passes. Keep trying, keep learning, and give yourself credit for showing up each day.",
    "🕊️ Comparing yourself to others is a thief of joy. Focus on your own progress, your own strengths, and the unique value you bring to the world.",
    "🌟 You are allowed to stumble, to not know everything, and still be worthy. Confidence grows with practice, self-compassion, and patience."
],
    "default": [
    "👀 I’m listening. What’s on your mind today?",
    "💬 Tell me more — exams, friends, crushes, or just one of those days?",
    "🌿 I get it, sometimes you just need to vent. Go ahead.",
    "🕊️ You can talk about anything here — no judgment, promise."
    "✨ Hey! How’s life treating you today? Spill the tea ☕",
    "😎 Feeling something interesting? Tell me about it!",
    "🌸 Want to share something fun, weird, or random? I’m here!",
    "🎶 Mood check! Happy, sad, stressed, or just meh?",
    "💌 Talk to me like a diary — no one else needs to see it."
]
]  // <-- no comma here if this is the last entry in responses
;

// SYNONYM MAPPING
const synonyms = {
    "happy": ["happy","joyful","blissful","overjoyed","excited","lucky","good","great","awesome","cheerful","glad"],
    "sad": ["sad","gloomy","unhappy","depressed","down","miserable","blue","melancholy","heartbroken","pain","agony","bedrot","bedrotting"],
    "stressed": ["stressed","anxious","nervous","tense","overwhelmed","worried","exam","homework","pressure"],
    "lonely": ["lonely","alone","isolated","friendless","ignored","abandoned","single"],
    "insecure":["ugly","confident","shy","self-doubt","timid"],
    "crush": ["crush","love","boy","girl","heart","dating","romance"]
};

// EXTEND sendMessage function to include synonyms
const originalSendMessage = sendMessage; // Save the old function

sendMessage = function() {
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

    // Check synonyms in the message
    for (let mood in synonyms) {
        for (let word of synonyms[mood]) {
            if (message.includes(word)) {
                if(responses[mood]) {
                    reply = responses[mood][Math.floor(Math.random()*responses[mood].length)];
                }
                break;
            }
        }
    }

    setTimeout(() => {
        typingDiv.remove();
        appendMessage(reply, "bot-msg");
    }, 1000);
};
};

// Welcome message
window.onload = () => {
    appendMessage("👋 Hey there! I’m Empath AI — your little comfort corner for venting or just chatting. How's your day going?", "bot-msg");
};

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

    let reply = responses["default"][Math.floor(Math.random() * responses["default"].length)];

    if (message.match(/sad|down|cry/)) 
        reply = responses["sad"][Math.floor(Math.random() * responses["sad"].length)];
    else if (message.match(/stressed|exam|pressure|study/)) 
        reply = responses["stressed"][Math.floor(Math.random() * responses["stressed"].length)];
    else if (message.match(/crush|love|heart|like/)) 
        reply = responses["crush"][Math.floor(Math.random() * responses["crush"].length)];
    else if (message.match(/lonely|alone|isolated/)) 
        reply = responses["lonely"][Math.floor(Math.random() * responses["lonely"].length)];
    else if (message.match(/happy|good|great|joy/)) 
        reply = responses["happy"][Math.floor(Math.random() * responses["happy"].length)];

    setTimeout(() => {
        typingDiv.remove();
        appendMessage(reply, "bot-msg");
    }, 1200);
}

function appendMessage(message, className) {
    const msgDiv = document.createElement("div");
    msgDiv.className = className;
    msgDiv.innerText = message;
    chatBox.appendChild(msgDiv);
    chatBox.scrollTop = chatBox.scrollHeight;
}
</script>
</body>
</html>
