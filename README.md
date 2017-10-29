### **1.AWK Linux Command**

- AWK is linux command
- Why AWK name ? it comes from inventors Alfred Aho, Peter Weinberger, and Brian Kernighan
- AWK is a programming language designed for text processing and typically used as a data extraction and reporting tool 
- AWK preinstalled all Linux/Unix OS 
- AWk having actions and/OR pattern,but can't eliminate both. Awk's action written into curly bracket {}. 

### ***2.Working with field,record and Pattern***

 2.1 Print all lines 
     
       filename.txt contains:
        
        The grand old Duke of York
        He had ten thousand men
        He marched them up to the top of the hill
        And he marched them down again
        And when they were up they were up
        And when they were down they were down
        And when they were only half-way up
        They were neither up nor down
       
       
       awk'{print}' filename.txt  OR awk'{print $0}' filename.txt
       
        
 
 2.2 Calculate number of Fields(NF)
   
       awk'{ print NF, $0 }' filename.txt
       
        6 The grand old Duke of York
        5 He had ten thousand men
        10 He marched them up to the top of the hill
        6 And he marched them down again
        8 And when they were up they were up
        8 And when they were down they were down
        7 And when they were only half-way up
        6 They were neither up nor down
       
       
       
       
 2.3 Displaying those lines which matched keyword mentioned in Pattern :
 
       awk '/marched/{print NF, $0}' dukeyourk.txt

       10 He marched them up to the top of the hill
       6 And he marched them down again

2.4 Eliminating action in awk :

     awk 'NF==6' dukeyourk.txt
   
   	 The grand old Duke of York
     And he marched them down again
     They were neither up nor down

2.5 Multiple Action and pattern:

    awk '/up/{print "Matched line which contating word up: ",$0} /down/{print "Matched line which contating word down: ",$0}' dukeyourk.txt
    
    Matched line which contating word up:  He marched them up to the top of the hill
    Matched line which contating word down:  And he marched them down again
    Matched line which contating word up:  And when they were up they were up
    Matched line which contating word down:  And when they were down they were down
    Matched line which contating word up:  And when they were only half-way up
    Matched line which contating word up:  They were neither up nor down
    Matched line which contating word down:  They were neither up nor down
    
2.6 Awk different flags

    - "-f " is used provide action and pattern from file inside of awk commandline
    - "-F" is used to give field seperaters. By default it is space
    - "-v" is used to define variable which used in awk's action like `awk -v number=3 '{print $1, number}'`
    
 ### ***3. Specifying field and record separators with variables***
 
- In awk, two important patterns are specified by the keywords "BEGIN" and "END". These two words specify actions to be taken before any lines are read, and after the last line is read. The AWK program below:


      awk 'BEGIN { print "---------This start of file---------------" } { print $0 } END { print "-------This end of file------------------" }' dukeyourk.txt
      
      ---------This start of file---------------
      The grand old Duke of York
      He had ten thousand men
      He marched them up to the top of the hill
      And he marched them down again
      And when they were up they were up
      And when they were down they were down
      And when they were only half-way up
      They were neither up nor down
      -------This end of file------------------

- FS - The Input Field Separator Variable:
  
  AWK can be used to parse many system administration files. However, many of these files do not have whitespace as a separator. as an example, the password file uses colons. You can easily change the field separator character to be a colon using the "-F" command line option. The following command will print out accounts that don't have passwords:

	  awk -F: '{ if ( $2=="") print $1 " : No password set for this user" }' fieldseperater.txt
      amit : No password set for this user
      amol : No password set for this user


  There is a way to do this without the command line option. The variable "FS" can be set like any variable, and has the same function as the "-F" command line option. The following is a script that has the same function as the one above.

      awk  'BEGIN{ FS=":"}{ if ( $2=="") print $1 " : No password set for this user" }' fieldseperater.txt
      amit : No password set for this user
      amol : No password set for this user
 
 Example of multiple Input Field Separator 
 
      awk 'BEGIN{ FS="[,: ]" } {  print $2 " : " $0}' multifieldseperater.txt
      two : one,two,three
      two : one two three
      two : one:two:three


- OFS - The Output Field Separator Variable

   There is an important difference between

		print $2 $3
and

		print $2, $3
The first example prints out one field, and the second prints out two fields. In the first case, the two positional parameters are concatenated together and output without a space. In the second case, AWK prints two fields, and places the output field separator between them. Normally this is a space, but you can change this by modifying the variable "OFS".

 	awk 'BEGIN{ FS=":"; OFS="," } { print $1,$2,$3,$4 }' fieldseperater.txt
        
        rupesh,x,1000,1000
        amit,,2000,2000
        amol,,2000,2000
        kalyani,x,2000,2000
        
- RS - The Record Separator Variable

  Normally, AWK reads one line at a time, and breaks up the line into fields. You can set the "RS" variable to change AWK's definition of a "line". If you set it to an empty string, then AWK will read the entire file into memory. You can combine this with changing the "FS" variable. This example treats each line as a field, and prints out the second and third line

      awk 'BEGIN { RS=""; FS"\n" } { print $1,$2,$3,$4 }' recordseperater.txt
      
      Rupesh Shewalkar Kharadi Pune
      Amit Shewalkar Vashi Mumbai
      Amol Shewalkar susgoan Pune
      Kalyani Shewalkar Vadgaon Pune
      
