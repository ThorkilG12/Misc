I have two PC's. HomePC and WorkPC.  
I want to controle my WorkPC from my HomePC

1) Use WorkPC:  
Press Win + R, input “sysdm.cpl”, and click OK. Choose Remote Tab  
Allow Remote and uncheck "Allow connections only..."  
Notice the IP number for the WorkPC. Use IPConfig from a CMD prompt  

2) Use HomePC:
Open Remote Desktop Connection program  
Enter WorkPC IP address  
Enter .\AzureAD\thj@biwise.dk as user  
Save a Profile "Save as". Close Remote Desktop  
Open the saved Profile file and add this two lines:  
**enablecredsspsupport:i:0**  
**authentication level:i:2**  
Open Remote Desktop Connection program again and open the profile that was edited before.  

Now you will connect whit this info:  
IP 192.168.1.nnn  
.\AzureAD\thj@biwise.dk  
Cjj.....  

Now the logon window from my WorkPC opens in a window on my HomePC ready for "Other user" and the user is "AzureAD\thj@biwise.dk"
I enter the password Cjj...

