import pandas as pd
import os 
import mt940
from os import listdir
import numpy as np
from os.path import isfile, join
import pprint
import json
import re
from random import randint
from tqdm import tqdm
from time import time 

class SWIFT(object):
    def __init__(self, file_dir):
        self.file_dir = file_dir
        self.header= ['transaction_reference','account_no',
                      'statement_number', 'sequence_number', 'open_balance_amount',
                     'open_balance_date', 'currency', 'close_balance_amount', 'close_balance_date']
        
    def split_check(self):
         with open(self.file_dir) as f:
            data = f.read()
            data_split_list = data.split('-}{5')
            data_parse_list = [mt940.parse(i).data for i in data_split_list]
            for a, b in zip(data_split_list, data_parse_list):
                print(a)
                print(b)
    
    def split_account(self, display=False):
        '''Produce account in dictionary'''
        with open(self.file_dir) as f:
            data = f.read()
            
            data_split_list = data.split('-}{5')
            data_parse_list = [mt940.parse(i).data for i in data_split_list]
            
            if display == True:
                print(f'No. of items in splited list: {len(data_split_list)}')
                print(f'No. of item in the parsed list: {len(data_parse_list)}')
            else:
                pass
            
            return data_parse_list
        
    def get_one_data(self,parsed_data, display=False):

        keys = parsed_data.keys()
        
        # account information
        transaction_reference = parsed_data['transaction_reference']
        account_no = parsed_data['account_identification']
        statement_number = parsed_data['statement_number']
        sequence_number = parsed_data['sequence_number']
        
        # opening balance
        open_balance_name = [i for i in keys if 'opening_balance' in i]
        open_balance = parsed_data[open_balance_name[0]]
        open_amount_currency  = open_balance.amount 
        
        open_amount = re.findall(r'<(.*)\s+', str(open_amount_currency))[0]
        open_date = open_balance.date
        
        # closing balance
        close_balance_name = [i for i in keys if 'closing_balance' in i]
        closing_balance = parsed_data[close_balance_name[0]]
        close_amount_currency  = closing_balance.amount
        
        currency = re.findall('\s+(\w+)>', str(close_amount_currency))[0]
        close_amount = re.findall(r'<(.*)\s+', str(close_amount_currency))[0]
        close_date = closing_balance.date
        
       
        if display == True:
            print(f'transaction_reference: {transaction_reference}')
            print(f'account_no: {account_no}')
            print(f'statement_number: {statement_number}')
            print(f'sequence_number: {sequence_number}')
            print(f'open_amount: {open_amount}')
            print(f'open_date: {open_date}')
            print(f'currency: {currency}')
            print(f'close_amount: {close_amount}')
            print(f'close_date: {close_date}')
        else:
            return [transaction_reference,account_no,statement_number, 
                    sequence_number, float(open_amount), str(open_date), currency, float(close_amount), str(close_date)]
        
    def get_clean_df(self,):
        data_list = self.split_account(self.file_dir)
        output_list = []
        for i in data_list:
            if bool(i) == True:
                output_list.append(self.get_one_data(i))
            else: pass
            
        return pd.DataFrame(output_list, columns=self.header)
