```python
sqlite3 alerts.db 'ALTER TABLE "alerts" ADD COLUMN "status" TEXT DEFAULT "NEEDS_REVIEW";'
sqlite3 alerts.db 'UPDATE "alerts" SET "status"="NEEDS_REVIEW" WHERE "status" IS NULL OR TRIM("status")="";'
```
