<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>AI ES Assistant</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: linear-gradient(120deg, #f0f4f8, #d9e2ec);
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
    }
    #chatBox {
      width: 400px;
      height: 600px;
      border-radius: 15px;
      box-shadow: 0 8px 20px rgba(0,0,0,0.2);
      background-color: #ffffff;
      display: flex;
      flex-direction: column;
      overflow: hidden;
    }
    #messages {
      flex: 1;
      padding: 10px;
      overflow-y: auto;
    }
    .message {
      margin: 5px 0;
      padding: 8px 12px;
      border-radius: 10px;
      max-width: 80%;
      word-wrap: break-word;
    }
    .user { background-color: #d0ebff; align-self: flex-end; }
    .ai { background-color: #e0f7fa; align-self: flex-start; }
    #inputBox {
      display: flex;
      border-top: 1px solid #ccc;
    }
    #inputBox input {
      flex: 1;
      padding: 10px;
      border: none;
      outline: none;
    }
    #inputBox button {
      padding: 10px;
      border: none;
      background-color: #0288d1;
      color: white;
      cursor: pointer;
    }
  </style>
</head>
<body>

<div id="chatBox">
  <div id="messages"></div>
  <div id="inputBox">
    <input type="text" id="userInput" placeholder="Say something to ES...">
    <button onclick="sendMessage()">Send</button>
  </div>
</div>

<script>
  const messages = document.getElementById('messages');

  function appendMessage(text, sender) {
    const div = document.createElement('div');
    div.textContent = text;
    div.className = 'message ' + sender;
    messages.appendChild(div);
    messages.scrollTop = messages.scrollHeight;
    if(sender === 'ai'){
      speak(text);
    }
  }

  function speak(text){
    const msg = new SpeechSynthesisUtterance();
    msg.text = text;
    msg.voice = speechSynthesis.getVoices()[0]; // Jarvis-like voice
    msg.rate = 1;
    msg.pitch = 1;
    speechSynthesis.speak(msg);
  }

  function generateAIResponse(input){
    // Simple logic for demo, can expand to use AI API later
    let response = "";
    const lower = input.toLowerCase();
    if(lower.includes("hello") || lower.includes("hi")){
      response = "ES: Hello! I am here to help you. Bye";
    } else if(lower.includes("stress") || lower.includes("tired")){
      response = "ES: Take a deep breath, focus on 3 slow breaths, everything will be fine. Bye";
    } else if(lower.includes("motivation")){
      response = "ES: Remember, small steps every day lead to big achievements. Bye";
    } else if(lower.includes("bye")){
      response = "ES: Bye! Talk to you later!";
    } else {
      response = "ES: I am listening... Tell me more. Bye";
    }
    return response;
  }

  function sendMessage(){
    const input = document.getElementById('userInput').value;
    if(!input) return;
    appendMessage(input, 'user');
    document.getElementById('userInput').value = '';
    const aiResponse = generateAIResponse(input);
    setTimeout(() => appendMessage(aiResponse, 'ai'), 500);
  }

  // Optional: Press Enter to send message
  document.getElementById('userInput').addEventListener('keypress', function(e){
    if(e.key === 'Enter') sendMessage();
  });
</script>

</body>
</html>
