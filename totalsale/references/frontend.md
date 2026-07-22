# Frontend reference

## Root and framework

- Repository: `D:\GolrangProject\TotalSales\Gng.TotalSales.UI`
- Angular workspace: `GIG.App.Sl\ClientApp`
- Sales library: `GIG.App.Sl\ClientApp\projects\gig-app-sl\src\lib`

## Payment and assignment flow

Historical analysis points to these locations; verify current line numbers before editing:

- Route: `collection-operations\collection-operations-routing.module.ts`
- Payment page: `collection-operations\payment\payment.component.ts`
- Payment editor: `collection-operations\payment\payment-detail-form\paymnet-detail-form.component.ts`
- Payment base validation: `collection-operations\payment\directives\base-payment-detail.component.ts`
- Payment selection: `collection-operations\payment-assign\components\payment-assign-payments\payment-assign-payments.component.ts`
- Invoice selection: `collection-operations\shared\components\assignable-doc\assignable-doc.component.ts`
- Assignment orchestration: `collection-operations\shared\components\base-assign-form\base-assign-form.component.ts`
- Shared assignment state: `collection-operations\shared\services\assign-share.service.ts`
- Existing DTO: `collection-operations\shared\models\assign-model.ts`

Current historical behavior:

- `/payment` inserts each payment when the user adds it; the visible temporary grid is not a durable client-side draft list.
- `/payment-assign` selects one persisted payment and multiple invoices.
- Existing `AssignModel` carries one payment ID, multiple sales document IDs, and an optional total custom amount.
- `AssignShareService` is root-scoped and may retain stale state across page/modal lifetimes.

Treat these statements as `Historical` until confirmed on the current UI branch.

## Bulk settlement UI design

The prior proposed feature uses a separate page/facade with this state machine:

```text
Editing -> Previewing -> PreviewReady -> PreviewStale
                                |
                                v
                           Committing -> Completed | Failed
```

Use these invariants when implementing or reviewing it:

- Adding a payment creates only a local draft until commit.
- Any change to payments, invoices, order, or amount invalidates the preview and disables commit.
- Keep stable client keys for unsaved payments and a stable sequence for invoices across grid paging and sorting.
- Preview should expose allocation rows, payment summaries, invoice summaries, KPI before/delta/after/limit, predicted effects, and navigable warnings/errors.
- Reuse method-specific editors and LOVs only after removing implicit HTTP insert behavior.

## Frontend checks

1. Trace the exact request DTO and response mapping; do not assume Engine preview fields survive Backend mapping.
2. Test validation at both editor and page/facade level.
3. Test stale preview after every editable dimension.
4. Test partial success and per-payment/per-invoice error display; bulk preview is not inherently all-or-nothing.
5. Test selection stability across page, sort, filter, modal close/reopen, and duplicate clicks.
