# TOOLS FOR EJPT

## Port Information:

| SERVICE               | PORT NUMBER |
|-----------------------|-------------|
| http, https           | 80, 443     |
| SMB                   | 445(TCP)    |
| SMB on top of NETBIOS | 139         |
| RDP                   | 3389        |

### Check what a command does

- `man`
    
    ```jsx
    man nmap
    ```
    
- `whatis`
    
    ```jsx
    whatis nmap
    ```
    
- `apropos` - if you want to search a string in the description of all the commands
    - eg - I want to search for a command to `change password`
    
    ```jsx
    apropos change password
    ```
    
    - if we want to filter it more
    
    ```jsx
    apropos -a <string>
    ```

### Directory busting

- gobuster
- dirb
- msfconsole
- dirbuster

### Payload generation

- msfvenom

### Password enumeration

- hydra
    - for MySQL user ‘root’ we can also provide a username list
        
        ```jsx
        hydra 192.140.141.3 -l root /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt  mysql
        ```
        
    - for WebDAV enumeration
        
        ```jsx
        hydra -L /usr/share/metasploit-framework/data/wordlists/common_users.txt -P /usr/share/metasploit-framework/data/wordlists/common_passwords.txt 10.5.19.162 http-get /webdav/
        ```
        
- metasploid(msfconsole)
    - for brute forcing a MySQL user password eg ‘root’
        
        ```jsx
        use auxiliary/scanner/mysql/mysql_login
        ```
        

### Download the source code

- curl
- wget

### Open the website in the command line

- browsh → open the browser in the command line

```jsx
browsh --startup-url <ip-address>
```

- lynx
  
```jsx
lynx https://domainname.com
```

## Ms-SQL(Microsoft based SQL) enumeration

- Nmap
    - for more info and NTLM info also
        
        ```jsx
        nmap 10.5.31.168 -sV -sC -p 1433
        ```
        
    - for information about the MSSQL database server
        
        ```jsx
        nmap 10.5.31.168 -p 1433 --script ms-sql-info
        ```
        
    - for brute forcing the users and their passwords on the MSSQL server
        
        ```jsx
        nmap 10.5.31.168 -p 1433 --script ms-sql-brute --script-args userdb=/root/Desktop/wordlist/common_users.txt,passdb=/root/Desktop/wordlist/100-common-passwords.txt
        ```
        
    - what user have an empty password(or anonymous login)
        
        ```jsx
        nmap 10.5.31.168 -p 1433 --script ms-sql-empty-password
        ```
        
    - Execute MSSQL query to extract sysusers(syslogins)
        
        ```jsx
        nmap 10.5.31.168 -p 1433 --script ms-sql-query --script-args mssql.username=admin,mssql.password=anamaria,ms-sql-query.query="SELECT * FROM master..syslogins" -oN output.txt
        ```
        
    - Execute a command on MSSQL
        
        ```jsx
        nmap 10.5.31.168 -p 1433 --script ms-sql-xp-cmdshell --script-args mssql.username=admin,mssql.password=anamaria,ms-sql-xp-cmdshell.cmd="type c:\flag.txt"
        ```
        
- msfconsole
    - brute forcing users and their passwords
        
        ```jsx
        use auxiliary/scanner/mssql/mssql_login
        ```
        
    - Running the MSSQL enumeration module to find all possible information
        
        ```jsx
        use auxiliary/admin/mssql/mssql_enum
        ```
        
    - Enumerate all MSSQL logins or extract MSSQL users
        
        ```jsx
        use auxiliary/admin/mssql/mssql_enum_sql_logins
        ```
        
    - Execute a command on the target machine
        
        ```jsx
        use auxiliary/admin/mssql/mssql_exec
        ```
        
    - Enumerate all available system users(or domain accounts)
        
        ```jsx
        use auxiliary/admin/mssql/mssql_enum_domain_accounts
        ```
        

## MySQL enumeration

- MySQL
    - connect with the database as ‘root’
        
        ```jsx
        mysql -h <IP address> -u root
        ```
        
- Nmap
    - MySQL user enumeration
        
        ```jsx
        nmap 192.99.192.3 -sV --script mysql-enum
        ```
        
    - what user have an empty password(or anonymous login)
        
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
        
    - Check whether File Privileges can be granted to non-admin users using mysql_audit nmap script.
        
        ```jsx
        nmap 192.99.192.3 -p 3306 -sV --script mysql-audit --script-args="mysql-audit.username='root', mysql-audit.password='',mysql-audit.filename='/usr/share/nmap/nselib/data/mysql-cis.audit'”
        ```
        
    - running our query on ‘root’
        
        ```jsx
        nmap 192.99.192.3 -p 3306 -sV --script mysql-query --script-args="query='Select * from books.authors;',username='root',password=''”
        ```
        
- *msfconsole*
    - for brute forcing a MySQL user password eg ‘root’
        
        ```jsx
        use auxiliary/scanner/mysql/mysql_login
        ```
        

## Basic my SQL commands

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
    
- to see the count of rows from a table
    
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
    
    ---
    

## WebDAV Exploitation

- using nmap
    - WebDAV directory enumeration
        
        ```jsx
        nmap 10.5.19.162 -p 80 --script http-enum
        ```
        
- using Metasploit

## Davtest → used to check what files can be uploaded on the WebDAV server

- davtest
    
    ```jsx
    davtest -auth bob:password_123321 -url http://10.5.19.162/webdav/
    ```
    

## Cadaver → connecting to the WebDAV server.

- cadaver
    
    ```jsx
    cadaver http://10.5.19.162/webdav/
    ```
    ---
