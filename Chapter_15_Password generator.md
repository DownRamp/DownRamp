# Password generator

---

## Description: 
Generate and store passwords for a user locally. Also have the ability to fetch forgotten passwords.

---

## Why: 
Useful for making long passwords that you don't have to remember

---

## Setup Instructions: 
1. python3

---

## To do: 
1. Create an encryption algorithm
2.  create a decryption algorithm
3.  create a gui for inputting size and reason
4.  generate a random string with all chars
5.  store passwords
6.  fetch passwords 

---

## Code: 
>import datetime


>import random


>import array


>from pysondb import db


>import string


>from tkinter import *


>from tkinter import messagebox








def gen_string():


    S = 10


    ran = ''.join(random.choices(string.ascii_uppercase + string.digits, k=S))


    return str(ran)








def password(pass_len, reason):


    ### check length


    UCASE = ['A', 'B', 'C', 'D', 'E', 'F', 'G', 'H',


    'I', 'J', 'K', 'M', 'N', 'O', 'p', 'Q',


    'R', 'S', 'T', 'U', 'V', 'W', 'X', 'Y',


    'Z']





    LCASE = ['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h',


    'i', 'j', 'k', 'm', 'n', 'o', 'p', 'q',


    'r', 's', 't', 'u', 'v', 'w', 'x', 'y',


    'z']





    DIGITS = ['0', '1', '2', '3', '4', '5', '6', '7', '8', '9']





    SYMBOLS = ['@', '###', '$', '%', '=', ':', '?', '|', '~', '>',


    '*', '(', ')']





    ### number of different characters


    ucase_amount = random.randint(1, pass_len-3)


    lcase_amount = random.randint(1, pass_len-2-ucase_amount)


    digit_amount = random.randint(1, pass_len-1-ucase_amount-lcase_amount)


    symbols_amount = pass_len - ucase_amount-lcase_amount-digit_amount





    temp = ""


    for i in range(ucase_amount):


        temp = temp + random.choice(UCASE)


        temp_list = array.array('u', temp)


        random.shuffle(temp_list)





    for i in range(lcase_amount):


        temp = temp + random.choice(LCASE)


        temp_list = array.array('u', temp)


        random.shuffle(temp_list)





    for i in range(digit_amount):


        temp = temp + random.choice(DIGITS)


        temp_list = array.array('u', temp)


        random.shuffle(temp_list)





    for i in range(symbols_amount):


        temp = temp + random.choice(SYMBOLS)


        temp_list = array.array('u', temp)


        random.shuffle(temp_list)





    password = ""


    for x in temp_list:


        password = password + x


    store(reason, password)








def store(reason, password):


    key = gen_string()


    date = datetime.datetime.now().strftime("%m/%d/%Y, %H:%M:%S")


    messagebox.showinfo('Secret', f"Please remember your password: {password}")


    a = db.getDb("../Secrets/password.json")


    b = db.getDb("../Secrets/hash.json")





    encMessage, key = encrypt(password, key)


    a.add({"reason": reason, "date": date, "password": encMessage})


    b.add({"reason": reason, "date": date, "key": key})








def retrieve(reason):


    query = {"reason": reason}


    a = db.getDb("../Secrets/password.json")


    hash = a.getByQuery(query=query)


    b = db.getDb("../Secrets/hash.json")





    key = b.getByQuery(query=query)





    decMessage = decrypt(hash[0]["password"], key[0]["key"])


    print(f"Password: {decMessage}")








def encrypt(password, key):


    l = 1


    encPass = ""


    for i in range(2, 8):


        if (i*i) >len(password):


            l = i


            break





    grid = [["" for i in range(l)] for j in range(l)]


    count = 0


    left = (l*l) - len(password)


    key = key[0:left]


    password = password + key


    for i in range(l):


        for j in range(l):


            grid[i][j] = password[count]


            count += 1





    for i in range(l):


        for j in range(l):


            encPass = encPass + grid[j][i]


    return encPass, key








def decrypt(password, key):


    l = 1


    decPass = ""


    for i in range(2, 8):


        if (i * i) == len(password):


            l = i





    grid = [["" for i in range(l)] for j in range(l)]


    count = 0


    real = (l*l) - len(key)





    for i in range(l):


        for j in range(l):


            grid[i][j] = password[count]


            count += 1





    for i in range(l):


        for j in range(l):


            decPass = decPass+grid[j][i]





    return decPass[0:real]








