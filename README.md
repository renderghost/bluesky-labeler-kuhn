# Bluesky Labeler - Thomas Kuhn Foundation

A Bluesky labeler service that automatically classifies research papers shared on Bluesky using Kuhnian epistemic framework analysis powered by the Thomas Kuhn Foundation's KGX3 API.

## What Does This Labeler Do?

This labeler monitors research papers shared on Bluesky and automatically applies classification labels based on the Thomas Kuhn Foundation's science intelligence framework. It categorizes papers into four epistemic categories:

- **Normal Science** (60%) - Research confirming existing frameworks
- **Model Drift** (25%) - Research introducing variations to established models
- **Model Crisis** (13%) - Research creating stress on current paradigms
- **Paradigm Shift** (2%) - Research fundamentally breaking established models

The classification is based on three dimensions:

- **(M)ethod** - Research approach
- **(N)evidence** - Supporting data
- **(P)osition** - Role in the field

## Why Is It Useful?

This labeler helps Bluesky users:

- **Identify emerging research trends** before they become mainstream
- **Track paradigm shifts** in real-time across scientific disciplines
- **Filter research content** based on epistemic significance
- **Discover breakthrough research** (Model Crisis and Paradigm Shift papers)
- **Curate their scientific feed** by subscribing to labels of interest

By applying stackable moderation labels, users can customize their Bluesky experience to surface the types of scientific contributions most relevant to their interests.

## Tech Stack

- **Runtime**: [Node.js](https://nodejs.org/) v22.11.0 (LTS)
- **Package Manager**: npm
- **Language**: TypeScript
- **Core Libraries**:
  - [@atproto/api](https://www.npmjs.com/package/@atproto/api) - AT Protocol API client
  - [@skyware/labeler](https://www.npmjs.com/package/@skyware/labeler) - Bluesky labeler utilities
  - [@skyware/jetstream](https://www.npmjs.com/package/@skyware/jetstream) - Real-time ATProto event streaming
  - [@skyware/bot](https://www.npmjs.com/package/@skyware/bot) - Bluesky bot framework
- **Web Framework**: Express.js
- **Database**: better-sqlite3
- **Monitoring**: Prometheus metrics with prom-client
- **Logging**: Pino

## Requirements

- [Node.js](https://nodejs.org/) v22.11.0 or higher
- npm (comes with Node.js)
- A Bluesky account to convert into a labeler
- Server with public-facing URL (for labeler endpoint)
- Access to Thomas Kuhn Foundation KGX3 API (for classification)

## Getting Started

### 1. Initial Setup

Clone the repository and install dependencies:

```bash
git clone https://github.com/yourusername/bluesky-labeler-kuhn.git
cd bluesky-labeler-kuhn
npm install
```

### 2. Convert Account to Labeler

Run the setup wizard to convert your Bluesky account into a labeler:

```bash
bunx @skyware/labeler setup
```

You can exit after converting the account - labels will be configured via code.

### 3. Configure Environment

Copy `.env.example` to `.env` and fill in your credentials:

```env
DID=did:plc:xxx
SIGNING_KEY=xxx
BSKY_IDENTIFIER=xxx
BSKY_PASSWORD=xxx
HOST=127.0.0.1
PORT=4100
METRICS_PORT=4101
FIREHOSE_URL=wss://jetstream.atproto.tools/subscribe
CURSOR_UPDATE_INTERVAL=10000
```

A `cursor.txt` file will be created automatically if it doesn't exist.

### 4. Configure Labels

Edit the Kuhnian classification labels in [src/constants.ts](src/constants.ts):

- Label IDs (normal-science, model-drift, model-crisis, paradigm-shift)
- Descriptions
- Related post content

Run these commands to create/update labels:

```bash
npm run set-posts    # Create announcement posts
npm run set-labels   # Create label definitions
```

Copy the generated post record keys back into [src/constants.ts](src/constants.ts).

### 5. Configure Reverse Proxy

The labeler needs to be publicly accessible. Example [Caddy](https://caddyserver.com/) configuration:

```Caddyfile
labeler.example.com {
    reverse_proxy 127.0.0.1:4100
}
```

### 6. Start the Labeler

```bash
npm run start
```

Verify it's working by checking: `https://labeler.example.com/xrpc/com.atproto.label.queryLabels`

A new labeler returns: `{"cursor":"0","labels":[]}`

### Monitoring

Prometheus metrics are exposed on the configured `METRICS_PORT` (default: 4101). Use [this Grafana dashboard](https://grafana.com/grafana/dashboards/11159-nodejs-application-dashboard/) to visualize the metrics.

## How to Contribute

Contributions are welcome! Here's how you can help:

1. **Fork the repository** and create a feature branch
2. **Make your changes** with clear commit messages
3. **Test thoroughly** - ensure labels are applied correctly
4. **Submit a pull request** with a description of your changes

### Development Commands

```bash
npm run dev        # Start with hot reload
npm run format     # Format code with Prettier
npm run lint       # Lint code with ESLint
npm run lint:fix   # Auto-fix linting issues
```

### Areas for Contribution

- Improving classification accuracy
- Adding support for additional research metadata
- Enhancing label descriptions
- Building analytics dashboards
- Documentation improvements
- Testing and bug reports

## Credits

### Technology Foundation

- **[Alice](https://bsky.app/profile/did:plc:by3jhwdqgbtrcc7q4tkkv3cf)** - Creator of the [Bluesky Labeler Starter Kit](https://github.com/aliceisjustplaying/bluesky-labeler-starter) and [Zodiac Sign Labels](https://github.com/aliceisjustplaying/zodiacsigns)
- **[Juliet](https://bsky.app/profile/did:plc:b3pn34agqqchkaf75v7h43dk)** - Author of the [Pronouns labeler](https://github.com/notjuliet/pronouns-bsky)
- **[futur](https://bsky.app/profile/did:plc:uu5axsmbm2or2dngy4gwchec)** - Creator of [skyware libraries](https://skyware.js.org/)

### Classification System

- **[Thomas Kuhn Foundation](https://thomaskuhnfoundation.org/)** - KGX3 science intelligence engine and Kuhnian epistemic framework

### References

- [Bluesky Labelers Documentation](https://docs.bsky.app/docs/advanced-guides/moderation#labelers)
- [KGX3 API Documentation](https://thomaskuhnfoundation.org/kgx3-api)

## License

MIT License

Copyright (c) 2024 Alice <aliceisjustplaying@gmail.com>

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
