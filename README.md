import anyio

async def save_parquet(df: pd.DataFrame, path: str):
    await anyio.to_thread.run_sync(df.to_parquet, path, index=False)

async def main():
    async with anyio.create_task_group() as tg:
        for df, path in zip(dfs, output_paths):
            tg.start_soon(save_parquet, df, path)
