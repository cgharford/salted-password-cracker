Offline Cracking with Salted Hashes

I wrote both of my programs for this part of the assignement in python and thus did not 
include a makefile. 

First, I wrote the findsalt program that essetially generated all possible two-character
combinations of the ascii characters 32-127. Using the username and password found in part 
2, I ran the following command: 

    $ python findsalt.py whoami $un-7zu ed4d5c30b1d09423d4407e04e4f7a0ef28c3ffa6 > results.out 

This did not give me the correct salt, as my program had issues parsing the $ in the password 
passed into the command line. I finally prepended a '\' to the beginning of the password
before running the command and that seemed to maintain the correct password: 

    $ python findsalt.py whoami \$un-7zu ed4d5c30b1d09423d4407e04e4f7a0ef28c3ffa6 > results.out 

However, I was still unsuccessful in finding the salt. I wasn't sure if the hash I was using
was correct, so I went into the yahoo_hashes file and found the correct hash corresponding to 
the whoami username. I ran the command again with the correct hash: 

    $ ./findsalt whoami \$un-7zu a99ae9d829db548d06cad216662ad8f3580fedc8 

This time the program succeeded! The salt found is y!

Next, I wrote the yahoocrack program that took in a list of passwords from stdin and stored them
as candidates. The program then tried to find the password for every username by combining the 
salt, username, and candidate password and hashing the entire thing. I then tried to see how 
many passwords I could crack by running this command: 

    $ nice john --wordlist --stdout | ./yahoocrack yahoo_hashes 

This reported that I had cracked 9.37% of the passwords. 

Wanting to reach a higher percentage, I then tried the following command: 

    $ nice john --wordlist --rules --stdout | ./yahoocrack yahoo_hashes 

This process took a long time and still only cracked 12.41% of the passwords. 

I then decided to save the passwords I had cracked in the cracked.txt file by running: 

    $ nice john --wordlist --stdout > words.txt   
    $ nice cat words.txt | ./yahoocrack yahoo_hashes > cracked.txt 

Still wanting to crack more passowrds, I downloaded a word list from http://www.lanmaster53.com/2011/02/creating-complex-password-lists-with-john-the-ripper/ and tried using it on the program 
with the following commands: 

    $ nice cat wordlist.txt | ./yahoocrack yahoo_hashes 

This process was going to take over 24 hours to complete, so I asked what percentage of passwords
we needed to crack in class, and found that 10% was sufficient! 

So I went back and put the cracked passwords into a cracked.txt file using the following command: 

    $ nice john --wordlist --stdout | ./yahoocrack yahoo_hashes > cracked.txt

All done!
