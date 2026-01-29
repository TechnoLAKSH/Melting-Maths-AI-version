<!DOCTYPE html>
<html>
<head>
  <title>Mealting Maths AI</title>
</head>
<body>

<h2>Mealting Maths AI Helper 🤖</h2>

<div id="chat"></div>

<input id="input" placeholder="Ask a maths question">
<button onclick="sendMsg()">Send</button>

<script>
async function sendMsg() {
  const text = document.getElementById("input").value;

  document.getElementById("chat").innerHTML += "<p><b>You:</b> " + text + "</p>";

  const res = await fetch("/chat", {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({ message: text })
  });

  const data = await res.json();

  document.getElementById("chat").innerHTML += "<p><b>AI:</b> " + data.reply + "</p>";
}
</script>

</body>
</html>


const express = require("express");
const OpenAI = require("openai");

const app = express();
app.use(express.json());
app.use(express.static(".")); 

const openai = new OpenAI({
  apiKey: "YOUR_API_KEY_HERE"
});

app.post("/chat", async (req, res) => {
  const userMsg = req.body.message;

  const completion = await openai.chat.completions.create({
    model: "gpt-4o-mini",
    messages: [
      {
        role: "system",
        content: "You are a helpful maths teacher for grade 1-10 students. Always explain step-by-step."
      },
      { role: "user", content: userMsg }
    ]
  });

  res.json({ reply: completion.choices[0].message.content });
});

app.listen(3000, () => console.log("AI running on http://localhost:3000"));
