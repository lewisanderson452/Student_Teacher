# Student_Teacher

"""
    Imports: convert.py  (Custom parquet-to-csv convertor)
    Dependancies: json
    
    Procedure:
        Choose whether to convert file 
        Enter name of students csv file
        Enter name of teachers csv file
        Enter name of output json file
        Data is processed and stored in output json file
        
"""

from convert import convert
import json

# Function to parse teacher data to select certain teacher for student
def get_teacher_data(id,data):
    for x in data:
        if x[-1] == id:
            res = {}
            res["ID"] = x[0]
            res["First Name"] = x[1]
            res["Last Name"] = x[2]
            res["Email"] = x[3]
            res["SSN"] = x[4]
            res["Address"] = x[5]
            return res
    
    return 0 # Not Found
            
            
# Function get students data from file
def student_file(name):
    try:
        with open(f"{name}", "r") as file:
            students = file.read().strip().split("\n")[1:]
            students = [x.split("_") for x in students]
            return students
    except Exception as e:
        print(e)
        return 0
   
   
   # Function to get teachers data from file
def teacher_file(name):
    try:
        with open(f"{name}", "r") as file:
            teachers = file.read().split("\n")[1:]
            teachers = [x.split(",") for x in teachers]
            return teachers
    except Exception as e:
        print(e)
        return 0
    
    
    # Function to process student and techers data into json
def process_data(students,teachers):
    output = {}
    try:
        for x in students:
            data = {}
            data["First Name"] = x[1]
            data["Last Name"] = x[2]
            data["Email"] = x[3]
            data["SSN"] = x[4]
            data["Address"] = x[5]
            data["CID"] = x[6]
            data["Teacher"] = get_teacher_data(x[6], teachers)
            output[x[0]] = data
        return output
    except Exception as e:
        print(e)
        print("Data not complete")
        return 0

# Opening JSON file
with open('Student-Teacher.json','r') as S:
    data = json.load(S)
data
