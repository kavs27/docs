---
layout:
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Logs

## Create a Log

**API Reference:** `POST /v1/logs`

This endpoint stores a log object in the Portkey log store.

### Log Object Format

The log object consists of three main parts:

1. `request`:
   - `url` (required)
   - `provider` (required)
   - `headers` (required)
   - `method` (optional, defaults to "POST")
   - `body` (required)

2. `response`:
   - `status` (optional, defaults to 200)
   - `headers` (required)
   - `body` (required)
   - `response_time` (required, represents response latency)

3. `metadata`:
   - Contains organization, user, and request-specific information
   - `trace_id` (optional)
   - `span_id` (optional)

Additional optional fields in metadata for trace instrumentation:
- `parent_span_id`
- `span_name`

### Request Format

- **Single Log**: Send a single log object directly in the request body.
- **Multiple Logs**: Submit an array of log objects in the request body.

This flexible format allows sending either individual logs or batches of logs in a single request.

### Sample Log Object

```json
{
  "request": {
    "url": "https://api.someprovider.com/model/generate",
    "method": "POST",
    "headers": {
      "Content-Type": "application/json"
    },
    "body": {
      "prompt": "What is AI?"
    }
  },
  "response": {
    "status": 200,
    "headers": {
      "Content-Type": "application/json"
    },
    "body": {
      "response": "AI stands for Artificial Intelligence..."
    },
    "response_time": 123
  },
  "metadata": {
    "organisation_id": "org123",
    "user_id": "usr456",
    "model": "text-embedding-ada-002",
    "provider": "azure-openai",
    "trace_id": "trace123",
    "span_id": "span456",
    "span_name": "chat_completions"
  }
}
```
### Response Format

The API will respond with a `200` acknowledging that the log has been stored and will be available on your dashboard soon.

