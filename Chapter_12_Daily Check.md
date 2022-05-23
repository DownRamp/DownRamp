# Daily Check


---

## Description: 
Code for running and storing what you will be doing that day. Creates tinter windows that accepts info. Also contains todo window which can give possible options for another day.


---

## Why: 
Useful for time management 


---

## Setup Instructions: 
1. Python 3


---

## To do: 
1. Create a window with a form
2.  Save information
3.  Delete day plan
4.  Create a window for todos
5.  Save todos
6.  update todos

---

## Code: 
>import tkinter.messagebox


>from tkinter import *


>from pysondb import db


>import datetime








## add to to-do


def add_todo(root):


    frame = Frame(root)


    ## show possible day tasks


    label = Label(frame, text="Enter todo", font=('Aerial 12'))


    label.pack()


    label1 = Label(frame, text="Task", font=('Aerial 12'))


    label1.pack()


    ent1 = Entry(frame)


    ent1.pack()


    label2 = Label(frame, text="Quadrant", font=('Aerial 12'))


    label2.pack()


    ent2 = Entry(frame)


    ent2.pack()


    enter = Button(frame, text='Enter', width=25, command=lambda:save_task(ent1.get(), ent2.get()))


    enter.pack()


    frame.pack()








## fetch to-do


def update_todo(root):


    frame = Frame(root)


    frame.grid()


    ## show possible tasks with ids


    ## enter id, updated task, updated quad


    Label(frame, text="Enter id, task, and quadrant", font=('Aerial 12')).grid(row=1)


    ent1 = Entry(frame)


    ent1.grid(column=0, row=2)


    ent2 = Entry(frame)


    ent2.grid(column=1, row=2)


    ent3 = Entry(frame)


    ent3.grid(column=2, row=2)


    Button(frame, text='Enter', width=25, command=lambda: update_task(ent1.get(), ent2.get(), ent3.get())).grid(row=3)


    ## add suggestions here


    Label(frame, text="List of todos", font=('Aerial 12')).grid(row=4)


    ## fetch to-do list


    todo = db.getDb("ToDo/todo.json")


    todo_list = todo.getAll()


    for i, item in enumerate(todo_list):


        w = Text(frame, width=15, height=2)


        w.grid(row=(i+5))


        value = f'{item["task"]} {item["quad"]} {item["id"]}'


        w.insert(END, value)








## create day workplan


def day_plan(root):


    frame = Frame(root)


    frame.grid()


    ## show possible day tasks


    Label(frame, text="Enter 4 todos", font=('Aerial 12')).grid(row=1)


    Label(frame, text="1. High Work and High quality: ", font=('Aerial 12')).grid(row=2, column=0)


    ent1 = Entry(frame)


    ent1.grid(row=2, column =1)


    Label(frame, text="2. High Work and Low quality", font=('Aerial 12')).grid(row=3, column=0)


    ent2 = Entry(frame)


    ent2.grid(row=3, column=1)


    Label(frame, text="3. Low Work and Low quality", font=('Aerial 12')).grid(row=4, column=0)


    ent3 = Entry(frame)


    ent3.grid(row=4, column=1)


    Label(frame, text="4. Low Work and High quality", font=('Aerial 12')).grid(row=5, column=0)


    ent4 = Entry(frame)


    ent4.grid(row=5, column=1)


    Button(frame, text='Enter', width=25, command=lambda: save_plan(ent1.get(), ent2.get(), ent3.get(), ent4.get())).grid(row=6)





    ## add suggestions here


    Label(frame, text="List of to-dos", font=('Aerial 12')).grid(column=2, row=2)


    ## fetch to-do list


    todo = db.getDb("ToDo/todo.json")


    todo_list = todo.getAll()


    for i, item in enumerate(todo_list):


        w = Text(frame, width=20, height=4)


        w.grid(column=2, row=(i+3))


        w.insert(END, item["task"]+" "+item["quad"])








def update_task(id, task, quad):


    update = {"task": task, "quad": quad}


    todo = db.getDb("ToDo/todo.json")


    todo.updateById(pk=id, new_data=update)


    tkinter.messagebox.showinfo("Update Task",  "SUCCESS")








def save_task(task, quad):


    todo = db.getDb("ToDo/todo.json")


    todo.add({"task": task, "quad": quad})


    tkinter.messagebox.showinfo("Save Task",  "SUCCESS")








def save_plan(ent1, ent2, ent3, ent4):


    ## save to db


    date = datetime.datetime.now().strftime("%m/%d/%Y")


    todo = db.getDb("ToDo/daily.json")


    new_items = [


        {"task": ent1, "quad": 1, "date": date},


        {"task": ent2, "quad": 2, "date": date},


        {"task": ent3, "quad": 3, "date": date},


        {"task": ent4, "quad": 4, "date": date},


    ]





    todo.addMany(new_items)


    tkinter.messagebox.showinfo("Save Plan",  "SUCCESS")








