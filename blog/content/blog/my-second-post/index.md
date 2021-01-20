---
title: Python Importing re package
date: "2021-01-19T16:20:30+08:00"

description: "Learning about the regular expression (re) python package "
---

I was able to use the python import statement to pull in the [re package](https://docs.python.org/3/library/re.html). Once I did this I was able to find patterns in content using high level method search() or lower level  methods such as compile, match and group(s). 

After reading the documentation I was able to create a simple class that would validate a phone number to ensure it has the the following format 3 digits then a hyphen then 3 additional digits and hyphen followed by 4 additional digits. ###-###-####. I first used the following regular expression and compiled it once so I didn't compile it on every example. 

```python
matcher = re.compile(r'^(\d{3})-(\d{3})-(\d{4})$')
```

One note is that I tried using a shorter regular expression since ```\d{3}``` is repeated in the pattern. I had issues with this because I wanted to be able to get the area code as group 1 but was not able to do that since grouping gives the last pattern match so it would always give the second match or the prefix not the area code.

To see if the content has a match I used the matcher variable and ran the match method

```python
result = matcher.match(self.number)
```

The result of the match method would give me None or a Match instance which I can use to get the groups. Since I am using parenthesis in my regular expression I can access the area code or first 3 digits after the match.

```python
(\d{3})
# can be accessed using
print("area code", result.group(1))
```

Here is the entire class I wrote 


```python

"""
    Using re - regular expression library in python

    you can use this library to perform sequence matches such as matching the
    format of a phone number, words in a list without any space or punctuation.

    https://docs.python.org/3/library/re.html

"""
import re

class PhoneNumberFormat:

    ## Use the re library to compile the regular expression to match a U.S.
    ## phone number format of -  ###-###-####
    matcher = re.compile(r'^(\d{3})-(\d{3})-(\d{4})$')
    areaCode = "unknown"

    def __init__(self, phone_number):
        self.number = phone_number

    ## Match phone number format ###-###-####
    def isValidPhoneNumber(self):
        groups = self.getPhoneNumberMatches()
        # after a match we can check if the value is not None and return true
        if(groups is not None):
            # you can use the group function to access the first match of the 
            # expression (\d{3})
            self.areaCode = groups[0]
            return True
        return False

    def getPhoneNumberMatches(self):
        result = self.matcher.match(self.number)
        if(result is None):
            return None
        return result.groups()

    def __str__(self):
        return "Is Valid Phone Number? {}\n\
        Area code: {}".format(self.isValidPhoneNumber(), self.areaCode)

print(PhoneNumberFormat("555-666-7777"))
print(PhoneNumberFormat("111-666-7777"))
print(PhoneNumberFormat("555-55-5555"))
print(PhoneNumberFormat("555-555-53355"))
print(PhoneNumberFormat("%%%%555-555-5555"))
print(PhoneNumberFormat("555-555-5555^^^^"))
print(PhoneNumberFormat("%$^&-#$%-@#$%"))
```


The resulting output is

```console
Is Valid Phone Number? True
        Area code: 555
Is Valid Phone Number? True
        Area code: 111
Is Valid Phone Number? False
        Area code: unknown
Is Valid Phone Number? False
        Area code: unknown
Is Valid Phone Number? False
        Area code: unknown
Is Valid Phone Number? False
        Area code: unknown
Is Valid Phone Number? False
        Area code: unknown
```

Next steps:

Could improve this to support different common telephone formats such as 1 (XXX) XXX-XXXX.