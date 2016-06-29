---
layout: post
title: "Wiki - Python"
description: ""
category: Python
tags:  [Python2]
---
{% include JB/setup %}

# Printing, Printing

```python
print "Mary had a little lamb."
print "Its fleece was white as %s." % 'snow'
print "." * 10

end1 = "C"
end2 = "h"
print end1 + end2

>>> python ex7.py
Mary had a little lamb.
Its fleece was white as snow.
..........
Ch
```

## `%r`

```python
# %r
formatter = "%r %r %r %r"
print formatter % (1, 2, 3, 4)
print formatter % ("one", "two", "three", "four")
print formatter % (True, False, False, True)
print formatter % (formatter, formatter, formatter, formatter)
print formatter % (
    "I had this thing.",
    "That you could type up right.",
    "But it didn't sing.",
    "So I said goodnight."
)

>>> python ex8.py
1 2 3 4
'one' 'two' 'three' 'four'
True False False True
'%r %r %r %r' '%r %r %r %r' '%r %r %r %r' '%r %r %r %r'
'I had this thing.' 'That you could type up right.' "But it didn't sing." 'So I said goodnight.'
```

Why does %r sometimes print things with single-quotes when I wrote them with double-quotes?
+ Python is going to print the strings in the most efficient way it can, not replicate exactly the way you wrote them.

## Escape Sequences

+ `\\`  Backslash `(\)`
+ `\'`  Single-quote `(')`
+ `\"`  Double-quote `(")`
+ `\a`  ASCII bell (BEL)
+ `\b`  ASCII backspace (BS)
+ `\f`  ASCII formfeed (FF)
+ `\n`  ASCII linefeed (LF)
+ `\N{name}`    Character named name in the Unicode database (Unicode only)
+ `\r`  Carriage Return (CR)
+ `\t`  Horizontal Tab (TAB)
+ `\uxxxx`  Character with 16-bit hex value xxxx (Unicode only)
+ `\Uxxxxxxxx`  Character with 32-bit hex value xxxxxxxx (Unicode only)
+ `\v`  ASCII vertical tab (VT)
+ `\ooo`    Character with octal value ooo
+ `\xhh`    Character with hex value hh

## `'''` or `"""`

```python
s1 = '''This string contains """ so use triple-single-quotes.''' # print """
s2 = """This string contains ''' so use triple-double-quotes.""" # print '''
```

## `%s` or `%r`

+ `%r` is printing out the raw representation of what you typed, include the original escape sequences. For debugging
+ `%s` for displaying.

# get user input
```python
age = raw_input()
x = int(raw_input())
input() # will try to convert things you enter as if they were Python code, but it has security problems so you should avoid it.
age = raw_input("How old are you? ")
```

# argv

```python
from sys import argv

script, first, second, third = argv

print "The script is called:", script
print "Your first variable is:", first
print "Your second variable is:", second
print "Your third variable is:", third
```

# file IO
```python
from sys import argv

script, filename = argv

txt = open(filename)

print "Here's your file %r:" % filename
print txt.read()

print "Type the filename again:"
file_again = raw_input("> ")

txt_again = open(file_again)

print txt_again.read()

indata = open(from_file).read()
```

+ `close` -- Closes the file. Like File->Save.. in your editor.
+ `read` -- Reads the contents of the file. You can assign the result to a variable.
+ `readline` -- Reads just one line of a text file.
+ `truncate` -- Empties the file. Watch out if you care about the file.
+ `write('stuff')` -- Writes "stuff" to the file.

# Functions

```python
def print_two(*args):
    arg1, arg2 = args
    print "arg1: %r, arg2: %r" % (arg1, arg2)

# ok, that *args is actually pointless, we can just do this
def print_two_again(arg1, arg2):
    print "arg1: %r, arg2: %r" % (arg1, arg2)

# this just takes one argument
def print_one(arg1):
    print "arg1: %r" % arg1

# this one takes no arguments
def print_none():
    print "I got nothin'."


print_two("Zed","Shaw")
print_two_again("Zed","Shaw")
print_one("First!")
print_none()
```

# Module - urllib
```python
import urllib2

response = urllib2.urlopen("http://www.baidu.com")
print response.read()

urlopen(url, data, timeout) #data默认为空None，timeout默认为 socket._GLOBAL_DEFAULT_TIMEOUT

# alternative way
request = urllib2.Request("http://www.baidu.com")
response = urllib2.urlopen(request)
print response.read()
```

## Post - eg. login
```python
import urllib
import urllib2

values = {"username":"1016903103@qq.com","password":"XXXX"}
data = urllib.urlencode(values)
#values = {}
#values['username'] = "1016903103@qq.com"
#values['password'] = "XXXX"
#data = urllib.urlencode(values)
url = "https://passport.csdn.net/account/login?from=http://my.csdn.net/my/mycsdn"
request = urllib2.Request(url,data)
response = urllib2.urlopen(request)
print response.read()

```

## get
```python
import urllib
import urllib2

values={}
values['username'] = "1016903103@qq.com"
values['password']="XXXX"
data = urllib.urlencode(values)
url = "http://passport.csdn.net/account/login"
geturl = url + "?"+data
request = urllib2.Request(geturl)
response = urllib2.urlopen(request)
print response.read()
```

## construct header
```python
import urllib
import urllib2

url = 'http://www.server.com/login'
user_agent = 'Mozilla/4.0 (compatible; MSIE 5.5; Windows NT)'
values = {'username' : 'cqc',  'password' : 'XXXX' }
headers = { 'User-Agent' : user_agent }
data = urllib.urlencode(values)
request = urllib2.Request(urldata, headers)
response = urllib2.urlopen(request)
page = response.read()
```

## `range()` vs `xrange`

+ In python 3, range() does what xrange() used to do and xrange() does not exist. If you want to write code that will run on both Python 2 and Python 3, you can't use xrange().
+ range() can actually be faster in some cases - eg. if iterating over the same sequence multiple times.  xrange() has to reconstruct the integer object every time, but range() will have real integer objects. (It will always perform worse in terms of memory however)
+ xrange() isn't usable in all cases where a real list is needed. For instance, it doesn't support slices, or any list methods.




























