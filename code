#!/usr/bin/env python3
# -*- coding: utf-8 -*-
"""
Created on Mon Nov 29 23:03 2021

"""

import random
import pandas as pd
import matplotlib.pyplot as plt

year = 450
totalPopulation = 50
startPopulation=50
growthFactor = 1.05

infantMortality= 20
disasterChance = 10 #Every 10 years, disaster will come

foodProduce = 1.5 #1 person creates food for 1.5 people
foodStock = 0 

dayCount = 0 #Every 1 months the population is reported

#ages between which women can get pregnant
fertilityx= 16
fertilityy= 35


#list of citizens of island
peopleDictionary = []
class Person:
    def __init__(self, age):
        self.gender= random.randint(0,1)
        self.age=age
        self.pregnant=0

"""      
while totalPopulation < 1000:
    totalPopulation *= growthFactor
    #Every 30th day, population is reported
    dayCount += 1
    if dayCount == 30: 
        dayCount = 0
        print(int(totalPopulation))
"""       
#harvesting food
def harvest(foodStock, foodProduce):
    
    ablePeople = 0
    for person in peopleDictionary:
        if person.age > 10:
            ablePeople +=1 #people working in field
    
    #creating food       
    foodStock += ablePeople * foodProduce
    
    #when population is more than food stock, people die due to famine 
    if foodStock < len(peopleDictionary): 
        died_by_starvation = 0
        died_by_starvation = len(peopleDictionary)-foodStock
        print(died_by_starvation, "peope died due to starvation")
        
        del peopleDictionary[0:int(len(peopleDictionary)-foodStock)] 
        foodStock = 0
        
    #remaining food is stored for next year
    else:
        foodStock -= len(peopleDictionary)
    
    print("Current food reserves: ",foodStock)

#reproducing        
def reproduce(fertilityx, fertilityy):
    baby=0
    for person in peopleDictionary:
        if person.gender == 1: 
            if person.age > fertilityx and person.age < fertilityy: #women is in reproducible age
                    if random.randint(0,3)==1: #33% chance of getting pregnant
                        if random.randint(0,100)>infantMortality:
                            peopleDictionary.append(Person(0))
                            baby+=1
    print(baby, " baby/babies were born this year.")

#printing demographic information
def demoInfo():
    
    global year 
    year += 1
    print("The year is", year)

    #calculating male and female population
    male, female=0,0
    maleAge = []
    femaleAge = []
    for person in peopleDictionary:
        if person.gender == 0:
            male += 1
            maleAge.append(person.age)
        else:
            female += 1
            femaleAge.append(person.age)
    print("The current population is : ", len(peopleDictionary), "\t[M=", male, "\tF=", female, "]")
    
    ageBins = [0,5,10,15,20,25,30,35,40,45,50,55,60]
    
    #sorting male population ages into class intervals
    maleAge_df= pd.DataFrame({'Age' : maleAge})
    maleAge_df['AgeGroup'] = pd.cut(maleAge_df['Age'], ageBins)
    maleAge_sorted = pd.cut(maleAge_df['Age'], bins=ageBins).value_counts().sort_values().sort_index()
    
    #sorting female population ages into class intervals
    femaleAge_df= pd.DataFrame({'Age' : femaleAge})
    femaleAge_df['AgeGroup'] = pd.cut(femaleAge_df['Age'], ageBins)
    femaleAge_sorted = pd.cut(femaleAge_df['Age'], bins=ageBins).value_counts().sort_values().sort_index()
    
    #creating data frame for population pyramid 
    a= {'Age': ['0-4','5-9','10-14','15-19','20-24','25-29','30-34','35-39','40-45','46-49','50-54','55-59','60+'],
                    'Male': maleAge_sorted, 
                    'Female': femaleAge_sorted}
    df = pd.DataFrame.from_dict(a, orient='index').fillna(0)
    df=df.transpose()
    
    #print(df)
    
    #PLOTTHING THE GRAPH
    
    #define x and y limits
    y = range(0, len(df))
    x_male = df['Male'].tolist()
    x_female = df['Female'].tolist()
    
    #define plot parameters
    fig, axes = plt.subplots(ncols=2, sharey=True, figsize=(9, 6))
    
    #specify background color and plot title
    fig.patch.set_facecolor('xkcd:light grey')
    plt.figtext(.5,.9,"Population Pyramid ", fontsize=15, ha='center')
        
    #define male and female bars
    axes[0].barh(y, x_male, align='center', color='royalblue')
    axes[0].set(title='Males')
    axes[1].barh(y, x_female, align='center', color='lightpink')
    axes[1].set(title='Females')
    
    
    #adjust grid parameters and specify labels for y-axis
    axes[1].grid()
    axes[0].set(yticks=y, yticklabels=df['Age'])
    axes[0].invert_xaxis()
    axes[0].grid()
    
    #display plot
    plt.show()

  
# spawning our first tribe members
def beginSim():
    for x in range(startPopulation):
        peopleDictionary.append(Person(random.randint(16,50)))
    demoInfo()

#normal course of year
def runYear(foodStock, foodProduce, fertilityx, fertilityy, disasterChance, infantMortality):
    
   
    harvest(foodStock, foodProduce) #harvesting food
    reproduce(fertilityx, fertilityy) #creating babies
    
    #aging people
    died_of_oldage = 0
    for person in peopleDictionary:
        if person.age > 60:         #killing people
            peopleDictionary.remove(person)
            died_of_oldage += 1
        else:       #aging people
            person.age +=1
    print(died_of_oldage, "people died of old age this year.")
        
    
    if random.randint(0,100)<disasterChance:
        died_by_disaster = int(random.uniform(0.05,0.10)*len(peopleDictionary))
        del peopleDictionary[0:died_by_disaster]
        print(died_by_disaster, "are dead due to natural disasters")

    demoInfo()

#simulation
while len(peopleDictionary) >= 0:

    if year == 450:
        beginSim()
        
    else:
        user_input = input("Press Enter key to continue\n")
        if user_input == '':
            runYear(foodStock, foodProduce, fertilityx, fertilityy, infantMortality, disasterChance)
