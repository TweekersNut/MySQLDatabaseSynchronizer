# MySQL Database Synchronizer v2.0

A modern Python GUI application for synchronizing databases between a master and multiple slave instances. **Now supports MySQL, PostgreSQL, and Microsoft SQL Server** with intelligent cross-database schema translation. Features schema validation, real-time progress tracking, and a clean, modern user interface.

## Features

### 🆕 Version 2.0 - Multi-Database Support
- **🔄 Cross-Database Synchronization**: Sync between MySQL, PostgreSQL, and Microsoft SQL Server
- **🧠 Intelligent Schema Translation**: Automatic data type conversion between database systems
- **🛠️ Enhanced Table Creation**: Improved handling of complex default values and constraints
- **⚡ Robust Error Handling**: Better error recovery and detailed logging for troubleshooting
- **🔧 Schema Compatibility Fixes**: Resolves issues with quoted defaults and function calls

### Core Features
- **🔒 MASTER DATABASE PROTECTION**: Multiple security layers ensure master database is **NEVER modified**
- **Modern GUI**: Built with CustomTkinter for a sleek, modern interface
- **Master-Slave Configuration**: Easy setup for one master and multiple slave databases
- **Schema Validation**: Automatic schema comparison before synchronization
- **Smart Synchronization**: Only syncs records that don't exist in slave databases
- **Progress Tracking**: Real-time progress bars and detailed status logging
- **Configuration Persistence**: Saves database configurations for future use
- **Error Handling**: Comprehensive error reporting and logging
- **Threading**: Non-blocking operations with background synchronization
- **Read-Only Master Access**: Master database connections are enforced as read-only
- **🚀 Large Database Support**: Optimized for databases with millions of records
- **Memory Efficient**: Chunked processing prevents out-of-memory issues
- **Performance Monitoring**: Real-time progress and performance metrics

## Requirements

- Python 3.7 or higher
- **Supported Databases**:
  - **MySQL** (master and/or slave instances)
  - **PostgreSQL** (master and/or slave instances)
  - **Microsoft SQL Server** (master and/or slave instances)
- Required Python packages (see requirements.txt)

### Database Driver Requirements
- **MySQL**: `mysql-connector-python`
- **PostgreSQL**: `psycopg2`
- **SQL Server**: `pyodbc` (requires ODBC driver)

## Installation

1. **Clone or download the project**:
   ```bash
   git clone <repository-url>
   cd mysql-sync
   ```

2. **Install dependencies**:
   ```bash
   pip install -r requirements.txt
   ```

3. **Run the application**:
   ```bash
   python main.py
   ```

## Usage

### 1. Configure Master Database

1. Open the application
2. Go to the "Configuration" tab
3. Click "Configure Master" button
4. Enter your master database credentials:
   - Host (e.g., localhost)
   - Port (e.g., 3306)
   - Username
   - Password
   - Database name
5. Click "Test Connection" to verify
6. Click "Save" to store the configuration

### 2. Add Slave Databases

1. In the "Configuration" tab, click "Add Slave"
2. **Select Database Type**: Choose from MySQL, PostgreSQL, or SQL Server
3. Enter slave database credentials:
   - Name (unique identifier for this slave)
   - Host, Port, Username, Password, Database
   - **Database Type**: MySQL, PostgreSQL, or SQL Server
4. Test the connection and save
5. Repeat for additional slave databases

### 🆕 Cross-Database Synchronization
You can now sync between different database types:
- **MySQL → PostgreSQL**: Automatic schema translation and data type conversion
- **MySQL → SQL Server**: Intelligent type mapping and constraint handling
- **PostgreSQL → MySQL**: Reverse compatibility with proper type conversion
- **Any combination**: Full support for mixed database environments

### 3. Start Synchronization

1. Switch to the "Synchronization" tab
2. Click "Start Sync" button
3. Monitor progress through:
   - Progress bar showing overall completion
   - Status log with detailed messages
   - Statistics showing tables processed, records synced, etc.

### 4. Monitor Progress

The synchronization process includes:
- **Schema Validation**: Compares table structures between master and slaves
- **Record Comparison**: Identifies missing records in slave databases
- **Batch Insertion**: Efficiently inserts missing records
- **Error Handling**: Reports and logs any issues encountered

## How It Works

