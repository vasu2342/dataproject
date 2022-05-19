import requests
import pandas as pd 
from bs4 import BeautifulSoup

def get_trans(res,identifier):
    #convert the result in to xml format 
    soup = BeautifulSoup(res.text, "xml")
    r=soup.prettify()

    #creating file and write content into that file
    with open('obs_trans_for.xml','w') as ff:
        dt=ff.write(r)
        print(dt)

    #read the content from file using r mode.
    with open('obs_trans_for.xml', 'r') as ffs:
        data11 = ffs.read()

    # get the content as a xmlformat
    bss_data = BeautifulSoup(data11, 'xml')

    #find the elements using particular generic:ObsValue
    bb_unique = bss_data.find_all('generic:ObsValue') 

    #Getting the all values from bb_unique variable
    obssVlaue=[val.get('value') for val in bb_unique]

    #Finding the all obsDimension using generic:ObsDimension
    bdd_unique = bss_data.find_all('generic:ObsDimension')

    #Getting the all obs Dimensions from bdd_unique variable
    obssDimension=[val.get('value') for val in bdd_unique]

    #generatin the id into multiple
    identifier_val=[identifier,]*len(obssVlaue)

    #zipping into onelist from multiple list
    rows=zip(identifier_val,obssDimension,obssVlaue)

    #creating data frame using pandas
    df_1 = pd.DataFrame (rows, columns = ['IDENTIFIER','TIME_PERIOD','OBS_VALUE'])
    return df_1
    
# ident="Q.N.I8.W1.S1.S1.T.A.FA.D.F._Z.EUR._T._X.N"
# res=get_trans(res_api,ident)
# print(res)