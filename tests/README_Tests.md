
# BI Assistant Tests

These tests check CSV loading, missing-price preservation,
database imports, schema inspection, and response generation.

## Install dependencies

Run from the project root:

    python -m pip install pytest pandas groq

Install the application's other dependencies from
requirements.txt if you have not already done so.

## Run tests

    python -m pytest -q

The tests use temporary CSV files and SQLite databases.
Groq API calls are mocked, so a real API key is not required
for these tests.

## Important limitations

These tests verify that missing prices remain missing during
CSV loading. They do not yet verify SQL safety, revenue
calculations, or exclusion of returned transactions.

Additional tests should cover those behaviors after the
SQL validation and execution functions are identified.
