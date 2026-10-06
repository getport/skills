# Port_ API endpoints

Generated from the table the API is built from. Every change to the API rewrites this file, so do not edit it by hand.

Base URL `https://getport.app`. Every request carries `Authorization: Bearer $PORT_API_KEY`. Every answer is the envelope `{ data, asOf, notCounted, sources, generatedAt }`, and the fields listed under each endpoint are the ones inside `data`. Amounts and USD figures are strings.

Port, holdings, positions, perps, predictions, NFTs and activity are about the wallets you own: watched wallets and wallets you have excluded are left out, as they are on those screens, and a refresh reads only the same wallets. PnL covers every wallet on the account, as the PnL screen does, and so does `walletCount` in `/api/v1/me`. `/api/v1/wallets` lists every wallet, each marked.

Limits, per account by plan: Pro+ 300 a minute, 25,000 a day and 3 refreshes a day; Ultra 600, 100,000 and 3. Free has no API, and Pro has Pro+'s while the open beta runs. Past one, the answer is 429 with `Retry-After`.

## GET /api/v1/me

Who the key belongs to, their tier, wallet count and limit, and the key itself.

Fields of `data`: `userId`, `email`, `tier`, `walletCount`, `walletLimit`, `key`.

```bash
curl -s https://getport.app/api/v1/me -H "Authorization: Bearer $PORT_API_KEY"
```

## GET /api/v1/wallets

Every wallet on the account, with its group and nickname.

MCP tool: `port_wallets`.

Fields of `data`: `wallets`.

```bash
curl -s https://getport.app/api/v1/wallets -H "Authorization: Bearer $PORT_API_KEY"
```

## GET /api/v1/port

The headline: net worth, its parts, the figure per wallet and what is not counted.

MCP tool: `port_overview`.

Fields of `data`: `netWorthUsd`, `parts`, `byWallet`, `hidden`, `chains`.

```bash
curl -s https://getport.app/api/v1/port -H "Authorization: Bearer $PORT_API_KEY"
```

## GET /api/v1/holdings

Every holding row with its amount, its price and the confidence behind it, paged at 500.

MCP tool: `port_holdings`.

Parameters:

- `wallet` (query, string): One wallet address, to read only its rows.
- `chain` (query, string): One chain id, to read only its rows.
- `hidden` (query, boolean): Set to 1 or true to include the rows the product folds. Each one says why.
- `cursor` (query, string): The cursor from the previous page.

Notes:

- Pages end when `cursor` is null; pass it back as `?cursor=` for the next one. The cursor is opaque, so never build one.
- `total` counts every row the wallets hold, folded ones included and before any `?chain=` filter, so the pages add up to it only with `?hidden=1` and no chain. `complete: false` means the server also capped the rows it read for a port this size, and then the pages end short of `total` whatever you ask.
- `counted` says whether a row is inside the headline, and `hiddenReason` why the app folds it; the default page is the rows with no `hiddenReason`. The two differ: a small row is counted and folded, and a bought token quoted below the confidence bar is on screen and not counted. `/port` is still the total, because DeFi, venue balances and perp equity are on no holdings row.

Fields of `data`: `rows`, `total`, `cursor`, `complete`.

```bash
curl -s https://getport.app/api/v1/holdings -H "Authorization: Bearer $PORT_API_KEY"
```

## GET /api/v1/positions

DeFi accounts: one protocol on one chain in one wallet, with what is inside it.

MCP tool: `port_positions`.

Fields of `data`: `positions`.

```bash
curl -s https://getport.app/api/v1/positions -H "Authorization: Bearer $PORT_API_KEY"
```

## GET /api/v1/perps

Perp positions, orders and fills per venue, with when each venue was last read.

MCP tool: `port_perps`.

Fields of `data`: `positions`, `orders`, `fills`, `equityUsd`, `marginUsedPct`, `funding7dUsd`, `fees30dUsd`, `maintenanceMarginUsd`, `marginRatioPct`, `withdrawableUsd`, `notionalUsd`, `accountLeverage`, `fundingDailyUsd`, `funding7dMeasured`, `vaults`, `subAccounts`, `equityHistory`, `pnlHistory`, `netFlow7dUsd`, `volume7dUsd`, `liquidations`, `asOf`, `venues`, `accounts`.

