# simple_jarvis
import speech_recognition as sr
import pyttsx3
import wikipedia
import datetime
import webbrowser
import os
import pyjokes

# Initialize the recognizer and TTS engine
listener = sr.Recognizer()
engine = pyttsx3.init()
engine.setProperty('rate', 150)  # Speed of speech

def speak(text):
    engine.say(text)
    engine.runAndWait()

def listen():
    try:
        with sr.Microphone() as source:
            print("Listening...")
            voice = listener.listen(source)
            command = listener.recognize_google(voice)
            command = command.lower()
            print(f"You said: {command}")
            return command
    except sr.UnknownValueError:
        speak("Sorry, I did not understand that.")
        return ""
    except sr.RequestError:
        speak("Could not request results. Please check your internet.")
        return ""

def run_jarvis():
    speak("Hello! I am your assistant, Jarvis. How can I help you?")
    while True:
        command = listen()

        # Stop the assistant
        if "stop" in command:
            speak("Goodbye, boss!")
            break
        # Respond to "hi" or "hello"
        elif "hi" in command or "hello" in command:
            speak("Hi boss!")
        # Tell time
        elif "time" in command:
            time = datetime.datetime.now().strftime('%I:%M %p')
            speak(f"The time is {time}")
        # Wikipedia search
        elif "wikipedia" in command:
            topic = command.replace("wikipedia", "").strip()
            speak(f"Searching Wikipedia for {topic}")
            try:
                info = wikipedia.summary(topic, sentences=2)
                print(info)
                speak(info)
            except:
                speak("Sorry, I couldn't find that.")
        # Open YouTube
        elif "open youtube" in command:
            speak("Opening YouTube")
            webbrowser.open("https://www.youtube.com")
        # Play music
        elif "play music" in command:
            music_dir = "/Users/yourusername/Music"  # Replace with your actual path
            songs = os.listdir(music_dir)
            if songs:
                os.system(f"open '{os.path.join(music_dir, songs[0])}'")
        # Tell a joke
        elif "joke" in command:
            speak(pyjokes.get_joke())
        # Open weather
        elif "weather" in command:
            speak("Opening the weather forecast.")
            webbrowser.open("https://www.weather.com/")
        # Handle unrecognized commands
        elif command:
            speak("Sorry, I don't understand that yet.")
        elif "thank you" in command:
            speak("That's my pleasure, boss")

if _name_ == "_main_":
    run_jarvis()
