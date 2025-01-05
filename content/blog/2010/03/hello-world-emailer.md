---
title: "Hello World Emailer"
date: 2010-03-25
draft: false
tags: ["IBM", "python", "testing"]
---
I thought that this would be worth posting, I've been in need of some
Jython code that would help me send out emails with 1 to *n* files
attached to the message. I puttered around my books and a few websites
but couldn't find exactly what I needed. A few failed attempts later
I pieced together some working code. Several times I swear some of the
Python examples just didn't work with Jython, the key was figuring out
which libraries to import.

Hopefully this post will find its way into the hands of another Jython
(or Python) aficionado that has a similar problem that they are trying
to solve.

```python
#!/usr/bin/env pythonimport smtplib, string, os
from email.MIMEMultipart import MIMEMultipart
from email.MIMEText import MIMEText
from email.MIMEBase import MIMEBase
from email.Encoders import encode_base64class Emailer():

'''Creates a simple object to interact with to send out mass emails'''    
def __init__(self):
    self.__server = smtplib.SMTP()    def send_email(self, from_address, to_addresses,
                          subject, message, files=[]):
    '''
        from_address - a string, the email for the from field.
        to_addrs - a list object containing the email address to send
                    the message to. Ex. ['thedude@freecog.com']
        subject - a short subject line for the email.
        message - the message to send to the recipients.
        files - a list of paths to the files to attache to the email.
    '''
    self.__construct_email(from_address, to_addresses,
                                      subject, message, files)
    self.__server.connect()
    self.__server.sendmail( from_address, to_addresses,
                                          self.msg.as_string() )
    self.__server.quit()    def __construct_email(self, from_address, to_addresses,
                                          subject, message, files):
    self.msg = MIMEMultipart()
    self.msg['subject'] = subject
    self.msg['From'] = from_address
    self.msg['To'] = string.join(to_addresses).replace(' ',',')        
    self.msg.attach( MIMEText(message) )        
    
    for file in files:
        part = MIMEBase('application', 'octet-stream')
        part.set_payload( open(file, 'rb').read() )
        encode_base64( part )
        part.add_header('Content-Disposition', 'attachment; filename="%s"'
                        % os.path.basename(file) )
    self.msg.attach( part )

if __name__ == '__main__':
    from_addr = 'autobot@localhost'
    to_addrs = ['matt@freecog.com']
    subject = 'see matt run'
    msg = 'run matt run'
    files = ['auto-what.txt']
    email_driver = Emailer()
    email_driver.send_email(from_addr, to_addrs, subject, msg, files)
```