def delete_plan():


    todo = db.getDb("ToDo/daily.json")


    date = datetime.datetime.now().strftime("%m/%d/%Y")


    for i in todo.getAll():


        if i["date"] != date:


            todo.deleteById(pk = i["id"])








if __name__ == "__main__":


    root = Tk()


    day_plan(root)


    root.mainloop()



---

## Translation: 
>Import package called  tkinter messagebox


>from tkinter Import package called  * (means grab everything) 


>from pysondb Import package called  db


>Import package called  datetime








## add to to-do


Define a function called  add_todofunction call with parameters: "root":


    frame variable equal to  Framefunction call with parameters: "root"


    ## show possible day tasks


    label variable equal to  Labelfunction call with parameters: "frame, textvariable equal to "Enter todo", fontvariable equal to function call with parameters: "'Aerial 12'""


    label packfunction call with parameters: ""


    label1 variable equal to  Labelfunction call with parameters: "frame, textvariable equal to "Task", fontvariable equal to function call with parameters: "'Aerial 12'""


    label1 packfunction call with parameters: ""


    ent1 variable equal to  Entryfunction call with parameters: "frame"


    ent1 packfunction call with parameters: ""


    label2 variable equal to  Labelfunction call with parameters: "frame, textvariable equal to "Quadrant", fontvariable equal to function call with parameters: "'Aerial 12'""


    label2 packfunction call with parameters: ""


    ent2 variable equal to  Entryfunction call with parameters: "frame"


    ent2 packfunction call with parameters: ""


    enter variable equal to  Buttonfunction call with parameters: "frame, textvariable equal to 'Enter', widthvariable equal to 25, commandvariable equal to lambda:save_taskfunction call with parameters: "ent1 getfunction call with parameters: "", ent2 getfunction call with parameters: """"


    enter packfunction call with parameters: ""


    frame packfunction call with parameters: ""








## fetch to-do


Define a function called  update_todofunction call with parameters: "root":


    frame variable equal to  Framefunction call with parameters: "root"


    frame gridfunction call with parameters: ""


    ## show possible tasks with ids


    ## enter id, updated task, updated quad


    Labelfunction call with parameters: "frame, textvariable equal to "Enter id, task, and quadrant", fontvariable equal to function call with parameters: "'Aerial 12'"" gridfunction call with parameters: "rowvariable equal to 1"


    ent1 variable equal to  Entryfunction call with parameters: "frame"


    ent1 gridfunction call with parameters: "columnvariable equal to 0, rowvariable equal to 2"


    ent2 variable equal to  Entryfunction call with parameters: "frame"


    ent2 gridfunction call with parameters: "columnvariable equal to 1, rowvariable equal to 2"


    ent3 variable equal to  Entryfunction call with parameters: "frame"


    ent3 gridfunction call with parameters: "columnvariable equal to 2, rowvariable equal to 2"


    Buttonfunction call with parameters: "frame, textvariable equal to 'Enter', widthvariable equal to 25, commandvariable equal to lambda: update_taskfunction call with parameters: "ent1 getfunction call with parameters: "", ent2 getfunction call with parameters: "", ent3 getfunction call with parameters: """" gridfunction call with parameters: "rowvariable equal to 3"


    ## add suggestions here


    Labelfunction call with parameters: "frame, textvariable equal to "List of todos", fontvariable equal to function call with parameters: "'Aerial 12'"" gridfunction call with parameters: "rowvariable equal to 4"


    ## fetch to-do list


    todo variable equal to  db getDbfunction call with parameters: ""ToDo/todo json""


    todo_list variable equal to  todo getAllfunction call with parameters: ""


    for i, item in enumeratefunction call with parameters: "todo_list":


        w variable equal to  Textfunction call with parameters: "frame, widthvariable equal to 15, heightvariable equal to 2"


        w gridfunction call with parameters: "rowvariable equal to function call with parameters: "i+5""


        value variable equal to  f'{item["task"]} {item["quad"]} {item["id"]}'


        w insertfunction call with parameters: "END, value"








## create day workplan


