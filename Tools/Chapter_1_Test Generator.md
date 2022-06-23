# Test Generator

---

## Description: 
Generate test format for tests that need to be made for your python script

---

## Why: 
Saves a bit of time typing them out and thinking about what tests you should make

---

## Setup Instructions: 
1. Python3

---

## To do: 
1. Generate class
2.  generate tests based on functions
3.  fill in basic pass fail case

---

## Code: 



code = []


>code.append("import unittest")


path = "chapter_maker.py"


test_path = "test_chapter_maker.py"





### go through a file with pathway


f = open(path, 'r+')


for line in f.readlines():


    if "class" in line:


        line = line.replace("(", "Test(unittest.TestCase")


        code.append(line.replace("\n", ""))


    elif "def" in line:


        line = line.replace("def ", "def test_")


        code.append(line.replace("\n", ""))


        code.append("        self.assertEqual(True, True)\n")


f.close()





code.append("if __name__ == '__main__':")

code.append("    unittest.main()")


with open(test_path, 'w') as f:

    for item in code:

        f.write("%s\n" % item)

---

## Translation: 



code variable equal to []


>code append function call with parameters: "" Import package called unittest""


path variable equal to "chapter_maker py"


test_path variable equal to "test_chapter_maker py"





### go through a file with pathway


f variable equal to open function call with parameters: "path, 'r+'"


for line in f readlines function call with parameters: "":


  if "class" in line:


    line variable equal to line replace function call with parameters: "" function call with parameters: "", "Test function call with parameters: "unittest TestCase""


    code append function call with parameters: "line replace function call with parameters: ""\n", """"


  elif " Define a function called " in line:


    line variable equal to line replace function call with parameters: "" Define a function called ", " Define a function called test_""


    code append function call with parameters: "line replace function call with parameters: ""\n", """"


    code append function call with parameters: ""    self assertEqual function call with parameters: "True, True"\n""


f close function call with parameters: ""





code append function call with parameters: ""if __name__ variable equal to variable equal to '__main__':""


code append function call with parameters: ""  unittest main function call with parameters: """"





with open function call with parameters: "test_path, 'w'" as f:


  for item in code:


    f write function call with parameters: ""%s\n" % item"


[Github link] https://github.com/DownRamp/Tools/blob/main/useful/test_gen.py
## Next Steps: 
1. Add extra details for pass cases
2.  change languages

---