```bash
curl -s https://getport.app/api/v1/perps -H "Authorization: Bearer $PORT_API_KEY"
```

## GET /api/v1/predictions

Polymarket and Limitless positions grouped by event, with whether the chain agreed on each.

MCP tool: `port_predictions`.

Fields of `data`: `positions`, `openUsd`, `openCostUsd`, `claimableUsd`, `realisedUsd`, `unverified`, `asOf`.

```bash
curl -s https://getport.app/api/v1/predictions -H "Authorization: Bearer $PORT_API_KEY"
```

## GET /api/v1/nfts

NFTs at floor, with their own total, which is not in net worth.

MCP tool: `port_nfts`.

Parameters:

- `hidden` (query, boolean): Set to 1 or true to include pieces with a phishing name and collections you hid.

Fields of `data`: `nfts`, `floorTotalUsd`, `note`, `folded`.

```bash
curl -s https://getport.app/api/v1/nfts -H "Authorization: Bearer $PORT_API_KEY"
```

## GET /api/v1/pnl

Trading profit from the ledger: realised and unrealised on swaps and fills, first in first out, with the open lots and what is missing.

MCP tool: `port_pnl`.

Fields of `data`: `method`, `realised30dUsd`, `realisedYtdUsd`, `unrealisedUsd`, `feesYtdUsd`, `fundingYtdUsd`, `byMonth`, `byAsset`, `bySource`, `unrealisedByWallet`, `lots`, `trades`, `fees`, `trades30dCount`, `complete`, `pricedEvents`, `unpricedEvents`, `unmatchedSales`, `unplacedMoves`, `income`, `hiddenByYou`, `viewableFrom`.

```bash
curl -s https://getport.app/api/v1/pnl -H "Authorization: Bearer $PORT_API_KEY"
```

## GET /api/v1/activity

The ledger, newest first, paged.

MCP tool: `port_activity`.

Parameters:

- `cursor` (query, string): The cursor from the previous page.
- `hidden` (query, boolean): Set to 1 or true to include what the Activity page folds: phishing names and what arrived unasked.

Notes:

- A page can come back empty with a cursor that is not null. Keep paging until the cursor is null.
- Transactions that arrived unasked or carry a phishing name are left out unless `hidden` is set, as the Activity page folds them, and `folded` counts them.
- Your plan decides how far back this reads: rows older than `viewableFrom` are not returned and the cursor ends there. Null means all of it. PnL is computed over everything either way.

Fields of `data`: `events`, `cursor`, `folded`, `viewableFrom`.

```bash
curl -s https://getport.app/api/v1/activity -H "Authorization: Bearer $PORT_API_KEY"
```

## GET /api/v1/briefing

The daily briefing as the server wrote it.

MCP tool: `port_briefing`.

Parameters:

- `day` (query, string): A day as YYYY-MM-DD. Defaults to the latest.

Notes:

- Answers 404 `no_briefing` when none has been written yet, or none for that day. That is the normal answer for a new account, not a fault.

Fields of `data`: `day`, `at`, `greeting`, `summary`, `stories`, `complete`, `news`.

```bash
curl -s https://getport.app/api/v1/briefing -H "Authorization: Bearer $PORT_API_KEY"
```

## GET /api/v1/alerts

Alerts that have fired, newest first.

MCP tool: `port_alerts`.

Fields of `data`: `alerts`.

```bash
curl -s https://getport.app/api/v1/alerts -H "Authorization: Bearer $PORT_API_KEY"
```

## POST /api/v1/refresh

Queues a fresh read of every wallet.

MCP tool: `port_refresh`.

Notes:

- Needs a key made with refresh allowed, or it answers 403 `refresh_not_allowed`. Counts against your plan's refreshes a day (3 a day on Pro+ and Ultra), and answers 202 with how many wallets were queued.

Fields of `data`: `queued`.

```bash
curl -s https://getport.app/api/v1/refresh -X POST -H "Authorization: Bearer $PORT_API_KEY"
```
