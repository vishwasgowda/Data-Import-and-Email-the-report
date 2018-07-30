# Data-Import-and-Email-the-report
This code queries your data and send a report/ table in HTML format to your email at specified time interval.

import cx_Oracle
import time
import numpy as np
import smtplib
from pandas import *
import pandas as pd
from email.mime.multipart import MIMEMultipart
from email.mime.text import MIMEText
from email.mime.image import MIMEImage
from array import array
from IPython.display import HTML

#db connection
connection = cx_Oracle.connect("")
 
#email function   
def sendHtmlMail(df_html):
      email_subject = 'Today Stock Price'
      email_from = "xyz@gaaaagle.com"
      email_to = ['TO YOUR EMAIL', 
                  'EMAIL',
                  'EMAIL']

      msg = MIMEMultipart()
      message="Hello All,\nPlease find the list of today LEE holds.\nHave a Good day"
      msg['To'] = ", ".join(email_to)
      #msg['CC'] = email_cc
      msg['Subject'] = email_subject
     #add in the message body
      msg.attach(MIMEText(message, 'plain'))
      msg.attach(MIMEText(df_html, 'html')) 
      server = smtplib.SMTP('')
      toaddrs = [email_to]
      server.sendmail(email_from, email_to, msg.as_string())
      server.quit()
      print ("successfully sent email to %s:" % (msg['To']))
    
    
while True: 
#SQL Query
    query= """YOUR QUERY Here"""

    pd.set_option('display.max_colwidth', -1)
    #pd.set_option('max_colwidth', 800)
    df=pd.read_sql(query, con=connection)
    
    df_html= df.to_html(index= False)
    HTML(df_html)
    sendHtmlMail(df_html)
    time.sleep(1800)
