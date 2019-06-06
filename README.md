# Squid-Proxy-Server-
Setting up squid proxy server with "Custom error page for ERROR 403"

# <NOTE : THESE ALL COMMANDS ARE INSIDE ROOT USER >
  ( TO be permanently as a root user type "sudo  su")

1. apt-get update && apt-get install squid apache2-utils -y

     
    <NOTE : GO TO FIREFOX AND CHANGE THE PROXY SETTING >
    1A> In firefox go to options -> Advanced -> Network -> configure how firefox connects 
  
    1B> Then in HTTP Proxy : <TYPE THE IP ADDRESS OF SERVER MACHINE ONLY>
  
    1C>  in PORT : 3128
    
    <NOTE : This port number is default port number for proxy server>
      
2. vim /etc/squid/squid.conf
     
     
     
     2A> search for keyword "http_access deny all"
     2B> change it to "http_access allow all"
     
     
     OPRIONAL STEPS FOR CHECHKING LOG FILE TRAFFIC
     #go to google and search something 
     #type the command "tail -f /var/log/squid/access.log"
    
 
 3. vim /etc/squid/squid.conf
 
 #now i am making a variable "blocked_url" which will hold "/etc/squid/blocked_sites.acl" path.
 #go to very first line and type ( for easy to locate lines i am typing at first line only , you could go to it's desired TAG: and type it there but make sure all uncommented commands are just below TAG to avoid any confusion)
 
 3a. acl blocked_url dstdomain "/etc/squid/blocked_sites.acl"
 #the very next line type as below
 3b. http_access deny blocked_url
 #save and quit to come out of vim

4. service squid restart

#now to setup a passwd file for squid user and also making a user

5. touch /etc/squid/passwd
6. htpasswd /etc/squid/passwd user1
        # type the password two time as it will ask for you

7. vim /etc/squid/squid.conf
  
  
  #IN 4rth , 5th and 6th line (just make sure these line are afterward the above lines typed in this config file )
  
7a>  auth_param basic program /usr/lib/squid/basic_ncsa_auth /etc/squid/passwd2
#-----------------------------------custom network---------------- 
7b> acl MyNetwork src 172.16.50.1-172.16.50.250     #this is the range of ip address
7c> acl squid_users proxy_auth REQUIRED 
7d> http_access allow squid_users 
7e> http_access allow MyNetwork
#-----------------------------------custom network--------------- #modified 
  
     
 8. service squid restart
 
 
 #in order to block selected websites open the file below 
 
 9. vim /etc/squid/blocked_sites.acl
 
    #type here
    9a> .msn.com
    9b> .sstsinc.com
    9c> .abc.com
    #these are some example websites domain (no hard feelings for any 😛)
 10. service squid restart
 
 
 #NOTE just go to firefox and you will see an authentication window , type the username and password there
 #then type .msn.com and you will find it blocked , and other websites also while google.com and other websites will be running
 
 
 
 **************************************************************************************************************************************
 #                                                                                                                                     #
 #                                                                                                                                     #
 #                        SETTING UP CUSTOM ERROR PAGE FOR 403 FORBIDDEN "ACCESS DENIED" BY WEBMASTER ERROR PAGE                       #
 #                                                                                                                                     #
 #                                                                                                                                     #
 ***************************************************************************************************************************************
 
 1. vim /etc/squid/squid.conf
   
   #search for "acl MyNetwork src 192.168.0.0/16"
   #below that line type the following 
  
  AS you can see i am creating custom error page only for google.com
  
   2. acl abcd dstdomain .google.com
      #note here abcd is again a variable
      #below that line type ...
      
   3. deny_info ERR_google abcd
   
   4. http_access deny abcd
       #save and quit
      
#generally error html page are in /usr/share/squid/errors/templates

   4A> cd  /usr/share/squid/errors/templates
   
   5. ls (to check all html specific error files)
   
   6. vim ERR_google 
   
   #since we want to show info from our own custom error page file .
     
     <html>
     <title> CUSTOM ERROR</title>
     <body>
     
     <h1>This is just a custom error page example to show you. YOU can link it with custom css and custom javascript file </h1> 
     
      <style>
      
      body {
      background-color: linen;
            }
      h1 {
        color: maroon;
        margin-left: 40px;
      } 
      
      </style>
      
      </body>
      </html>
     
#save and quit 
#now you have to restart proxy server

7. service squid restart


#NOTE ++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
#check it again by going to www.google.com since i have only created custom error for that page only
#you can just add lines manually like "acl abcd dstdomain .anysite.com" in the config file or
#you can create a path as created befor "/etc/squid/blocked_sites.acl" and assign a variable "abcd" to it.


# IMPORTANT TO NOTE

1> Firefox setting of the client machine has to be changed to manual proxy setting and the ip address will be set to ip address of server machine only .
here proxy :172.16.50.72 will be set with port 3128





#### Thank YOU ####
#### Created by Anukool Srivastava #####
 
