# Clothing recommender

---

## Description: 
Recommends clothing based on weather 

---

## Why: 
I usually don't check the weather before leaving my house

---

## Setup Instructions: 
1. python3

---

## To do: 
1. Fetch weather data
2.  add wardrobe
3.  based on weather pick an outfit
4.  store outfits

---

## Code: 
>import requests, json


>import os


>from dotenv import load_dotenv


>from pysondb import db





load_dotenv()





api_key = os.getenv('API_KEY')


base_url = "http://api.openweathermap.org/data/2.5/weather?"





### temp: clothing array


clothing_map = {}


### temp: outfit array


outfits_map = {}





### Fetch weather data based on location


def weather(city_name):


    complete_url = base_url + "appid=" + api_key + "&q=" + city_name


    ### temp and conditions


    response = requests.get(complete_url)


    x = response.json()


    if x["cod"] != "404":


        y = x["main"]





        current_temperature = y["feels_like"]


        current_temperature -= 273.15


        current_pressure = y["pressure"]


        current_humidity = y["humidity"]


        z = x["weather"]


        weather_description = z[0]["description"]


        ### if high humidity add 5 degrees to temp


        print(" Temperature (in celcius unit) = " +


                        str(current_temperature) +


            "\n atmospheric pressure (in hPa unit) = " +


                        str(current_pressure) +


            "\n humidity (in percentage) = " +


                        str(current_humidity) +


            "\n description = " +


                        str(weather_description))





        return str(current_temperature)+"/"+weather_description





### pick clothes based on situation


def clothing_selection(temp_weather):


    ### previous clothing selected


    weather = temp_weather.split("/")


    today_temp = float(weather[0].strip())


    today_weath = weather[1].strip()





    for key, outfit in outfits_map.items():


        inner_list =key.split("/")


        weath_strip = inner_list[1].strip()


        temp_strip = inner_list[0].strip()





        if today_weath in weath_strip:


            print(outfit)


            response = input("Do you like this outfit?(y/n):")


            if response == 'y':


                return ", ".join(outfit)





        ### above temp


        if "+" in temp_strip:


            plus_remove = temp_strip.replace("+","")


            t = float(plus_remove)


            if today_temp >= t:


                print(outfit)


                response = input("Do you like this outfit?(y/n):")


                if response == 'y':


                    return ", ".join(outfit)





        ### check temp range


        elif "-" in inner_list[0].strip():


            ### separate range


            nums = inner_list[0].split("-")


            front = nums[0].replace("!","-")


            front = float(front)


            back = float(nums[1])


            if front <= today_temp and back >= today_temp:


                print(outfit)


                response = input("Do you like this outfit?(y/n):")


                if response == 'y':


                    return ", ".join(outfit)





    fin_outfit = ""


    ### Check our map for clothes fitting description


    for key, clothes in clothing_map.items():


        inner_list = key.split("/")


        weath_strip = inner_list[1].strip()


        temp_strip = inner_list[0].strip()





        if today_weath in weath_strip:


            print(clothes)


            response = input("Add to your outfit?(y/n):")


            if response == 'y':


                fin_outfit = ", ".join(clothes)


                break





        ### above temp


        if "+" in temp_strip:


            plus_remove = temp_strip.replace("+","")


            t = float(plus_remove)


            if today_temp >= t:


                print(clothes)


                response = input("Add to your outfit?(y/n):")


                if response == 'y':


                    fin_outfit = ", ".join(clothes)


                    break





        ### check temp range


        elif "-" in inner_list[0].strip():


            ### separate range


            nums = inner_list[0].split("-")


            front = nums[0].replace("!","-")


            front = float(front)


            back = float(nums[1])


            if front <= today_temp and back >= today_temp:


                print(clothes)


                response = input("Add to your outfit?(y/n):")


                if response == 'y':


                    fin_outfit = ", ".join(clothes)


                    break





    ### save selected


    b = db.getDb("Clothes/outfit.json")


    vals_list = temp_weather.split("/")


    b.add({"temp": vals_list[0].strip(), "weather":vals_list[1].strip(),"outfit": fin_outfit})


    return fin_outfit








### enter clothing selection


def enter_wardrobe():


    clothing = input("Enter Item: ")


    temp_weather = input("Enter temp+weather (separate with /): ")


    clothing_map[temp_weather] = clothing


    b = db.getDb("Clothes/clothes.json")


    vals_list = temp_weather.split("/")


    b.add({"temp": vals_list[0].strip(), "weather":vals_list[1].strip(),"cloth": clothing})





