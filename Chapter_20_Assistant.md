# Assistant

---

## Description: 
a gui that calls for other assistant programs

---

## Why: 
Useful for controlling what tool to use

---

## Setup Instructions: 
1. python3

---

## To do: 
1. Add a button for each program
2.  create gui for functions
3.  call daily run programs at initiation of main
4.  get users name for more intimate feel 

---

## Code: 
>import logging


>from pathlib import Path


>import tkinter


>from pysondb import db


>from datetime import date


>from datetime import datetime


>import customtkinter


>import filemanager





>from Tools import events, affirmation, clocks, cold_storage, daily_check, password_gen, email_sender, reader, chat


    ### hypnosis





customtkinter.set_appearance_mode("System")  ### Modes: "System" (standard), "Dark", "Light"


customtkinter.set_default_color_theme("blue")  ### Themes: "blue" (standard), "green", "dark-blue"








class Assistant:





    ### if data present then greet else create


    def initialize(self):


        logging.debug("initializing")


        file = "Boss/boss.json"


        my_file = Path(file)


        if my_file.exists():


            b = db.getDb("Boss/boss.json")


            return b.getAll()[0]["name"], b.getAll()[0]["goals"]


        else:


            boss = input("Please enter your name: ")


            goals1 = input("Input first goal: ")


            goals2 = input("Input second goal: ")


            goals3 = input("Input third goal: ")


            goals4 = input("Input fourth goal: ")


            goals5 = input("Input fifth goal: ")


            goals = []


            goals.append(goals1)


            goals.append(goals2)


            goals.append(goals3)


            goals.append(goals4)


            goals.append(goals5)


            b = db.getDb("Boss/boss.json")


            b.add({"name": boss, "goals": goals})


            return boss, goals





    def check_events(self):


        logging.debug("Events Schedule")


        events = db.getDb("Events/events.json")


        event_list = events.getAll()


        build_list = []


        for item in event_list:


            build_list.append(item["name"] + " " + item["time"])


        return build_list





    def check_daily(self):


        events = db.getDb("ToDo/daily.json")


        event_list = events.getAll()


        build_list = []


        for item in event_list:


            build_list.append(f"{item['task']}, {item['quad']}")


        return build_list





    def check_chores(self):


        chores = []


        logging.debug("Events checked")





        for i in db.getDb("Chores/daily.json").getAll():


            chores.append(i["name"])





        for i in db.getDb("Chores/weekly.json").getAll():


            if i["time"] == datetime.today().weekday():


                chores.append(i["name"])





        for i in db.getDb("Chores/monthly.json").getAll():


            week = datetime.now().day / 7


            if i["time"] == week:


                chores.append(i["name"])





        for i in db.getDb("Chores/quarterly.json").getAll():


            quarter = datetime.now().month % 3


            if i["time"] == quarter:


                chores.append(i["name"])





        for i in db.getDb("Chores/yearly.json").getAll():


            quarter = datetime.now().month / 3


            if i["time"] == week:


                chores.append(i["name"])





        return chores





    def send_email(self):


        email_sender.email()





    def ebook_reader(self,main):


        audio = reader.EbookToAudio()


        audio.window(main)





    def main(self,root, person, goals):


        greeting = "Hello " + person





        ### Day's events


        list_events = self.check_events()


        list_daily = self.check_daily()


        list_chores = self.check_chores()





        frm = customtkinter.CTkFrame(master=root, corner_radius=15)


        frm.grid(row=0, column=0, padx=20, pady=20, sticky="nsew")





        customtkinter.CTkLabel(master=frm, justify=tkinter.LEFT, text=greeting).grid(column=1, row=0, padx=20, pady=20, sticky="nsew")


        customtkinter.CTkButton(master=frm, text="Add events", command=lambda: events.add_event(tkinter.Toplevel(root))).grid(column=0, row=2, padx=20, pady=20, sticky="nsew")


        customtkinter.CTkButton(master=frm, text="Update events", command=lambda: events.update_event(tkinter.Toplevel(root))).grid(column=1, row=2, padx=20, pady=20, sticky="nsew")


        customtkinter.CTkButton(master=frm, text="Add To-Do list", command=lambda: daily_check.add_todo(tkinter.Toplevel(root))).grid(column=2, row=2, padx=20, pady=20, sticky="nsew")


        customtkinter.CTkButton(master=frm, text="Update To-Do list", command=lambda: daily_check.update_todo(tkinter.Toplevel(root))).grid(column=3, row=2, padx=20, pady=20, sticky="nsew")


        customtkinter.CTkButton(master=frm, text="Clocks", command=lambda: clocks.MainWindow(tkinter.Toplevel(root))).grid(column=0, row=3, padx=20, pady=20, sticky="nsew")


        customtkinter.CTkButton(master=frm, text="Password generator", command=lambda: password_gen.main(tkinter.Toplevel(root))).grid(column=1, row=3, padx=20, pady=20, sticky="nsew")


        customtkinter.CTkButton(master=frm, text="Reader", command=lambda: self.ebook_reader(tkinter.Toplevel(root))).grid(column=2, row=3, padx=20, pady=20, sticky="nsew")


        customtkinter.CTkButton(master=frm, text="Talk to HR", command=lambda: chat.window(tkinter.Toplevel(root))).grid(column=3, row=3, padx=20, pady=20, sticky="nsew")





        customtkinter.CTkButton(master=frm, text="Quit", fg_color=("gray75", "gray30"), command=root.destroy).grid(column=1, row=5, padx=20, pady=20, sticky="nsew")


        customtkinter.CTkButton(master=frm, text="Refresh", fg_color=("gray75", "gray30"), command=lambda: self.refresh(root, boss, goals, frm)).grid(column=2, row=5, padx=20, pady=20, sticky="nsew")





        customtkinter.CTkLabel(master=frm,


                  text="Upcoming Events",


                  text_font=("Roboto Medium", -16)).grid(column = 0, row = 6)


        list_items = tkinter.StringVar(value=list_events)


        tkinter.Listbox(frm, listvariable=list_items, height=len(list_events)).grid(column=0, row=7, padx=20, pady=20, sticky="nsew")





        customtkinter.CTkLabel(master=frm,


                  text="Today's Chores",


                  text_font=("Roboto Medium", -16)).grid(column = 1, row = 6)


        list_ch = tkinter.StringVar(value=list_chores)


        tkinter.Listbox(frm, listvariable=list_ch, height=len(list_chores)).grid(column=1, row=7, padx=20, pady=20, sticky="nsew")





        customtkinter.CTkLabel(master=frm,


                  text="Daily ToDo",


                  text_font=("Roboto Medium", -16)).grid(column = 2, row = 6)


        list_todo = tkinter.StringVar(value=list_daily)


        tkinter.Listbox(frm, listvariable=list_todo, height=len(list_daily)).grid(column=2, row=7, padx=20, pady=20, sticky="nsew")





        customtkinter.CTkLabel(master=frm,


                  text="Life Goals",


                  text_font=("Roboto Medium", -16)).grid(column = 3, row = 6)


        list_goals = tkinter.StringVar(value=goals)


        tkinter.Listbox(frm, listvariable=list_goals, height=len(goals)).grid(column=3, row=7, padx=20, pady=20, sticky="nsew")





        root.mainloop()





    def change_mode(self):


        if self.switch_2.get() == 1:


            customtkinter.set_appearance_mode("dark")


        else:


            customtkinter.set_appearance_mode("light")





    def on_closing(self, event=0):


        self.destroy()





    def start(self):


        self.mainloop()





    def refresh(self, root, boss, goals, frm):


        frm.destroy()


        frm.__init__()


        self.main(root, boss, goals)





