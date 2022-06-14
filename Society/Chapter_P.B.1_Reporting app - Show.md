# Reporting app - Show

---

## Description: 
Show reports that have been created and search through. Allows community to stay informed in small area creepiness

---

## Why: 
Quick showing of reports for users to check through

---

## Setup Instructions: 
1. follow readme

---

## To do: 
1. Fetch memory
2.  create page
3.  show good and bad news
4.  hold onto current information

---

## Code: 
>import streamlit as st


>import geocoder


>from streamlit_autorefresh import st_autorefresh


>import pysondb


>import os


>import pytz


>import matplotlib.pyplot as plt





st_autorefresh(interval=5 * 60 * 1000, key="framerefresh")





data_show = []


g = geocoder.ip('me')


user_city = g.json["city"].lower()


user_country = pytz.country_names[g.json["country"]].lower()





def local_css(file_name):


    with open(file_name) as f:


        st.markdown(f'<style>{f.read()}</style>', unsafe_allow_html=True)








def remote_css(url):


    st.markdown(f'<link href="{url}" rel="stylesheet">', unsafe_allow_html=True)








def fetch_files(dirName):


    list_of_files = list()


    for (dirpath, dirnames, filenames) in os.walk(dirName):


        list_of_files += [os.path.join(dirpath, file) for file in filenames if file.endswith('.json')]


    return list_of_files








### fill search data


def retrieve_all_reports():


    global data_show


    ### walk through subdirectories and fetch all files


    files = fetch_files("data")


    ### open files and put into data_show


    for file in files:


        data_show.extend(pysondb.getDb(file).getAll())





retrieve_all_reports()





def retrieve_reports():


    global data_show


    ### fetch all places


    path = f"data/{user_country.lower()}/{user_city.lower()}"


    if os.path.exists(path):


        files = fetch_files(path)


        reports = []


        for file in files:


            reports.extend(pysondb.getDb(file).getAll())


        return reports


    return []








def split_reports(reports):


    good, bad = [], []


    for report in reports:


        if report["status"] == "good":


            good.append(report)


        else:


            bad.append(report)


    return good, bad








def search(stat_type, value):


    global data_show


    filter_data = []





    for data in data_show:


        if value in data[stat_type]:


            filter_data.append(data)


    return filter_data








def app():


    global data_show





    local_css("assets/style.css")


    remote_css('https://fonts.googleapis.com/icon?family=Material+Icons')


    st.title('Show Report')


    col1, col2 = st.columns((3, 1))


    query = col1.text_input("", "Ex. location:123 blah street")


    ok = col2.button("OK")


    st.text("Tags:[title, description, date, location]")





    plt.style.use("dark_background")


    if not ok:


        good, bad = split_reports(retrieve_reports())


        st.write("Good news: ")


        st.dataframe(good)


        st.write("Bad news: ")


        st.dataframe(bad)


    else:


        st.success("Results found")


        if(query == ""):


            good, bad = split_reports(retrieve_reports())


        else:


            stat_type, value = query.split(":")


            good, bad = split_reports(search(stat_type.strip(), value.strip()))


        st.write("Good news: ")


        st.dataframe(good)


        st.write("Bad news: ")


        st.dataframe(bad)



---

## Translation: 
> Import package called streamlit as st


> Import package called geocoder


>from streamlit_autorefresh Import package called st_autorefresh


> Import package called pysondb


> Import package called os


> Import package called pytz


> Import package called matplotlib pyplot as plt





st_autorefresh function call with parameters: "interval variable equal to 5 * (means grab everything) 60 * (means grab everything) 1000, key variable equal to "framerefresh""





data_show variable equal to []


g variable equal to geocoder ip function call with parameters: "'me'"


user_city variable equal to g json["city"] lower function call with parameters: ""