def read_wardrobe():


    cloth = db.getDb("Clothes/clothes.json")


    cloth_list = cloth.getAll()


    ### clothing_map


    for c in cloth_list:


        key = c["temp"]+"/"+c["weather"]


        value = c["cloth"]


        if key in clothing_map:


            clothing_map.get(key).append(value)


        else:


            l = []


            l.append(value)


            clothing_map[key] = l





    ### outfits_map


    outfit = db.getDb("Clothes/outfit.json")


    outfit_list = outfit.getAll()


    for o in outfit_list:


        key = o["temp"]+"/"+o["weather"]


        value = o["outfit"]


        if key in outfits_map:


            outfits_map.get(key).append(value)


        else:


            l = []


            l.append(value)


            outfits_map[key] = l





if __name__ == '__main__':


    read_wardrobe()


    while(True):


        clothes = input("Would you like to add to your wardrobe?(y/n): ")


        if(clothes == 'y'):


            enter_wardrobe()


        else:


            break





    city_name = input("Enter city name: ")


    weather = weather(city_name)


    selection = clothing_selection(weather)


    print("Recommended clothing: "+ selection)



---

## Translation: 
> Import package called requests, json


> Import package called os


>from dotenv Import package called load_dotenv


>from pysondb Import package called db





load_dotenv function call with parameters: ""





api_key variable equal to os getenv function call with parameters: "'API_KEY'"


base_url variable equal to "http://api openweathermap org/data/2 5/weather?"





### temp: clothing array


clothing_map variable equal to {}


### temp: outfit array


outfits_map variable equal to {}





### Fetch weather data based on location


 Define a function called weather function call with parameters: "city_name":


  complete_url variable equal to base_url + "appid variable equal to " + api_key + "&q variable equal to " + city_name


  ### temp and conditions


  response variable equal to requests get function call with parameters: "complete_url"


  x variable equal to response json function call with parameters: ""


  if x["cod"] not variable equal to "404":


    y variable equal to x["main"]





    current_temperature variable equal to y["feels_like"]


    current_temperature subtract from 273 15


    current_pressure variable equal to y["pressure"]


    current_humidity variable equal to y["humidity"]


    z variable equal to x["weather"]


    weather_description variable equal to z[0]["description"]


    ### if high humidity add 5 degrees to temp


    print function call with parameters: "" Temperature function call with parameters: "in celcius unit" variable equal to " +


            str function call with parameters: "current_temperature" +


      "\n atmospheric pressure function call with parameters: "in hPa unit" variable equal to " +


            str function call with parameters: "current_pressure" +


      "\n humidity function call with parameters: "in percentage" variable equal to " +


            str function call with parameters: "current_humidity" +


      "\n description variable equal to " +


            str function call with parameters: "weather_description""





    return str function call with parameters: "current_temperature"+"/"+weather_description





