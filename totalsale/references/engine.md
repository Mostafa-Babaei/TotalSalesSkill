# Engine reference

## Contents

- Root and current payment preview branch
- Existing insert and assign pipeline
- Verified bulk preview behavior
- Preview safety checklist
- Receive discrepancy anchors

## Root and current payment preview branch

- Repository: `D:\GolrangProject\TotalSales\Gng.TotalSales.Engines`
- Verified branch on 2026-07-22: `zendehdel/preview-bulk-paymentassign`
- Verified commit: `d10b1213b1` (`preview bulk insert payment an dassign`)

Re-check the branch and commit before relying on branch-specific behavior.

## Existing insert and assign pipeline

The historical flow is:

```text
PaymentCommandController.Insert
  -> PaymentInsertCommand / PaymentInsertAction
  -> PaymentManager + PaymentGetter
  -> collection state machine, rules, aggregators, saver

PaymentAssignMultiStepCommand
  -> SalesDocMultiStepAssigner (invoice input order)
  -> AssignCalculator
  -> PaymentAssignCommand / PaymentAssignAction
  -> collection discount + invoice aggregators
  -> transaction per payment/invoice pair
```

Relevant anchors:

- `Gng.TotalSales.Engines.Api\Controllers\CollectionDoc\Command\PaymentCommandController.cs`
- `Gng.TotalSales.Engines.CollectionCommands\Domains\Collections\Payments`
- `Gng.TotalSales.Engines.CollectionCommands\Domains\Collections\PaymentAssigns`
- `Gng.TotalSales.Engines.CollectionCommands\Domains\Collections\CollectionDocs\SalesDocMultiStepAssigner.cs`
- `Gng.TotalSales.Engines.DomainService\Domains\Collections\PaymentAssigns\AssignCalculator.cs`
- `Gng.TotalSales.Engines.DiscountEngine\Domains\Payments\Rules\CollectionDiscountCalculation.cs`
- `Gng.TotalSales.Engines.SalesFlowCommands\Domains\SalesDocs\Assign\Aggregators\UpdateSalesDocAfterAssignAggregator.cs`

Invoice remaining amount historically follows:

```text
NetAmount
- TotalCollectionDiscountAmount
+ TotalAdditiveAmount
- TotalReductiveAmount
- AssignAmount
- EffectiveReturnAmount
```

Paid status also depends on settlement rounding. Therefore invoice shadow state cannot be represented by one remaining-amount number.

## Verified bulk preview behavior

At commit `d10b1213b1`, `PaymentCommandController.BatchInsertAndAssignAsync`:

- Routes `IsPreview=true` to a dedicated in-memory preview path.
- Begins `IPaymentAssignPreviewContext` and `IPreviewKpiStore` request-local overlays.
- Builds unsaved payment drafts, assigns them with `Id=0` and `PreviewPayment`, and carries projected invoice/payment state across the batch.
- Applies KPI deltas to the preview overlay only; the singleton credit cache remains untouched.
- Resets both contexts in `finally`.
- Returns structured payment, invoice, and allocation results.
- Collects errors per payment/invoice and remains partial-success capable; it does not require all-or-nothing behavior.

Supporting source and tests:

- `Gng.TotalSales.Engines.DomainCore\Domains\Collections\PaymentAssigns\Services\IPaymentAssignPreviewContext.cs`
- `Gng.TotalSales.Engines.DomainService\Domains\Collections\PaymentAssigns\PaymentAssignPreviewContext.cs`
- `Gng.TotalSales.Engines.Credit\Interfaces\Getter\IPreviewKpiStore.cs`
- `Gng.TotalSales.Engines.Credit\Services\PreviewKpiStore.cs`
- `Gng.TotalSales.Engines.DomainService.Test\Domains\Collections\PaymentAssigns\PaymentAssignPreviewContextTests.cs`
- `Gng.TotalSales.Engines.Credit.Test\Services\PreviewKpiStoreTests.cs`
- `Gng.TotalSales.Engines.RuleEngine.Test\Domains\Payments\StateMachine\CollectionDocStateMachineStopperTests.cs`

## Preview safety checklist

Verify every item for any new preview path:

- No EF/business `SaveChanges`.
- No mutation of singleton `ICreditCache`.
- No metadata cache mutation.
- No internal or external notifier execution.
- No `AfterSave` manipulator execution.
- No pre-Engine Backend writes such as cheque bank-account creation.
- Request-local contexts are initialized, isolated across concurrent requests, and reset in `finally`.
- Later payments observe earlier projected KPI and allocation state.
- Duplicate business keys inside the same unsaved batch are detected if relevant.
- Assignment candidates must be positive; stop when payment remainder reaches zero.
- Input order is explicit and deterministic.

Database rollback alone does not protect caches or external HTTP effects.

## Receive discrepancy anchors

Historical investigation identifies:

- `Gng.TotalSales.Engines.Api\Controllers\SalesDoc\SalesReceive\Command\SalesReceiveHeaderCommandController.cs`
- `SalesReceiveDetailsManipulateCommand`
- `SalesReceiveFlowCommandFactory`
- `SalesReceiveHeaderRepository`
- `SalesReturnPermitHeaderRepository`

The proposed invariant is:

```text
ReceivedQty + Sum(NotReceivedReasonQty) = ReturnPermitQty
```

Treat the proposed matching and unlock/relock flow in the source backlog as historical design until verified in current code.
