import unittest
import requests
import pandas as pd 
from bs4 import BeautifulSoup


def get_exchange_rate(response_API):
    #converting to xml format from string using bs4

    soup = BeautifulSoup(response_API.text, "xml")
    r=soup.prettify()

    #Dumping data into file with in obs.xml
    with open('obs.xml','w') as f:
        data=f.write(r)
        print(data)

    # reading data form file
    with open('obs.xml', 'r') as fs:
        data1 = fs.read()

    # get the content as a xmlformat
    bs_data = BeautifulSoup(data1, 'xml')

    #find the elements using particular generic:ObsValue

    b_unique = bs_data.find_all('generic:ObsValue') 

    #Getting the all values from bb_unique variable
    obsVlaue=[val.get('value') for val in b_unique]

    #Finding the all obsDimension using generic:ObsDimension
    bd_unique = bs_data.find_all('generic:ObsDimension')

    #Getting the all obs Dimensions from bdd_unique variable
    obsDimension=[val.get('value') for val in bd_unique]

    
    # making data frmaes using pandas
    rows=zip(obsDimension,obsVlaue)
    df = pd.DataFrame (rows, columns = ['TIME_PERIOD','OBS_VALUE'])
    return df



response_api=requests.get("https://sdw-wsrest.ecb.europa.eu/service/data/BP6/M.N.I8.W1.S1.S1.T.N.FA.F.F7.T.EUR._T.T.N?")
def get_raw_data(identifier):
    soup_1 = BeautifulSoup(response_api.text, "xml")
    r_1=soup_1.prettify()
    #Dumping data into file with in obs.xml
    with open('obs_1.xml','w') as ff:
        data_1=ff.write(r_1)
        print(data_1)
    # reading data form file
    with open('obs_1.xml', 'r') as ffs:
        data11 = ffs.read()
    bss_data = BeautifulSoup(data11, 'xml')
    #finding the values from xml
    bb_unique = bss_data.find_all('generic:ObsValue') 
    obssVlaue=[val.get('value') for val in bb_unique]
    bdd_unique = bss_data.find_all('generic:ObsDimension')
    obssDimension=[val.get('value') for val in bdd_unique]
    # making data frmaes using pandas
    rows=zip(obssDimension,obssVlaue)
    df_1 = pd.DataFrame (rows, columns = ['TIME_PERIOD','OBS_VALUE'])
    return df_1

# class TestExchng(unittest.TestCase):
#     def test_get_exchg(self):
#         response_API = requests.get('https://sdw-wsrest.ecb.europa.eu/service/data/EXR/M.GBP.EUR.SP00.A?')
#         res=get_exchange_rate(response_API)
#         return res[('TIME_PERIOD')]!=None
        
#     def test_get_raw(self):
#         response_api=requests.get("https://sdw-wsrest.ecb.europa.eu/service/data/BP6/M.N.I8.W1.S1.S1.T.N.FA.F.F7.T.EUR._T.T.N?")
#         res_1=get_raw_data(response_api)
#         return res_1[('TIME_PERIOD')]!=None

# if __name__ == '__main__':
#     unittest.main()
