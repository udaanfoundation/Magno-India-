<!DOCTYPE html><html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Magno India | Booking Site</title>
  <link rel="stylesheet" href="style.css" />
  <style>
    #chat-box {
      position: fixed;
      bottom: 80px;
      right: 20px;
      width: 300px;
      background: white;
      border: 1px solid #ccc;
      border-radius: 10px;
      box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
      display: none;
      flex-direction: column;
      overflow: hidden;
      z-index: 1000;
    }
    #chat-header {
      background: #001f3f;
      color: white;
      padding: 10px;
      font-weight: bold;
      text-align: center;
    }
    #chat-messages {
      padding: 10px;
      height: 200px;
      overflow-y: auto;
      font-size: 14px;
    }
    #chat-input {
      display: flex;
      border-top: 1px solid #ccc;
    }
    #chat-input input {
      flex: 1;
      padding: 10px;
      border: none;
      font-size: 14px;
    }
    #chat-input button {
      padding: 10px;
      background: #0074D9;
      color: white;
      border: none;
      cursor: pointer;
    }
    #chat-toggle {
      position: fixed;
      bottom: 20px;
      right: 20px;
      background: #0074D9;
      color: white;
      border: none;
      border-radius: 50%;
      width: 50px;
      height: 50px;
      font-size: 24px;
      cursor: pointer;
      z-index: 999;
    }
  </style>
</head>
<body>
  <!-- ... (existing content from booking site) ... -->  <!-- Chat Support Widget --><button id="chat-toggle">💬</button>

  <div id="chat-box">
    <div id="chat-header">AI Support</div>
    <div id="chat-messages"></div>
    <div id="chat-input">
      <input type="text" id="chat-message-input" placeholder="Ask a question..." />
      <button onclick="sendMessage()">Send</button>
    </div>
  </div>  <script>
    let userName = "";
    window.onload = function () {
      userName = prompt("Welcome to Magno India! What's your name?");
      if (!userName) userName = "User";
    };

    const toggleButton = document.getElementById("chat-toggle");
    const chatBox = document.getElementById("chat-box");
    const messageInput = document.getElementById("chat-message-input");
    const messageDisplay = document.getElementById("chat-messages");

    toggleButton.onclick = () => {
      chatBox.style.display = chatBox.style.display === "flex" ? "none" : "flex";
    };

    async function sendMessage() {
      const userText = messageInput.value.trim();
      if (!userText) return;
      appendMessage(userName, userText);
      messageInput.value = "";

      try {
        const res = await fetch("https://api.openai.com/v1/chat/completions", {
          method: "POST",
          headers: {
            "Content-Type": "application/json",
            "Authorization": "Bearer YOUR_OPENAI_API_KEY"
          },
          body: JSON.stringify({
            model: "gpt-3.5-turbo",
            messages: [
              { role: "system", content: `You are a helpful assistant for Magno India, talking to a user named ${userName}.` },
              { role: "user", content: userText }
            ]
          })
        });
        const data = await res.json();
        const reply = data.choices[0].message.content.trim();
        appendMessage("Bot", reply);
      } catch (error) {
        appendMessage("Bot", "Sorry, something went wrong.");
      }
    }

    function appendMessage(sender, text) {
      const msg = document.createElement("div");
      msg.innerHTML = `<strong>${sender}:</strong> ${text}`;
      messageDisplay.appendChild(msg);
      messageDisplay.scrollTop = messageDisplay.scrollHeight;
    }
  </script></body>
</html>
