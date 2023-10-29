# jarvis-project
[JARVIS report..docx](https://github.com/kshitiz2811/jarvis-project/files/13197890/JARVIS.report.docx)
import pyttsx3
import speech_recognition
import datetime
import wikipedia
import webbrowser
import os
import smtplib

engine = pyttsx3.init('sapi5')
voices = engine.getProperty('voices')
# print(voices[1].id)
engine.setproperty('voices', voices[0].id)


def speak(audio):
    engine.say(audio)
    engine.runAndWait()


def wishMe():
    hour = int(datetime.datetime.now().hour)
    if hour>=0 and hour<12:
        speak("Good Morning!")

    elif hour>=12 and hour<18:
        speak("Good Afternoon!")

    else:
        speak("Good Evening")

    speak("I am Jarvis Sir. Please tell me how may I help you")

def takeCommand():
    #It takes microphone input from the user and returns string output

    r = speech_recognition.Recognizer()
    with r.Microphone() as source:
        print("Listening...")
        r.pause_threshold = 1
        audio = r.listen(source)

    try:
        print("Recogning...")
        query = r.recognize_google(audio, language='en-in')
        print(f"User said: {query}\n")

    except Exception as e:
        #print(e)
        print("Say that again please...")
        return "None"
    return query

def sendEmail(to, content):
    server = smtplib.SMTP('smtp.gmail.com', 587)
    server.ehlo()
    server.starttls()
    server.login('youremail@gmail.com', 'your-password')
    server.sendmail('youremail@gmail.com', to, content)
    server.close()

if __name__ == "_main_":
    wishMe()
    #while True:
    if 1:  
        query = takeCommand().lower()
        
        # logic for executing tasks based on query 
        if 'wikpedia' in query:
            speak('Searching Wikipedia...')
            query = query.replace("wikipedia", "")
            result = wikipedia.summary(query, sentences=2)
            speak("According to Wikipedia")
            print(result)
            speak(result)

        elif 'open youtube' in query:
            webbrowser.open("youtube.com")

        elif 'open google' in query:
            webbrowser.open("google.com")

        elif 'open stackoverflow' in query:
            webbrowser.open("openstackoverflow.com")

        
        elif 'play music' in query:
            music_dir = 'D:\\Non Critical\\songs\\Favorite Songs2'
            songs = os.listdir(music_dir)
            print(songs)
            os.startfile(os.path.join(music_dir, songs[0]))

        elif 'the time' in query:
            strTime = datetime.datetime.now().strftime("%H:%M:%S")
            speak(f"Sir, the time is {strTime}")

        elif'open code' in query:
            codePath = "//Users//apple//Desktop//vs code"
            os.startfile(codePath)

        elif 'email to kshitiz' in query:
            try:
                speak("What should I say")
                content = takeCommand()
                to = "kshitizyourEmail@gmail.com"
                sendEmail(to, content)
                speak("email has been sent")
            except Exception as e:
                print(e)
                speak("sorry my friend kshitiz bhai. I am not able to send this email")
