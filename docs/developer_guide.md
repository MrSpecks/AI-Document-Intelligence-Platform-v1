# Developer Integration Guide

## Overview

This guide walks through integrating with the AI Document Intelligence Platform. The platform provides RESTful APIs for document upload, processing, and insight retrieval.

## Quick Start

### 1. Authentication

All API requests require authentication via API key or JWT token.

```javascript
// Example: Using API Key in header
const headers = {
  'Authorization': 'Bearer YOUR_API_KEY',
  'Content-Type': 'application/json'
}
```

### 2. Upload a Document

```javascript
// Pseudocode: Upload document for processing
const formData = new FormData()
formData.append('file', documentFile)
formData.append('document_type', 'invoice') // optional

const response = await fetch(
  'https://api.documentintelligence.com/api/documents/upload',
  {
    method: 'POST',
    headers: { 'Authorization': 'Bearer API_KEY' },
    body: formData
  }
)

const document = await response.json()
// Returns: { id: 'doc_xyz', status: 'processing', created_at: '...' }
```

### 3. Trigger Analysis

```javascript
// Pseudocode: Request analysis for document
const response = await fetch(
  `https://api.documentintelligence.com/api/documents/${documentId}/analyze`,
  {
    method: 'POST',
    headers: {
      'Authorization': 'Bearer API_KEY',
      'Content-Type': 'application/json'
    }
  }
)

const result = await response.json()
// Returns: { success: true, message: 'Analysis started' }
```

### 4. Retrieve Insights

```javascript
// Pseudocode: Poll for analysis results
const response = await fetch(
  `https://api.documentintelligence.com/api/documents/${documentId}/insights`,
  {
    method: 'GET',
    headers: { 'Authorization': 'Bearer API_KEY' }
  }
)

const insights = await response.json()
// Returns analysis results with summary, entities, risks, etc.
```

---

## Integration Patterns

### Pattern 1: Synchronous Upload & Analyze

```javascript
// 1. Upload document
const uploadResponse = await uploadDocument(file)
const documentId = uploadResponse.id

// 2. Wait for processing (poll)
let document = await getDocumentStatus(documentId)
while (document.status !== 'completed') {
  await sleep(5000) // Wait 5 seconds
  document = await getDocumentStatus(documentId)
}

// 3. Trigger analysis
await analyzeDocument(documentId)

// 4. Retrieve insights
const insights = await getInsights(documentId)
console.log(insights.summary, insights.entities)
```

### Pattern 2: Webhook Integration

```javascript
// Register webhook for document completion
await registerWebhook({
  event: 'document.completed',
  url: 'https://yourapp.com/webhooks/document-completed'
})

// Your endpoint receives:
// {
//   event: 'document.completed',
//   document_id: 'doc_xyz',
//   status: 'completed',
//   timestamp: '2025-01-15T10:30:00Z'
// }
```

### Pattern 3: Batch Processing

```javascript
// Upload multiple documents
const documents = await Promise.all(
  files.map(file => uploadDocument(file))
)

// Analyze all documents
await Promise.all(
  documents.map(doc => analyzeDocument(doc.id))
)

