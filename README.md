# Algolia MCP Cursor Integration

_This is a submission for the [Algolia MCP Server Challenge](https://dev.to/challenges/algolia-2025-07-09)_

## What I Built  
A Cursor-powered integration that "listens" for a `Run Setup for project "X"` command and, using Algolia's MCP Server, automatically discovers your application, indices, and settings—and writes them into a persistent `ALGOLIA_PROJECT_CONFIG.md` file. This ensures your search configuration lives in your repo (not just in LLM context), survives restarts, and can be version-controlled alongside your code.

## 🚀 Quick Start

### Prerequisites
- [Cursor IDE](https://cursor.dev) installed
- MCP (Model Context Protocol) server configured

### 1. Setup Cursor Rules
1. Copy the `algolia-main-rules.mdc` and `algolia-reference.mdc` files to your Cursor rules folder.

2. The rule will automatically activate when Cursor detects Algolia-related queries

### 2. Initialize Your Project
Simply ask Cursor:
```
Run setup for project "your-algolia-app-name"
```

Or more specifically:
```
List apps I have in Algolia and if it is the only one, run setup project for the app
```

### 3. That's It! 🎉
The integration will automatically:
- Discover your Algolia applications
- Fetch all indices and their configurations
- Generate a persistent `ALGOLIA_PROJECT_CONFIG.md` file
- Set up your project for immediate use

## 📋 How It Works

### Automatic Discovery Flow
1. **Authentication**: Connects to your Algolia account via MCP
2. **Application Discovery**: Calls `getUserInfo()` → `getApplications()` 
3. **Index Discovery**: Runs `listIndices()` for each application
4. **Configuration Extraction**: For each index, calls `getSettings()` to collect:
   - Custom ranking criteria
   - Faceting attributes
   - Searchable attributes
   - Typo tolerance settings
   - Analytics configuration
5. **File Generation**: Creates/updates `ALGOLIA_PROJECT_CONFIG.md` with all discovered data

### Generated Configuration Structure
```markdown
# Algolia Project Configuration

## Application Details
- **Application ID**: `YOUR_APP_ID`
- **Application Name**: `Your App Name`
- **Region**: `us` or `eu`
- **Owner**: `true/false`

## Available Indices
- `products` - Product catalog (10,000 records)
- `content` - Blog content (5,000 records)

## Index Configurations
### products
- **Custom Ranking**: `desc(rating), desc(popularity)`
- **Faceting Attributes**: `category, brand, price, rating`
- **Searchable Attributes**: `title, description, category`
- **Last Updated**: `2024-01-15`

## Recent Changes
- 2024-01-15-10:30: Project configuration initialized
- 2024-01-15-10:35: Added faceting for brand attribute
```

## 🔥 Key Features

### 1. **Project Memory**
- Configuration persists across Cursor restarts
- No more losing search settings when LLM context resets
- Version control your Algolia configuration alongside your code

### 2. **Zero Configuration**
- No manual setup required
- Automatic discovery of all applications and indices
- Intelligent defaults based on your existing Algolia setup

### 3. **Idempotent Operations**
- Safe to rerun multiple times
- Automatically detects changes and updates configuration
- Maintains change history in the configuration file

### 4. **Cursor Integration**
- Works seamlessly with Cursor
- Natural language commands for complex operations
- Intelligent suggestions based on your configuration

## 💡 Usage Examples

### Basic Search Setup
```
Search for "laptop" in my products index with category and brand facets
```

### Data Management
```
Add a new product to my index with the following details: [product info]
```

### Analytics Insights
```
Show me the top 10 searches from last month and their performance
```

### Configuration Updates
```
Add price as a faceting attribute to my products index
```

## How I Utilized the Algolia MCP Server

### Core MCP Integration
- **Discovery**: `getUserInfo()` → `getApplications()` → `listIndices()` to fetch app and index metadata
- **Configuration**: For each index, `getSettings()` to collect ranking, facets, searchable attributes, typo tolerance, etc.
- **Search Operations**: `searchSingleIndex()` for querying data with intelligent parameter handling
- **Data Management**: `saveObject()`, `partialUpdateObject()`, `batch()` for adding/updating records
- **Analytics**: `getTopSearches()`, `getNoResultsRate()`, `getTopHits()` for performance insights

### Intelligent Automation
- **Context Awareness**: Automatically references stored configuration for consistent operations
- **Smart Defaults**: Uses your existing index settings as defaults for new operations
- **Change Tracking**: Maintains a complete audit log of all configuration changes

## 🔧 Advanced Configuration

### Custom Rules
You can extend the Cursor rules by modifying `algolia-main-rules.mdc`:

```markdown

## 📊 Key Takeaways

### Project Memory
- **Problem**: Algolia settings vanish when LLM context resets
- **Solution**: Persistent `ALGOLIA_PROJECT_CONFIG.md` file stores all configuration
- **Benefit**: Consistent search behavior across development sessions

### Automation
- **Problem**: Manual setup for each new project is time-consuming
- **Solution**: Embedded MCP calls in Cursor rules automate discovery
- **Benefit**: Developers can bootstrap new projects in seconds

## 🚨 Troubleshooting

### Common Issue

**MCP connection issues**
```bash
# Check your MCP server configuration:
# - Verify Algolia auth
# - Ensure MCP server is running
# - Check network connectivity
```

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- [Algolia](https://www.algolia.com) for providing the MCP Server
- [Cursor](https://cursor.com) for the amazing AI-powered IDE

---

_Solo project by @turazashvili_ 
