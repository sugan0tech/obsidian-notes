linux:
- special script has to be executed [ref](https://learn.microsoft.com/en-us/sql/linux/quickstart-install-connect-ubuntu?view=sql-server-ver16&tabs=ubuntu2004)
	or this command `sudo /opt/mssql/bin/mssql-conf setup`
- Default administrator name will be of `sa` with the password given during setup
- connection string: `Server=localhost;Database=Matrimony;User Id=sa;Password=Sugan@123;TrustServerCertificate=True;
```json
  "ConnectionStrings": {
    "defaultConnection": "Server=localhost;Database=DB;User Id=sa;Password=Pass;TrustServerCertificate=True;"
  },
```

windows:
- Standard installation ( download from web )
- connection string: `Data Source=B4RBBX3\\SQLEXPRESS;Integrated Security=true;TrustServerCertificate=True;Initial Catalog=DBName;`
