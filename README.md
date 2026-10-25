# Jobicy Next.js Job Board

A small, production-shaped remote job board using Next.js App Router, TypeScript, server rendering, hourly caching, and original Jobicy application links.

## Start locally

```bash
git clone https://github.com/Jobicy/nextjs-job-board.git
cd nextjs-job-board
npm install
npm run dev
```

Open [http://localhost:3000](http://localhost:3000). Node.js 20.9 or newer is required.

```bash
npm run typecheck
npm test
npm run build
npm start
```

## Filtering and pagination

Use the keyword, location slug, and industry slug fields. The application forwards these as the official Jobicy API `tag`, `geo`, and `industry` query parameters. Discover current [location slugs](https://jobicy.com/api/jobs?get=locations) and [industry slugs](https://jobicy.com/api/jobs?get=industries).

The application requests `count=12` from Jobicy and renders that API page. **Older listings** passes the opaque `nextCursor` as `cursor`, retaining the filters; **Latest listings** starts a fresh traversal. It shows the number of listings on the current page without inventing a total page count. Filter and cursor state live in URL query parameters. Cursors expire after 24 hours; an HTTP 400 continuation shows a restart link retaining the filters. The feed includes only the last seven days. Upstream responses are cached and revalidated once per hour.

[OpenAPI JSON](https://jobicy.com/api/openapi.json) · [OpenAPI YAML](https://jobicy.com/api/openapi.yaml)

Each card displays the employer, optional logo, remote location, employment type, categories, publication date, optional salary, short excerpt, and canonical Jobicy URL. HTML-bearing descriptions are not rendered or republished. Separate loading, empty, and error states remain accessible on narrow screens.

Every public interface includes **[Jobs powered by Jobicy](https://jobicy.com/)** attribution. Keep this attribution and the original listing URLs when adapting the project.

## Stored listings

If you extend this integration to retain listings, use the [batch status endpoint](https://github.com/Jobicy/remote-jobs-api#batch-status-check) to check up to 100 IDs at a time. Older open jobs can remain `active` after leaving the seven-day feed. Do not mark jobs closed just because a fresh feed omits them, or because a check returns `unknown` or fails. The sample does not automatically reconcile stored listings.

## Developer resources

[All integrations](https://github.com/Jobicy/jobicy-api-examples) · [API contract](https://github.com/Jobicy/remote-jobs-api) · [MCP](https://github.com/Jobicy/remote-jobs-mcp-server)

Public interfaces must show **[Jobs powered by Jobicy](https://jobicy.com/)** and preserve original listing URLs. Start automated feed synchronization at most once per hour.

## License

[MIT](LICENSE) for code; see [source and data terms](PROVENANCE.md).

## Deploy

Use this repository as a GitHub template, then import your copy into Vercel. Select the Next.js framework preset and keep the repository root as the project root. No Jobicy API key or environment variables are needed for the public feed. Deployment happens in your own hosting account.

For another Node.js host, run `npm ci`, `npm run build`, then `npm start`. Configure the host's public URL and port as usual. Keep the original Jobicy links and source attribution when rebranding the page.

The repository includes automated type, pagination and production-build checks. Credential-free local checks do not establish connectivity from your hosting provider to Jobicy; verify a search after deployment.
