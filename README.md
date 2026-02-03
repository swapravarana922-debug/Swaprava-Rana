<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Wake-word AI ES</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f0f4f8;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
      margin: 0;
    }
    .chat-container {
      width: 400px;
      height: 600px;
      background: #fff;
      border-radius: 12px;
      box-shadow: 0 8px 20px rgba(0,0,0,0.15);
      display: flex;
      flex-direction: column;
      overflow: hidden;
    }
    .chat-header {
      background: #0288d1;
      color: white;
      padding: 15px;
      font-weight: bold;
      text-align: center;
    }
    .messages {
      flex: 1;
      padding: 10px;
      overflow-y: auto;
      display: flex;
      flex-direction: column;
      gap: 8px;
    }
    .message {
      max-width: 80%;
      padding: 10px;
      border-radius: 10px;
      word-wrap: break-word;
    }
    .user-message { background: #dcf8c6; align-self: flex-end; }
    .ai-message { background: #f1f0f0; align-self: flex-start; }
    .info { text-align: center; padding: 5px; font-size: 12px; color: #555; }
  </style>
</head>
<body>
  <div class="chat-container">
    <div class="chat-header">Wake-word AI ES</div>
    <div class="messages" id="messages"></div>
    <div class="info">Say "Hey ES" to wake me up.</div>
  </div>

  <script>
    const messagesContainer = document.getElementById('messages');
    let recognition;
    let listening = false;

    // Setup speech recognition
    if('webkitSpeechRecognition' in window){
      recognition = new webkitSpeechRecognition();
    } else if('SpeechRecognition' in window){
      recognition = new SpeechRecognition();
    } else {
      alert("Your browser does not support voice recognition");
    }

    recognition.continuous = true;
    recognition.interimResults = false;
    recognition.lang = 'en-US';

    recognition.onresult = function(event){
      const transcript = event.results[event.results.length-1][0].transcript.trim();
      console.log("Heard: ", transcript);

      if(!listening && transcript.toLowerCase().includes("hey es")){
        listening = true;
        appendMessage("Hey! ES is awake and ready to help you.", 'ai');
      } 
      else if(listening){
        appendMessage(transcript, 'user');
        generateAIResponse(transcript);
      }
    }

    recognition.onerror = function(event){
      console.log("Error: " + event.error);
    }

    recognition.onend = function(){
      recognition.start(); // restart listening continuously
    }

    recognition.start();

    function appendMessage(text, sender){
      const msgDiv = document.createElement('div');
      msgDiv.textContent = text;
      msgDiv.className = 'message ' + (sender==='user' ? 'user-message' : 'ai-message');
      messagesContainer.appendChild(msgDiv);
      messagesContainer.scrollTop = messagesContainer.scrollHeight;
      if(sender==='ai') speak(text);
    }

    function speak(text){
      const utterance = new SpeechSynthesisUtterance(text);
      utterance.rate = 1;
      utterance.pitch = 1;
      speechSynthesis.speak(utterance);
    }

    function generateAIResponse(input){
      const lower = input.toLowerCase();
      let response = "";
      if(lower.includes("stress") || lower.includes("tired")){
        response = "ES: Take a deep breath. I am here to calm you.";
      } else if(lower.includes("motivation")){
        response = "ES: Remember, every small step counts!";
      } else if(lower.includes("bye")){
        response = "ES: Goodbye! Talk later.";
        listening = false; // stop active session
      } else {
        response = "ES: I am listening, tell me more.";
      }
      setTimeout(() => appendMessage(response, 'ai'), 500);
    }
  </script>
</body>
</html>
