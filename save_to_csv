import requests
import re
import pandas as pd

headers = {
    'user-agent':
    'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/105.0.0.0 Safari/537.36'
}

url = 'https://tendencias.mercadolivre.com.br/'

r = requests.get(url, headers=headers)

result = re.findall(r'"name":"(.*?)","url"',r.text)
keyword = re.findall(r'"keyword":"(.*?)","url"',r.text)

df = pd.DataFrame(keyword)
headers = ['Keywords']
df.columns = headers
df.to_csv('mercadolivre_keywords.csv')

print('saved to csv file')
