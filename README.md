```python
from __future__ import annotations

import sqlite3
from pathlib import Path
from typing import Any, Iterable, Optional

import pandas as pd


class SQLiteQueryRunner:
    """
    Ad-hoc SQLite runner:
    - One short-lived connection per call (safe for notebooks/ad-hoc usage)
    - SELECT -> pandas DataFrame
    - UPDATE/DELETE/ALTER/INSERT -> commits automatically
    """

    def __init__(self, db_path: str | Path):
        self.db_path = str(db_path)

    def _connect(self) -> sqlite3.Connection:
        conn = sqlite3.connect(self.db_path)
        conn.row_factory = sqlite3.Row
        return conn

    def select_df(
        self,
        query: str,
        params: Optional[Iterable[Any]] = None,
    ) -> pd.DataFrame:
        q = query.strip().upper()
        if not q.startswith("SELECT"):
            raise ValueError("select_df only accepts SELECT queries.")
        with self._connect() as conn:
            return pd.read_sql_query(query, conn, params=params)

    def execute_write(
        self,
        query: str,
        params: Optional[Iterable[Any]] = None,
    ) -> dict:
        q = query.strip().upper()
        allowed = ("UPDATE", "DELETE", "INSERT", "ALTER", "CREATE", "DROP")
        if not q.startswith(allowed):
            raise ValueError(
                "execute_write only accepts UPDATE/DELETE/INSERT/ALTER/CREATE/DROP queries."
            )

        conn = self._connect()
        try:
            cur = conn.cursor()
            cur.execute(query, tuple(params) if params is not None else ())
            conn.commit()  # <-- critical for persistence
            return {
                "rowcount": cur.rowcount,
                "lastrowid": cur.lastrowid,
                "query": query,
            }
        except Exception:
            conn.rollback()
            raise
        finally:
            conn.close()


runner = SQLiteQueryRunner("alerts.db")

# SELECT
df = runner.select_df('SELECT status, COUNT(*) AS cnt FROM alerts GROUP BY status')
print(df)

# UPDATE
out = runner.execute_write(
    'UPDATE "alerts" SET "status" = ? WHERE "status" = ?',
    params=("NEEDS_REVIEW", "status"),
)
print(out)

# ALTER
runner.execute_write(
    'ALTER TABLE "alerts" ADD COLUMN "status" TEXT DEFAULT "NEEDS_REVIEW"'
)

```
