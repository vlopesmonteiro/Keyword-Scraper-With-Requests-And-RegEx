import gspread
from gspread_dataframe import set_with_dataframe
from google.oauth2.service_account import Credentials
from pydrive.auth import GoogleAuth
from pydrive.drive import GoogleDrive
import requests
import re
import pandas as pd


headers = {
    'user-agent':
    'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/105.0.0.0 Safari/537.36'
}

url = 'https://tendencias.mercadolivre.com.br/'


r = requests.get(url, headers=headers)

scopes = ['https://www.googleapis.com/auth/spreadsheets',
          'https://www.googleapis.com/auth/drive']

credentials = {
  "type": "service_account",
  "project_id": "gspreadscraper-363709",
  "private_key_id": "eb5e78f2127e29a169222969b127a088a16a8b3e",
  "private_key": "-----BEGIN PRIVATE KEY-----\nMIIEvwIBADANBgkqhkiG9w0BAQEFAASCBKkwggSlAgEAAoIBAQDoxG4moTejY+Z3\nWyb1H6SI+apngoR5723mDEb4kMuIaErMAu/mtKyb1N4bVZOwalhxeQbR12cXiiNq\n13TMQyU/MDbngQYXNHY33PfeUiytzMXsyQJIKA6kJC+9Gy0pMXnIGNvBGdLZUn4p\nLnYu4rYBSmVGpWz4dAzRGfh/htK5aSSoSiFm7ZJhfaHHaVY7mr5FztjrEfrwUlCi\nJIkdSkAZmgd1FV3F9dpiR4J4oZUWbXfbb7z/g7KxAqzQlQ/kPzt7nFhHMLPK5jSX\nyAWVwDsw6oZgUZtdNLPI+xjAe9HRf1DgLkQH5eslQtUx+LSiS96BkyrsrZy+fDZp\nbbvIpeHzAgMBAAECggEARMp8xVTjFRo6q+6X+lyiK0sh9dpoXYMJxzG8rcxZIpSz\n+kwp61RdKObFx21IqxwazFpYzh5rXNUZ94L6hT2Y3e5ZY5zJmIUMJSFcbet6Qdkh\na4PSdVHFVfRN2YUVGTYCiET0eUKxAkIzf+c3zU56PLJocPMF2/2sJgYXX1UQpPOi\nFXxmUBBnEys0bmzBA/ta9IqNKjPpjv79Lz5Qn7PA+zqbgRG51fWQE/tafH7ws2jT\ns9zyb1UrAhdRmTFLQagh5erYFQtPMJNHfranEK8Hn0PUJ6RshhuC0DO/SXXBMWBY\njv7RkGNU9K2mcFUr+Ax9LPThqibHokcc3NHTBMkVrQKBgQD+X/5R9INOFqkYeQgy\nZqVIjwfgR5gvRUSRC0SBhTszoIVhq5WVHYf2N77cAg1iGu+HzYwPHl9mauBiXxZH\neu/xRBLjnN69eBD+/4bH1BCXkNJn1DEcsaxlu/gm5lsoMzuRVrPlnx/zcf2mnzzh\nvnbAoM4zyDHH1z3ryhu3/dkdxQKBgQDqQRl5ipSFUGXLJzotEVL5KxpZn8EKSVm7\ng+2V9tyb406mGGq1xJViWfpzsoihoiEQijrAR+luFkvMHlVLV8xYU2cJrCVFc1/U\n9M6rb6vXfH50o426L/MrCQhDza0iaOR8Lv5lX0BokB+6ozUnq29GL1vizqT65VEj\nNzs68ln0VwKBgQCWcV+QSPR/cpd/idV3OY33Y+BcnPs8udLblbZmg82HsyAvq7NE\nBbsru7x9khkoNJYF02NKbcQuZndetq4OiH7wSjqBs31owWIL7kRgWuOVQGmwTqbd\nOZekc49IMmUnWWWZh5XN8FaNPJWvve3b9TF2q6RIq9YFQx+0HExYfSGzoQKBgQDg\nlelGjYY3Cg2N593ut11FZf2tT2xT4F8XWDTAQhzfl94ff+lOu1o9IObtZY146Wep\n49zP3CIAWX/yAmLkCRjw1YVD0LcrDqIiGVLLKhUmU0UprmSCzNXlvJMf7mC/TLFC\nHgvoRJoLpaHF3hNoJQRZ1a3SlSu3H4Par2kp6pAzoQKBgQD3od3RgQyoJsjYWItY\nTrpjnJQawngKTQZvIWS19HS91MMuom2npW7h6p6vSIFgWmuu8mwdXu/A88Rn3nAy\nhsDu+GPfBdDnkPMHLyXlNlUr43XlbV0W0FTSGhc4G29FjfDzbWamEb8sI7xVWvJo\nJW8LyaApiV7lg809wKmYVzCQPQ==\n-----END PRIVATE KEY-----\n",
  "client_email": "test-674@gspreadscraper-363709.iam.gserviceaccount.com",
  "client_id": "106998174361119533519",
  "auth_uri": "https://accounts.google.com/o/oauth2/auth",
  "token_uri": "https://oauth2.googleapis.com/token",
  "auth_provider_x509_cert_url": "https://www.googleapis.com/oauth2/v1/certs",
  "client_x509_cert_url": "https://www.googleapis.com/robot/v1/metadata/x509/test-674%40gspreadscraper-363709.iam.gserviceaccount.com"
}

gc = gspread.service_account_from_dict(credentials)

gauth = GoogleAuth()
drive = GoogleDrive(gauth)

# open a google sheet
gs = gc.open_by_url('https://docs.google.com/spreadsheets/d/1pp76lAdscCe59CXc1bC2ZrzEgELdbVvVKDV3r2bphBw/edit#gid=0')

# select a work sheet from its name
worksheet1 = gs.worksheet('BR')

# Get the competitor popular/trending keyword
result = re.findall(r'"name":"(.*?)","url"',r.text)
keyword = re.findall(r'"keyword":"(.*?)","url"',r.text)

# Store keyword in a dataframe and append to google sheets
df = pd.DataFrame(keyword)
headers = ['Keywords']
df.columns = headers
df_values = df.values.tolist()
gs.values_append('BR', {'valueInputOption': 'RAW'}, {'values': df_values})
