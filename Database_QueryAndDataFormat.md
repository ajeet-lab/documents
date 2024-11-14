### Creating Database
```
CREATE DATABASE 
```

### Creating a Database
```
CREATE TABLE MessageTracking ( CorrelationID VARCHAR(50) PRIMARY KEY,
    OriginSubsystem VARCHAR(255),
    DestinationSubsystem VARCHAR(255),
    Status VARCHAR(255) DEFAULT 'Pending',
    CreatedAt DATETIME DEFAULT GETDATE(),
    ProcessedAt DATETIME NULL );
```

### DateTime for using Java
```
Date ProcessedAt= new java.sql.Timestamp(new Date().getTime());
Output: 2024-11-14 14:09:06.787
```