### 🆕 Cross-Database Schema Translation (v2.0)
- **Intelligent Type Mapping**: Automatic conversion between MySQL, PostgreSQL, and SQL Server data types
- **Default Value Translation**: Proper handling of database-specific default values and functions
- **Constraint Conversion**: Automatic translation of primary keys, foreign keys, and indexes
- **Function Mapping**: Converts database-specific functions (e.g., `current_timestamp()` → `current_timestamp`)
- **Compatibility Validation**: Pre-sync validation ensures successful schema translation
- **Error Recovery**: Robust error handling for complex schema translation scenarios

### Schema Validation & Auto-Creation
- **🆕 Automatic Table Creation**: Missing tables in slaves are created automatically with proper schema translation
- **Missing Tables = AUTO-FIX**: App creates missing tables using translated master schema
- **Missing Columns = CRITICAL**: Column differences require manual schema updates
- **Extra Content = INFO**: Slave databases CAN have additional tables/columns (ignored)
- **Data Type Compatibility**: Automatic type conversion between different database systems
- **Primary Key Validation**: Ensures primary keys exist for synchronization logic
- **Smart Sync**: Only syncs master columns, ignores extra slave columns
- **User Choice**: When issues found, user can Skip problem slaves or Force sync
- **Detailed Reports**: Clear explanation of what each issue means

### Record Synchronization
- **🆕 Works with Empty Databases**: Can sync to completely empty slave databases
- **Automatic Table Creation**: Creates missing tables before data sync
- Uses primary keys to identify existing records
- Only inserts records that don't exist in slave databases
- Processes records in batches for optimal performance
- Maintains data integrity throughout the process
- **Flexible Column Handling**: Syncs only master columns, ignores extra slave columns

### Schema Compatibility Rules

| Scenario | Result | Action |
|----------|--------|--------|
| Slave missing master table | 🔧 **AUTO-FIX** | Table created automatically using translated schema |
| Slave has extra table | ✅ **OK** | Extra table ignored during sync |
| Slave missing master column | ❌ **ERROR** | Sync blocked - requires manual schema update |
| Slave has extra column | ✅ **OK** | Extra column ignored during sync |
| Cross-database type conversion | 🔧 **AUTO-CONVERT** | Types automatically converted (e.g., MySQL INT → PostgreSQL INTEGER) |
| Missing primary key in slave | ❌ **ERROR** | Sync blocked - primary keys required for sync logic |
| Extra constraints in slave | ✅ **OK** | Additional constraints are fine |
| Empty slave database | 🔧 **AUTO-FIX** | All master tables created automatically with proper translation |
| Different database systems | 🔧 **AUTO-TRANSLATE** | Full schema translation between MySQL, PostgreSQL, SQL Server |

### 🆕 Cross-Database Type Mappings (v2.0)

#### MySQL → PostgreSQL
- `INT` → `INTEGER`, `VARCHAR(n)` → `VARCHAR(n)`, `TEXT` → `TEXT`
- `DATETIME` → `TIMESTAMP`, `TINYINT(1)` → `BOOLEAN`
- `AUTO_INCREMENT` → `SERIAL`, `LONGTEXT` → `TEXT`

#### MySQL → SQL Server
- `INT` → `INT`, `VARCHAR(n)` → `NVARCHAR(n)`, `TEXT` → `NTEXT`
- `DATETIME` → `DATETIME2`, `TINYINT(1)` → `BIT`
- `AUTO_INCREMENT` → `IDENTITY(1,1)`, `LONGTEXT` → `NTEXT`

#### PostgreSQL → MySQL
- `SERIAL` → `INT AUTO_INCREMENT`, `BOOLEAN` → `TINYINT(1)`
- `TIMESTAMP` → `DATETIME`, `TEXT` → `LONGTEXT`

### 🔒 Security & Safety Features
- **GUARANTEED READ-ONLY MASTER**: Multiple security layers prevent any master database modifications
- **Connection-Level Protection**: Master connections automatically set to read-only mode
- **Query Validation**: All queries checked and write operations blocked on master
- **Method-Level Safeguards**: Write methods explicitly reject master database operations
- **Security Violation Logging**: All unauthorized attempts are logged and blocked
- **Transaction-based insertions**: Safe insertions on slave databases only
- **Comprehensive audit trails**: Complete logging of all operations
- **Connection testing**: Verification before any operations

## Configuration Files

The application creates the following files:
- `config/database_config.json`: Stores database configurations
- `logs/mysql_sync.log`: Application logs

## Troubleshooting

### Common Issues

1. **Connection Failed**:
   - Verify database credentials
   - Check network connectivity
   - Ensure database server is running (MySQL/PostgreSQL/SQL Server)
   - Verify firewall settings
   - **SQL Server**: Ensure ODBC driver is installed