if __name__ == "__main__":


    daily_check.delete_plan()


    filemanager.MoverHandler().activate()


    ### start day


    ### affirmation.affirm()


    root = customtkinter.CTk()


    root.title("Assistant")


    root.geometry("1000x700")


    ### Daily check-in


    daily_check.day_plan(tkinter.Toplevel(root))


    ### start assistant


    assistant = Assistant()


    ### Boss name check


    boss, goals = assistant.initialize()


    ### Run cold storage check


    cold_storage.freezer()


    ### Start main functions


    assistant.main(root, boss, goals)



---

## Translation: 
>Import package called  logging


>from pathlib Import package called  Path


>Import package called  tkinter


>from pysondb Import package called  db


>from datetime Import package called  date


>from datetime Import package called  datetime


>Import package called  customtkinter


>Import package called  filemanager





>from Tools Import package called  events, affirmation, clocks, cold_storage, daily_check, password_gen, email_sender, reader, chat


    ### hypnosis





customtkinter set_appearance_mode function call with parameters: ""System""  ### Modes: "System"  function call with parameters: "standard", "Dark", "Light"


customtkinter set_Define a function called ault_color_theme function call with parameters: ""blue""  ### Themes: "blue"  function call with parameters: "standard", "green", "dark-blue"








class Assistant:





    ### if data present then greet else create


    Define a function called  initialize function call with parameters: "self":


        logging debug function call with parameters: ""initializing""


        file variable equal to  "Boss/boss json"


        my_file variable equal to  Path function call with parameters: "file"


        if my_file exists function call with parameters: "":


            b variable equal to  db getDb function call with parameters: ""Boss/boss json""


            return b getAll function call with parameters: ""[0]["name"], b getAll function call with parameters: ""[0]["goals"]


        else:


            boss variable equal to  input function call with parameters: ""Please enter your name: ""


            goals1 variable equal to  input function call with parameters: ""Input first goal: ""


            goals2 variable equal to  input function call with parameters: ""Input second goal: ""


            goals3 variable equal to  input function call with parameters: ""Input third goal: ""


            goals4 variable equal to  input function call with parameters: ""Input fourth goal: ""


            goals5 variable equal to  input function call with parameters: ""Input fifth goal: ""


            goals variable equal to  []


            goals append function call with parameters: "goals1"


            goals append function call with parameters: "goals2"


            goals append function call with parameters: "goals3"


            goals append function call with parameters: "goals4"


            goals append function call with parameters: "goals5"


            b variable equal to  db getDb function call with parameters: ""Boss/boss json""


            b add function call with parameters: "{"name": boss, "goals": goals}"


            return boss, goals





    Define a function called  check_events function call with parameters: "self":


        logging debug function call with parameters: ""Events Schedule""


        events variable equal to  db getDb function call with parameters: ""Events/events json""


        event_list variable equal to  events getAll function call with parameters: ""


        build_list variable equal to  []


        for item in event_list:


            build_list append function call with parameters: "item["name"] + " " + item["time"]"


        return build_list





    Define a function called  check_daily function call with parameters: "self":


        events variable equal to  db getDb function call with parameters: ""ToDo/daily json""


        event_list variable equal to  events getAll function call with parameters: ""


        build_list variable equal to  []


        for item in event_list:


            build_list append function call with parameters: "f"{item['task']}, {item['quad']}""


        return build_list





    Define a function called  check_chores function call with parameters: "self":


        chores variable equal to  []


        logging debug function call with parameters: ""Events checked""





        for i in db getDb function call with parameters: ""Chores/daily json"" getAll function call with parameters: "":


            chores append function call with parameters: "i["name"]"





        for i in db getDb function call with parameters: ""Chores/weekly json"" getAll function call with parameters: "":


            if i["time"] variable equal to variable equal to  datetime today function call with parameters: "" weekday function call with parameters: "":


                chores append function call with parameters: "i["name"]"





        for i in db getDb function call with parameters: ""Chores/monthly json"" getAll function call with parameters: "":


            week variable equal to  datetime now function call with parameters: "" day / 7


            if i["time"] variable equal to variable equal to  week:


                chores append function call with parameters: "i["name"]"





        for i in db getDb function call with parameters: ""Chores/quarterly json"" getAll function call with parameters: "":


            quarter variable equal to  datetime now function call with parameters: "" month % 3


            if i["time"] variable equal to variable equal to  quarter:


                chores append function call with parameters: "i["name"]"





        for i in db getDb function call with parameters: ""Chores/yearly json"" getAll function call with parameters: "":


            quarter variable equal to  datetime now function call with parameters: "" month / 3


            if i["time"] variable equal to variable equal to  week:


                chores append function call with parameters: "i["name"]"





        return chores





    Define a function called  send_email function call with parameters: "self":


        email_sender email function call with parameters: ""





    Define a function called  ebook_reader function call with parameters: "self,main":


        audio variable equal to  reader EbookToAudio function call with parameters: ""


        audio window function call with parameters: "main"





    Define a function called  main function call with parameters: "self,root, person, goals":


        greeting variable equal to  "Hello " + person





        ### Day's events


        list_events variable equal to  self check_events function call with parameters: ""


        list_daily variable equal to  self check_daily function call with parameters: ""


        list_chores variable equal to  self check_chores function call with parameters: ""





        frm variable equal to  customtkinter CTkFrame function call with parameters: "mastervariable equal to root, corner_radiusvariable equal to 15"


        frm grid function call with parameters: "rowvariable equal to 0, columnvariable equal to 0, padxvariable equal to 20, padyvariable equal to 20, stickyvariable equal to "nsew""





        customtkinter CTkLabel function call with parameters: "mastervariable equal to frm, justifyvariable equal to tkinter LEFT, textvariable equal to greeting" grid function call with parameters: "columnvariable equal to 1, rowvariable equal to 0, padxvariable equal to 20, padyvariable equal to 20, stickyvariable equal to "nsew""


        customtkinter CTkButton function call with parameters: "mastervariable equal to frm, textvariable equal to "Add events", commandvariable equal to lambda: events add_event function call with parameters: "tkinter Toplevel function call with parameters: "root""" grid function call with parameters: "columnvariable equal to 0, rowvariable equal to 2, padxvariable equal to 20, padyvariable equal to 20, stickyvariable equal to "nsew""


        customtkinter CTkButton function call with parameters: "mastervariable equal to frm, textvariable equal to "Update events", commandvariable equal to lambda: events update_event function call with parameters: "tkinter Toplevel function call with parameters: "root""" grid function call with parameters: "columnvariable equal to 1, rowvariable equal to 2, padxvariable equal to 20, padyvariable equal to 20, stickyvariable equal to "nsew""


        customtkinter CTkButton function call with parameters: "mastervariable equal to frm, textvariable equal to "Add To-Do list", commandvariable equal to lambda: daily_check add_todo function call with parameters: "tkinter Toplevel function call with parameters: "root""" grid function call with parameters: "columnvariable equal to 2, rowvariable equal to 2, padxvariable equal to 20, padyvariable equal to 20, stickyvariable equal to "nsew""


        customtkinter CTkButton function call with parameters: "mastervariable equal to frm, textvariable equal to "Update To-Do list", commandvariable equal to lambda: daily_check update_todo function call with parameters: "tkinter Toplevel function call with parameters: "root""" grid function call with parameters: "columnvariable equal to 3, rowvariable equal to 2, padxvariable equal to 20, padyvariable equal to 20, stickyvariable equal to "nsew""


        customtkinter CTkButton function call with parameters: "mastervariable equal to frm, textvariable equal to "Clocks", commandvariable equal to lambda: clocks MainWindow function call with parameters: "tkinter Toplevel function call with parameters: "root""" grid function call with parameters: "columnvariable equal to 0, rowvariable equal to 3, padxvariable equal to 20, padyvariable equal to 20, stickyvariable equal to "nsew""


        customtkinter CTkButton function call with parameters: "mastervariable equal to frm, textvariable equal to "Password generator", commandvariable equal to lambda: password_gen main function call with parameters: "tkinter Toplevel function call with parameters: "root""" grid function call with parameters: "columnvariable equal to 1, rowvariable equal to 3, padxvariable equal to 20, padyvariable equal to 20, stickyvariable equal to "nsew""


        customtkinter CTkButton function call with parameters: "mastervariable equal to frm, textvariable equal to "Reader", commandvariable equal to lambda: self ebook_reader function call with parameters: "tkinter Toplevel function call with parameters: "root""" grid function call with parameters: "columnvariable equal to 2, rowvariable equal to 3, padxvariable equal to 20, padyvariable equal to 20, stickyvariable equal to "nsew""


        customtkinter CTkButton function call with parameters: "mastervariable equal to frm, textvariable equal to "Talk to HR", commandvariable equal to lambda: chat window function call with parameters: "tkinter Toplevel function call with parameters: "root""" grid function call with parameters: "columnvariable equal to 3, rowvariable equal to 3, padxvariable equal to 20, padyvariable equal to 20, stickyvariable equal to "nsew""





        customtkinter CTkButton function call with parameters: "mastervariable equal to frm, textvariable equal to "Quit", fg_colorvariable equal to  function call with parameters: ""gray75", "gray30"", commandvariable equal to root destroy" grid function call with parameters: "columnvariable equal to 1, rowvariable equal to 5, padxvariable equal to 20, padyvariable equal to 20, stickyvariable equal to "nsew""


        customtkinter CTkButton function call with parameters: "mastervariable equal to frm, textvariable equal to "Refresh", fg_colorvariable equal to  function call with parameters: ""gray75", "gray30"", commandvariable equal to lambda: self refresh function call with parameters: "root, boss, goals, frm"" grid function call with parameters: "columnvariable equal to 2, rowvariable equal to 5, padxvariable equal to 20, padyvariable equal to 20, stickyvariable equal to "nsew""





        customtkinter CTkLabel function call with parameters: "mastervariable equal to frm,


                  textvariable equal to "Upcoming Events",


                  text_fontvariable equal to  function call with parameters: ""Roboto Medium", -16"" grid function call with parameters: "column variable equal to  0, row variable equal to  6"


        list_items variable equal to  tkinter StringVar function call with parameters: "valuevariable equal to list_events"


        tkinter Listbox function call with parameters: "frm, listvariablevariable equal to list_items, heightvariable equal to len function call with parameters: "list_events"" grid function call with parameters: "columnvariable equal to 0, rowvariable equal to 7, padxvariable equal to 20, padyvariable equal to 20, stickyvariable equal to "nsew""





        customtkinter CTkLabel function call with parameters: "mastervariable equal to frm,


                  textvariable equal to "Today's Chores",


                  text_fontvariable equal to  function call with parameters: ""Roboto Medium", -16"" grid function call with parameters: "column variable equal to  1, row variable equal to  6"


        list_ch variable equal to  tkinter StringVar function call with parameters: "valuevariable equal to list_chores"


        tkinter Listbox function call with parameters: "frm, listvariablevariable equal to list_ch, heightvariable equal to len function call with parameters: "list_chores"" grid function call with parameters: "columnvariable equal to 1, rowvariable equal to 7, padxvariable equal to 20, padyvariable equal to 20, stickyvariable equal to "nsew""





        customtkinter CTkLabel function call with parameters: "mastervariable equal to frm,


                  textvariable equal to "Daily ToDo",


                  text_fontvariable equal to  function call with parameters: ""Roboto Medium", -16"" grid function call with parameters: "column variable equal to  2, row variable equal to  6"


        list_todo variable equal to  tkinter StringVar function call with parameters: "valuevariable equal to list_daily"


        tkinter Listbox function call with parameters: "frm, listvariablevariable equal to list_todo, heightvariable equal to len function call with parameters: "list_daily"" grid function call with parameters: "columnvariable equal to 2, rowvariable equal to 7, padxvariable equal to 20, padyvariable equal to 20, stickyvariable equal to "nsew""





        customtkinter CTkLabel function call with parameters: "mastervariable equal to frm,


                  textvariable equal to "Life Goals",


                  text_fontvariable equal to  function call with parameters: ""Roboto Medium", -16"" grid function call with parameters: "column variable equal to  3, row variable equal to  6"


        list_goals variable equal to  tkinter StringVar function call with parameters: "valuevariable equal to goals"


        tkinter Listbox function call with parameters: "frm, listvariablevariable equal to list_goals, heightvariable equal to len function call with parameters: "goals"" grid function call with parameters: "columnvariable equal to 3, rowvariable equal to 7, padxvariable equal to 20, padyvariable equal to 20, stickyvariable equal to "nsew""





        root mainloop function call with parameters: ""





    Define a function called  change_mode function call with parameters: "self":


        if self switch_2 get function call with parameters: "" variable equal to variable equal to  1:


            customtkinter set_appearance_mode function call with parameters: ""dark""


        else:


            customtkinter set_appearance_mode function call with parameters: ""light""





    Define a function called  on_closing function call with parameters: "self, eventvariable equal to 0":


        self destroy function call with parameters: ""





    Define a function called  start function call with parameters: "self":


        self mainloop function call with parameters: ""





    Define a function called  refresh function call with parameters: "self, root, boss, goals, frm":


        frm destroy function call with parameters: ""


        frm __init__ function call with parameters: ""


        self main function call with parameters: "root, boss, goals"





if __name__ variable equal to variable equal to  "__main__":


    daily_check delete_plan function call with parameters: ""


    filemanager MoverHandler function call with parameters: "" activate function call with parameters: ""


    ### start day


    ### affirmation affirm function call with parameters: ""


    root variable equal to  customtkinter Cinitiate tkinter 


    root title function call with parameters: ""Assistant""


    root geometry function call with parameters: ""1000x700""


    ### Daily check-in


    daily_check day_plan function call with parameters: "tkinter Toplevel function call with parameters: "root""


    ### start assistant


    assistant variable equal to  Assistant function call with parameters: ""


    ### Boss name check


    boss, goals variable equal to  assistant initialize function call with parameters: ""


    ### Run cold storage check


    cold_storage freezer function call with parameters: ""


    ### Start main functions


    assistant main function call with parameters: "root, boss, goals"


[Github link] https://github.com/DownRamp/Assistant/blob/main/assistant.py
## Next Steps: 
1. Add your own programs
2.  turn into web app
3.  add more verbal comments 

---