### pick clothes based on situation


 Define a function called clothing_selection function call with parameters: "temp_weather":


  ### previous clothing selected


  weather variable equal to temp_weather split function call with parameters: ""/""


  today_temp variable equal to float function call with parameters: "weather[0] strip function call with parameters: """


  today_weath variable equal to weather[1] strip function call with parameters: ""





  for key, outfit in outfits_map items function call with parameters: "":


    inner_list variable equal to key split function call with parameters: ""/""


    weath_strip variable equal to inner_list[1] strip function call with parameters: ""


    temp_strip variable equal to inner_list[0] strip function call with parameters: ""





    if today_weath in weath_strip:


      print function call with parameters: "outfit"


      response variable equal to input function call with parameters: ""Do you like this outfit? function call with parameters: "y/n":""


      if response variable equal to variable equal to 'y':


        return ", " join function call with parameters: "outfit"





    ### above temp


    if "+" in temp_strip:


      plus_remove variable equal to temp_strip replace function call with parameters: ""+","""


      t variable equal to float function call with parameters: "plus_remove"


      if today_temp greater then or equal to t:


        print function call with parameters: "outfit"


        response variable equal to input function call with parameters: ""Do you like this outfit? function call with parameters: "y/n":""


        if response variable equal to variable equal to 'y':


          return ", " join function call with parameters: "outfit"





    ### check temp range


    elif "-" in inner_list[0] strip function call with parameters: "":


      ### separate range


      nums variable equal to inner_list[0] split function call with parameters: ""-""


      front variable equal to nums[0] replace function call with parameters: "" not ","-""


      front variable equal to float function call with parameters: "front"


      back variable equal to float function call with parameters: "nums[1]"


      if front less then or equal to today_temp and back greater then or equal to today_temp:


        print function call with parameters: "outfit"


        response variable equal to input function call with parameters: ""Do you like this outfit? function call with parameters: "y/n":""


        if response variable equal to variable equal to 'y':


          return ", " join function call with parameters: "outfit"





  fin_outfit variable equal to ""


  ### Check our map for clothes fitting description


  for key, clothes in clothing_map items function call with parameters: "":


    inner_list variable equal to key split function call with parameters: ""/""


    weath_strip variable equal to inner_list[1] strip function call with parameters: ""


    temp_strip variable equal to inner_list[0] strip function call with parameters: ""





    if today_weath in weath_strip:


      print function call with parameters: "clothes"


      response variable equal to input function call with parameters: ""Add to your outfit? function call with parameters: "y/n":""


      if response variable equal to variable equal to 'y':


        fin_outfit variable equal to ", " join function call with parameters: "clothes"


        break





    ### above temp


    if "+" in temp_strip:


      plus_remove variable equal to temp_strip replace function call with parameters: ""+","""


      t variable equal to float function call with parameters: "plus_remove"


      if today_temp greater then or equal to t:


        print function call with parameters: "clothes"


        response variable equal to input function call with parameters: ""Add to your outfit? function call with parameters: "y/n":""


        if response variable equal to variable equal to 'y':


          fin_outfit variable equal to ", " join function call with parameters: "clothes"


          break





    ### check temp range


    elif "-" in inner_list[0] strip function call with parameters: "":


      ### separate range


      nums variable equal to inner_list[0] split function call with parameters: ""-""


      front variable equal to nums[0] replace function call with parameters: "" not ","-""


      front variable equal to float function call with parameters: "front"


      back variable equal to float function call with parameters: "nums[1]"


      if front less then or equal to today_temp and back greater then or equal to today_temp:


        print function call with parameters: "clothes"


        response variable equal to input function call with parameters: ""Add to your outfit? function call with parameters: "y/n":""


        if response variable equal to variable equal to 'y':


          fin_outfit variable equal to ", " join function call with parameters: "clothes"


          break





  ### save selected


  b variable equal to db getDb function call with parameters: ""Clothes/outfit json""


  vals_list variable equal to temp_weather split function call with parameters: ""/""


  b add function call with parameters: "{"temp": vals_list[0] strip function call with parameters: "", "weather":vals_list[1] strip function call with parameters: "","outfit": fin_outfit}"


  return fin_outfit








### enter clothing selection


 Define a function called enter_wardrobe function call with parameters: "":


  clothing variable equal to input function call with parameters: ""Enter Item: ""


  temp_weather variable equal to input function call with parameters: ""Enter temp+weather function call with parameters: "separate with /": ""


  clothing_map[temp_weather] variable equal to clothing


  b variable equal to db getDb function call with parameters: ""Clothes/clothes json""


  vals_list variable equal to temp_weather split function call with parameters: ""/""


  b add function call with parameters: "{"temp": vals_list[0] strip function call with parameters: "", "weather":vals_list[1] strip function call with parameters: "","cloth": clothing}"





 Define a function called read_wardrobe function call with parameters: "":


  cloth variable equal to db getDb function call with parameters: ""Clothes/clothes json""


  cloth_list variable equal to cloth getAll function call with parameters: ""


  ### clothing_map


  for c in cloth_list:


    key variable equal to c["temp"]+"/"+c["weather"]


    value variable equal to c["cloth"]


    if key in clothing_map:


      clothing_map get function call with parameters: "key" append function call with parameters: "value"


    else:


      l variable equal to []


      l append function call with parameters: "value"


      clothing_map[key] variable equal to l





  ### outfits_map


  outfit variable equal to db getDb function call with parameters: ""Clothes/outfit json""


  outfit_list variable equal to outfit getAll function call with parameters: ""


  for o in outfit_list:


    key variable equal to o["temp"]+"/"+o["weather"]


    value variable equal to o["outfit"]


    if key in outfits_map:


      outfits_map get function call with parameters: "key" append function call with parameters: "value"


    else:


      l variable equal to []


      l append function call with parameters: "value"


      outfits_map[key] variable equal to l





if __name__ variable equal to variable equal to '__main__':


  read_wardrobe function call with parameters: ""


  while function call with parameters: "True":


    clothes variable equal to input function call with parameters: ""Would you like to add to your wardrobe? function call with parameters: "y/n": ""


    if function call with parameters: "clothes variable equal to variable equal to 'y'":


      enter_wardrobe function call with parameters: ""


    else:


      break





  city_name variable equal to input function call with parameters: ""Enter city name: ""


  weather variable equal to weather function call with parameters: "city_name"


  selection variable equal to clothing_selection function call with parameters: "weather"


  print function call with parameters: ""Recommended clothing: "+ selection"


[Github link] https://github.com/DownRamp/Life/blob/main/clothing.py
## Next Steps: 
1. Add your clothes
2.  add humidity
3.  add wind chill
4.  umbrella
5.  boots
6.  sunglasses
7.   recommend staying home

---