// Retrieve results as they complete
const results = []
for (const doc of documents) {
  const insight = await getInsights(doc.id)
  results.push(insight)
}
```

---

## Common Use Cases

### Legal Document Review

```javascript
// Review contracts for risk
async function reviewContract(contractFile) {
  const doc = await uploadDocument(contractFile, {
    document_type: 'contract'
  })

  await analyzeDocument(doc.id)

  const insights = await getInsights(doc.id)
  return {
    parties: insights.entities.filter(e => e.type === 'organization'),
    risks: insights.risk_assessment.specific_risks,
    compliance: insights.compliance_issues
  }
}
```

### Invoice Validation

```javascript
// Validate and extract invoice data
async function processInvoice(invoiceFile) {
  const doc = await uploadDocument(invoiceFile, {
    document_type: 'invoice'
  })

  await analyzeDocument(doc.id)

  const insights = await getInsights(doc.id)
  return {
    vendor: extractEntity(insights, 'vendor'),
    invoice_number: extractEntity(insights, 'invoice_number'),
    total_amount: extractEntity(insights, 'amount'),
    line_items: insights.key_findings
  }
}
```

### Resume Screening

```javascript
// Screen and rank resumes
async function screenResumes(resumeFiles, jobRequirements) {
  const results = await Promise.all(
    resumeFiles.map(async (file) => {
      const doc = await uploadDocument(file, {
        document_type: 'cv'
      })

      await analyzeDocument(doc.id)

      const insights = await getInsights(doc.id)

      return {
        candidate: extractEntity(insights, 'person'),
        skills: insights.entities.filter(e => e.type === 'skill'),
        experience_years: calculateExperience(insights),
        match_score: calculateMatch(insights, jobRequirements)
      }
    })
  )

  return results.sort((a, b) => b.match_score - a.match_score)
}
```

---

## Error Handling

### API Response Codes

| Code | Meaning | Action |
|------|---------|--------|
| 200 | Success | Process response |
| 400 | Bad Request | Check parameters |
| 401 | Unauthorized | Verify API key |
| 402 | Payment Required | Check credit balance |
| 404 | Not Found | Verify resource ID |
| 429 | Rate Limited | Implement backoff |
| 500 | Server Error | Retry with exponential backoff |

### Example Error Handling

```javascript
async function safeAnalyze(documentId) {
  let retries = 3

  while (retries > 0) {
    try {
      const response = await analyzeDocument(documentId)
      return response
    } catch (error) {
      if (error.status === 429) {
        // Rate limited - wait and retry
        await sleep(Math.pow(2, 3 - retries) * 1000)
        retries--
      } else if (error.status >= 500) {
        // Server error - retry with backoff
        await sleep(Math.pow(2, 3 - retries) * 1000)
        retries--
      } else {
        // Client error - don't retry
        throw error
      }
    }
  }

  throw new Error('Max retries exceeded')
}
```

---

## Rate Limits & Quotas

| Tier | Documents/Day | Max File Size | Concurrent | Price |
|------|---------------|---------------|-----------|-------|
| Free | 5 | 5 MB | 1 | $0 |
| Starter | 50 | 10 MB | 2 | $299/mo |
| Professional | 200 | 10 MB | 5 | $999/mo |
| Enterprise | Custom | Custom | Custom | Custom |

---

## Best Practices

1. **Always use exponential backoff** for retries
2. **Implement webhooks** instead of polling for better performance
3. **Batch upload documents** when processing multiple files
4. **Cache results** to avoid redundant API calls
5. **Handle rate limiting gracefully** with appropriate delays
6. **Log all API calls** for debugging and audit trails
7. **Use document_type hints** for better accuracy
8. **Validate inputs** before sending to API

---

## SDKs & Libraries

### Official SDKs
- **Python**: `pip install doc-intelligence`
- **JavaScript/Node.js**: `npm install @doc-intelligence/sdk`
- **Go**: `go get github.com/docint/go-sdk`

### Community Libraries
- See [CONTRIBUTING.md](../CONTRIBUTING.md) for community contributions

---

## Testing & Sandbox

### Test Documents

Use these test documents for development:

```
Invoice: /examples/test-invoice.pdf
Contract: /examples/test-contract.docx
Resume: /examples/test-resume.pdf
Company Profile: /examples/test-company.txt
```

### Mock Responses

For local testing without API calls:

```javascript
const mockResponse = {
  id: 'doc_test_123',
  status: 'completed',
  summary: 'This is a test invoice from Acme Corp...',
  entities: [
    { type: 'organization', value: 'Acme Corp' },
    { type: 'amount', value: '$1,500.00' }
  ],
  key_findings: ['Invoice dated 2025-01-15', 'Payment due in 30 days']
}
```

---

## Troubleshooting

### Document Not Processing
- Check document format is supported (PDF, DOCX, TXT, PNG, JPG)
- Verify file size < 10 MB
- Ensure document has extractable text (not image-only PDF)

### Slow Processing
- Check file size (large files take longer)
- Verify API tier has sufficient concurrency
- Consider batch uploads for multiple documents

### Incorrect Results
- Verify document_type is correctly specified
- Check if document is clear and readable
- Review log output for processing warnings

### Rate Limiting
- Implement exponential backoff
- Upgrade to higher tier if needed
- Contact support for custom rate limits

---

## Support & Resources

- **API Reference**: See [api_reference.md](./api_reference.md)
- **FAQ**: Check [faq.md](./faq.md)
- **Security**: Review [SECURITY.md](../SECURITY.md)

---

**Ready to integrate?** Start with the [Quick Start](#quick-start) section above!
