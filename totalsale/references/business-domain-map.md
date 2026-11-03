# Business domain learning index

This reference is a **topic index for planned investigation**, not a claim that every capability is implemented exactly as named.

| Domain | Core decisions to learn | Key layers |
|---|---|---|
| Sales documents | document types, relations, create/edit/finalize/cancel | UI, BE, Engine, metadata |
| State transitions | status-flow configs, permissions, rules, commands, retries | BE, Engine, SQL |
| Discounts | condition/result modeling, precedence, recalculation | Engine, DB, UI |
| Inventory | availability, reservation, issuance/return | BE, Engine, external |
| Collection and credit | payment, assignment, credit/KPI cache, settling | Engine, BE, SQL |
| Returns | return permit, receipt, source document effects | UI, BE, Engine |
| Field service | dispatch, delivery, linked docs and returns | UI, BE, Engine |
| SFA and distribution | external usage of docs, state inquiries | gateway, BE, Engine |
| Tax/e-invoice | invoice mapping, submit/cancel/reconciliation | BE, integration |
| Manufacturer vs distributor | policy/config differences vs source differences | all layers |

## Primary teaching method
Choose one real document, trace `UI → BE → Engine → DB/metadata → integrations`, record exceptions and actual observed side effects, and compare expected vs actual results. Mark each finding Verified / Historical / Proposed.

## Deep dive starter
State transition and command execution. Determine where the definitive business transition occurs before inferring rule execution order.

Refer to the current learning [roadmap](../../ROADMAP.md) and [checkpoint](../learning/PROGRESS.md).
