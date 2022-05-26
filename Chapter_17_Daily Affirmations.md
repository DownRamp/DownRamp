# Daily Affirmations

---

## Description: 
Questions that you can ask yourself everyday to get you ready for the day.

---

## Why: 
Motivation for the day to come

---

## Setup Instructions: 
1. python3

---

## To do: 
1. Create a text to speech function
2.  read through list and feed to read function

---

## Code: 
>import pyttsx3


>import time





### Ask a series of questions


questions = ["Why are you working so hard?",


             "What is your dream?",


             "What are you grateful for?",


             "How will you work towards your goals today?",


             "How are you proud of yourself?",


             "Will you be proud of yourself?"]








def affirm():


    engine = pyttsx3.init()


    voices = engine.getProperty('voices')


    ### I like this one more (7)


    engine.setProperty('voice', voices[17].id)


    engine.say("Good Morning!")


    engine.runAndWait()


    for i in questions:


        engine.say(i)


        engine.runAndWait()


        time.sleep(5)


    engine.say("Make it a great day")


    engine.runAndWait()


    engine.stop()








if __name__ == '__main__':


    affirm()



---

## Translation: 
>Import package called  pyttsx3


>Import package called  time





### Ask a series of questions


questions variable equal to  ["Why are you working so hard?",


             "What is your dream?",


             "What are you grateful for?",


             "How will you work towards your goals today?",


             "How are you proud of yourself?",


             "Will you be proud of yourself?"]








Define a function called  affirm function call with parameters: "":


    engine variable equal to  pyttsx3 initiate 


    voices variable equal to  engine getProperty function call with parameters: "'voices'"


    ### I like this one more  function call with parameters: "7"


    engine setProperty function call with parameters: "'voice', voices[17] id"


    engine say function call with parameters: ""Good Morningnot ""


    engine runAndWait function call with parameters: ""


    for i in questions:


        engine say function call with parameters: "i"


        engine runAndWait function call with parameters: ""


        time sleep function call with parameters: "5"


    engine say function call with parameters: ""Make it a great day""


    engine runAndWait function call with parameters: ""


    engine stop function call with parameters: ""








if __name__ variable equal to variable equal to  '__main__':


    affirm function call with parameters: ""


[Github link] https://github.com/DownRamp/Assistant/blob/main/Tools/affirmation.py
## Next Steps: 
1. Put in your own questions
2.  have it say things to get you motivated
3.  set to an alarm

---

