+++
draft = true
date = 2021-06-30T13:01:18+03:00
title = "M-Pesa Number Validation with Regular Expressions"
description = "Regex Patterns for Kenyan Mobile Phone Numbers"
slug = "regex-101-phonenumber-validation"
tags = ['regex', 'safaricom', 'validation']
categories = []
externalLink = ""
series = []

+++
## Background

Recognizing patterns and exploiting them is a crucial skill for any developer lest we hardcode all possible outcomes. For example, we can get the format all possible of Safaricom Phone Numbers from this [wiki](https://en.wikipedia.org/wiki/Telephone_numbers_in_Kenya) in high-level. Using this, we can choose to write a script that checks if a user input matches all possible Safaricom numbers. Or, we can find what's common in all the numbers and let our compiler do the work.

All mobile phone numbers follow a standard known as the Mobile Station International Subscriber Directory Number ([MSISDN](https://en.wikipedia.org/wiki/MSISDN)), that uniquely maps a SIM card to a real user. Take my own number, for example, ```+254 769 703150```. The ```+254``` part represents the country code. The ```769``` part is the National Destination Code (NDC), which represent a part of Safaricom's Network Infrastructure. The ```703150``` part refers to the subscriber number.

## Creating the Regex Pattern
We are going to create regex patterns for the aforementioned three parts and then concatenate them to get a Bonafide Mpesa number pattern.
##### The Country Code
The Kenyan MSISDN country code typically begins with ```+254``` or its shorthand, ```0```. In RegEx, we are going to represent it as:
```
/(\+?254|0){1}/
```
All regex patterns begin with a slash, ```/```, and terminate with a slash. The ```|``` symbol represents the logical operator ```OR```, because a Safaricom number can either start with ```+254``` or ```0```. The ```()``` sign represents a RegEx grouping. The ```{1}``` symbol tells the RegEx pattern to choose one between ```0``` or ```+254``` in the user's input of the Country Code group.

##### The National Destination Code
We know the story, it used to be that Safaricom numbers were ```070-something``` to ```072-something```, but they got more NDCs and we got to adapt. For example, there is a ```0769``` Safaricom number or a ```0110``` one. Anyway, here is a breakdown of their current NDCs as of June 2021, as per the [wiki](https://en.wikipedia.org/wiki/Telephone_numbers_in_Kenya):
* 700 to 729
```
/7([0-2]{1}[0-9]{1})/
```
* 740 to 743
```
/7([4]{1}[0-3]{1})/
```
* 745 to 746
```
/7([4]{1}[5-6]{1})/
```
* 748
```
/7([4]{1}[8]{1})/
```
* 757 to 759
```
/7([5]{1}[7-9]{1})/
```
* 768 to 769
```
/7([6]{1}[8-9]{1})/
```
* 790 to 799
```
/7([9]{1}[0-9]{1})/
```
* 110 to 119
```
/1([1]{1}[0-9]{1}/
```

##### The Subscriber Number
This is the easiest part
```
/[0-9]{6}/
```
##### Combining the Three Parts
To combine all the parts, we will use regex grouping. Specifically, we are going to use ```()``` as our capturing group. A capturing group is one that matches every of the above sub-patterns and remembers that match. To read more about regex grouping read [this article](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions/Groups_and_Ranges).
We will start by capturing the country code and omit the start and trailing backslashes:
```
((\+?254|0){1})
```
Next, we capture the NDC combinations and omit the backslashes as well. We will combine all the possible NDCs with the boolean or operator, ```|```. Also, we will combine the ones starting with a ```7``` separate from the one starting with a ```1``` to simplify. The final product of the  NDCs will be in this format: ```(7((NDC1)|(NDC2)))|(1((NDC1)|(NDC2)))```.
```
((7(([0-2]{1}[0-9]{1})|([9]{1}[0-9]{1})|([4]{1}[0-3]{1})|([4]{1}[5-6]{1})|([4]{1}[8]{1})|([5]{1}[7-9]{1})|([6]{1}[8-9]{1})))|(1([1]{1}[0-9]{1})))
```
The subscriber number's pattern will not change. There will be no need for enclosing it in a capturing group. Lastly, we will combine the three parts and add a preceding and trailing backslash to the combo. The final regex will thereby take the following form.

```
/((\+?254|0){1})((7(([0-2]{1}[0-9]{1})|([9]{1}[0-9]{1})|([4]{1}[0-3]{1})|([4]{1}[5-6]{1})|([4]{1}[8]{1})|([5]{1}[7-9]{1})|([6]{1}[8-9]{1})))|(1([1]{1}[0-9]{1})))[0-9]{6}/
```

## Conclusion
Using the same wiki, I am confident that you can create expressions for other mobile networks in Kenya. For any issues creating a RegEx pattern, reach me at [hi@cyprian.dev](mailto:hi@cyprian.dev). Cheers.
