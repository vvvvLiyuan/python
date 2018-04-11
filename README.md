# python
def read_data(filename):
    data = {}
    new_data = {}
    with open(filename) as csvfile:
        reader = csv.DictReader(csvfile)
        for row in reader:
            ID = row["ID"]
            del row["ID"]
            for key in row:
                if not row[key]:
                    row[key] = None
            data[ID]=row
            new_data[ID] = dict(list(row.items()))
    return new_data

#get rid of null, zero and cast year stats into strings
def clean_data():
#new data that has been cleaned
    new_data = {}
    #the number of data that need to be cleaned
    clean_sum = 0
    #loop all the row
    for i,value in data.items():

        new_data[i] = {}
        for col,d in value.items():
            #check the value is zero or null and clean them into '0'
            if d == 'zero' or d == 'null':
                new_data[i][col] = '0'
                clean_sum += 1
            else:
                new_data[i][col] = d
    return new_data,clean_sum
                
                
#sum the number of the key crime
def countCrimes(data, key):

    #the sum of the crime
    crimes_sum = 0
    #loop all the row
    for i,value in data.items():
        if key in value:
        #check the key in the value
            crimes_sum += int(value[key])
    return crimes_sum   

#summates the values of each Subdivision of each year 
def worstCrime(data):

    #the sum result of crime in each subdivion 
    result = {}
    except_col = ['ID', 'Statistical Division or Subdivision', 'LGA', 'Offence category', 'Subcategory']
    #loop all the row
    for i,value in data.items():
        if value["Statistical Division or Subdivision"] not in result:
            result[value["Statistical Division or Subdivision"]] = 0
        for key in value.keys():
            #add all the year static
            if key not in except_col:
            #save the year col data
                result[value["Statistical Division or Subdivision"]] += int(value[key])
    return result      


def mostActive(data):
    
    result = {}
    except_col = ['ID', 'Statistical Division or Subdivision', 'LGA', 'Offence category', 'Subcategory']
    #loop all the row
    for i,value in data.items():
        if value["Offence category"] not in result:
            result[value["Offence category"]] = 0
        for key in value.keys():
            #add all the year static
            if key not in except_col:
            #save the year col data
                result[value["Offence category"]] += int(value[key])
    return result  
    
def main(datafile):
    ## change this filename variable to point to the location of the csv file
    ## you are wishing to work with.  Use the small set (supplied on LMS) to
    ## test your functions.  Then move on to the large sets, when you are happy
    
    
    ## read_data returns a dictionary of dictionaries.  
    data = read_data(datafile) 

    ## Perform your functions calls here
    data,clean_sum = clean_data(data)
    worst_c = worstCrime(data)
    type_of_crime = mostActive(data)

    most_crime = 0
    most_crime_area = None
    #calc the worst area from the value from the worstCrime
    for k,v in worst_c.items():

        if v >= most_crime:
            most_crime = v
            most_crime_area = k
            
    most_crime = 0
    most_crime_offence = None
    #calc the worst offence from the value from the mostActive
    for k,v in type_of_crime.items():

        if v >= most_crime:
            most_crime = v
            most_crime_offence = k
    #show the report!         
    print("The total number of rows in the data file: %d"%(len(data)))
    print("The total number of Subdivisions examined in the data: %d"%(len(worst_c)))
    print("The total number of Offence Categories: %d"%(len(type_of_crime)))
    print("The worst area for crime: %s"%(most_crime_area))
    print("The most active type of crime: %s"%(most_crime_offence))
    print("On behalf of the MUC (Made Up Company), I, Liyuan Zhu, have analysed {} units of the crime statistics data. \
          This data covered {} Subdivisions and found {} types of crimes.  \
          I conclude that the worst area for crime is {}, and that the most active category of \
          crime is {}".format(len(data),len(worst_c),len(type_of_crime),most_crime_area,most_crime_offence))
filename ='crimeDataSetDirty.csv'
main(filename)





