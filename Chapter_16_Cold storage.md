# Cold storage

---

## Description: 
Put older files into cold storage for more effective storage

---

## Why: 
Reduce size of files left over

---

## Setup Instructions: 
1. python3

---

## To do: 
1. Search through files
2.  check age of files
3.  move old files into a folder
4.  unpack zip files
5.  zip folder

---

## Code: 
>import os


>import gzip


>from datetime import datetime, timedelta


>import shutil


>import pysondb as db








def start_search(name):


    listOfFiles = search("/Filing_System")


    for file in listOfFiles:


        if name in file:


            return file





    cold = db.getDb("/Filing_System/cold.json")


    cold_list = cold.getAll()


    for item in cold_list:


        if name in item["path"]:


            thaw(item["path"])


            return item["path"]








def search(dirName):


    listOfFiles = list()


    for (dirpath, dirnames, filenames) in os.walk(dirName):


        listOfFiles += [os.path.join(dirpath, file) for file in filenames]


    return listOfFiles








def freezer():


    ### Go through files


    files = search("Filing_System")


    week_time = datetime.now() - timedelta(days=7)


    ### Grab ones that are older than a week


    for file in files:


        file_stats = os.stat(file)


        last_access = datetime.fromtimestamp(file_stats.st_atime)


        if last_access < week_time:


            ### move to cold storage folder and gzip


            if os.path.exists("Filing_System/Freezer.zip"):


                shutil.unpack_archive("Filing_System/Freezer.zip")


            ### save path location on file for putting back


            cold = db.getDb("Filing_System/cold.json")


            cold.add({"path": file})


            shutil.move(file, "Filing_System/Freezer/"+os.path.basename(file))


            shutil.make_archive("Filing_System/Freezer", 'zip', "Filing_System/Freezer")








def thaw(name):


    ### Grab requested file


    cold = db.getDb("Filing_System/cold.json")


    cold_list = cold.getAll()


    for item in cold_list:


        if name == item["path"]:


            cold.deleteById(pk=item["id"])


            break





    if os.path.exists("Filing_System/Freezer.zip"):


        shutil.unpack_archive("Filing_System/Freezer.zip")


    ### return to path and move back to path


    shutil.move("Filing_System/Freezer"+os.path.basename(file), file)








def window(main):


    frame = Frame(main)


    frame.grid()


    Label(frame, text="Search", font=('Aerial 12')).grid(row=1, column =0)


    ent1 = Entry(frame)


    ent1.grid(row=1, column=1)


    Button(frame, text='Enter', width=25, command=lambda: start_search(ent1.get())).grid(row=1, column=2)








if __name__ == '__main__':


    freezer()



---

## Translation: 
>Import package called  os


>Import package called  gzip


>from datetime Import package called  datetime, timedelta


>Import package called  shutil


>Import package called  pysondb as db








Define a function called  start_search function call with parameters: "name":


    listOfFiles variable equal to  search function call with parameters: ""/Filing_System""


    for file in listOfFiles:


        if name in file:


            return file





    cold variable equal to  db getDb function call with parameters: ""/Filing_System/cold json""


    cold_list variable equal to  cold getAll function call with parameters: ""


    for item in cold_list:


        if name in item["path"]:


            thaw function call with parameters: "item["path"]"


            return item["path"]








Define a function called  search function call with parameters: "dirName":


    listOfFiles variable equal to  list function call with parameters: ""


    for  function call with parameters: "dirpath, dirnames, filenames" in os walk function call with parameters: "dirName":


        listOfFiles add to  [os path join function call with parameters: "dirpath, file" for file in filenames]


    return listOfFiles








Define a function called  freezer function call with parameters: "":


    ### Go through files


    files variable equal to  search function call with parameters: ""Filing_System""


    week_time variable equal to  datetime now function call with parameters: "" - timedelta function call with parameters: "daysvariable equal to 7"


    ### Grab ones that are older than a week


    for file in files:


        file_stats variable equal to  os stat function call with parameters: "file"


        last_access variable equal to  datetime fromtimestamp function call with parameters: "file_stats st_atime"


        if last_access less then  week_time:


            ### move to cold storage folder and gzip


            if os path exists function call with parameters: ""Filing_System/Freezer zip"":


                shutil unpack_archive function call with parameters: ""Filing_System/Freezer zip""


            ### save path location on file for putting back


            cold variable equal to  db getDb function call with parameters: ""Filing_System/cold json""


            cold add function call with parameters: "{"path": file}"


            shutil move function call with parameters: "file, "Filing_System/Freezer/"+os path basename function call with parameters: "file""


            shutil make_archive function call with parameters: ""Filing_System/Freezer", 'zip', "Filing_System/Freezer""








Define a function called  thaw function call with parameters: "name":


    ### Grab requested file


    cold variable equal to  db getDb function call with parameters: ""Filing_System/cold json""


    cold_list variable equal to  cold getAll function call with parameters: ""


    for item in cold_list:


        if name variable equal to variable equal to  item["path"]:


            cold deleteById function call with parameters: "pkvariable equal to item["id"]"


            break





    if os path exists function call with parameters: ""Filing_System/Freezer zip"":


        shutil unpack_archive function call with parameters: ""Filing_System/Freezer zip""


    ### return to path and move back to path


    shutil move function call with parameters: ""Filing_System/Freezer"+os path basename function call with parameters: "file", file"








Define a function called  window function call with parameters: "main":


    frame variable equal to  Frame function call with parameters: "main"


    frame grid function call with parameters: ""


    Label function call with parameters: "frame, textvariable equal to "Search", fontvariable equal to  function call with parameters: "'Aerial 12'"" grid function call with parameters: "rowvariable equal to 1, column variable equal to 0"


    ent1 variable equal to  Entry function call with parameters: "frame"


    ent1 grid function call with parameters: "rowvariable equal to 1, columnvariable equal to 1"


    Button function call with parameters: "frame, textvariable equal to 'Enter', widthvariable equal to 25, commandvariable equal to lambda: start_search function call with parameters: "ent1 get function call with parameters: """" grid function call with parameters: "rowvariable equal to 1, columnvariable equal to 2"








if __name__ variable equal to variable equal to  '__main__':


    freezer function call with parameters: ""


[Github link] https://github.com/DownRamp/Assistant/blob/main/Tools/cold_storage.py
## Next Steps: 
1. Move zip file off computer (s3 bucket)
2.  Delete old files 

---

