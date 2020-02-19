# ParserHH
0. Установка python, pandas, requests, jupyter-notebook

1. Создание парсера

import requests
import pandas as pd



number_of_pages = 100
#number_of_ads = number_of_pages * per_page
for i in range(number_of_pages):
	url = 'https://api.hh.ru/vacancies'
	par = {'text': job, 'area':'27','per_page':'10', 'page':i}
	r = requests.get(url, params=par)
	e=r.json()
	data.append(e)
	vacancy_details = data[0]['items'][0].keys()
	df = pd.DataFrame(columns= list(vacancy_details))
	ind = 0
	for i in range(len(data)):
	    	for j in range(len(data[i]['items'])):
	    	f.loc[ind] = data[i]['items'][j]
	    	ind+=1
csv_name = job".csv"
df.to_csv(csv_name)


В итоге мы получили файл job.csv

2. Загрузка файла:
Список команд:
import pandas as pd

df = pd.read_csv("job.csv")


df.rename(colums={'Unnamed: 0':'index'}, inplace=True)
df.set_index('index', inplace=True)


df = head()

3. Получение средней зарплаты

3.1 Разделение ЗП на валюты

import ast # run code from string for example ast.literal_eval("1+1") 

salaries = df.salary.dropna() # remove all NA's from dataframe
currencies = [ast.literal_eval(salaries.iloc[i])['currency'] for i in range(len(salaries))]
curr = set(currencies) #{'EUR', 'RUR', 'USD'}

#divide dataframe salararies by currency
rur = [ast.literal_eval(salaries.iloc[i]) for i in range(len(salaries)) if ast.literal_eval(salaries.iloc[i])['currency']=='RUR']
eur = [ast.literal_eval(salaries.iloc[i]) for i in range(len(salaries)) if ast.literal_eval(salaries.iloc[i])['currency']=='EUR']
usd = [ast.literal_eval(salaries.iloc[i]) for i in range(len(salaries)) if ast.literal_eval(salaries.iloc[i])['currency']=='USD']

3.2 Средняя зп в рублях

fr = [x['from'] for x in rur] # lower range of salary
fr = list(filter(lambda x: x is not None, fr)) # remove NA's from lower range [0, 100, 200,...]

to = [x['to'] for x in rur] #upper range of salary
to = list(filter(lambda x: x is not None, to)) #remove NA's from upper range [100, 200, 300,...]

import numpy as np
salary_range = list(zip(fr, to)) # concatenate upper and lower range  [(0,100), (100, 200), (200, 300)...]
av = map(np.mean, salary_range) # convert [(0,100), (100, 200), (200, 300)...] to [50, 150, 250,...]
av = round(np.mean(list(av)),1) # average value from [50, 150, 250,...]

print("average salary as Data Scientist ", av, "rubles")

