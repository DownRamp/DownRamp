# Reader

---

## Description: 
Reader function for people that prefer to listen then read.

---

## Why: 
Fast reading as well as getting to use text to speech function

---

## Setup Instructions: 
1. python3

---

## To do: 
1. select a book
2.  read out the text
3.  create a gui to start

---

## Code: 
###!/usr/bin/env python





>import glob


>import pyttsx3


>from tkinter import *


>import PyPDF2


>import threading


>import os


>import tkinter.messagebox








stop_flag = False


book_selection = ""








class EbookToAudio:





    ### to test later


    ### pick a book


    def pick_book(self,root):


        frame = Frame(root)


        frame.grid()


        list_events = glob.glob("Books/*")


        list_items = StringVar(value=list_events)


        listing = Listbox(frame, listvariable=list_items, height=len(list_events))


        listing.grid(row=1)


        Button(frame, text="Select", command=lambda: self.selection(root, listing.get(listing.curselection())))\


            .grid(row=5)





    def selection(self, main, book):


        global book_selection


        book_selection = book


        tkinter.messagebox.showinfo("Selection made", "Selection Made")


    ### turn pdf to readable


    def pdf_to_read(self, book):


        file = open(book, 'rb')


        file_reader = PyPDF2.PdfFileReader(file)


        return file_reader



    ### turn pdf to readable


    def txt_to_read(self, book):


        file = open(book, "r")


        words = file.read().splitlines()


        file.close()


        return words





    ### save point to read from


    def save_point(self, file, i):


        ### stop reading


        f = open(file, "w")


        f.write(i)


        f.close()





    def stop(self):


        global stop_flag


        stop_flag = True





    def fetch_bookmark(self, filename):


        try:


            if os.path.exists(filename):


                with open(filename) as f:


                    return f.read()


            return ''


        except IOError:


            return ''





    def window(self, root):


        frame = Frame(root)


        frame.grid()


        Label(frame, text="Read a book").grid(row=1)


        Button(frame, text="Pick a book", command=lambda: self.pick_book(Toplevel(root))).grid(row=3)


        Button(frame, text="Start", command=lambda: self.main()).grid(row=4)


        Button(frame, text="Stop", command=lambda: self.stop()).grid(row=5)





    ### main


    def main(self):


        global book_selection, stop_flag


        stop_flag = False


        ### take book name and grab txt file with bookmark


        engine = pyttsx3.init()


        voices = engine.getProperty('voices')


        ### I like this one more (7)


        engine.setProperty('voice', voices[7].id)


        engine.say("Hello Reader!")


        engine.runAndWait()


        ### for picking book feature later


        ### book = pick_book()


        ### fetch any saved points


        bookmark_file = f"/Bookmarks/{book_selection}"


        bookmark = self.fetch_bookmark(bookmark_file)


        if bookmark == '':


            bookmark_value = 0


        else:


            bookmark_value = int(bookmark)


        if ".txt" in book_selection:


            file_reader = self.txt_to_read(book_selection)


            for i in range(bookmark_value, len(file_reader)):


                if not stop_flag:


                    value = file_reader[i].strip()


                    if value == "":


                        continue


                    engine.say(value)


                    engine.runAndWait()


                else:


                    self.save_point(bookmark_file, i)


            ### archive book


            path = book_selection[:6]+"Done/"+book_selection[6:]


            os.rename(book_selection, "path/to/new/destination/for/file.foo")


        engine.stop()








if __name__ == '__main__':


    root = Tk()


    root.title("Audio from ebook")


    root.geometry("350x200")


    EbookToAudio.window(root)


    root.mainloop()






---

## Translation: 
###not /usr/bin/env python





>Import package called  glob


>Import package called  pyttsx3


>from tkinter Import package called  * (means grab everything) 


>Import package called  PyPDF2


>Import package called  threading


>Import package called  os


>Import package called  tkinter messagebox








stop_flag variable equal to  False


book_selection variable equal to  ""








