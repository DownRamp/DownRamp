# Clocks

---

## Description: 
Make different clocks (pomodorro and time) to organize your time better.

---

## Why: 
Good practice for making GUIs as well as  a useful tool

---

## Setup Instructions: 
1. python3

---

## To do: 
1. Create function that counts down from set number of seconds
2.  create a window to select which amount of seconds to send
3.  show visual of time passing
4.  add beeper when time ends

---

## Code: 
>import time


>import datetime


>import tkinter as tk


>from tkinter import *


>import beepy as beep





frequency = 2500


duration = 1000





pomodoro_state = ""


pomodoro_time = ""


day_time = ""





curr = 0


t = 0








class Pomodoro:


    def __init__(self, master):


        global pomodoro_time, pomodoro_state


        pomodoro_time = tk.StringVar()


        pomodoro_state


        self.master = master


        self.frame = tk.Frame(master, padx=20, pady=50)


        self.lbl = Label(master, text="Day time")


        self.lbl.pack()


        self.message = tk.Label(self.frame, textvariable=pomodoro_time)


        self.message.pack()


        self.message_state = tk.Label(self.frame, textvariable=pomodoro_state)


        self.message_state.pack()


        self.frame.pack(expand=True)


        self.startTime(1500, master)


        self.startTime(300, master)





    ### get current time


    def startTime(self, t, root):


        global pomodoro_time


        for i in range(t):


            mins = (t - i) // 60


            secs = (t - i) % 60


            timer = 'Pomodoro time {:02d}:{:02d}'.format(mins, secs)


            pomodoro_time.set(timer)


            root.update()


            time.sleep(1)


        pomodoro_time.set("END")


        beep.beep(1)








class Day:


    def __init__(self, master):


        global day_time


        day_time = tk.StringVar()


        self.master = master


        self.frame = tk.Frame(master, padx=20, pady=50)


        self.lbl = Label(master, text="Day time")


        self.lbl.pack()


        self.message = tk.Label(self.frame, textvariable=day_time)


        self.message.pack()


        self.frame.pack(expand=True)


        self.current_time(master)





    def set_current(self):


        global curr


        now = datetime.datetime.now()


        midnight = now.replace(hour=0, minute=0, second=0, microsecond=0)


        seconds = (now - midnight).seconds


        curr = 86400 - seconds





    def current_time(self, root):


        self.set_current()


        global curr, day_time


        for i in range(curr):


            curr_hours = (curr-i)//3600


            curr_mins = (curr-i)//60 - (curr_hours*60)


            curr_secs = (curr-i)%60


            curr_timer = 'Time left in day {:02d}:{:02d}:{:02d}'.format(curr_hours, curr_mins, curr_secs)


            day_time.set(curr_timer)


            root.update()


            time.sleep(1)


        day_time.set("END")








class MainWindow():


    def __init__(self, master):


        self.master = master


        self.frame = tk.Frame(master, padx=20, pady=50)


        self.lbl = Label(master, text="Time selection")


        self.lbl.pack()


        self.btn1 = Button(master, text="Start Pomodoro", command=self.command1)


        self.btn1.pack()


        self.btn2 = Button(master, text="Start Day", command=self.command2)


        self.btn2.pack()


        self.frame.pack(expand=True)





    def command1(self):


        self.newWindow = tk.Toplevel(self.master)


        Pomodoro(self.newWindow)





    def command2(self):


        self.newWindow = tk.Toplevel(self.master)


        Day(self.newWindow)








def main():


    root = Tk()


    root.title("Times")


    root.geometry("350x100")


    win = MainWindow(root)


    root.mainloop()





if __name__ == '__main__':


    root = Tk()


    root.title("Times")


    root.geometry("350x100")


    win = MainWindow(root)


    root.mainloop()



---

## Translation: 
>Import package called  time


>Import package called  datetime


>Import package called  tkinter as tk


>from tkinter Import package called  * (means grab everything) 


>Import package called  beepy as beep





frequency variable equal to  2500


duration variable equal to  1000





pomodoro_state variable equal to  ""


pomodoro_time variable equal to  ""


day_time variable equal to  ""





curr variable equal to  0


t variable equal to  0








