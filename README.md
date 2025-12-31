# MCP India Tenders Server

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.0-blue.svg)](https://www.typescriptlang.org/)
[![Node.js](https://img.shields.io/badge/Node.js-18+-green.svg)](https://nodejs.org/)
[![MCP](https://img.shields.io/badge/MCP-SDK-purple.svg)](https://github.com/anthropics/anthropic-sdk-typescript)

An **MCP (Model Context Protocol) server** for searching and analyzing Indian government tenders from multiple portals including CPPP (Central Public Procurement Portal), eProc Rajasthan, and Defence Ministry tenders.

## Overview

This server provides AI assistants (Claude, Cursor, etc.) with real-time access to Indian government tender data in **OCDS (Open Contracting Data Standard)** format. It enables automated tender discovery, analysis, and monitoring for businesses bidding on government contracts.

### Features
- **Multi-Portal Support**: CPPP, eProc Rajasthan, Defence portals
- **OCDS-Compliant**: Structured data following Open Contracting standard
- **Advanced Search**: Filter by keyword, state, value, closing date
- **Tender Analytics**: Budget, funding, bidder analysis
- **Caching**: Optimized performance with intelligent caching
- **AI-Friendly**: Natural language queries through MCP interface
- **Docker Ready**: Easy deployment with Docker/Docker Compose

## Quick Start

### Installation

```bash
git clone https://github.com/switchr24/mcp-india-tenders.git
cd mcp-india-tenders
npm install
npm run build
```

### Running the Server

**Development:**
```bash
npm run dev
```

**Production:**
```bash
npm start
```

### With Docker

```bash
docker build -t mcp-india-tenders .
docker run -p 3000:3000 mcp-india-tenders
```

## MCP Tools

### `search_tenders`
Search for tenders across all portals with filters

**Parameters:**
- `keyword` (required): Tender search term (e.g., "road sweeper", "waste compactor")
- `state` (optional): State filter (e.g., "RAJASTHAN", "MAHARASHTRA")
- `minValue` (optional): Minimum tender value in INR
- `closingWithinDays` (optional): Tenders closing in N days (default: 60)
- `limit` (optional): Max results (default: 20, max: 100)

**Example:**
```json
{
  "keyword": "road sweeper Rajasthan",
  "minValue": 1000000,
  "closingWithinDays": 30,
  "limit": 10
}
```

### `get_tender_details`
Fetch detailed OCDS-compliant tender information

**Parameters:**
- `tenderId` (required): Tender ID or OCID
- `format` (optional): 'full' or 'summary'

### `search_by_location`
Search tenders by city/district and state

**Parameters:**
- `location` (required): City name (e.g., "Jaipur")
- `state` (required): State code (e.g., "RJ")
- `limit` (optional): Max results

### `search_by_organisation`
Find tenders by procuring ministry/department

**Parameters:**
- `organisation` (required): Ministry/Department name
- `limit` (optional): Max results

## Configuration

### Claude Desktop Integration

Edit `~/.config/Claude/claude_desktop_config.json`:

```json
{
  "mcpServers": {
    "india-tenders": {
      "command": "node",
      "args": ["/path/to/mcp-india-tenders/build/index.js"]
    }
  }
}
```

### VS Code Cline Integration

Add to Cline MCP settings:

```json
{
  "mcpServers": {
    "india-tenders": {
      "command": "node",
      "args": ["/path/to/mcp-india-tenders/build/index.js"]
    }
  }
}
```

## Usage Examples

### Example 1: Find Rajasthan Tenders
```
User: "Show me all waste equipment tenders in Rajasthan above 5 lakhs closing in 30 days"

MCP Call: search_tenders(
  keyword="waste equipment",
  state="RAJASTHAN",
  minValue=5000000,
  closingWithinDays=30
)
```

### Example 2: Analyze Specific Tender
```
User: "Get full details and analysis for tender CPPP-12345"

MCP Call: get_tender_details(
  tenderId="CPPP-12345",
  format="full"
)
```

### Example 3: Location-based Search
```
User: "Find all government tenders in Jaipur"

MCP Call: search_by_location(
  location="Jaipur",
  state="RJ",
  limit=20
)
```

## Supported Portals

| Portal | URL | Coverage | Status |
|--------|-----|----------|--------|
| CPPP | eprocure.gov.in | All Central Ministries | ✅ Supported |
| eProc Rajasthan | eproc.rajasthan.gov.in | Rajasthan State | ✅ Supported |
| Defence | defproc.gov.in | Ministry of Defence | ✅ Supported |
| GeM | gem.gov.in | Goods & Services | ⚠️ Planning |
| State Portals | Various | Individual States | 🔄 Expanding |

## Project Structure

```
mcp-india-tenders/
├── src/
│   ├── index.ts                 # MCP server entry
│   ├── api/
│   │   ├── eprocure-client.ts   # CPPP API client
│   │   ├── rajasthan-client.ts  # Rajasthan eProc client
│   │   └── defence-client.ts    # Defence portal client
│   ├── handlers/
│   │   ├── tools.ts             # MCP tool definitions
│   │   └── resources.ts         # MCP resources
│   ├── models/
│   │   ├── tender.ts            # OCDS schemas
│   │   └── portal.ts            # Portal config
│   └── utils/
│       ├── logger.ts            # Logging
│       └── cache.ts             # Caching layer
├── build/                       # Compiled JavaScript
├── package.json
├── tsconfig.json
├── Dockerfile
└── docker-compose.yml
```

## Development

### Building
```bash
npm run build
```

### Testing
```bash
npm run test
```

### Using MCP Inspector
```bash
npm run inspector
```

## Roadmap

- [ ] Add more state portals (Gujarat, Maharashtra, Karnataka)
- [ ] Integration with n8n for automation
- [ ] Tender analytics dashboard
- [ ] Email alerts for matching tenders
- [ ] Bid success prediction model
- [ ] Document OCR for tender analysis
- [ ] API Gateway for REST access

## Performance Optimization

- **Caching**: 5-minute cache for searches, 1-hour for details
- **Rate Limiting**: Respectful scraping with delays
- **Pagination**: Efficient data retrieval
- **Compression**: Gzip response compression

## Error Handling

The server gracefully handles:
- Portal downtime
- Network timeouts
- CAPTCHA blocking
- Malformed data
- Rate limiting

## Legal & Terms

- Uses publicly available government tender data
- Respects portal terms of service
- No authentication credentials stored
- Respectful scraping practices

## Contributing

Contributions welcome! Please:
1. Fork the repository
2. Create a feature branch
3. Submit a pull request

## Support

- **Issues**: Report bugs on GitHub Issues
- **Discussions**: Ask questions in GitHub Discussions
- **Email**: support@ensol.com

## License

MIT License - see LICENSE file

## Author

Created by Ensol Multiclean Equipment for government tender automation

---

**Last Updated:** December 31, 2025  
**Version:** 1.0.0
