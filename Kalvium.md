```python
import requests
from bs4 import BeautifulSoup
import pandas as pd
url = "https://results.eci.gov.in/PcResultGenJune2024/index.htm"
response = requests.get(url)
soup = BeautifulSoup(response.content, 'html.parser')
tables = soup.find_all('table')
for table in tables:
    if "Party Wise Results Status" in table.text:
        break
data = []
for row in table.find_all('tr')[1:]:
    cells = row.find_all('td')
    if len(cells) > 3:
        data.append({
            'Party': cells[0].text.strip(),
            'Won': cells[1].text.strip(),
            'Leading': cells[2].text.strip(),
            'Total': cells[3].text.strip()
        })
df = pd.DataFrame(data)
df.to_csv('lok_sabha_election_results_2024.csv', index=False)
print("Data has been scraped and saved to lok_sabha_election_results_2024.csv")
```

    Data has been scraped and saved to lok_sabha_election_results_2024.csv
    

key insights using pandas



```python
import pandas as pd
df = pd.read_csv('lok_sabha_election_results_2024.csv')
total_seats = df['Total'].astype(int).sum()
top_parties = df.nlargest(10, 'Total')
party_vote_share = df.groupby('Party')['Total'].sum()
print(f"Total seats: {total_seats}")
print("Top 10 parties by seats won:")
print(top_parties)
print("Party vote share:")
print(party_vote_share)
```

    Total seats: 543
    Top 10 parties by seats won:
                                                   Party  Won  Leading  Total
    0                       Bharatiya Janata Party - BJP  240        0    240
    1                     Indian National Congress - INC   99        0     99
    2                               Samajwadi Party - SP   37        0     37
    3                All India Trinamool Congress - AITC   29        0     29
    4                    Dravida Munnetra Kazhagam - DMK   22        0     22
    5                                 Telugu Desam - TDP   16        0     16
    6                       Janata Dal  (United) - JD(U)   12        0     12
    7     Shiv Sena (Uddhav Balasaheb Thackrey) - SHSUBT    9        0      9
    8  Nationalist Congress Party – Sharadchandra Paw...    8        0      8
    9                                    Shiv Sena - SHS    7        0      7
    Party vote share:
    Party
    AJSU Party - AJSUP                                                           1
    Aam Aadmi Party - AAAP                                                       3
    Aazad Samaj Party (Kanshi Ram) - ASPKR                                       1
    All India Majlis-E-Ittehadul Muslimeen - AIMIM                               1
    All India Trinamool Congress - AITC                                         29
    Apna Dal (Soneylal) - ADAL                                                   1
    Asom Gana Parishad - AGP                                                     1
    Bharat Adivasi Party - BHRTADVSIP                                            1
    Bharatiya Janata Party - BJP                                               240
    Communist Party of India  (Marxist) - CPI(M)                                 4
    Communist Party of India  (Marxist-Leninist)  (Liberation) - CPI(ML)(L)      2
    Communist Party of India - CPI                                               2
    Dravida Munnetra Kazhagam - DMK                                             22
    Hindustani Awam Morcha (Secular) - HAMS                                      1
    Independent - IND                                                            7
    Indian National Congress - INC                                              99
    Indian Union Muslim League - IUML                                            3
    Jammu & Kashmir National Conference - JKN                                    2
    Janasena Party - JnP                                                         2
    Janata Dal  (Secular) - JD(S)                                                2
    Janata Dal  (United) - JD(U)                                                12
    Jharkhand Mukti Morcha - JMM                                                 3
    Kerala Congress - KEC                                                        1
    Lok Janshakti Party(Ram Vilas) - LJPRV                                       5
    Marumalarchi Dravida Munnetra Kazhagam - MDMK                                1
    Nationalist Congress Party - NCP                                             1
    Nationalist Congress Party – Sharadchandra Pawar - NCPSP                     8
    Rashtriya Janata Dal - RJD                                                   4
    Rashtriya Lok Dal - RLD                                                      2
    Rashtriya Loktantrik Party - RLTP                                            1
    Revolutionary Socialist Party - RSP                                          1
    Samajwadi Party - SP                                                        37
    Shiromani Akali Dal - SAD                                                    1
    Shiv Sena (Uddhav Balasaheb Thackrey) - SHSUBT                               9
    Shiv Sena - SHS                                                              7
    Sikkim Krantikari Morcha - SKM                                               1
    Telugu Desam - TDP                                                          16
    United People’s Party, Liberal - UPPL                                        1
    Viduthalai Chiruthaigal Katchi - VCK                                         2
    Voice of the People Party - VOTPP                                            1
    Yuvajana Sramika Rythu Congress Party - YSRCP                                4
    Zoram People’s Movement - ZPM                                                1
    Name: Total, dtype: int64
    
