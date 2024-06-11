# coursera_etl_and_datapipelines

											--- 2 MODULE---

				/* --- Exercise 1 - Extracting data using 'cut' command --- */
		
		*** The filter command -cut helps us extract selected characters or fields from a line of text.

---
theia@theiadocker-fsrcodefsr:/home/project$ echo "database" | cut -c1-4
data

---
theia@theiadocker-fsrcodefsr:/home/project$ echo "database" | cut -c5-8
base

---
theia@theiadocker-fsrcodefsr:/home/project$ echo "database" | cut -c1,5
db

---
theia@theiadocker-fsrcodefsr:/home/project$ cut -d":" -f1 /etc/passwd
root
daemon
bin
sys
sync
games
man
lp
mail
news
uucp
proxy
www-data
backup
list
irc
gnats
nobody
_apt
systemd-network
systemd-resolve
messagebus
systemd-timesync
sshd
theia
cassandra
mongodb

---
theia@theiadocker-fsrcodefsr:/home/project$ cut -d":" -f1,3,6 /etc/passwd
root:0:/root
daemon:1:/usr/sbin
bin:2:/bin
sys:3:/dev
sync:4:/bin
games:5:/usr/games
man:6:/var/cache/man
lp:7:/var/spool/lpd
mail:8:/var/mail
news:9:/var/spool/news
uucp:10:/var/spool/uucp
proxy:13:/bin
www-data:33:/var/www
backup:34:/var/backups
list:38:/var/list
irc:39:/run/ircd
gnats:41:/var/lib/gnats
nobody:65534:/nonexistent
_apt:100:/nonexistent
systemd-network:101:/run/systemd
systemd-resolve:102:/run/systemd
messagebus:103:/nonexistent
systemd-timesync:104:/run/systemd
sshd:105:/run/sshd
theia:1000:/home/theia
cassandra:106:/var/lib/cassandra
mongodb:107:/home/mongodb

---
theia@theiadocker-fsrcodefsr:/home/project$ cut -d":" -f3-6 /etc/passwd
0:0:root:/root
1:1:daemon:/usr/sbin
2:2:bin:/bin
3:3:sys:/dev
4:65534:sync:/bin
5:60:games:/usr/games
6:12:man:/var/cache/man
7:7:lp:/var/spool/lpd
8:8:mail:/var/mail
9:9:news:/var/spool/news
10:10:uucp:/var/spool/uucp
13:13:proxy:/bin
33:33:www-data:/var/www
34:34:backup:/var/backups
38:38:Mailing List Manager:/var/list
39:39:ircd:/run/ircd
41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats
65534:65534:nobody:/nonexistent
100:65534::/nonexistent
101:102:systemd Network Management,,,:/run/systemd
102:103:systemd Resolver,,,:/run/systemd
103:105::/nonexistent
104:106:systemd Time Synchronization,,,:/run/systemd
105:65534::/run/sshd
1000:1000:,,,:/home/theia
106:109:Cassandra database,,,:/var/lib/cassandra
107:65534::/home/mongodb

---




			/* --- Exercise 2 - Transforming data using 'tr' --- */
		*** -tr is a filter command used to translate, squeeze, and/or delete characters.

---
theia@theiadocker-fsrcodefsr:/home/project$ echo "Shell Scripting" | tr "[a-z]" "[A-Z]"
SHELL SCRIPTING

---
theia@theiadocker-fsrcodefsr:/home/project$ echo "Shell Scripting" | tr "[:lower:]" "[:upper:]"
SHELL SCRIPTING

---
theia@theiadocker-fsrcodefsr:/home/project$ echo "Shell Scripting" | tr  "[A-Z]" "[a-z]"
shell scripting

---
theia@theiadocker-fsrcodefsr:/home/project$ ps
    PID TTY          TIME CMD
    356 pts/2    00:00:00 bash
  23253 pts/2    00:00:00 ps
  
  
---
theia@theiadocker-fsrcodefsr:/home/project$ ps | tr -s " "
 PID TTY TIME CMD
 356 pts/2 00:00:00 bash
 24174 pts/2 00:00:00 ps
 24175 pts/2 00:00:00 tr
 
---
theia@theiadocker-fsrcodefsr:/home/project$ echo "My login pin is 5634" | tr -d "[:digit:]"
My login pin is

---

	

		/* Exercise 3 - Start the PostgreSQL database. */

		/* Exercise 4 - Create a table */

---
\c template1

---
create table users(username varchar(50),userid int,homedirectory varchar(100));


		/* Exercise 5 - Loading data into a PostgreSQL table. */
		
---
touch csv2db.sh

---
csv2db.sh > 
	# This script
	# Extracts data from /etc/passwd file into a CSV file.

	# The csv data file contains the user name, user id and
	# home directory of each user account defined in /etc/passwd

	# Transforms the text delimiter from ":" to ",".
	# Loads the data from the CSV file into a table in PostgreSQL database.

	#1
	# Extract phase

	echo "Extracting data"

	# Extract the columns 1 (user name), 2 (user id) and 
	# 6 (home directory path) from /etc/passwd

	cut -d":" -f1,3,6 /etc/passwd > extracted-data.txt

	#2
	# Transform phase
	echo "Transforming data"
	# read the extracted data and replace the colons with commas.

	tr ":" "," < extracted-data.txt  > transformed-data.csv


---
csv2db.sh >

	# This script
	# Extracts data from /etc/passwd file into a CSV file.

	# The csv data file contains the user name, user id and
	# home directory of each user account defined in /etc/passwd

	# Transforms the text delimiter from ":" to ",".
	# Loads the data from the CSV file into a table in PostgreSQL database.

	#1
	# Extract phase

	echo "Extracting data"

	# Extract the columns 1 (user name), 2 (user id) and 
	# 6 (home directory path) from /etc/passwd

	cut -d":" -f1,3,6 /etc/passwd > extracted-data.txt

	#2
	# Transform phase
	echo "Transforming data"
	# read the extracted data and replace the colons with commas.

	tr ":" "," < extracted-data.txt  > transformed-data.csv

	#3
	# Load phase
	echo "Loading data"
	# Send the instructions to connect to 'template1' and
	# copy the file to the table 'users' through command pipeline.

	echo "\c template1;\COPY users  FROM '/home/project/transformed-data.csv' DELIMITERS ',' CSV;" | psql --username=postgres --host=localhost



---
echo '\c template1; \\SELECT * from users;' | psql --username=postgres --host=localhost
