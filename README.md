# Kali-Linux-Enumeration-Tools-Reference-Repository
Your go-to resource for Kali Linux's top enumeration tools. Explore commands and techniques for efficient network reconnaissance, information gathering, and vulnerability assessment. Enhance your security prowess with this comprehensive guide


### Directory busting

- gobuster
- dirb
- msfconsole
- dirbuster

### Download the source code

- curl
- wget

### Open the website in the command line

- browsh → open the browser in the command line

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/f47dde30-36bf-4128-a5a2-df9580b08968/48ebad61-d33a-4908-83d9-1422799200d2/Untitled.png)

- lynx

 

## MySQL enumeration

- MySQL
    - connect with the database as ‘root’
        
        ```jsx
        mysql -h <IP address> -u root
        ```
        
- nmap
    - MySQL user enumeration
        
        ```jsx
        nmap 192.99.192.3 -sV --script mysql-enum
        ```
        
    - what user have an empty password(or anonymouslogin)
        
        ```jsx
        nmap 192.99.192.3 -p 3306 -sV --script mysql-empty-password
        ```
        
    - finding user through ‘root’
        
        ```jsx
        nmap 192.99.192.3 -p 3306 -sV --script mysql-users --script-args="mysqluser='root',mysqlpass=""”
        ```
        
    - finding databases on the SQL server through ‘root’
        
        ```jsx
        nmap 192.99.192.3 -p 3306 -sV --script mysql-databases --script-args="mysqluser='root',mysqlpass=""”
        ```
        
    - finding the data dir(or other variables) through ‘root’
        
        ```jsx
        nmap 192.99.192.3 -p 3306 -sV --script mysql-variables --script-args="mysqluser='root',mysqlpass=""”
        ```
        
    - Check whether File Privileges can be granted to non admin users using mysql_audit nmap script.
        
        ```jsx
        nmap 192.99.192.3 -p 3306 -sV --script mysql-audit --script-args="mysql-audit.username='root', mysql-audit.password='',mysql-audit.filename='/usr/share/nmap/nselib/data/mysql-cis.audit'”
        ```
        
    - running our query on ‘root’
        
        ```jsx
        nmap 192.99.192.3 -p 3306 -sV --script mysql-query --script-args="query='Select * from books.authors;',username='root',password=''”
        ```
        
- *msfconsole*

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/f47dde30-36bf-4128-a5a2-df9580b08968/0dcde4d3-1daf-4413-9ff0-4949c7123ca3/Untitled.png)

## Basic my SQL commads

- Connect to the SQL server
    
    ```jsx
    mysql -h <IP address> -u <user>;
    ```
    
- to see the databases
    
    ```jsx
    Show databases;
    ```
    
- to select a database
    
    ```jsx
    Use <database name>;
    ```
    
- to see tables
    
    ```jsx
    Show tables;
    ```
    
- to see the count of rows from table
    
    ```jsx
    Select count(*) from <tablename>;
    ```
    
- to see the values inside the table
    
    ```jsx
    Select * from <tablename>;
    ```
    
- to open a file in the SQL database
    
    ```jsx
    Select load_file("/etc/shadow");
    ```