class Pomodoro:


    Define a function called  __init__ function call with parameters: "self, master":


        fetch global variable  pomodoro_time, pomodoro_state


        pomodoro_time variable equal to  tk StringVar function call with parameters: ""


        pomodoro_state


        self master variable equal to  master


        self frame variable equal to  tk Frame function call with parameters: "master, padxvariable equal to 20, padyvariable equal to 50"


        self lbl variable equal to  Label function call with parameters: "master, textvariable equal to "Day time""


        self lbl pack function call with parameters: ""


        self message variable equal to  tk Label function call with parameters: "self frame, textvariablevariable equal to pomodoro_time"


        self message pack function call with parameters: ""


        self message_state variable equal to  tk Label function call with parameters: "self frame, textvariablevariable equal to pomodoro_state"


        self message_state pack function call with parameters: ""


        self frame pack function call with parameters: "expandvariable equal to True"


        self startTime function call with parameters: "1500, master"


        self startTime function call with parameters: "300, master"





    ### get current time


    Define a function called  startTime function call with parameters: "self, t, root":


        fetch global variable  pomodoro_time


        for i in range function call with parameters: "t":


            mins variable equal to   function call with parameters: "t - i" // 60


            secs variable equal to   function call with parameters: "t - i" % 60


            timer variable equal to  'Pomodoro time {:02d}:{:02d}' format function call with parameters: "mins, secs"


            pomodoro_time set function call with parameters: "timer"


            root update function call with parameters: ""


            time sleep function call with parameters: "1"


        pomodoro_time set function call with parameters: ""END""


        beep beep function call with parameters: "1"








class Day:


    Define a function called  __init__ function call with parameters: "self, master":


        fetch global variable  day_time


        day_time variable equal to  tk StringVar function call with parameters: ""


        self master variable equal to  master


        self frame variable equal to  tk Frame function call with parameters: "master, padxvariable equal to 20, padyvariable equal to 50"


        self lbl variable equal to  Label function call with parameters: "master, textvariable equal to "Day time""


        self lbl pack function call with parameters: ""


        self message variable equal to  tk Label function call with parameters: "self frame, textvariablevariable equal to day_time"


        self message pack function call with parameters: ""


        self frame pack function call with parameters: "expandvariable equal to True"


        self current_time function call with parameters: "master"





    Define a function called  set_current function call with parameters: "self":


        fetch global variable  curr


        now variable equal to  datetime datetime now function call with parameters: ""


        midnight variable equal to  now replace function call with parameters: "hourvariable equal to 0, minutevariable equal to 0, secondvariable equal to 0, microsecondvariable equal to 0"


        seconds variable equal to   function call with parameters: "now - midnight" seconds


        curr variable equal to  86400 - seconds





    Define a function called  current_time function call with parameters: "self, root":


        self set_current function call with parameters: ""


        fetch global variable  curr, day_time


        for i in range function call with parameters: "curr":


            curr_hours variable equal to   function call with parameters: "curr-i"//3600


            curr_mins variable equal to   function call with parameters: "curr-i"//60 -  function call with parameters: "curr_hours* (means grab everything) 60"


            curr_secs variable equal to   function call with parameters: "curr-i"%60


            curr_timer variable equal to  'Time left in day {:02d}:{:02d}:{:02d}' format function call with parameters: "curr_hours, curr_mins, curr_secs"


            day_time set function call with parameters: "curr_timer"


            root update function call with parameters: ""


            time sleep function call with parameters: "1"


        day_time set function call with parameters: ""END""








class MainWindow function call with parameters: "":


    Define a function called  __init__ function call with parameters: "self, master":


        self master variable equal to  master


        self frame variable equal to  tk Frame function call with parameters: "master, padxvariable equal to 20, padyvariable equal to 50"


        self lbl variable equal to  Label function call with parameters: "master, textvariable equal to "Time selection""


        self lbl pack function call with parameters: ""


        self btn1 variable equal to  Button function call with parameters: "master, textvariable equal to "Start Pomodoro", commandvariable equal to self command1"


        self btn1 pack function call with parameters: ""


        self btn2 variable equal to  Button function call with parameters: "master, textvariable equal to "Start Day", commandvariable equal to self command2"


        self btn2 pack function call with parameters: ""


        self frame pack function call with parameters: "expandvariable equal to True"





    Define a function called  command1 function call with parameters: "self":


        self newWindow variable equal to  tk Toplevel function call with parameters: "self master"


        Pomodoro function call with parameters: "self newWindow"





    Define a function called  command2 function call with parameters: "self":


        self newWindow variable equal to  tk Toplevel function call with parameters: "self master"


        Day function call with parameters: "self newWindow"








Define a function called  main function call with parameters: "":


    root variable equal to  initiate tkinter 


    root title function call with parameters: ""Times""


    root geometry function call with parameters: ""350x100""


    win variable equal to  MainWindow function call with parameters: "root"


    root mainloop function call with parameters: ""





if __name__ variable equal to variable equal to  '__main__':


    root variable equal to  initiate tkinter 


    root title function call with parameters: ""Times""


    root geometry function call with parameters: ""350x100""


    win variable equal to  MainWindow function call with parameters: "root"


    root mainloop function call with parameters: ""


[Github link] https://github.com/DownRamp/Assistant/blob/main/Tools/clocks.py
## Next Steps: 
1. Add your own clocks you could use (different countaries)
2.  improve the visual
3.  have the clocks run at the same time

---

