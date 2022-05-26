# File manager

---

## Description: 
Organize your files into multiple sections.
(Please note I watched a tutorial to make some of this)
---

## Why: 
Useful for people that don't sort their download folder(like me)

---

## Setup Instructions: 
1. python3
2. Make an .env file(Google it if needed)

---

## To do: 
1. Get path for folders to check
2.  check folder for file names
3.  compare file name endings
4.  move file to appropriate folder

---

## Code: 
>import os


>import shutil


>import time


>import logging


>from watchdog.observers import Observer


>from watchdog.events import FileSystemEventHandler


>from dotenv import load_dotenv


load_dotenv()





source_dir = os.environ.get("source")


dest_dir_sfx = os.environ.get("sfx")


dest_dir_music = os.environ.get("music")


dest_dir_video = os.environ.get("video")


dest_dir_image = os.environ.get("image")


dest_dir_pdf = os.environ.get("pdf")


dest_dir_zip = os.environ.get("zip")


dest_dir_excel = os.environ.get("excel")





###### ADD MORE TYPES





def makeUnique(path):


    filename, extension = os.path.splitext(path)


    counter = 1


    ###### IF FILE EXISTS, ADDS NUMBER TO THE END OF THE FILENAME


    while os.path.exists(path):


        path = filename + " (" + str(counter) + ")" + extension


        counter += 1





    return path





def move(dest, entry, name):


    file_exists = os.path.exists(dest + "/" + name)


    if file_exists:


        unique_name = makeUnique(name)


        os.rename(entry, unique_name)


    shutil.move(entry,dest)





###class MoverHandler(FileSystemEventHandler):


class MoverHandler:


    def activate(self):


        with os.scandir(source_dir) as entries:


            for entry in entries:


                name = entry.name


                dest = source_dir


                if name.endswith('.wav') or name.endswith('.mp3'):


                    if entry.stat().st_size < 25000000 or "SFX" in name:


                        dest = dest_dir_sfx


                    else:


                        dest = dest_dir_music


                    move(dest, entry, name)


                elif name.endswith('.mov') or name.endswith('.mp4'):


                    dest = dest_dir_video


                    move(dest, entry, name)


                elif name.endswith('.jpg') or name.endswith('.jpeg') or name.endswith('.png'):


                    dest = dest_dir_image


                    move(dest, entry, name)


                elif name.endswith('.pdf') or name.endswith('.txt') or name.endswith('.doc'):


                    dest = dest_dir_pdf


                    move(dest, entry, name)


                elif name.endswith('.zip') or name.endswith('.tar') or name.endswith('.7zip'):


                    dest = dest_dir_zip


                    move(dest, entry, name)


                elif name.endswith('.xlsx'):


                    dest = dest_dir_excel


                    move(dest, entry, name)





if __name__ == "__main__":


    logging.basicConfig(level=logging.INFO,


                        format='%(asctime)s - %(message)s',


                        datefmt='%Y-%m-%d %H:%M:%S')


    path = source_dir


    MoverHandler().activate()



---

## Translation: 
>Import package called  os


>Import package called  shutil


>Import package called  time


>Import package called  logging


>from watchdog observers Import package called  Observer


>from watchdog events Import package called  FileSystemEventHandler


>from dotenv Import package called  load_dotenv


load_dotenv function call with parameters: ""





source_dir variable equal to  os environ get function call with parameters: ""source""


dest_dir_sfx variable equal to  os environ get function call with parameters: ""sfx""


dest_dir_music variable equal to  os environ get function call with parameters: ""music""


dest_dir_video variable equal to  os environ get function call with parameters: ""video""


dest_dir_image variable equal to  os environ get function call with parameters: ""image""


dest_dir_pdf variable equal to  os environ get function call with parameters: ""pdf""


dest_dir_zip variable equal to  os environ get function call with parameters: ""zip""


dest_dir_excel variable equal to  os environ get function call with parameters: ""excel""





###### ADD MORE TYPES





Define a function called  makeUnique function call with parameters: "path":


    filename, extension variable equal to  os path splitext function call with parameters: "path"


    counter variable equal to  1


    ###### IF FILE EXISTS, ADDS NUMBER TO THE END OF THE FILENAME


    while os path exists function call with parameters: "path":


        path variable equal to  filename + "  function call with parameters: "" + str function call with parameters: "counter" + """ + extension


        counter add to  1





    return path





Define a function called  move function call with parameters: "dest, entry, name":


    file_exists variable equal to  os path exists function call with parameters: "dest + "/" + name"


    if file_exists:


        unique_name variable equal to  makeUnique function call with parameters: "name"


        os rename function call with parameters: "entry, unique_name"


    shutil move function call with parameters: "entry,dest"





###class MoverHandler function call with parameters: "FileSystemEventHandler":


class MoverHandler:


    Define a function called  activate function call with parameters: "self":


        with os scandir function call with parameters: "source_dir" as entries:


            for entry in entries:


                name variable equal to  entry name


                dest variable equal to  source_dir


                if name endswith function call with parameters: "' wav'" or name endswith function call with parameters: "' mp3'":


                    if entry stat function call with parameters: "" st_size less then  25000000 or "SFX" in name:


                        dest variable equal to  dest_dir_sfx


                    else:


                        dest variable equal to  dest_dir_music


                    move function call with parameters: "dest, entry, name"


                elif name endswith function call with parameters: "' mov'" or name endswith function call with parameters: "' mp4'":


                    dest variable equal to  dest_dir_video


                    move function call with parameters: "dest, entry, name"


                elif name endswith function call with parameters: "' jpg'" or name endswith function call with parameters: "' jpeg'" or name endswith function call with parameters: "' png'":


                    dest variable equal to  dest_dir_image


                    move function call with parameters: "dest, entry, name"


                elif name endswith function call with parameters: "' pdf'" or name endswith function call with parameters: "' txt'" or name endswith function call with parameters: "' doc'":


                    dest variable equal to  dest_dir_pdf


                    move function call with parameters: "dest, entry, name"


                elif name endswith function call with parameters: "' zip'" or name endswith function call with parameters: "' tar'" or name endswith function call with parameters: "' 7zip'":


                    dest variable equal to  dest_dir_zip


                    move function call with parameters: "dest, entry, name"


                elif name endswith function call with parameters: "' xlsx'":


                    dest variable equal to  dest_dir_excel


                    move function call with parameters: "dest, entry, name"





if __name__ variable equal to variable equal to  "__main__":


    logging basicConfig function call with parameters: "levelvariable equal to logging INFO,


                        formatvariable equal to '% function call with parameters: "asctime"s - % function call with parameters: "message"s',


                        datefmtvariable equal to '%Y-%m-%d %H:%M:%S'"


    path variable equal to  source_dir


    MoverHandler function call with parameters: "" activate function call with parameters: ""


[Github link] https://github.com/DownRamp/Assistant/blob/main/filemanager.py
## Next Steps: 
1. Add in your folders
2.  add in another location you want to check
3.  add a removal function for some files you don't want

---

