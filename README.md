# Simplified-Virtual-Assistant


import speech_recognition as sr
import pyttsx3
import datetime
import pywhatkit
import wikipedia

listener = sr.Recognizer()
assistant = pyttsx3.init()

voices = assistant.getProperty('voices')
assistant.setProperty('voice', voices[1].id)


def talk(text):
    assistant.say(text)
    assistant.runAndWait()


def take_command():
    try:
        with sr.Microphone() as source:
            print('Sir,I am hearing you,please tell me how can I help you?')
            voice = listener.listen(source)
            command = listener.recognize_google(voice)
            command = command.lower()
            if 'assistant' in command:
                command = command.replace('assistant', '')
    except:
        pass
    return command


def run_assistant():
    command = take_command()

    if 'time' in command:
        time = datetime.datetime.now().strftime('%I:%M %p')
        print('Sir, Current time is ' + time)
        talk('Sir, Current time is ' + time)
    elif 'play' in command:
        song = command.replace('play', '')
        talk('playing ' + song)
        pywhatkit.playonyt(song)
    elif 'tell me about' in command:
        wiki = command.replace('tell me about', '')
        info = wikipedia.summary(wiki, 2)
        print(info)
        talk(info)
    else:
        talk('Sorry sir, I did not get your question, I can search it from google')
        pywhatkit.search(command)


run_assistant()