def main(root):


    lbl1 = Label(root, text="Enter length of password: ")


    lbl1.pack()


    enteryPassLen = Entry(root, width=30)


    enteryPassLen.insert(END, "10")


    enteryPassLen.pack()


    lbl2 = Label(root, text="Enter reason for password: ")


    lbl2.pack()


    enteryReason = Entry(root, width=30)


    enteryReason.pack()


    btn = Button(root, text="Enter", command=lambda: password(int(enteryPassLen.get()), enteryReason.get()))


    btn.pack()


    root.mainloop()








if __name__ == '__main__':


    root = Tk()


    root.title("Times")


    root.geometry("350x200")


    main(root)






---

## Translation: 
>Import package called  datetime


>Import package called  random


>Import package called  array


>from pysondb Import package called  db


>Import package called  string


>from tkinter Import package called  * (means grab everything) 


>from tkinter Import package called  messagebox








Define a function called  gen_string function call with parameters: "":


    S variable equal to  10


    ran variable equal to  '' join function call with parameters: "random choices function call with parameters: "string ascii_uppercase + string digits, kvariable equal to S""


    return str function call with parameters: "ran"








Define a function called  password function call with parameters: "pass_len, reason":


    ### check length


    UCASE variable equal to  ['A', 'B', 'C', 'D', 'E', 'F', 'G', 'H',


    'I', 'J', 'K', 'M', 'N', 'O', 'p', 'Q',


    'R', 'S', 'T', 'U', 'V', 'W', 'X', 'Y',


    'Z']





    LCASE variable equal to  ['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h',


    'i', 'j', 'k', 'm', 'n', 'o', 'p', 'q',


    'r', 's', 't', 'u', 'v', 'w', 'x', 'y',


    'z']





    DIGITS variable equal to  ['0', '1', '2', '3', '4', '5', '6', '7', '8', '9']





    SYMBOLS variable equal to  ['@', '###', '$', '%', 'variable equal to ', ':', '?', '|', '~', '>',


    '* (means grab everything) ', ' function call with parameters: "', '"']





    ### number of different characters


    ucase_amount variable equal to  random randint function call with parameters: "1, pass_len-3"


    lcase_amount variable equal to  random randint function call with parameters: "1, pass_len-2-ucase_amount"


    digit_amount variable equal to  random randint function call with parameters: "1, pass_len-1-ucase_amount-lcase_amount"


    symbols_amount variable equal to  pass_len - ucase_amount-lcase_amount-digit_amount





    temp variable equal to  ""


    for i in range function call with parameters: "ucase_amount":


        temp variable equal to  temp + random choice function call with parameters: "UCASE"


        temp_list variable equal to  array array function call with parameters: "'u', temp"


        random shuffle function call with parameters: "temp_list"





    for i in range function call with parameters: "lcase_amount":


        temp variable equal to  temp + random choice function call with parameters: "LCASE"


        temp_list variable equal to  array array function call with parameters: "'u', temp"


        random shuffle function call with parameters: "temp_list"





    for i in range function call with parameters: "digit_amount":


        temp variable equal to  temp + random choice function call with parameters: "DIGITS"


        temp_list variable equal to  array array function call with parameters: "'u', temp"


        random shuffle function call with parameters: "temp_list"





    for i in range function call with parameters: "symbols_amount":


        temp variable equal to  temp + random choice function call with parameters: "SYMBOLS"


        temp_list variable equal to  array array function call with parameters: "'u', temp"


        random shuffle function call with parameters: "temp_list"





    password variable equal to  ""


    for x in temp_list:


        password variable equal to  password + x


    store function call with parameters: "reason, password"