- ORS - The Output Record Separator Variable

	The default output record separator is a newline, like the input. This can be set to be a newline and carriage return.
    
    Example Without ORS Separator:
    
      awk 'BEGIN{ FS=",";RS="!" } { print $2 }' outputrecordseperater.txt
      two
      six
      nine
      345
   
    Example With ORS Separator:
    
      awk 'BEGIN{ FS=",";RS="!"; ORS="," } { print $2 }' outputrecordseperater.txt
  	  two,six,nine,345,
    
- NR- The Number of Records Variable

	Another useful variable is "NR". This tells you the number of records, or the line number. You can use AWK to only examine certain lines.
    
   	 awk '{ print NR,$1}' dukeyourk.txt
     
      1 The
      2 He
      3 He
      4 And
      5 And
      6 And
      7 And
      8 They


- FILENAME - The Current Filename Variable
	
    The last variable known to regular AWK is "FILENAME", which tells you the name of the file being read.
    
      awk '{ print FILENAME,NR,FNR,$0}' fieldseperater.txt multifieldseperater.txt
      
      fieldseperater.txt 1 1 rupesh:x:1000:1000:Rupesh Shewalkar:/home/rupesh:/bin/bash
      fieldseperater.txt 2 2 amit::2000:2000:Amit Shewalkar:/home/amit:/bin/bash
      fieldseperater.txt 3 3 amol::2000:2000:Amol Shewalkar:/home/amol:/bin/bash
      fieldseperater.txt 4 4 kalyani:x:2000:2000:Kalyani Shewalkar:/home/kalyani:/bin/bash
      multifieldseperater.txt 5 1 one,two,three
      multifieldseperater.txt 6 2 one two three
      multifieldseperater.txt 7 3 one:two:three

### ***3. Using Built-in variable***

As we know NF (The Number of Fields Variable) useful to know how many fields are on a line. Which also help to find last column from file i.e

Print last column value:

     awk '{ print $(NF) }' dukeyourk.txt
    
    York
    men
    hill
    again
    up
    down
    up
    down

Print second last column value:

    awk '{ print $(NF -1) }' dukeyourk.txt
    
    York
    men
    hill
    again
    up
    down
    up
    down

 it is not limit to read value from field, you can assign new value to field like:

   
      awk '{ $2="two"; print }' dukeyourk.txt
      
      The two old Duke of York
      He two ten thousand men
      He two them up to the top of the hill
      And two marched them down again
      And two they were up they were up
      And two they were down they were down
      And two they were only half-way up
      They two neither up nor down
      
      
### ***4.Regex in AWK***

A regular expression enclosed in slashes ('/') is an awk pattern that matches every input record whose text belongs to that set.For example, this prints the second field of each record that contains the three characters `foo' anywhere in it:

          $ awk '/foo/ { print $2 }' BBS-list
          -| 555-1234
          -| 555-6699
          -| 555-6480
          -| 555-2127

Regular expressions can also be used in matching expressions. These expressions allow you to specify the string to match against; it need not be the entire current input record. The two operators, `~' and `!~', perform regular expression comparisons. Expressions using these operators can be used as patterns or in if, while, for, and do statements.

exp ~ /regexp/

This is true if the expression exp (taken as a string) is matched by regexp. The following example matches, or selects, all input records with the upper-case letter `J' somewhere in the first field:
    
    $ awk '$1 ~ /J/' inventory-shipped
    -| Jan  13  25  15 115
    -| Jun  31  42  75 492
    -| Jul  24  34  67 436
    -| Jan  21  36  64 620

So does this:

	awk '{ if ($1 ~ /J/) print }' inventory-shipped

exp !~ /regexp/


This is true if the expression exp (taken as a character string) is not matched by regexp. The following example matches, or selects, all input records whose first field does not contain the upper-case letter `J':

          $ awk '$1 !~ /J/' inventory-shipped
          -| Feb  15  32  24 226
          -| Mar  15  24  34 228
          -| Apr  31  52  63 420
          -| May  16  34  29 208
          ...


When a regexp is written enclosed in slashes, like /foo/, we call it a regexp constant, much like 5.27 is a numeric constant, and "foo" is a string constant.


### ***5. Formatting output of AWK with printf***

printf function help to formatting output of AWK,mainly controlling width of output columns, precision specifier on columns & many other stuff. printf function same as "C programming" langauge. 

Syntax:
  
       awk '{printf "format", Arguments}' filename
       
The printf can be useful when specifying data type such as integer, decimal, octal etc. Below are the list of some data types which are available in AWK

        %i or d --Decimal
        %o --Octal
        %x --hex
        %c --ASCII number character
        %s --String
        %f --floating number
        
Note: Make sure that you pass exact data types when using corresponding formats as shown below. If you pass a string to a decimal formatting, it will print just zero instead of that string.


**PADDING BETWEEN COLUMNS USING AWK PRINTF**

-n --Pad n spaces on right hand side of a column.

n --Pad n spaces on left hand side of a column.

.m --Add zeros on left side.

-n.m --Pad n spaces right hand side and add m zeros before that number.

n.m --Pad n spaces left hand side and add m zeros before that.


Example without printf 

      awk '{ print $1,$2,$3,$5}' printfoutput.txt                                 
      
      Rupesh 163 123 567.212432                                                       
      Amit 167 21 678.2314523                                                         
      Kalyani 4 222 92.7887                                                           
      Amol 135 132 999.423
 
Example with Printf 

      awk '{ printf "|%-25s|%-7.3d|%5.3d|%3.2f\n", $1,$2,$3,$5}' printfoutput.txt 
      |Rupesh                   |163    |  123|567.21                                 
      |Amit                     |167    |  021|678.23                                 
      |Kalyani                  |004    |  222|92.79                                  
      |Amol                     |135    |  132|999.42