user_country variable equal to pytz country_names[g json["country"]] lower function call with parameters: ""





 Define a function called local_css function call with parameters: "file_name":


  with open function call with parameters: "file_name" as f:


    st markdown function call with parameters: "f' less then style>{f read function call with parameters: ""} less then /style>', unsafe_allow_html variable equal to True"








 Define a function called remote_css function call with parameters: "url":


  st markdown function call with parameters: "f' less then link href variable equal to "{url}" rel variable equal to "stylesheet">', unsafe_allow_html variable equal to True"








 Define a function called fetch_files function call with parameters: "dirName":


  list_of_files variable equal to list function call with parameters: ""


  for function call with parameters: "dirpath, dirnames, filenames" in os walk function call with parameters: "dirName":


    list_of_files add to [os path join function call with parameters: "dirpath, file" for file in filenames if file endswith function call with parameters: "' json'"]


  return list_of_files








### fill search data


 Define a function called retrieve_all_reports function call with parameters: "":


   fetch global variable data_show


  ### walk through subdirectories and fetch all files


  files variable equal to fetch_files function call with parameters: ""data""


  ### open files and put into data_show


  for file in files:


    data_show extend function call with parameters: "pysondb getDb function call with parameters: "file" getAll function call with parameters: """





retrieve_all_reports function call with parameters: ""





 Define a function called retrieve_reports function call with parameters: "":


   fetch global variable data_show


  ### fetch all places


  path variable equal to f"data/{user_country lower function call with parameters: ""}/{user_city lower function call with parameters: ""}"


  if os path exists function call with parameters: "path":


    files variable equal to fetch_files function call with parameters: "path"


    reports variable equal to []


    for file in files:


      reports extend function call with parameters: "pysondb getDb function call with parameters: "file" getAll function call with parameters: """


    return reports


  return []








 Define a function called split_reports function call with parameters: "reports":


  good, bad variable equal to [], []


  for report in reports:


    if report["status"] variable equal to variable equal to "good":


      good append function call with parameters: "report"


    else:


      bad append function call with parameters: "report"


  return good, bad








 Define a function called search function call with parameters: "stat_type, value":


   fetch global variable data_show


  filter_data variable equal to []





  for data in data_show:


    if value in data[stat_type]:


      filter_data append function call with parameters: "data"


  return filter_data








 Define a function called app function call with parameters: "":


   fetch global variable data_show





  local_css function call with parameters: ""assets/style css""


  remote_css function call with parameters: "'https://fonts googleapis com/icon?family variable equal to Material+Icons'"


  st title function call with parameters: "'Show Report'"


  col1, col2 variable equal to st columns function call with parameters: " function call with parameters: "3, 1""


  query variable equal to col1 text_input function call with parameters: """, "Ex location:123 blah street""


  ok variable equal to col2 button function call with parameters: ""OK""


  st text function call with parameters: ""Tags:[title, description, date, location]""





  plt style use function call with parameters: ""dark_background""


  if not ok:


    good, bad variable equal to split_reports function call with parameters: "retrieve_reports function call with parameters: """


    st write function call with parameters: ""Good news: ""


    st dataframe function call with parameters: "good"


    st write function call with parameters: ""Bad news: ""


    st dataframe function call with parameters: "bad"


  else:


    st success function call with parameters: ""Results found""


    if function call with parameters: "query variable equal to variable equal to """:


      good, bad variable equal to split_reports function call with parameters: "retrieve_reports function call with parameters: """


    else:


      stat_type, value variable equal to query split function call with parameters: "":""


      good, bad variable equal to split_reports function call with parameters: "search function call with parameters: "stat_type strip function call with parameters: "", value strip function call with parameters: """"


    st write function call with parameters: ""Good news: ""


    st dataframe function call with parameters: "good"


    st write function call with parameters: ""Bad news: ""


    st dataframe function call with parameters: "bad"


[Github link] https://github.com/DownRamp/Society/blob/main/pages/show_reports.py
## Next Steps: 
1. Add a real cache
2.  better searching

---

