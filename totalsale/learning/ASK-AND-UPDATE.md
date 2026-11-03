# TotalSales Q&A and teaching workflow

When a user asks a TotalSales question:
1. Read `totalsale/SKILL.md`, `ROADMAP.md`, and `totalsale/learning/PROGRESS.md`.
2. Route the question using `references/topic-index.md` and relevant layer docs; use `business-domain-map.md` for new topics.
3. Give a **business explanation first**, then technical path, failure/side effects, and management decision implications.
4. Grade each claim as **Verified (current source), Historical (dated snapshot), Proposed/Unknown**.
5. If code access is unavailable, do not invent class names or transition behavior. Offer an explicit verification plan.
6. End a teaching lesson with a short scenario quiz and answer review.
7. Persist concrete verified new facts to the relevant reference and link source + revision; mark the issue checklist and update PROGRESS.md after material progress.

## Reusable answer template

**سؤال و فرض‌ها**  
**پاسخ بیزینسی**  
**مسیر اجرا در سیستم (با وضعیت شواهد)**  
**ریسک و Side Effectها**  
**نتیجه مدیریتی**  
**فایل/کامیت/تاریخ بررسی**  
**سؤال تمرینی یا اقدام بعدی**

For an impact analysis, add company/branch, document type, flow status, environment, related documents, rollback and integration matrix.

## Data safety
Repository is public. Never put confidential live traces, private work item descriptions, credentials or proprietary code in a GitHub Issue or docs.