Define a function called  day_planfunction call with parameters: "root":


    frame variable equal to  Framefunction call with parameters: "root"


    frame gridfunction call with parameters: ""


    ## show possible day tasks


    Labelfunction call with parameters: "frame, textvariable equal to "Enter 4 todos", fontvariable equal to function call with parameters: "'Aerial 12'"" gridfunction call with parameters: "rowvariable equal to 1"


    Labelfunction call with parameters: "frame, textvariable equal to "1  High Work and High quality: ", fontvariable equal to function call with parameters: "'Aerial 12'"" gridfunction call with parameters: "rowvariable equal to 2, columnvariable equal to 0"


    ent1 variable equal to  Entryfunction call with parameters: "frame"


    ent1 gridfunction call with parameters: "rowvariable equal to 2, column variable equal to 1"


    Labelfunction call with parameters: "frame, textvariable equal to "2  High Work and Low quality", fontvariable equal to function call with parameters: "'Aerial 12'"" gridfunction call with parameters: "rowvariable equal to 3, columnvariable equal to 0"


    ent2 variable equal to  Entryfunction call with parameters: "frame"


    ent2 gridfunction call with parameters: "rowvariable equal to 3, columnvariable equal to 1"


    Labelfunction call with parameters: "frame, textvariable equal to "3  Low Work and Low quality", fontvariable equal to function call with parameters: "'Aerial 12'"" gridfunction call with parameters: "rowvariable equal to 4, columnvariable equal to 0"


    ent3 variable equal to  Entryfunction call with parameters: "frame"


    ent3 gridfunction call with parameters: "rowvariable equal to 4, columnvariable equal to 1"


    Labelfunction call with parameters: "frame, textvariable equal to "4  Low Work and High quality", fontvariable equal to function call with parameters: "'Aerial 12'"" gridfunction call with parameters: "rowvariable equal to 5, columnvariable equal to 0"


    ent4 variable equal to  Entryfunction call with parameters: "frame"


    ent4 gridfunction call with parameters: "rowvariable equal to 5, columnvariable equal to 1"


    Buttonfunction call with parameters: "frame, textvariable equal to 'Enter', widthvariable equal to 25, commandvariable equal to lambda: save_planfunction call with parameters: "ent1 getfunction call with parameters: "", ent2 getfunction call with parameters: "", ent3 getfunction call with parameters: "", ent4 getfunction call with parameters: """" gridfunction call with parameters: "rowvariable equal to 6"





    ## add suggestions here


    Labelfunction call with parameters: "frame, textvariable equal to "List of to-dos", fontvariable equal to function call with parameters: "'Aerial 12'"" gridfunction call with parameters: "columnvariable equal to 2, rowvariable equal to 2"


    ## fetch to-do list


    todo variable equal to  db getDbfunction call with parameters: ""ToDo/todo json""


    todo_list variable equal to  todo getAllfunction call with parameters: ""


    for i, item in enumeratefunction call with parameters: "todo_list":


        w variable equal to  Textfunction call with parameters: "frame, widthvariable equal to 20, heightvariable equal to 4"


        w gridfunction call with parameters: "columnvariable equal to 2, rowvariable equal to function call with parameters: "i+3""


        w insertfunction call with parameters: "END, item["task"]+" "+item["quad"]"








Define a function called  update_taskfunction call with parameters: "id, task, quad":


    update variable equal to  {"task": task, "quad": quad}


    todo variable equal to  db getDbfunction call with parameters: ""ToDo/todo json""


    todo updateByIdfunction call with parameters: "pkvariable equal to id, new_datavariable equal to update"


    tkinter messagebox showinfofunction call with parameters: ""Update Task",  "SUCCESS""








Define a function called  save_taskfunction call with parameters: "task, quad":


    todo variable equal to  db getDbfunction call with parameters: ""ToDo/todo json""


    todo addfunction call with parameters: "{"task": task, "quad": quad}"


    tkinter messagebox showinfofunction call with parameters: ""Save Task",  "SUCCESS""








Define a function called  save_planfunction call with parameters: "ent1, ent2, ent3, ent4":


    ## save to db


    date variable equal to  datetime datetime nowfunction call with parameters: "" turn time into string function call with parameters: ""%m/%d/%Y""


    todo variable equal to  db getDbfunction call with parameters: ""ToDo/daily json""


    new_items variable equal to  [


        {"task": ent1, "quad": 1, "date": date},


        {"task": ent2, "quad": 2, "date": date},


        {"task": ent3, "quad": 3, "date": date},


        {"task": ent4, "quad": 4, "date": date},


    ]





    todo addManyfunction call with parameters: "new_items"


    tkinter messagebox showinfofunction call with parameters: ""Save Plan",  "SUCCESS""








Define a function called  delete_planfunction call with parameters: "":


    todo variable equal to  db getDbfunction call with parameters: ""ToDo/daily json""


    date variable equal to  datetime datetime nowfunction call with parameters: "" turn time into string function call with parameters: ""%m/%d/%Y""


    for i in todo getAllfunction call with parameters: "":


        if i["date"] not variable equal to  date:


            todo deleteByIdfunction call with parameters: "pk variable equal to  i["id"]"








if __name__ variable equal to variable equal to  "__main__":


    root variable equal to  initiate tkinter


    day_planfunction call with parameters: "root"


    root mainloopfunction call with parameters: ""


[Github link] https://github.com/DownRamp/Assistant/blob/main/Tools/daily_check.py
## Next Steps: 
1. Setup self organizing day plan
2.  Create a web page for this info gathering
3.  Send to a db for access anywhere


---

