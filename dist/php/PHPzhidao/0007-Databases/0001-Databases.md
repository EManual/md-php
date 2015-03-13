# Databases 
Many times your PHP code will use a database to persist information. You have a few options to connect and interact
with your database. The recommended option **until PHP 5.1.0** was to use native drivers such as [mysqli], [pgsql],
[mssql], etc.

Native drivers are great if you are only using _one_ database in your application, but if, for example, you are using
MySQL and a little bit of MSSQL, or you need to connect to an Oracle database, then you will not be able to use the
same drivers. You'll need to learn a brand new API for each database &mdash; and that can get silly.


[mysqli]: http://php.net/mysqli
[pgsql]: http://php.net/pgsql
[mssql]: http://php.net/mssql
