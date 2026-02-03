<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>ES AI Assistant</title>

<style>
body {
    margin: 0;
    background: radial-gradient(circle, #0f2027, #000);
    color: #00ffff;
    font-family: Arial, sans-serif;
    text-align: center;
}

#jarvis {
    width: 250px;
    height: 250px;
    margin: 40px auto;
    border-radius: 50%;
    border: 3px solid #00ffff;
    animation: pulse 2s infinite;
}

@keyframes pulse {
    0% { box-shadow: 0 0 10px #00ffff; }
    50% { box-shadow: 0 0 40px #00ffff; }
    100% { box-shadow: 0 0 10px #00ffff; }
}

#status {
    font-size: 18px;
    margin-top: 10px;
}

button {
    margin: 10px;
    padding: 12px 25px;
    font-size: 16px;
    border-radius: 25px;
    border: none;
    background: #00ffff;
    cursor: pointer;
}

input {
    padding: 8px;
    width: 60%;
    border-radius: 10px;
    border: none;
}
</style>
</head>

<body>

<h1>🤖 ES – Personal AI</h1>
<div id="jarvis"></div>
<div id="status">Sleeping… Say "ES"</div>

<button onclick="startAI()">🎙 Start Listening</button><br><br>

<input id="reminderText" placeholder="Reminder text">
<input id="reminderTime" type="time">
<button onclick="setReminder()">⏰ Set Reminder</button>

<script>
let active = false;

const SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;
const recognition = new SpeechRecognition();
recognition.continuous = true;
recognition.lang = "en-US";

const synth = window.speechSynthesis;

function speak(text) {
    const utter = new SpeechSynthesisUtterance(text);
    utter.rate = 0.95;
    synth.speak(utter);
}

recognition.onresult = function(event) {
    const transcript = event.results[event.results.length - 1][0].transcript.toLowerCase();
    console.log(transcript);

    if (!active && transcript.includes("es")) {
        active = true;
        document.getElementById("status").innerText = "ES Online 🟢";
        speak("Yes, I am online.");
        return;
    }

    if (active) {
        if (transcript.includes("bye")) {
            active = false;
            document.getElementById("status").innerText = "ES Sleeping 😴";
            speak("Going to sleep. Bye.");
        }
        else if (transcript.includes("time")) {
            speak("The time is " + new Date().toLocaleTimeString());
        }
        else if (transcript.includes("hello")) {
            speak("Hello. How can I assist you?");
        }
    }
};

function startAI() {
    recognition.start();
    speak("Listening activated.");
}

// Reminder system
function setReminder() {
    const text = document.getElementById("reminderText").value;
    const time = document.getElementById("reminderTime").value;

    if (!text || !time) return alert("Enter reminder and time");

    localStorage.setItem("reminder", JSON.stringify({ text, time }));
    speak("Reminder set successfully.");
}

setInterval(() => {
    const r = JSON.parse(localStorage.getItem("reminder"));
    if (!r) return;

    const now = new Date().toLocaleTimeString().slice(0,5);
    if (now === r.time) {
        speak("Reminder. " + r.text);
        localStorage.removeItem("reminder");
    }
}, 30000);
</script>

</body>
</html>
