import speech_recognition as sr # type: ignore
import webbrowser
import pyttsx3   # type: ignore
import musiclibrary  # type: ignore

recognizer = sr.Recognizer()
engine = pyttsx3.init()


def speak(text):
    engine.say(text)
    engine.runAndWait()

def processCommand(c):
    c_l = c.lower()
    if "open google" in c_l:
        speak("opening google")
        webbrowser.open("https://www.google.com")
    elif "open youtube" in c_l:
        speak("opening youtube")
        webbrowser.open("https://www.youtube.com")
    elif "open facebook" in c_l:
        speak("opening facebook")
        webbrowser.open("https://www.facebook.com")
    elif c.lower().startswith("play"):
        song=  c.lower().split("  ")[1]
        link= musiclibrary.music[song]
        webbrowser.open(link)

    

if __name__ == "__main__":
    speak("initializing jarvis....")
    while True:
        try:
            print("recognizing... ")
            with sr.Microphone() as source:
                print("listening... ")
                audio = recognizer.listen(source, timeout=2, phrase_time_limit=5)
            word = recognizer.recognize_google(audio)
            print("heard:", word)
            if word.lower() == "jarvis":
                speak("yes?")
                with sr.Microphone() as source:
                    print("listening for command... ")
                    audio = recognizer.listen(source, timeout=5, phrase_time_limit=7)
                try:
                    command = recognizer.recognize_google(audio)
                    print("command:", command)
                    processCommand(command)
                except Exception as e:
                    print("Could not understand command:", e)
        except Exception as e:
            print("error: {0}".format(e))
