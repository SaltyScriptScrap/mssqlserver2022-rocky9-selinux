Tutorial to install  MSSQL Server 2022   on   Redhat 9 / Rocky Linux 9 / CentOS Stream 9 / Alma Linux 9

summarized from https://learn.microsoft.com/en-us/sql/linux/quickstart-install-connect-red-hat?view=sql-server-linux-ver16&preserve-view=true&tabs=rhel9#install

# 1. Repository for MS SQL Server

```
sudo curl -o /etc/yum.repos.d/mssql-server.repo https://packages.microsoft.com/config/rhel/9/mssql-server-2022.repo
```

# 2. Repository for MS SQL Server Tools

```
sudo curl https://packages.microsoft.com/config/rhel/9/prod.repo | sudo tee /etc/yum.repos.d/mssql-release.repo
```
# 3. Download and Install SQL Server 
(Must choose with selinux or not! check with `getenforce` at terminal)

```
# with selinux status at gentenforce : Enforcing
sudo yum install -y mssql-server-selinux
```
OR 
```
# with selinux status at gentenforce : Permissive or Disabled 
sudo yum install -y mssql-server
```

# 4. Download and Install SQL Server Tools
```
sudo yum install -y mssql-tools
sudo yum install -y unixODBC-devel
```

# 5. Run the MS SQL Server setup
Choose Edition, Accept License Terms, Create *sa* (the system administator) password
```
sudo /opt/mssql/bin/mssql-conf setup
```
then check after installed
```
sudo systemctl status mssql-server
```

# 6. Set the firewall to open SQL Server on the network
```
sudo firewall-cmd --zone=public --add-port=1433/tcp --permanent
sudo firewall-cmd --reload
```

# 7. Setup SQL Server Agent 
(optional - for scheduling within MS SQL Server, not available for Express Editions)
```
sudo /opt/mssql/bin/mssql-conf set sqlagent.enabled true
```
then check after setup
```
sudo systemctl status mssql-server
```

# 8. Install SQL Server Full-Text Search on Linux 
(optional - for advanced search text in query, available on Express Edition with Advanced Service an above.)

```
# if you have add the repo no.2 above, you don't need to run this again:
sudo curl https://packages.microsoft.com/config/rhel/9/prod.repo | sudo tee /etc/yum.repos.d/mssql-release.repo
```
```
sudo yum install -y mssql-server-fts
```

then check after installed
```
sudo systemctl restart mssql-server
```

script to check within SQL Server Client
```
SELECT 
    CASE 
         WHEN 
             FULLTEXTSERVICEPROPERTY('IsFullTextInstalled') = 1 
         THEN 
              'INSTALLED' 
         ELSE 
              'NOT INSTALLED' 
    END IsFullTextInstalled
```




# 9. Download SQL Server Management Studio (Windows Client), 
open and download link: https://aka.ms/ssmsfullsetup then follow the instruction




# 10. Download Azure Data Studio (Linux Client, use Redhat .rpm file), 
open and find the download link at:
https://learn.microsoft.com/en-us/azure-data-studio/download-azure-data-studio


then install:
```
sudo yum install ./Downloads/azuredatastudio-linux-*<version string>*.rpm
```

after installation, run, just type at the terminal:
```
azuredatastudio
```

 # 11. Other useful link

 - MS SQL Server 2022 Editions : https://learn.microsoft.com/en-us/sql/sql-server/editions-and-components-of-sql-server-2022?view=sql-server-ver16
 - CLI Tools, sqlcmd : https://learn.microsoft.com/en-us/sql/tools/sqlcmd/sqlcmd-utility?view=sql-server-ver16&tabs=go%2Cwindows&pivots=cs1-bash
