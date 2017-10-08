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




