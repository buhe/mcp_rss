[![MseeP.ai Security Assessment Badge](https://mseep.net/pr/buhe-mcp-rss-badge.png)](https://mseep.ai/app/buhe-mcp-rss)

# MCP RSS

MCP RSS is a Model Context Protocol (MCP) server for interacting with RSS feeds.

## Features

- Parse OPML files to import RSS feed subscriptions
- Automatically fetch and update articles from RSS feeds
- Expose RSS content through MCP API
- Mark articles as favorites
- Filter articles by source and status

## Installation

### Prerequisites

- Node.js (v14 or higher)
- MySQL database

### Setup MySQL

```bash
docker run -itd --name mysql-test -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 mysql
```

### Install MCP

This MCP uses stdio for communication. Fill in ``` npx mcp_rss ``` in the command, fill in the configuration that needs to be customized in the environment variable, and at least use your own opml file.

Use in claude desktop
```json
{
  "key": "rss",
  "command": "npx",
  "args": [
    "mcp_rss"
  ],
  "env": {
    "OPML_FILE_PATH": "/PATH/YOUR_FEED_FILE.opml"
  }
}

```

## Configuration

Set the following environment variables to configure MCP RSS:

### Configuration Options

| Option | Description | Default Value |
|--------|-------------|--------------|
| DB_HOST | Database host | localhost |
| DB_PORT | Database port | 3306 |
| DB_USERNAME | Database username | root |
| DB_PASSWORD | Database password | 123456 |
| DB_DATABASE | Database name | mcp_rss |
| OPML_FILE_PATH | Path to your OPML file with RSS feeds | "./feeds.opml" |
| RSS_UPDATE_INTERVAL | Interval to fetch RSS updates (in minutes) | 1 |

## MCP API Reference

The MCP RSS server exposes the following API methods:

### get_content

Get articles from subscribed RSS feeds.

**Parameters:**

| Parameter | Type | Description | Required |
|-----------|------|-------------|---------|
| status | string | Filter by article status ("normal" or "favorite") | No |
| source | string | Filter by source (feed title) | No |
| limit | number | Maximum number of articles to return | No (default: 10) |

**Response:**

```json
{
  "articles": [
    {
      "id": 1,
      "title": "Article Title",
      "content": "Article content...",
      "link": "https://example.com/article",
      "pubDate": "2023-01-01T12:00:00Z",
      "fetchDate": "2023-01-01T12:30:00Z",
      "status": "normal",
      "feedTitle": "Example Feed",
      "feedCategory": "Technology"
    }
  ],
  "success": true
}
```

### get_sources

Get all available RSS feed sources.

**Parameters:** None

**Response:**

```json
{
  "sources": [
    {
      "id": 1,
      "title": "Example Feed",
      "category": "Technology"
    }
  ],
  "success": true
}
```

### set_tag

Set the status of an article (normal or favorite).

**Parameters:**

| Parameter | Type | Description | Required |
|-----------|------|-------------|---------|
| status | string | Article status ("normal" or "favorite") | Yes |
| articleId | number | ID of the article to update | Yes |

**Response:**

```json
{
  "success": true,
  "message": "Article 1 status has been updated to favorite"
}
```

## License

MIT
