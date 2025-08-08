# Articulate v1.0 Specification

This document defines the `v1.0` specification for the `articulate.yml` file format. The CLI tool and all related SDKs will use this document as the source of truth for parsing, validation, and execution.

## Top-Level Keys

An `articulate.yml` file is a YAML document containing a list of prompt objects. Each prompt object has the following top-level keys:

| Key               | Type          | Required | Description                                                  |
| ----------------- | ------------- | -------- | ------------------------------------------------------------ |
| `id`              | `string`      | Yes      | The unique identifier for this prompt, e.g., `summarize_v2`. |
| `description`     | `string`      | No       | A human-readable description of what the prompt does.        |
| `model_config`    | `object`      | Yes      | Configuration for the target LLM.                            |
| `template`        | `string`      | Yes      | The multiline prompt template.                               |
| `input_variables` | `list<object>`| No       | A list of variables used in the template.                    |
| `tests`           | `list<object>`| No       | A suite of tests to validate the prompt's output.            |

---

### `model_config` Object

This object specifies which model to use and its settings.

| Key           | Type     | Required | Description                                                  |
| ------------- | -------- | -------- | ------------------------------------------------------------ |
| `provider`    | `string` | Yes      | The LLM provider, e.g., `"google"`, `"openai"`, `"anthropic"`. |
| `model`       | `string` | Yes      | The specific model name, e.g., `"gemini-1.5-pro"`, `"gpt-4o"`. |
| `temperature` | `float`  | No       | The sampling temperature (0.0 to 1.0). Defaults to provider's default. |

---

### `tests` Object

Each item in the `tests` list defines a single test case.

| Key      | Type     | Required | Description                                       |
| -------- | -------- | -------- | ------------------------------------------------- |
| `name`   | `string` | No       | An optional, descriptive name for the test.       |
| `input`  | `object` | Yes      | A key-value map of `input_variables` for this test run. |
| `assert` | `object` | Yes      | The assertion to run against the LLM's output.    |

#### `assert` Object

The `assert` object must contain a `type` key and any parameters required by that type.

**v1.0 Assertion Types:**

* **`is_json`**: Asserts the output is a syntactically valid JSON string.
    * *Parameters*: None.
* **`contains_string`**: Asserts the output contains a specific substring.
    * *Parameters*:
        * `value` (string, required): The substring to search for.
        * `case_sensitive` (boolean, optional): Defaults to `false`.
* **`max_length`**: Asserts the output is under a certain character length.
    * *Parameters*:
        * `value` (integer, required): The maximum number of characters allowed.
* **`field_contains`**: For JSON output, asserts a specific field contains one of a list of values.
    * *Parameters*:
        * `field` (string, required): The JSON field to check (e.g., `"sentiment"`).
        * `values` (list<string>, required): The list of allowed values.

---

### ## Complete v1.0 Example

```yaml
- id: extract_invoice_details_v1
  description: "Extracts key details from raw invoice text and returns them as JSON."
  model_config:
    provider: "google"
    model: "gemini-1.5-pro"
    temperature: 0.2
  template: |
    From the invoice text below, extract the following fields:
    - invoice_id (string)
    - total_amount (float)
    - currency (string, e.g., "USD")

    Return the output as a single, valid JSON object with no other text.

    Invoice Text:
    ---
    {invoice_data}
    ---
  input_variables:
    - name: "invoice_data"
      type: "string"
  tests:
    - name: "should extract details from a simple invoice"
      input:
        invoice_data: "Invoice #123 from ACME Corp. Total due: 599.99 USD."
      assert:
        type: "is_json"
    - name: "total amount should be correct"
      input:
        invoice_data: "Invoice #123 from ACME Corp. Total due: 599.99 USD."
      assert:
        type: "contains_string"
        value: "599.99"
```
