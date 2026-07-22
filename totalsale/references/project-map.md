# Project map

## Logical ownership

| Layer | Default root | Primary responsibility |
|---|---|---|
| Frontend | `D:\GolrangProject\TotalSales\Gng.TotalSales.UI` | Angular routes, forms, grids, client state, API calls |
| Backend | `D:\GolrangProject\TotalSales\Gng.TotalSales.BE` | Sales API, services, DTOs, authorization, Engine adapters |
| Engine | `D:\GolrangProject\TotalSales\Gng.TotalSales.Engines` | Commands, rules, state machines, domain models, repositories, caches |
| Database | `Gng.TotalSales.BE\Gng.Sales.Core\DBScripts` plus runtime metadata | SQL objects, reporting/read models, metadata configuration, indexes |
| Prior analysis | `D:\GolrangProject\TotalSales\SalesDistributor` | Dated investigations, business stories, and implementation proposals |

Database is a separate logical layer even though most checked-in SQL currently lives in the Backend repository.

## Repository boundaries

- UI, Backend, and Engine are separate Git repositories. Run Git commands at the owning root.
- Do not assume all repositories use the same branch name or are clean.
- `Gng.TotalSales.Engines.ApplicationCore` at the workspace root is not a Git root in the 2026-07-22 snapshot. Prefer the ApplicationCore project inside the Engine repository when tracing current Engine behavior.

## Fast discovery

Use `rg --files` before recursive directory listings.

```powershell
rg -n "PaymentCommandService|BatchInsertAndAssign" D:\GolrangProject\TotalSales\Gng.TotalSales.Engines
rg -n "InsertWithEngineAsync|SalesRuleEngineApi" D:\GolrangProject\TotalSales\Gng.TotalSales.BE
rg -n "payment-assign|AssignModel" D:\GolrangProject\TotalSales\Gng.TotalSales.UI
rg -n --glob "*.sql" "PaymentAssign" D:\GolrangProject\TotalSales\Gng.TotalSales.BE\Gng.Sales.Core\DBScripts
```

## Cross-layer trace

For a feature or bug, build this chain and note where the contract changes:

```text
Angular route/component
  -> Backend controller/service/DTO
  -> Engine API input/command/rules
  -> Repository + database tables/metadata
  -> response mapping back to UI
```

Also trace non-database effects separately: cache, notifier, external HTTP, audit/technical logging, outbox, and inventory dispatch.
