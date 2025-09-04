# SQLite Library for Universal Windows Platform (UWP)

[![NuGet Package](https://img.shields.io/nuget/v/SQLite.Universal.svg)](https://www.nuget.org/packages/SQLite.Universal/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

A comprehensive SQLite library for Universal Windows Platform (UWP) applications, providing native SQLite support for **x86**, **x64**, **ARM**, and **ARM64** architectures.

## 🎯 Why This Package?

**Replace Visual Studio SQLite Extension** - This NuGet package is designed as a **drop-in replacement** for the [SQLite for Universal Windows Platform](https://marketplace.visualstudio.com/items?itemName=SQLiteDevelopmentTeam.SQLiteforUniversalWindowsPlatform) Visual Studio extension.

### ✅ Advantages Over VS Extension:

- 🚫 **No Extension Installation Required** - Works immediately after NuGet package installation
- 🔄 **Easier CI/CD Integration** - No need to install extensions on build servers
- 📦 **Project-Level Dependency** - SQLite version managed per project, not globally
- 🔧 **Simplified Setup** - No manual reference management or project configuration
- 🏗️ **Better Build Reliability** - Eliminates extension dependency issues in automated builds
- 👥 **Team-Friendly** - All team members get the same SQLite version automatically

### 🔄 Migration from VS Extension:

If you're currently using the Visual Studio SQLite extension:

1. **Uninstall the extension** (optional, but recommended to avoid conflicts)
2. **Remove SQLite references** from your project
3. **Install this NuGet package**: `Install-Package SQLite.Universal`
4. **Rebuild your project** - that's it!

No code changes required - your existing SQLite code will work unchanged.

## ✨ Features

- 🚀 **Multi-Architecture Support**: Native binaries for x86, x64, ARM, and ARM64
- 📦 **Easy Integration**: Simple NuGet package installation - **replaces VS SQLite extension**
- 🔧 **No Extension Required**: Works without Visual Studio SQLite extension dependency
- 🏗️ **Build-Ready**: Automatic file copying to output directory
- ⚡ **High Performance**: Native SQLite implementation
- 🔒 **Thread-Safe**: Full mutex and shared cache support
- 🚫 **CI/CD Friendly**: No extension installation needed on build servers

## 🚀 Quick Start

### Basic Usage Example

```csharp
using SQLite;
using System.IO;
using Windows.Storage;

// Define your data model
public class Person
{
    [PrimaryKey, AutoIncrement]
    public int Id { get; set; }
    
    [MaxLength(50)]
    public string Name { get; set; }
    
    public int Age { get; set; }
}

// Initialize database connection
var databasePath = Path.Combine(ApplicationData.Current.LocalFolder.Path, "MyDatabase.db");
var connection = new SQLiteConnection(databasePath);

// Create table
connection.CreateTable<Person>();

// Insert data
var person = new Person { Name = "John Doe", Age = 30 };
connection.Insert(person);

// Query data
var people = connection.Table<Person>().Where(p => p.Age > 25).ToList();
```

### Advanced Connection Setup

```csharp
var connectionString = new SQLiteConnectionString(
    databasePath, 
    false  // storeDateTimeAsTicks
);

var connection = new SQLiteConnectionWithLock(
    connectionString,
    SQLiteOpenFlags.ReadWrite | 
    SQLiteOpenFlags.Create | 
    SQLiteOpenFlags.FullMutex | 
    SQLiteOpenFlags.SharedCache
);
```

## 🏗️ Building from Source

### Prerequisites

- Visual Studio 2017 or later
- Windows 10 SDK
- UWP development workload

### Build Steps

1. **Clone the repository**:
   ```bash
   git clone https://github.com/danny8002/SQLite3.Universal.git
   cd SQLite3.Universal
   ```

2. **Open the solution**:
   ```bash
   start SQLite.Universal.sln
   ```

3. **Build for all platforms**:
   - Set solution configuration to `Release`
   - Build for `x86`, `x64`, `ARM`, and `ARM64` platforms
   - Output files will be in `Build/{Platform}/Release/`

4. **Package NuGet** (optional):
   ```bash
   cd NuGet
   nuget.exe pack package.nuspec
   ```

## 🔄 Upgrading SQLite Version

To upgrade to a newer SQLite version from the official SQLite website:

### Step 1: Download SQLite Source

1. Visit [SQLite Download Page](https://www.sqlite.org/download.html)
2. Download the **Source Code** → **sqlite-amalgamation-XXXXXXX.zip**
3. Extract the downloaded archive

### Step 2: Replace Source Files

Replace the following files in [`SQLite.Universal/SourceCode/`](SQLite.Universal/SourceCode/) with the new versions:

- `sqlite3.c` → Main SQLite implementation
- `sqlite3.h` → Header file with function declarations
- `sqlite3ext.h` → Extension header (if available)

### Step 3: Update Version Information

1. **Update package version** in [`NuGet/package.nuspec`](NuGet/package.nuspec):
   ```xml
   <version>3.XX.X</version>
   ```

2. **Update README** (this file):
   ```markdown
   - **Current SQLite Version**: [3.XX.X](https://www.sqlite.org/chronology.html)
   ```

### Step 4: Build and Test

1. **Rebuild the solution** for all platforms:
   - Clean solution: `Build` → `Clean Solution`
   - Rebuild: `Build` → `Rebuild Solution`

2. **Test with sample application**:
   - Run [`Samples/TestApplicationStorage`](Samples/TestApplicationStorage/) project
   - Verify database operations work correctly

3. **Run compatibility tests**:
   - Test on different UWP target versions
   - Verify on all supported architectures

### Step 5: Version Verification

Add version checking to your application:

```csharp
// Get SQLite version at runtime
var version = SQLite3.LibVersionNumber();
var versionString = SQLite3.LibVersion();
Console.WriteLine($"SQLite Version: {versionString} ({version})");
```

### Step 6: Update Documentation

- Update changelog with breaking changes (if any)
- Document new SQLite features available
- Update NuGet package and publish

### 🔧 Troubleshooting Upgrades

**Build Errors**: Check for new compilation flags or dependencies in the new SQLite version.

**Runtime Issues**: Test thoroughly as newer SQLite versions may have behavioral changes.

**Performance**: Benchmark critical operations to ensure no regression.

**Compatibility**: Verify database files created with older versions still work.

## 📁 Project Structure

```
SQLite3.Universal/
├── SQLite.Universal/          # Main library project
│   ├── SourceCode/           # SQLite source files
│   │   ├── sqlite3.c         # SQLite implementation
│   │   ├── sqlite3.h         # Main header
│   │   └── sqlite3ext.h      # Extension header
│   └── SQLite.Universal.vcxproj
├── NuGet/                    # NuGet package configuration
│   ├── package.nuspec        # Package specification
│   └── uap10.0/              # Build properties
├── Samples/                  # Example applications
│   └── TestApplicationStorage/
└── README.md
```

## 📋 Requirements

- **Target Framework**: UWP (UAP 10.0 or later)
- **Supported Architectures**: x86, x64, ARM, ARM64
- **Minimum Windows Version**: Windows 10 version 1803 (Build 17134)
- **Development**: Visual Studio 2017 or later with UWP workload

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 📞 Support

- **Issues**: [GitHub Issues](https://github.com/danny8002/SQLite3.Universal/issues)
- **Documentation**: [SQLite.org Documentation](https://www.sqlite.org/docs.html)
- **NuGet**: [Package Page](https://www.nuget.org/packages/SQLite.Universal/)

## 🙏 Acknowledgments

- [SQLite Development Team](https://www.sqlite.org/crew.html) for the excellent database engine
- [sqlite-net](https://github.com/praeclarum/sqlite-net) for inspiration and ORM patterns
- UWP developer community for feedback and contributions

---

**Note**: This library provides the native SQLite binaries. For ORM functionality, consider using it with [sqlite-net-pcl](https://www.nuget.org/packages/sqlite-net-pcl/) or similar packages.
