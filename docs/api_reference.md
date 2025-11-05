# API Reference

## Base URL

```
https://api.documentintelligence.com/v1
```

All API requests require authentication and return JSON responses.

---

## Authentication

All requests must include an `Authorization` header with your API key:

```
Authorization: Bearer YOUR_API_KEY
```

API keys can be obtained from the [Dashboard Settings](https://app.documentintelligence.com/settings/api-keys).

---

## Endpoints

### 1. Upload Document

Upload a document for processing.

**Request:**
```
POST /documents/upload
Content-Type: multipart/form-data

Parameters:
- file (required): Binary file upload (PDF, DOCX, TXT, PNG, JPG, TIFF)
- document_type (optional): Type hint for better accuracy
  Allowed values: invoice, contract, cv, company_profile, tender, other
- metadata (optional): JSON object with custom metadata
```

**Response (200 OK):**
```json
{
  "id": "doc_abc123xyz789",
  "filename": "invoice_2025_01.pdf",
  "file_type": "pdf",
  "file_size": 245120,
  "document_type": "invoice",
  "status": "uploaded",
  "created_at": "2025-01-15T10:30:00Z",
  "metadata": {}
}
```

**Response (400 Bad Request):**
```json
{
  "error": {
    "code": "INVALID_FILE_TYPE",
    "message": "File type .exe not supported. Allowed: PDF, DOCX, TXT, PNG, JPG, TIFF"
  }
}
```

**Response (402 Payment Required):**
```json
{
  "error": {
    "code": "INSUFFICIENT_CREDITS",
    "message": "Insufficient credits. Upload requires 1 credit. Current balance: 0"
  }
}
```

---

### 2. Get Document

Retrieve document metadata and processing status.

**Request:**
```
GET /documents/{document_id}
```

**Response (200 OK):**
```json
{
  "id": "doc_abc123xyz789",
  "filename": "invoice_2025_01.pdf",
  "file_type": "pdf",
  "file_size": 245120,
  "document_type": "invoice",
  "status": "completed",
  "created_at": "2025-01-15T10:30:00Z",
  "processing_started_at": "2025-01-15T10:31:00Z",
  "processing_completed_at": "2025-01-15T10:55:00Z",
  "error_message": null,
  "metadata": {
    "page_count": 3,
    "token_count": 1250,
    "extraction_confidence": 0.98
  }
}
```

**Response (404 Not Found):**
```json
{
  "error": {
    "code": "DOCUMENT_NOT_FOUND",
    "message": "Document with ID 'doc_invalid' not found or access denied"
  }
}
```

---

### 3. Trigger Analysis

Start analysis and insight extraction for a document.

**Request:**
```
POST /documents/{document_id}/analyze

Body (optional):
{
  "analysis_type": "full",  // full, summary, entities, risks, compliance
  "include_compliance": true,
  "risk_assessment_model": "default"
}
```

**Response (200 OK):**
```json
{
  "success": true,
  "message": "Analysis started",
  "document_id": "doc_abc123xyz789",
  "status": "processing",
  "estimated_completion": "2025-01-15T10:58:00Z"
}
```

**Response (409 Conflict):**
```json
{
  "error": {
    "code": "ANALYSIS_IN_PROGRESS",
    "message": "Analysis already in progress for this document"
  }
}
```

---

### 4. Get Insights

Retrieve analysis results and extracted insights.

**Request:**
```
GET /documents/{document_id}/insights

Query Parameters (optional):
- insight_type=summary,entities,risks  // Filter by type
- include_confidence=true                // Include confidence scores
```

**Response (200 OK):**
```json
{
  "document_id": "doc_abc123xyz789",
  "status": "completed",
  "summary": {
    "title": "Invoice from Acme Corp",
    "description": "Invoice for services rendered in January 2025. Total amount: $1,500.00. Payment due in 30 days.",
    "key_dates": ["2025-01-01", "2025-01-31"],
    "confidence": 0.98
  },
  "entities": [
    {
      "type": "organization",
      "value": "Acme Corporation",
      "context": "Vendor",
      "confidence": 0.99
    },
    {
      "type": "amount",
      "value": "$1,500.00",
      "context": "Total invoice amount",
      "confidence": 0.97
    },
    {
      "type": "date",
      "value": "2025-01-31",
      "context": "Payment due date",
      "confidence": 0.95
    }
  ],
  "risks": [
    {
      "risk_type": "payment_term_risk",
      "severity": "low",
      "description": "30-day payment terms are standard",
      "confidence": 0.92
    }
  ],
  "compliance": {
    "status": "compliant",
    "issues": [],
    "notes": "Document meets all invoice compliance requirements"
  },
  "key_findings": [
    "Invoice is properly formatted with all required fields",
    "No suspicious patterns detected",
    "Amount matches line items sum"
  ],
  "analysis_metadata": {
    "model_used": "claude-3-sonnet",
    "chunks_analyzed": 5,
    "processing_time_seconds": 15,
    "timestamp": "2025-01-15T10:55:00Z"
  }
}
```

**Response (202 Accepted):**
```json
{
  "status": "processing",
  "message": "Analysis in progress. Check again in 30 seconds.",
  "estimated_completion": "2025-01-15T10:58:00Z"
}
```

---

### 5. List Documents

Retrieve paginated list of user's documents.

**Request:**
```
GET /documents

Query Parameters:
- page=1              // Page number (default: 1)
- limit=10            // Results per page (default: 10, max: 100)
- status=completed    // Filter by status: uploaded, processing, completed, failed
- document_type=invoice  // Filter by type
- sort=-created_at    // Sort field (prefix with - for descending)
```

**Response (200 OK):**
```json
{
  "pagination": {
    "page": 1,
    "limit": 10,
    "total": 47,
    "total_pages": 5
  },
  "documents": [
    {
      "id": "doc_abc123xyz789",
      "filename": "invoice_2025_01.pdf",
      "document_type": "invoice",
      "status": "completed",
      "file_size": 245120,
      "created_at": "2025-01-15T10:30:00Z",
      "processing_completed_at": "2025-01-15T10:55:00Z"
    },
    {
      "id": "doc_def456uvw123",
      "filename": "contract_2025.docx",
      "document_type": "contract",
      "status": "processing",
      "file_size": 512000,
      "created_at": "2025-01-15T09:15:00Z",
      "processing_started_at": "2025-01-15T09:16:00Z"
    }
  ]
}
```

---

### 6. Vector Search

Search across all documents using semantic similarity.

**Request:**
```
POST /search

Body:
{
  "query": "What are the payment terms?",
  "limit": 5,
  "document_id": "doc_abc123xyz789",  // Optional: search within single document
  "threshold": 0.7,                    // Similarity threshold (0-1)
  "document_type": "invoice"           // Optional: filter by type
}
```

**Response (200 OK):**
```json
{
  "query": "What are the payment terms?",
  "results": [
    {
      "document_id": "doc_abc123xyz789",
      "chunk_id": "chunk_45",
      "content": "Payment terms: Net 30 days from invoice date. Early payment discount of 2% available if paid within 10 days.",
      "similarity_score": 0.94,
      "page_number": 1,
      "document_filename": "invoice_2025_01.pdf"
    },
    {
      "document_id": "doc_abc123xyz789",
      "chunk_id": "chunk_46",
      "content": "All payments should be made to the account details listed below. Wire transfers are preferred.",
      "similarity_score": 0.87,
      "page_number": 1,
      "document_filename": "invoice_2025_01.pdf"
    }
  ]
}
```

---

### 7. Delete Document

Permanently delete a document and its associated data.

**Request:**
```
DELETE /documents/{document_id}
```

**Response (200 OK):**
```json
{
  "success": true,
  "message": "Document and associated data deleted successfully",
  "document_id": "doc_abc123xyz789"
}
```

**Response (404 Not Found):**
```json
{
  "error": {
    "code": "DOCUMENT_NOT_FOUND",
    "message": "Document not found"
  }
}
```

---

## Data Models

### Document Object

```json
{
  "id": "doc_abc123xyz789",
  "filename": "invoice.pdf",
  "file_type": "pdf",
  "file_size": 245120,
  "document_type": "invoice|contract|cv|company_profile|tender|other",
  "status": "uploaded|processing|completed|failed",
  "created_at": "2025-01-15T10:30:00Z",
  "processing_started_at": "2025-01-15T10:31:00Z",
  "processing_completed_at": "2025-01-15T10:55:00Z",
  "error_message": null,
  "metadata": {
    "page_count": 3,
    "token_count": 1250,
    "extraction_confidence": 0.98
  }
}
```

### Entity Object

```json
{
  "type": "person|organization|date|amount|location|email|phone|account_number|tax_number|contract_term",
  "value": "John Smith",
  "context": "Document signatory",
  "confidence": 0.95
}
```

### Insight Object

```json
{
  "type": "summary|entities|risks|compliance|key_terms|anomalies",
  "content": {
    "title": "...",
    "description": "...",
    "findings": []
  },
  "confidence_score": 0.96,
  "timestamp": "2025-01-15T10:55:00Z"
}
```

---

## Error Handling

All error responses follow this format:

```json
{
  "error": {
    "code": "ERROR_CODE",
    "message": "Human-readable error message",
    "details": {}
  }
}
```

### Common Error Codes

| Code | HTTP Status | Description | Solution |
|------|------------|-------------|----------|
| INVALID_API_KEY | 401 | API key missing or invalid | Verify API key in header |
| INVALID_FILE_TYPE | 400 | Unsupported file format | Use PDF, DOCX, TXT, PNG, JPG, or TIFF |
| FILE_TOO_LARGE | 400 | File exceeds 10 MB limit | Reduce file size |
| INSUFFICIENT_CREDITS | 402 | Account has insufficient credits | Purchase credits or upgrade plan |
| RATE_LIMITED | 429 | API rate limit exceeded | Implement exponential backoff |
| DOCUMENT_NOT_FOUND | 404 | Document doesn't exist | Verify document ID |
| ANALYSIS_FAILED | 500 | Processing error occurred | Retry or contact support |
| SERVER_ERROR | 500 | Internal server error | Retry with exponential backoff |

---

## Rate Limiting

API requests are rate-limited based on subscription tier:

| Tier | Requests/Minute | Requests/Day | Concurrent |
|------|-----------------|--------------|-----------|
| Free | 10 | 100 | 1 |
| Starter | 30 | 500 | 2 |
| Professional | 100 | 2000 | 5 |
| Enterprise | Custom | Custom | Custom |

**Rate Limit Headers:**
```
X-RateLimit-Limit: 100
X-RateLimit-Remaining: 87
X-RateLimit-Reset: 1642255000
```

When rate limited, the API returns `429 Too Many Requests` with a `Retry-After` header indicating seconds to wait.

---

## Webhooks

Register webhooks to receive real-time notifications about document processing events.

**Register Webhook:**
```
POST /webhooks

Body:
{
  "event": "document.completed|document.failed|analysis.completed",
  "url": "https://your-domain.com/webhooks/document-processed",
  "active": true
}
```

**Webhook Event Payload:**
```json
{
  "event": "document.completed",
  "document_id": "doc_abc123xyz789",
  "status": "completed",
  "timestamp": "2025-01-15T10:55:00Z",
  "data": {
    "filename": "invoice.pdf",
    "document_type": "invoice",
    "processing_time_seconds": 25
  }
}
```

**Webhook Signature Verification:**

All webhook requests include an `X-Signature` header. Verify authenticity:

```javascript
const crypto = require('crypto')
const signature = req.headers['x-signature']
const body = req.rawBody
const secret = process.env.WEBHOOK_SECRET

const expected = crypto
  .createHmac('sha256', secret)
  .update(body)
  .digest('hex')

if (signature !== expected) {
  throw new Error('Invalid webhook signature')
}
```

---

## Code Examples

### JavaScript/Node.js

```javascript
const API_KEY = 'your_api_key_here'
const BASE_URL = 'https://api.documentintelligence.com/v1'

// Upload document
async function uploadDocument(filePath, documentType = 'invoice') {
  const formData = new FormData()
  const file = fs.readFileSync(filePath)
  formData.append('file', file)
  formData.append('document_type', documentType)

  const response = await fetch(`${BASE_URL}/documents/upload`, {
    method: 'POST',
    headers: {
      'Authorization': `Bearer ${API_KEY}`
    },
    body: formData
  })

  return response.json()
}

// Get insights with polling
async function getInsightsWithPolling(documentId, maxAttempts = 30) {
  for (let attempt = 0; attempt < maxAttempts; attempt++) {
    const response = await fetch(`${BASE_URL}/documents/${documentId}/insights`, {
      headers: { 'Authorization': `Bearer ${API_KEY}` }
    })

    const data = await response.json()
    if (response.status === 200) return data
    if (response.status === 202) {
      await new Promise(r => setTimeout(r, 2000))
      continue
    }
    throw new Error(`Error: ${data.error.message}`)
  }

  throw new Error('Analysis timeout')
}

// Main workflow
async function processInvoice(filePath) {
  const doc = await uploadDocument(filePath, 'invoice')
  await new Promise(r => setTimeout(r, 1000))

  const response = await fetch(`${BASE_URL}/documents/${doc.id}/analyze`, {
    method: 'POST',
    headers: { 'Authorization': `Bearer ${API_KEY}` }
  })

  const insights = await getInsightsWithPolling(doc.id)
  return insights
}
```

### Python

```python
import requests
import time
import json

API_KEY = 'your_api_key_here'
BASE_URL = 'https://api.documentintelligence.com/v1'

def upload_document(file_path, document_type='invoice'):
    with open(file_path, 'rb') as f:
        files = {'file': f}
        data = {'document_type': document_type}
        headers = {'Authorization': f'Bearer {API_KEY}'}

        response = requests.post(
            f'{BASE_URL}/documents/upload',
            files=files,
            data=data,
            headers=headers
        )
        return response.json()

def get_insights_with_polling(document_id, max_attempts=30):
    headers = {'Authorization': f'Bearer {API_KEY}'}

    for attempt in range(max_attempts):
        response = requests.get(
            f'{BASE_URL}/documents/{document_id}/insights',
            headers=headers
        )

        if response.status_code == 200:
            return response.json()
        elif response.status_code == 202:
            time.sleep(2)
            continue
        else:
            raise Exception(f"Error: {response.json()['error']['message']}")

    raise Exception('Analysis timeout')

# Main workflow
doc = upload_document('invoice.pdf', 'invoice')
time.sleep(1)

requests.post(
    f'{BASE_URL}/documents/{doc['id']}/analyze',
    headers={'Authorization': f'Bearer {API_KEY}'}
)

insights = get_insights_with_polling(doc['id'])
print(json.dumps(insights, indent=2))
```

---

## Best Practices

1. **Always verify webhook signatures** before processing events
2. **Implement exponential backoff** for rate-limited requests (429)
3. **Poll for insights** with 2-5 second intervals, maximum 30 attempts
4. **Cache results** to avoid redundant API calls
5. **Handle 202 Accepted responses** by checking again after delay
6. **Log all API calls** for debugging and audit trails
7. **Use document_type hints** for better accuracy
8. **Validate file types** before uploading to save credits
9. **Monitor credit balance** before high-volume operations
10. **Use batch operations** when processing multiple documents

---

## Support

- **Documentation**: https://docs.documentintelligence.com
- **Status Page**: https://status.documentintelligence.com
- **Support Email**: support@documentintelligence.com
- **Community Forum**: https://community.documentintelligence.com

---

**Last Updated**: January 2025