2. **Schema Validation Failed**:
   - Review schema differences in the log
   - **Cross-database**: Check type conversion compatibility
   - Check for missing tables or columns
   - **v2.0 Fix**: Improved handling of quoted defaults and function calls

3. **Synchronization Errors**:
   - Check database permissions (INSERT privileges required on slaves)
   - Verify primary key constraints
   - Review error logs for specific issues
   - **Cross-database**: Ensure target database supports converted data types

4. **🆕 Table Creation Issues (Fixed in v2.0)**:
   - **Previous Issue**: Tables failing due to quoted NULL defaults (`'NULL'`)
   - **Previous Issue**: Function calls in quotes (`'current_timestamp()'`)
   - **Previous Issue**: Double-quoted string values (`''0''`)
   - **✅ Fixed**: All schema translation issues resolved in v2.0

### 🚀 Performance for Large Databases

The application is optimized to handle large databases efficiently:

- **Chunked Processing**: Processes records in configurable chunks (default: 10K records)
- **Memory Management**: Never loads entire tables into memory
- **Batch Inserts**: Inserts multiple records per query (default: 1K records)
- **Large Table Detection**: Automatically optimizes for tables > 100K records
- **Progress Tracking**: Real-time progress updates during sync
- **Configurable Settings**: Tune performance for your specific database size

**Expected Performance:**
- Small databases (< 1M records): 5,000-10,000 records/second
- Large databases (> 10M records): 1,000-3,000 records/second

**Performance Tips:**
- Use SSD storage for better I/O performance
- Ensure proper indexing on primary key columns
- Monitor system resources during synchronization
- Run during off-peak hours for large databases
- See [PERFORMANCE.md](PERFORMANCE.md) for detailed optimization guide

## 🔒 Security Guarantee

**MASTER DATABASE IS 100% PROTECTED** - This application implements multiple independent security layers:

1. **Database Session Level**: Master connections are set to `READ ONLY` mode
2. **Query Validation**: All queries are analyzed and write operations are blocked
3. **Method Protection**: Write methods explicitly reject master database operations
4. **Runtime Verification**: Every operation verifies target database before execution
5. **Exception Handling**: Security violations immediately stop execution

**See [SECURITY.md](SECURITY.md) for complete security documentation.**

### 🆕 New Components in v2.0
- **schema_translator.py**: Handles cross-database schema translation
- **cross_db_converter.py**: Manages data type conversions between database systems
- **postgresql_manager.py**: PostgreSQL-specific database operations
- **sqlserver_manager.py**: SQL Server-specific database operations
- **database_factory.py**: Factory pattern for database manager selection

## 📋 Changelog

### Version 2.0 (Latest)
**🚀 Major Release: Multi-Database Support**

#### ✨ New Features
- **Cross-Database Synchronization**: Full support for MySQL, PostgreSQL, and Microsoft SQL Server
- **Intelligent Schema Translation**: Automatic data type conversion between database systems
- **Enhanced Table Creation**: Improved handling of complex default values and constraints
- **Robust Error Handling**: Better error recovery and detailed logging

#### 🐛 Bug Fixes
- **Fixed**: Table creation failures due to quoted NULL defaults (`'NULL'` → proper handling)
- **Fixed**: Function calls in quotes (`'current_timestamp()'` → `current_timestamp`)
- **Fixed**: Double-quoted string values (`''0''` → `'0'`)
- **Fixed**: Tuple index out of range errors in primary key detection
- **Fixed**: Schema format validation issues

#### 🔧 Improvements
- **Enhanced**: Error handling with better exception management
- **Enhanced**: Schema validation with cross-database compatibility checks
- **Enhanced**: Logging with more detailed error information
- **Enhanced**: UI responsiveness at 200% zoom scale

#### 📊 Performance
- **Resolved**: Issue where only 158/269 tables were created (now creates all tables successfully)
- **Improved**: Schema translation performance for large databases
- **Optimized**: Memory usage during cross-database operations

### Version 1.0
- Initial release with MySQL-to-MySQL synchronization
- Basic schema validation and table creation
- GUI interface with progress tracking
- Master database protection features

## 💝 Created with Love

**Database Synchronizer v2.0** is created with ❤️ by **TweekersNut Network**

- **Contact**: admin@tweekersnut.com
- **Website**: https://tweekersnut.com
- **Support**: Professional database solutions and custom development

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests if applicable
5. Submit a pull request

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Support

For issues and questions:
1. Check the troubleshooting section above
2. Review the application logs in `logs/mysql_sync.log`
3. Contact us at admin@tweekersnut.com
4. Visit https://tweekersnut.com for professional support
