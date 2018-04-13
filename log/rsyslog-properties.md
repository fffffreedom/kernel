# rsyslog

Syslog message properties are used inside templates. They are accessed by putting them between percent signs. 
Properties can be modified by the property replacer. The full syntax is as follows:  
```
%property:fromChar:toChar:options%
```

There is also support for regular expressions. To use them, you need to place a “R” into FromChar. 
This tells rsyslog that a regular expression instead of position-based extraction is desired. 
The actual regular expression must then be provided in toChar. The regular expression must be
followed by the string “–end”. It denotes the end of the regular expression and will not become part of it. 
If you are using regular expressions, the property replacer will return the part of the property text 
that matches the regular expression. An example for a property replacer sequence with a regular expression is: 
“%msg:R:.*Sev:. \(.*\) \[.*–end%”

It is possible to specify some parametes after the “R”. These are comma-separated. They are:  
```
R,<regexp-type>,<submatch>,<nomatch>,<match-number>
```

regexp-type is either “BRE” for Posix basic regular expressions or “ERE” for extended ones. 
The string must be given in upper case. The default is “BRE” to be consistent with earlier 
versions of rsyslog that did not support ERE. The submatch identifies the submatch to be used 
with the result. A single digit is supported. Match 0 is the full match, while 1 to 9 are the 
acutal submatches. The match-number identifies which match to use, if the expression occurs 
more than once inside the string. Please note that the first match is number 0, the second 1 
and so on. Up to 10 matches (up to number 9) are supported. Please note that it would be more 
natural to have the match-number in front of submatch, but this would break backward-compatibility. 
So the match-number must be specified after “nomatch”.

nomatch specifies what should be used in case no match is found.  

## reference
https://www.rsyslog.com/doc/v8-stable/configuration/property_replacer.html  
https://www.rsyslog.com/doc/v8-stable/configuration/templates.html  
https://www.rsyslog.com/doc/v8-stable/configuration/nomatch.html  

## vip的 harbor log 配置说明  
http://www.mamicode.com/info-detail-1777837.html  