Define a function called  store function call with parameters: "reason, password":


    key variable equal to  gen_string function call with parameters: ""


    date variable equal to  datetime datetime now function call with parameters: "" turn time into string  function call with parameters: ""%m/%d/%Y, %H:%M:%S""


    messagebox showinfo function call with parameters: "'Secret', f"Please remember your password: {password}""


    a variable equal to  db getDb function call with parameters: ""  /Secrets/password json""


    b variable equal to  db getDb function call with parameters: ""  /Secrets/hash json""





    encMessage, key variable equal to  encrypt function call with parameters: "password, key"


    a add function call with parameters: "{"reason": reason, "date": date, "password": encMessage}"


    b add function call with parameters: "{"reason": reason, "date": date, "key": key}"








Define a function called  retrieve function call with parameters: "reason":


    query variable equal to  {"reason": reason}


    a variable equal to  db getDb function call with parameters: ""  /Secrets/password json""


    hash variable equal to  a getByQuery function call with parameters: "queryvariable equal to query"


    b variable equal to  db getDb function call with parameters: ""  /Secrets/hash json""





    key variable equal to  b getByQuery function call with parameters: "queryvariable equal to query"





    decMessage variable equal to  decrypt function call with parameters: "hash[0]["password"], key[0]["key"]"


    print function call with parameters: "f"Password: {decMessage}""








Define a function called  encrypt function call with parameters: "password, key":


    l variable equal to  1


    encPass variable equal to  ""


    for i in range function call with parameters: "2, 8":


        if  function call with parameters: "i* (means grab everything) i" >len function call with parameters: "password":


            l variable equal to  i


            break





    grid variable equal to  [["" for i in range function call with parameters: "l"] for j in range function call with parameters: "l"]


    count variable equal to  0


    left variable equal to   function call with parameters: "l* (means grab everything) l" - len function call with parameters: "password"


    key variable equal to  key[0:left]


    password variable equal to  password + key


    for i in range function call with parameters: "l":


        for j in range function call with parameters: "l":


            grid[i][j] variable equal to  password[count]


            count add to  1





    for i in range function call with parameters: "l":


        for j in range function call with parameters: "l":


            encPass variable equal to  encPass + grid[j][i]


    return encPass, key








Define a function called  decrypt function call with parameters: "password, key":


    l variable equal to  1


    decPass variable equal to  ""


    for i in range function call with parameters: "2, 8":


        if  function call with parameters: "i * (means grab everything)  i" variable equal to variable equal to  len function call with parameters: "password":


            l variable equal to  i





    grid variable equal to  [["" for i in range function call with parameters: "l"] for j in range function call with parameters: "l"]


    count variable equal to  0


    real variable equal to   function call with parameters: "l* (means grab everything) l" - len function call with parameters: "key"





    for i in range function call with parameters: "l":


        for j in range function call with parameters: "l":


            grid[i][j] variable equal to  password[count]


            count add to  1





    for i in range function call with parameters: "l":


        for j in range function call with parameters: "l":


            decPass variable equal to  decPass+grid[j][i]





    return decPass[0:real]








Define a function called  main function call with parameters: "root":


    lbl1 variable equal to  Label function call with parameters: "root, textvariable equal to "Enter length of password: ""


    lbl1 pack function call with parameters: ""


    enteryPassLen variable equal to  Entry function call with parameters: "root, widthvariable equal to 30"


    enteryPassLen insert function call with parameters: "END, "10""


    enteryPassLen pack function call with parameters: ""


    lbl2 variable equal to  Label function call with parameters: "root, textvariable equal to "Enter reason for password: ""


    lbl2 pack function call with parameters: ""


    enteryReason variable equal to  Entry function call with parameters: "root, widthvariable equal to 30"


    enteryReason pack function call with parameters: ""


    btn variable equal to  Button function call with parameters: "root, textvariable equal to "Enter", commandvariable equal to lambda: password function call with parameters: "int function call with parameters: "enteryPassLen get function call with parameters: """, enteryReason get function call with parameters: """"


    btn pack function call with parameters: ""


    root mainloop function call with parameters: ""








if __name__ variable equal to variable equal to  '__main__':


    root variable equal to  initiate tkinter 


    root title function call with parameters: ""Times""


    root geometry function call with parameters: ""350x200""


    main function call with parameters: "root"





[Github link] https://github.com/DownRamp/Assistant/blob/main/Tools/password_gen.py
## Next Steps: 
1. Create a stronger algorithm
2.  store passwords in a db for global access
3.  set alarms for updating passwords

---

