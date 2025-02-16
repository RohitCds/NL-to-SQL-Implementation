# Natural Language to SQL Query Generator

This project converts natural language (NL) queries into SQL queries and executes them on a SQLite database. The goal is to bridge the gap between non-technical users and databases, allowing them to fetch data using plain English.

## Model Choice and Reasoning

### 1. **Initial Attempts with General-Purpose Models**
Initially, models like `facebook/bart-base` and `google/t5-base` were tested. However, these models were not specifically trained for SQL generation and struggled to generate structured and accurate queries. While they are powerful sequence-to-sequence models, their lack of SQL-specific fine-tuning led to inconsistent results.

### 2. **Switching to a Fine-Tuned NL-to-SQL Model**
A better approach was to use `SwastikM/bart-large-nl2sql`, a BART model fine-tuned on `gretelai/synthetic_text_to_sql`. This model was explicitly trained for translating natural language prompts into SQL queries, leading to:
- **More structured SQL output**
- **Better handling of schema context**
- **Higher accuracy in query generation**

BART was chosen because it is a robust transformer-based encoder-decoder model, which performs well in text generation tasks. The fine-tuned version significantly improved SQL query correctness.

## Workflow

1. **Generate SQL from NL Query**
   - The input query (e.g., *"List all employees earning more than 50,000"*) is passed to the model.
   - The model generates a structured SQL query based on the database schema.

2. **Execute SQL on SQLite Database**
   - The generated SQL query is executed against a SQLite database.
   - Results are fetched and displayed.

3. **Handling Schema Information**
   - The model requires schema information to generate contextually correct SQL queries.
   - The schema is included in the input prompt to improve accuracy.

## Example Query Flow

### **Example NL Query:**
```plaintext
"List all employees earning more than 50000"
```

### **Generated SQL Query:**
```sql
SELECT * FROM employees WHERE salary > 50000;
```

### **Query Execution Output:**
| id | name  | department  | salary |
|----|-------|------------|--------|
| 2  | Bob   | Engineering | 75000  |
| 3  | Charlie | Marketing | 60000  |

## Optional Streamlit Integration

For those who want a simple web interface, the project includes a commented-out Streamlit implementation. This allows users to enter natural language queries via a web UI and fetch results dynamically.

To enable Streamlit, uncomment the relevant code in `app.py` and run:
```bash
streamlit run app.py
```

## Future Improvements
- Fine-tuning the model further on a larger NL-to-SQL dataset
- Expanding database schema support for more complex queries
- Adding more robust error handling for invalid queries

This project provides a lightweight yet effective approach for translating human-readable questions into SQL, making databases more accessible to everyone. 🚀

