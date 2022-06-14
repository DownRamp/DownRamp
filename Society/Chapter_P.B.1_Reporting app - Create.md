# Reporting app - Create

---

## Description: 
Create a report that will be stored in file.

---

## Why: 
Needed to make data and database

---

## Setup Instructions: 
1. follow readme

---

## To do: 
1. store report
2.  create directories if not existent

---

## Code: 
>import streamlit as st


>import pysondb


>import os


>import json


### report bad/weird things around you


def report(title, status, description, date, country, city, intersection, location):


    ### check if path exists for each part


    check_directory(country, city)


    path = f"data/{country}/{city}/{intersection}.json"


    report_db = pysondb.getDb(path)


    report_db.add({"title": title, "status": status, "description": description, "date": date, "location": location})



def check_directory(country, city):


    path_sub1 = f"data/{country}"


    path_sub2 = f"data/{country}/{city}"


    is_exist1 = os.path.exists(path_sub1)


    if is_exist1:


        is_exist2 = os.path.exists(path_sub2)


        if not is_exist2:


            create_folder(path_sub2)

    else:

        create_folder(path_sub2)

### if directory doesn't already exist for search


### data/location


def create_folder(path):


    os.makedirs(path)

def app():

    with st.form("my_form"):


        st.title('Create a report')


        title = st.text_input("Title")


        status = st.selectbox("Status", ('Good', 'Bad'))


        description = st.text_area("Description")


        date = st.date_input("Date")


        country = st.text_input("Country")


        city = st.text_input("City")


        intersection = st.text_input("Intersection")


        location = st.text_input("Location")


        submitted = st.form_submit_button("Submit")

        if submitted:

            intersection = intersection.replace(" ", "")

            formatted_datetime = date.isoformat()

            json_datetime = json.dumps(formatted_datetime)

            json_datetime = json_datetime.replace("-", "")

            report(title.lower(), status.lower(), description.lower(), json_datetime,

                   country.lower(), city.lower(), intersection.lower(),

                   location.lower())

            st.success("Added To Data")

---

## Translation: 
> Import package called streamlit as st


> Import package called pysondb


> Import package called os


> Import package called json

### report bad/weird things around you

 Define a function called report function call with parameters: "title, status, description, date, country, city, intersection, location":

  ### check if path exists for each part

  check_directory function call with parameters: "country, city"

  path variable equal to f"data/{country}/{city}/{intersection} json"

  report_db variable equal to pysondb getDb function call with parameters: "path"

  report_db add function call with parameters: "{"title": title, "status": status, "description": description, "date": date, "location": location}"

 Define a function called check_directory function call with parameters: "country, city":

  path_sub1 variable equal to f"data/{country}"

  path_sub2 variable equal to f"data/{country}/{city}"

  is_exist1 variable equal to os path exists function call with parameters: "path_sub1"


  if is_exist1:


    is_exist2 variable equal to os path exists function call with parameters: "path_sub2"


    if not is_exist2:


      create_folder function call with parameters: "path_sub2"


  else:


    create_folder function call with parameters: "path_sub2"








### if directory doesn't already exist for search


### data/location


 Define a function called create_folder function call with parameters: "path":


  os makedirs function call with parameters: "path"








 Define a function called app function call with parameters: "":


  with st form function call with parameters: ""my_form"":


    st title function call with parameters: "'Create a report'"


    title variable equal to st text_input function call with parameters: ""Title""


    status variable equal to st selectbox function call with parameters: ""Status", function call with parameters: "'Good', 'Bad'""


    description variable equal to st text_area function call with parameters: ""Description""


    date variable equal to st date_input function call with parameters: ""Date""


    country variable equal to st text_input function call with parameters: ""Country""


    city variable equal to st text_input function call with parameters: ""City""


    intersection variable equal to st text_input function call with parameters: ""Intersection""


    location variable equal to st text_input function call with parameters: ""Location""


    submitted variable equal to st form_submit_button function call with parameters: ""Submit""





    if submitted:


      intersection variable equal to intersection replace function call with parameters: "" ", """


      formatted_datetime variable equal to date isoformat function call with parameters: ""





      json_datetime variable equal to json dumps function call with parameters: "formatted_datetime"


      json_datetime variable equal to json_datetime replace function call with parameters: ""-", """





      report function call with parameters: "title lower function call with parameters: "", status lower function call with parameters: "", description lower function call with parameters: "", json_datetime,


          country lower function call with parameters: "", city lower function call with parameters: "", intersection lower function call with parameters: "",


          location lower function call with parameters: """


      st success function call with parameters: ""Added To Data""


[Github link] https://github.com/DownRamp/Society/blob/main/pages/create_report.py
## Next Steps: 
1. Add different data points
2.  save to another database

---

