# AI Business Intelligence Assistant

A natural-language business intelligence assistant that allows users to ask questions about sales data and receive data-backed answers without manually writing SQL queries.

## Features

- **Natural Language Queries:** Ask business questions in plain English.
- **Dynamic SQL Generation:** Uses Groq LLMs to generate SQLite queries based on the database schema.
- **SQL Validation:** Validates generated queries before execution.
- **Business-Friendly Responses:** Converts query results into readable explanations.
- **Sales Analytics:** Supports revenue analysis, product performance, regional comparisons, and monthly trends.
- **CSV Data Import:** Loads sales data into SQLite.
- **Graceful Unsupported-Question Handling:** Provides a friendly response when questions cannot be answered from the available data.
- **Automated Tests:** Includes tests for database operations, response generation, analytics, and SQL validation.

## Dataset and Data Management

The project currently includes the original sales dataset in `sample_sales.csv` for demonstration and evaluation. The dataset is replaceable, allowing users to analyze their own CSV files without changing the application code.

The application dynamically reads the database schema, enabling SQL generation based on the available tables and columns rather than relying on hardcoded column definitions.

**Data and code are kept separate by design:**
- Replace the CSV file with another compatible dataset.
- Keep business logic and application code independent of the dataset's contents.
- Import the replacement dataset into SQLite before querying it.
- Keep sensitive datasets, database files, and credentials out of public version control.

This design makes the assistant adaptable to different datasets while keeping the core application reusable.

## Technology Stack

| Component | Technology |
|---|---|
| User interface | Streamlit |
| Programming language | Python |
| LLM integration | Groq API |
| Database | SQLite |
| Data processing | Pandas |
| Testing | Pytest |

## Project Structure

```text
ai-business-intelligence-assistant/
├── app.py
├── src/
│   ├── database.py
│   ├── interpreter.py
│   ├── query_executor.py
│   ├── response_generator.py
│   ├── sql_validator.py
│   └── text_to_sql.py
├── tests/
│   ├── test_analytics.py
│   ├── test_database.py
│   ├── test_response_generator.py
│   └── test_sql_validator.py
├── sample_sales.csv
├── requirements.txt
├── README.md
├── README_TESTS.md
├── .env.example
└── .gitignore
```

## Prerequisites

- Python 3.10 or later
- A Groq API key
- Internet access for LLM requests

## Installation and Setup

**1. Clone the repository**

```bash
git clone https://github.com/koushiksiripuram/ai-business-intelligence-assistant.git
cd ai-business-intelligence-assistant
```

**2. Create and activate a virtual environment**

Windows:

```bash
python -m venv venv
venv\Scripts\activate
```

**3. Install dependencies**

```bash
pip install -r requirements.txt
```

**4. Configure environment variables**

Create a `.env` file in the project root:

```env
GROQ_API_KEY=your_groq_api_key
GROQ_MODEL=openai/gpt-oss-120b
DB_PATH=data/sales.db
CSV_PATH=data/sample_sales.csv
```

Keep your API key private. Never commit `.env` to version control.
Here you can specify the path accordingly

**5. Start the application**

```bash
streamlit run app.py
```

Open the local URL displayed in the terminal.

## Usage

1. Start the Streamlit application.
2. Ask a business question about the available sales data.
3. The assistant retrieves the database schema and generates a SQL query.
4. The query is validated and executed against SQLite.
5. The assistant presents a natural-language answer based on the query results.

Example questions:

- What is the total revenue from completed transactions?
- Which product generated the highest revenue?
- Show monthly revenue trends.
- Which region generated the highest sales?
- How did revenue change between January and February 2026?

Questions outside the available dataset should receive a helpful unsupported-question response.

## Business Rules

Revenue calculations follow these intended rules:

- Include completed transactions only.
- Exclude returned transactions.
- Exclude records with missing unit prices or non-positive quantities.
- Calculate transaction revenue as quantity multiplied by unit price, minus the transaction's total discount.
- Preserve missing prices as missing values rather than treating them as zero.

Results depend on the data loaded into the database and the correctness of the generated SQL.

## Running Tests

Run the test suite from the project root:

```bash
python -m pytest -q
```

The tests cover selected database, analytics, SQL validation, and response-generation behaviors. Passing tests do not guarantee that every generated query or business calculation is correct.

See `README_TESTS.md` for additional testing details.

## Security Considerations

- Store API credentials in environment variables.
- Validate generated SQL before execution.
- Restrict database queries to read-only operations.
- Do not commit private datasets, database files, or API credentials.
- Treat LLM-generated SQL as untrusted input.

SQL validation should be combined with read-only database access and appropriate query execution safeguards.

## Limitations

- Answers are limited to the information available in the database.
- LLM-generated queries may require validation and testing.
- Invalid or inconsistent source data can affect analytical results.
- Natural-language responses depend on the accuracy of the executed query results.
- External LLM requests require API connectivity.

## Future Improvements

- Improve SQL validation and execution safeguards.
- Expand automated tests for revenue calculations and date handling.
- Improve error handling and query-result explanations.
- Add support for additional datasets and business domains.
- Add more analytics visualizations and evaluation metrics.

## License

Add a license before distributing or publishing this project.
