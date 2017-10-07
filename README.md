### **1.AWK Linux Command**

- AWK is linux command
- Why AWK name ? it comes from inventors Alfred Aho, Peter Weinberger, and Brian Kernighan
- AWK is a programming language designed for text processing and typically used as a data extraction and reporting tool 
- AWK preinstalled all Linux/Unix OS 


### ***2.Working with field,record and Pattern***

 2.1 Print all lines 
     
       awk'{print}' filename.txt  OR awk'{print $0}' 
 
 2.2 Calculate number of Fields(NF)
   
       awk'{ print NF, $0 }'
       
 2.3 
     