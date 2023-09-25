# Herck

Herck is a platform built to retrieve market information from various source and centralized the information in a database. The centralized information are gather to analyze it and perform mathematical/financial operation on it.
All entire project is built with python and use the microservice architecture.
## Service
  <li>Account</li>
  <li>Bot</li>
  <li>datalake</li>
  <li>Log</li>
  <li>Authentication</li>
  <li>Status</li>
  <li>Analysis</li>

## Explenation

We'll follow the pattern " One Database per Server " to maintain service atomic and independenty. All of services above communicate each other with classic http method to maintain the architecture design simple.
At the top of all service we mount an API Gateway to proceed with authentication steps. If the authentication steps it goes weel, the user obtain a token(and a role based on his permission) to contact service
between a browser view to retrieve Data(datalake) from centralized server, or analyze data(Analysis) and so on.

<h2>Technicism</h2>

<h3>Account Service</h3>

The first service will be code,build and test is <b>Account Service</b>. It provides all method that are necessaries to <b>CRUD</b> operation on account. We'll use a simple <b>SQLite</b> database with no complicated
database server or RDBS as first approach. Next we tested the logic of service, we provide to integrate a better solution to store Account information and provide a backup system.

The <b>model</b> contains simple information to any single account.

Username
Email
FirstName
LastName

For the password we follow a flow:

User digit (Username,Email,FirstName,LastName) and sent to server, after a check if it is all Ok, server sent a mail with a temporary link to confirm the registration. After confirm the registration, user will be redirect to another page when he set the password. The password for safety will be store hashed and salted well in another database. The password database can be contacted through appropriate methods called by <b>Authentication Service</b> to  proceed with various check for authentication.

<h3>Log Service</h3>

It necessary that we can generate and store logs. It's a simple service that provides method for analyze,read,write,delete and performs operation on logs. All services under this architecture will be provide a method to sent a log stream data. Then the log service itself will be provide a mechanism to receive stream logs data and store in a Nosql db. 
Why NOSQL DB? It simple to maintain log file I think that a Mongodb or whatever nosql db based is a good solution to store document data type and with no context or relation between them.

The filename format for all logs file under Log Service will be: <b>{NAMESERVICE}_{DATETIME}.log (UTF-8 encode)</b>. If a log file exceed a certain filesize threshold(we'll be decide after a test on nosql db), an automatic script
send an email to notify the administrator. In case a file exceed the filesize, we store in a new file the most recent information and compress the old information in order to save space.

As for other microservice we provide (specially for log service) a backup solution that will be implementing ASAP.

