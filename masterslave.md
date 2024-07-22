Setting up a master-slave (or primary-secondary) replication setup in SQL Server along with a C# application involves several steps. Here's a detailed guide to help you through the process.

### Step 1: Configure SQL Server for Replication

#### 1. Install SQL Server on Master and Slave Machines
- Install SQL Server on both the master and the slave machines. You can use SQL Server Express, Developer, or any other edition that supports replication.

#### 2. Configure the Master Server
1. **Enable SQL Server Agent**:
   - Open SQL Server Configuration Manager.
   - Ensure that the SQL Server Agent service is running.

2. **Configure Distribution**:
   - Open SQL Server Management Studio (SSMS).
   - Connect to the master server.
   - Right-click on the Replication folder and select "Configure Distribution".
   - Follow the wizard to configure the distribution database.

3. **Create a Publication**:
   - Right-click on the "Local Publications" folder and select "New Publication".
   - Follow the wizard to create a new publication, selecting the database and tables you want to replicate.

#### 3. Configure the Slave Server
1. **Enable SQL Server Agent**:
   - Ensure that the SQL Server Agent service is running on the slave server.

2. **Create a Subscription**:
   - Connect to the slave server using SSMS.
   - Right-click on the "Local Subscriptions" folder and select "New Subscription".
   - Follow the wizard to subscribe to the publication created on the master server.

### Step 2: Implement the C# Application

#### 1. Install Required NuGet Packages
Make sure you have the necessary NuGet packages installed in your project:
- `System.Data.SqlClient`

```bash
dotnet add package System.Data.SqlClient
```

#### 2. Implement Database Connection and Failover Logic
Here's an example of how to set up a C# application to connect to the master database and failover to the slave database if the master is unavailable.

```csharp
using System;
using System.Data.SqlClient;

namespace MasterSlaveReplication
{
    class Program
    {
        static string masterConnectionString = "Server=MASTER_SERVER_NAME;Database=DATABASE_NAME;User Id=USERNAME;Password=PASSWORD;";
        static string slaveConnectionString = "Server=SLAVE_SERVER_NAME;Database=DATABASE_NAME;User Id=USERNAME;Password=PASSWORD;";

        static void Main(string[] args)
        {
            try
            {
                // Attempt to connect to the master server
                using (var connection = new SqlConnection(masterConnectionString))
                {
                    connection.Open();
                    Console.WriteLine("Connected to master database.");
                    // Perform database operations
                }
            }
            catch (SqlException ex)
            {
                Console.WriteLine("Failed to connect to master database. Attempting to connect to slave database.");
                // Attempt to connect to the slave server
                try
                {
                    using (var connection = new SqlConnection(slaveConnectionString))
                    {
                        connection.Open();
                        Console.WriteLine("Connected to slave database.");
                        // Perform read-only database operations
                    }
                }
                catch (SqlException exSlave)
                {
                    Console.WriteLine("Failed to connect to slave database. Error: " + exSlave.Message);
                }
            }
        }
    }
}
```

### Step 3: Testing the Setup

1. **Insert Test Data**: Insert data into the master database and verify that it is replicated to the slave database.
2. **Simulate Failover**: Turn off the master server and ensure that the application connects to the slave server.
3. **Check Data Consistency**: Ensure that the data on the slave database is consistent with the master database.

### Conclusion

Setting up master-slave replication in SQL Server and implementing failover logic in a C# application involves configuring replication on the SQL Server instances and writing code to handle failover scenarios. This setup helps ensure high availability and reliability of your database system.