class EbookToAudio:





    ### to test later


    ### pick a book


    Define a function called  pick_book function call with parameters: "self,root":


        frame variable equal to  Frame function call with parameters: "root"


        frame grid function call with parameters: ""


        list_events variable equal to  glob glob function call with parameters: ""Books/* (means grab everything) ""


        list_items variable equal to  StringVar function call with parameters: "valuevariable equal to list_events"


        listing variable equal to  Listbox function call with parameters: "frame, listvariablevariable equal to list_items, heightvariable equal to len function call with parameters: "list_events""


        listing grid function call with parameters: "rowvariable equal to 1"


        Button function call with parameters: "frame, textvariable equal to "Select", commandvariable equal to lambda: self selection function call with parameters: "root, listing get function call with parameters: "listing curselection function call with parameters: """""\


             grid function call with parameters: "rowvariable equal to 5"





    Define a function called  selection function call with parameters: "self, main, book":


        fetch global variable  book_selection


        book_selection variable equal to  book


        tkinter messagebox showinfo function call with parameters: ""Selection made", "Selection Made""





    ### turn pdf to readable


    Define a function called  pdf_to_read function call with parameters: "self, book":


        file variable equal to  open function call with parameters: "book, 'rb'"


        file_reader variable equal to  PyPDF2 PdfFileReader function call with parameters: "file"


        return file_reader





    ### turn pdf to readable


    Define a function called  txt_to_read function call with parameters: "self, book":


        file variable equal to  open function call with parameters: "book, "r""


        words variable equal to  file read function call with parameters: "" splitlines function call with parameters: ""


        file close function call with parameters: ""


        return words





    ### save point to read from


    Define a function called  save_point function call with parameters: "self, file, i":


        ### stop reading


        f variable equal to  open function call with parameters: "file, "w""


        f write function call with parameters: "i"


        f close function call with parameters: ""





    Define a function called  stop function call with parameters: "self":


        fetch global variable  stop_flag


        stop_flag variable equal to  True





    Define a function called  fetch_bookmark function call with parameters: "self, filename":


        try:


            if os path exists function call with parameters: "filename":


                with open function call with parameters: "filename" as f:


                    return f read function call with parameters: ""


            return ''


        except IOError:


            return ''





    Define a function called  window function call with parameters: "self, root":


        frame variable equal to  Frame function call with parameters: "root"


        frame grid function call with parameters: ""


        Label function call with parameters: "frame, textvariable equal to "Read a book"" grid function call with parameters: "rowvariable equal to 1"


        Button function call with parameters: "frame, textvariable equal to "Pick a book", commandvariable equal to lambda: self pick_book function call with parameters: "Toplevel function call with parameters: "root""" grid function call with parameters: "rowvariable equal to 3"


        Button function call with parameters: "frame, textvariable equal to "Start", commandvariable equal to lambda: self main function call with parameters: """ grid function call with parameters: "rowvariable equal to 4"


        Button function call with parameters: "frame, textvariable equal to "Stop", commandvariable equal to lambda: self stop function call with parameters: """ grid function call with parameters: "rowvariable equal to 5"





    ### main


    Define a function called  main function call with parameters: "self":


        fetch global variable  book_selection, stop_flag


        stop_flag variable equal to  False


        ### take book name and grab txt file with bookmark


        engine variable equal to  pyttsx3 initiate 


        voices variable equal to  engine getProperty function call with parameters: "'voices'"


        ### I like this one more  function call with parameters: "7"


        engine setProperty function call with parameters: "'voice', voices[7] id"


        engine say function call with parameters: ""Hello Readernot ""


        engine runAndWait function call with parameters: ""


        ### for picking book feature later


        ### book variable equal to  pick_book function call with parameters: ""


        ### fetch any saved points


        bookmark_file variable equal to  f"/Bookmarks/{book_selection}"


        bookmark variable equal to  self fetch_bookmark function call with parameters: "bookmark_file"


        if bookmark variable equal to variable equal to  '':


            bookmark_value variable equal to  0


        else:


            bookmark_value variable equal to  int function call with parameters: "bookmark"


        if " txt" in book_selection:


            file_reader variable equal to  self txt_to_read function call with parameters: "book_selection"


            for i in range function call with parameters: "bookmark_value, len function call with parameters: "file_reader"":


                if not stop_flag:


                    value variable equal to  file_reader[i] strip function call with parameters: ""


                    if value variable equal to variable equal to  "":


                        continue


                    engine say function call with parameters: "value"


                    engine runAndWait function call with parameters: ""


                else:


                    self save_point function call with parameters: "bookmark_file, i"


            ### archive book


            path variable equal to  book_selection[:6]+"Done/"+book_selection[6:]

            os rename function call with parameters: "book_selection, "path/to/new/destination/for/file foo""

        engine stop function call with parameters: ""








if __name__ variable equal to variable equal to  '__main__':


    root variable equal to  initiate tkinter 


    root title function call with parameters: ""Audio from ebook""


    root geometry function call with parameters: ""350x200""


    EbookToAudio window function call with parameters: "root"


    root mainloop function call with parameters: ""





[Github link] https://github.com/DownRamp/Assistant/blob/main/Tools/reader.py
## Next Steps: 
1. Add stop button
2.  read pdf
3.  fix bookmark function pls

---

