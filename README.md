# zmail

[![PyPI](https://img.shields.io/pypi/v/yagmail.svg?style=flat-square)]()
[![platform](https://img.shields.io/badge/python-3.5-green.svg)]()
[![license](https://img.shields.io/github/license/mashape/apistatus.svg?style=flat-square)]()

Zmail allows you to send and get emails as possible as it can be in python.There is no need to check server address or make your own MIME string.With zmail, you only need to care about your mail content.

## Installation 

Zmail only running in python3 without third-party modules required. **Do not support python2**.

### Option 1:Install via pip（Better）

```
$ pip3 install zmail
```

or

```
$ pip install zmail
```

If that means your pip is also working for python3.

### Option 2:Download from Github

You can download the master branch of zmail,unzip it ,and do

```
$ python3 setup.py install
```

## Features

- Automatic looks for server address and it's port.
- Automatic converts a python dictionary to MIME object(with attachments).
- Automatic add mail header and local name to avoid server reject your mail.
- Easily custom your mail header.
- Only require python >= 3.5 , you can embed it in your project without other module required.

## Usage

Before using it, please ensure:

- Using python3
- Open SMTP/POP3 function in your mail (For **@163.com** and **@gmail.com** you need to set your app private password)

Then, all you need to do is just import zmail.

## Examples

### Send your mail

```python
import zmail
mail = {
    'subject': 'Success!',  # Anything you want.
    'content': 'This message from zmail!',  # Anything you want.
    'attachments': '/Users/zyh/Documents/example.zip',  # Absolute path will be better.
}

server = zmail.server('yourmail@example.com, 'yourpassword')

server.send_mail('yourfriend@example.com', mail)
```

### Retrieve your mail

- Get the latest mail.

```python
import zmail

server = zmail.server('yourmail@example.com, 'yourpassword')

mail = server.get_latest()
```

- Retrieve mail by its id.

```python
mail = server.get_mail(2)
```

- Get all mails' info, include each mail's header.A list of dictionary, each dictionary include all headers can be extracted.

```
mail_info = server.get_info()
```

- Get mailbox info. 

```
mailbox_info = server.stat()
```

The result is a tuple of 2 integers: `(message count, mailbox size)`.

### Parse your mail

Show you mail, use

```
import zmail
server = zmail.server('yourmail@example.com, 'yourpassword')
mail = server.get_latest()
zmail.show(mail)
```

Output, example :

```
content-type multipart/mixed
subject Success!
to zmail_user
from zmail<zmail@126.com>
date 2018-2-3 01:42:29 +0800
boundary ===============9196441298519098157==
content ['This message from zmail!']
contents [[b'Content-Type: text/plain; charset="utf-8"', b'MIME-Version: 1.0', b'Content-Transfer-Encoding: base64', b'', b'VGhpcyBtZXNzYWdlIGZyb20gem1haWwh', b'']]
attachments None
id 5
```

**Mail structure**

- content-type: Mail content type
- subject: Mail subject
- to
- from
- date: year-month-day time TimeZone
- boundary: If mail is multiple parts, you can get the boundary
- content: Mail content as text/plain
- contents: Mail's body as bytes,divided by boundary
- attachments: None or [['attachment-name;Encoding','ATTACHMENT-DATA']...]
- id: Mailbox id

**Get attachment**

```
import zmail
server = zmail.server('yourmail@example.com, 'yourpassword')
mail = server.get_latest()
zmail.get_attachment(mail)
```

you can rename your attachment file, by

```
zmail.get_attachment(mail,'example.zip')
```



## Supported mail server

The mail server in this list has been tested and approved.

**If your mail server not in it, don't worry, zmail will handle it automatically.If there any problems in use, pls tell me in the Github.**  



| Server address | Send mail | Retrieve mail | Remarks                        |
| -------------- | --------- | ------------- | ------------------------------ |
| @163.com       | ✓         | ✓             | Need app private password      |
| @qq.com        | ✓         | ✓             | POP3 need app private password |
| @126.com       | ✓         | ✓             |                                |
| @yeah.net      | ✓         | ✓             |                                |
| @gmail.com     | ✓         | ✓             | Need app private password      |
| @sina.com      | ✓         | ✓             |                                |
| @outlook       | ✓         | ✓             |                                |

## API Changes

- Ver 0.0.5 ：Do not use zmail.encode(mail) before send your mail. Direct use server.send_mail('yourfriend@examample.com', mail_as_dict)
