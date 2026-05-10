# kite-mcp-papertrading

[![Go Reference](https://pkg.go.dev/badge/github.com/algo2go/kite-mcp-papertrading.svg)](https://pkg.go.dev/github.com/algo2go/kite-mcp-papertrading)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

Paper trading virtual portfolio engine for the algo2go ecosystem.
Provides middleware interception of order placement to redirect to
an in-memory portfolio with ₹1 crore default cash, background
monitor for LIMIT fills, store with foreign-key integrity,
leak-sentinel for goroutine cleanup, and integration with
`kite-mcp-riskguard` for the same pre-trade safety checks as live
trading.

Used by [`Sundeepg98/kite-mcp-server`](https://github.com/Sundeepg98/kite-mcp-server)
across kc/manager_*, app/*, kc/ops/* (paper handlers), kc/telegram/*
(paper-mode commands), mcp/* (paper tool group, helpers test).

## Why a separate module?

Paper trading is an end-user feature applicable to any algo2go
consumer that wants risk-free strategy testing without committing
real capital. Hosting as its own module:

- Centralizes the Engine + Store + Monitor + Middleware contracts
- Lets virtual-portfolio policies (default cash, slippage rules,
  fill-watcher cadence) version independently
- Decouples paper-mode middleware from any one MCP server runtime

## Stability promise

**v0.x — unstable.** Pin `v0.1.0` deliberately.

## Install

```bash
go get github.com/algo2go/kite-mcp-papertrading@v0.1.0
```

## Public API

- `Engine` — orchestrates order placement, fills, P&L, cash
  accounting
- `Store` — in-memory portfolio + orders + positions with
  foreign-key integrity
- `Monitor` — background LIMIT fill watcher; configurable cadence
- `Middleware` — middleware-mode interception of `place_order`
  tool calls; routes to engine instead of broker
- Riskguard integration — same 8+ safety checks as live trading

## Dependencies

- `github.com/algo2go/kite-mcp-alerts` v0.1.0
- `github.com/algo2go/kite-mcp-broker` v0.1.0 (incl. /mock subpkg)
- `github.com/algo2go/kite-mcp-domain` v0.1.0
- `github.com/algo2go/kite-mcp-logger` v0.1.0
- `github.com/algo2go/kite-mcp-oauth` v0.1.0
- `github.com/algo2go/kite-mcp-riskguard` v0.1.0
- `github.com/stretchr/testify` v1.10.0

All algo2go deps published; no upstream `replace` directives needed.

## Reference consumer

[`Sundeepg98/kite-mcp-server`](https://github.com/Sundeepg98/kite-mcp-server)
— consumed across 18 .go files: kc/manager_*, kc/interfaces.go,
kc/broker_services.go, kc/ops/* (paper handlers, dashboard
credentials, admin dashboard, api handlers), kc/telegram/* (handler,
commands, bot edge), app/wire.go, app/app.go, mcp/helpers_test.go,
mcp/tools_session_test.go.

## License

MIT — see [LICENSE](LICENSE).

## Authors

Original design: [Sundeepg98](https://github.com/Sundeepg98) (Zerodha
Tech). Multi-module promotion (2026-05-10): algo2go contributors.
