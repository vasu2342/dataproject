import unittest
from data_evaluation import get_exchange_rate,get_raw_data
from transaction_excercise import get_transactions
from trans_formula import get_trans
import requests

class TestExchng(unittest.TestCase):
    def test_get_exchg(self):
        """ Getting the data from api and passed the parameter to function
        (get_exchange_rate) ,called the function and stored in a variable,validated the result(value)
        using assert function
        """
        response_API = requests.get('https://sdw-wsrest.ecb.europa.eu/service/data/EXR/M.GBP.EUR.SP00.A?')
        res=get_exchange_rate(response_API)
        print('data frames are passed')
        assert res.shape[0]>1,'tests failed'
        
    def test_get_raw(self):
        """ Getting the data from api and passed the parameter to function
        (get_raw_data) ,called the function and stored in a variable,validated the result(value)
        using assert function
        """
        response_api=requests.get("https://sdw-wsrest.ecb.europa.eu/service/data/BP6/M.N.I8.W1.S1.S1.T.N.FA.F.F7.T.EUR._T.T.N?")
        res_1=get_raw_data(response_api)
        print("data frames are passed ")
        assert res_1.shape[0]>1,'test failed'

    def test_get_transactions(self):
        """ Getting the data from api and passed the parameter to function
        (get_transactions) ,called the function and stored in a variable,validated the result(value)
        using assert function
        """
        res_api=requests.get("https://sdw-wsrest.ecb.europa.eu/service/data/BSI/Q.HR.N.A.A20.A.1.AT.2000.Z01.E?")
        ident="Q.DE.N.A.A20.A.1.AT.2000.Z01.E"
        res=get_transactions(res_api,ident)
        print("data frames are passed")
        assert res.shape[0]>1,'test failed'
    def test_get_trans(self):
        """ Getting the data from api and passed the parameter to function
        (get_trans) ,called the function and stored in a variable,validated the result(value)
        using assert function
        """
        api1=requests.get("https://sdw-wsrest.ecb.europa.eu/service/data/BP6/Q.N.I8.W1.S1.S1.T.A.FA.D.F._Z.EUR._T._X.N?")
        ident="Q.N.I8.W1.S1.S1.T.A.FA.D.F._Z.EUR._T._X.N"
        res1=get_trans(api1,ident)
        print("data frames are passed")
        assert res1.shape[0]>1,'test failed'
        

if __name__ == '__main__':
    unittest.main()

    
