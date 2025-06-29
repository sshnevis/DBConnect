# DBConnect
an open source library class to connect to MSSQL for select and execute scripts easily.

# Very Simple Database Connection Class for C# Applications

## Introduction

Using a database is essential for storing and retrieving data in applications, whether you're a beginner or a professional programmer. The `DBConnect` class is designed to simplify reading data from an MS SQL database using the `DataTable` object. You can execute a query by calling a single function.

`DBConnect` is compatible with C# applications and ASP.NET Framework websites.

Suggest you see our websites: [Talashnet](https://talashnet.com/) and [Exir](https://exirmatab.com)

## Total Code

```csharp
using System;
using System.Collections.Generic;
using System.Data;
using System.Data.SqlClient;
using System.Linq;
using System.Text;

namespace EasyTeb
{
    public class DBConnect
    {
        SqlConnection connection;
        SqlCommand cmd;
        SqlDataAdapter adapter;
        bool isset = false;

        public DBConnect()
        {
            connection = new SqlConnection("Server=.\\SQLEXPRESS;Database=TalashNet;Integrated Security=True;");
        }

        public string Script(string Query)
        {
            if (isset)
            {
                try
                {
                    cmd = new SqlCommand(CheckInject(Query), connection);
                    object result = cmd.ExecuteScalar();
                    if (result == null)
                        return "1";
                    else
                        return result.ToString();
                }
                catch (Exception ex)
                {
                    return ex.Message;
                }
            }
            return "0";
        }

        public DataTable Select(string Query)
        {
            if (isset)
            {
                DataTable dt = new DataTable();
                adapter = new SqlDataAdapter(CheckInject(Query), connection);
                adapter.Fill(dt);
                return dt;
            }
            return new DataTable();
        }

        public void Connect()
        {
            if (!isset)
            {
                connection.Open();
                isset = true;
            }
        }

        public void DisConnect()
        {
            if (isset)
            {
                connection.Close();
                //connection = null;
                adapter = null;
                cmd = null;
                isset = false;
            }
        }

        public string CheckInject(string sql)
        {
            sql = sql.Replace("--", " ");
            sql = sql.Replace("/*", " ");
            //sql = sql.Replace('%', ' ');
            //sql.Replace('*', ' ');
            return sql;
        }

        public string CheckInjectText(string sql)
        {
            sql = sql.Replace(',', ' ');
            sql.Replace('$', ' ');
            sql.Replace('^', ' ');
            sql.Replace('%', ' ');
            return sql;
        }
    }
}
```

## Using the Code

1. Create a `.cs` class file and add the above code to it.
2. In your form, use the following code to call a query from SQL Server.

### Executing a `SELECT` Query

To perform a `SELECT` query and retrieve data into a `DataTable`:

```csharp
string query = "SELECT * FROM [MyTable]";
DBConnect db = new DBConnect();
db.Connect();
DataTable dt = db.Select(query);
db.DisConnect();
```

Now, the `DataTable` contains the results of your query. You can display it in a `DataGridView` or use the data in your code.

### Displaying `DataTable` in ASP.NET `GridView`

```csharp
GridView1.DataSource = dt;
GridView1.DataBind();
```

### Displaying `DataTable` in C# Windows Application `DataGridView`

```csharp
DataGridView1.DataSource = dt;
```

### Executing `DELETE`, `INSERT`, or `UPDATE` Queries

To perform `DELETE`, `INSERT`, or `UPDATE` queries:

```csharp
DBConnect db = new DBConnect();
db.Connect();
db.Script(query);
db.DisConnect();
```

## Points of Interest

The `DBConnect` class simplifies interaction with SQL Server, enabling programmers to write database-driven applications more efficiently. It is designed to streamline database operations and improve development speed.

## History

This class was first written in 2008 and is currently in its second version.
