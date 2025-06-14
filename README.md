# MySQL Database Synchronizer

A modern Python GUI application for synchronizing MySQL databases between a master and multiple slave instances. The application features schema validation, real-time progress tracking, and a clean, modern user interface.

## Features

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
- MySQL databases (master and slave instances)
- Required Python packages (see requirements.txt)

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
2. Enter slave database credentials:
   - Name (unique identifier for this slave)
   - Host, Port, Username, Password, Database
3. Test the connection and save
4. Repeat for additional slave databases

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

### Schema Validation (Flexible Approach)
- **Missing = CRITICAL**: Slave databases MUST have all master tables and columns
- **Extra = INFO**: Slave databases CAN have additional tables and columns (ignored during sync)
- **Data Type Compatibility**: Critical data type mismatches prevent synchronization
- **Primary Key Validation**: Ensures primary keys exist for synchronization logic
- **Smart Sync**: Only syncs master columns, ignores extra slave columns
- **User Choice**: When issues found, user can Skip problem slaves or Force sync
- **Detailed Reports**: Clear explanation of what each issue means

### Record Synchronization
- Uses primary keys to identify existing records
- Only inserts records that don't exist in slave databases
- Processes records in batches for optimal performance
- Maintains data integrity throughout the process
- **Flexible Column Handling**: Syncs only master columns, ignores extra slave columns

### Schema Compatibility Rules

| Scenario | Result | Action |
|----------|--------|--------|
| Slave missing master table | ❌ **ERROR** | Sync blocked - slave must have all master tables |
| Slave has extra table | ✅ **OK** | Extra table ignored during sync |
| Slave missing master column | ❌ **ERROR** | Sync blocked - slave must have all master columns |
| Slave has extra column | ✅ **OK** | Extra column ignored during sync |
| Data type mismatch | ❌ **ERROR** | Sync blocked - data types must be compatible |
| Missing primary key in slave | ❌ **ERROR** | Sync blocked - primary keys required for sync logic |
| Extra constraints in slave | ✅ **OK** | Additional constraints are fine |

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
   - Ensure MySQL server is running
   - Verify firewall settings

2. **Schema Validation Failed**:
   - Review schema differences in the log
   - Ensure slave databases have the same structure as master
   - Check for missing tables or columns

3. **Synchronization Errors**:
   - Check database permissions (INSERT privileges required on slaves)
   - Verify primary key constraints
   - Review error logs for specific issues

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

## 💝 Created with Love

**MySQL Database Synchronizer** is created with ❤️ by **TweekersNut Network**

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
