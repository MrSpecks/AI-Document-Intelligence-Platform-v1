# Frequently Asked Questions

## Table of Contents

1. [Business & Licensing](#business--licensing)
2. [Technical Questions](#technical-questions)
3. [Security & Compliance](#security--compliance)
4. [Integration & API](#integration--api)
5. [Performance & Scalability](#performance--scalability)
6. [Roadmap & Future](#roadmap--future)
7. [Support & Help](#support--help)
8. [Troubleshooting](#troubleshooting)

---

## Business & Licensing

### What is the AI Document Intelligence Platform?

The AI Document Intelligence Platform is an enterprise SaaS solution that automatically analyzes business documents (invoices, contracts, CVs, company profiles, tenders, and custom documents) using advanced AI, Retrieval-Augmented Generation (RAG), and vector embeddings to extract structured intelligence, identify risks, and ensure compliance.

### How is the platform priced?

We offer flexible pricing with multiple tiers:

- **Free Tier**: 5 documents/month (perfect for trials)
- **Starter Plan**: $299/month (50 documents/month, 2 concurrent)
- **Professional Plan**: $999/month (200 documents/month, 5 concurrent)
- **Enterprise Plan**: Custom pricing (unlimited documents, custom SLA)

All tiers include API access, webhooks, and real-time processing updates.

### Can I purchase credits instead of a subscription?

Yes! We offer pay-as-you-go credits:
- 10 credits: $5 (Invoice: $0.50/document)
- 50 credits: $20 ($0.40/document)
- 500 credits: $150 ($0.30/document)

Credits are valid for 12 months and can be used on any subscription tier.

### What document types do you support?

We support 6 primary document types:
- **Invoices** – Line items, totals, payment terms, vendor info
- **Contracts** – Parties, dates, terms, obligations, risk factors
- **CVs/Resumes** – Experience, skills, education, certifications
- **Company Profiles** – Industry, financials, employees, description
- **Tenders/RFPs** – Requirements, evaluation criteria, deadlines
- **Custom Documents** – Any business document with custom templates

### Is there a free trial available?

Yes! Sign up for a free account and get 5 documents/month to try the platform. No credit card required. Upgrade anytime to unlock more documents and features.

### Do you offer white-label solutions?

Yes, for enterprise customers. We provide white-label SaaS options where you can brand the platform as your own. Contact us at partnerships@documentintelligence.com for details.

### Can I use the platform for internal-only applications?

Absolutely! Many customers run the platform internally:
- **On-Premise Deployment** – Full data control
- **Hybrid Deployment** – SaaS for standard workloads, on-premise for sensitive data
- **Private Cloud** – Deploy to your AWS/Azure account

Contact sales for deployment options.

### What are the licensing terms?

The platform is proprietary, licensable software. All users and integrators must accept our Terms of Service. See the [LICENSE](../LICENSE) file for complete terms.

### Do you offer discounts for non-profits or educational institutions?

Yes! We offer:
- **Non-profits**: 50% discount on annual subscriptions
- **Educational institutions**: Free tier + academic research access
- **NGOs**: Custom pricing for humanitarian work

Contact us at partnerships@documentintelligence.com with proof of status.

### What's your payment method policy?

We accept:
- Credit cards (Visa, Mastercard, American Express)
- Bank transfers (for annual contracts)
- Purchase orders (for enterprise customers)
- Stripe-compatible payment methods

All payments are processed securely via Stripe.

---

## Technical Questions

### What file formats are supported?

We support:
- **PDF** – Including scanned PDFs with OCR fallback
- **DOCX** – Microsoft Word documents
- **TXT** – Plain text files
- **PNG** – Image documents
- **JPG/JPEG** – Image documents
- **TIFF** – Multi-page image documents

Maximum file size: 10 MB per document. For larger files, contact enterprise support.

### What is the maximum document size?

The platform supports documents up to 10 MB. Most documents are much smaller:
- Average invoice: 100-500 KB
- Average contract: 500 KB - 2 MB
- Average resume: 50-200 KB

For larger documents, use batch processing or contact support.

### How long does document analysis take?

Typical processing times:
- **Text Extraction**: 2-5 seconds
- **Embedding Generation**: 5-10 seconds (50 chunks)
- **LLM Analysis**: 10-30 seconds
- **Total**: 30-60 seconds per document (average)

Large documents (500+ pages) may take up to 2 minutes.

### What languages are supported?

Currently: **English**

In development (Q2 2025): Spanish, French, German, Mandarin Chinese. Additional languages available on request.

### Can I use the platform programmatically (via API)?

Yes! Full REST API with webhooks. See the [API Reference](./api_reference.md) for complete documentation. SDKs available for Python, JavaScript/Node.js, and Go.

### What is RAG and why does it matter?

**RAG (Retrieval-Augmented Generation)** combines:
1. **Vector embeddings** – Convert document text to semantic vectors
2. **Vector search** – Find relevant document sections
3. **LLM context** – Feed relevant sections to language model
4. **Structured output** – Get analyzed results

This approach is 98%+ accurate and avoids hallucinations compared to pure LLM analysis.

### How do you ensure document privacy?

- **Encryption in Transit**: TLS 1.3 for all connections
- **Encryption at Rest**: AES-256 for stored documents
- **Access Control**: Row-level security (RLS) prevents cross-user access
- **Audit Logging**: All access logged with timestamps
- **Data Residency**: EU data stays in EU, support for other regions

See [SECURITY.md](../SECURITY.md) for full details.

### Do you train models on my documents?

By default, **no**. Your documents are processed and deleted (unless you request storage). We do not use customer documents to train models.

**Enterprise Option**: Custom fine-tuning available where you retain ownership of custom models. Contact us for details.

### What happens to my documents after analysis?

- **Free/Starter**: Documents stored for 30 days, then deleted
- **Professional/Enterprise**: Documents stored indefinitely (as configured)
- **On-Premise**: You control storage and retention

You can also delete documents at any time via API or dashboard.

### Can I export my documents and data?

Yes! You can:
- Export insights in JSON format via API
- Download analysis results as CSV
- Bulk export all documents and metadata
- Export to third-party systems via webhooks

See our [Developer Guide](./developer_guide.md) for export patterns.

---

## Security & Compliance

### What compliance certifications do you have?

- ✅ **SOC 2 Type II** – Audited security, availability, integrity
- ✅ **HIPAA-Ready** – HIPAA compliance available for healthcare
- ✅ **GDPR-Compliant** – Full GDPR compliance with data residency
- ✅ **CCPA-Compliant** – California privacy regulations
- ✅ **ISO 27001-Ready** – Information security management

Enterprise deployments can achieve additional certifications.

### How do you handle data breaches?

We maintain:
- **Incident Response Plan** – Immediate response protocol
- **Breach Notification** – 72-hour regulatory notification
- **Cyber Insurance** – Coverage for security incidents
- **Regular Audits** – Third-party penetration testing

See [SECURITY.md](../SECURITY.md) for breach notification policy.

### Is penetration testing allowed?

Yes! With restrictions:
- Notify us via security@documentintelligence.com before testing
- Only test systems you own or have permission to test
- Follow responsible disclosure guidelines
- See [SECURITY.md](../SECURITY.md) for full policy

### How do you handle API security?

- **API Key Authentication** – Secure key-based auth
- **Rate Limiting** – Prevents abuse (5-100 req/min by tier)
- **Request Signing** – Webhook signatures verify authenticity
- **HTTPS Only** – All API traffic encrypted

See [API Reference](./api_reference.md) for security details.

### Do you support OAuth/SSO?

- **Standard Plans**: Username/password or Google/Microsoft OAuth
- **Enterprise Plans**: SAML 2.0, OpenID Connect (OIDC), custom SSO

Contact support for enterprise SSO setup.

### How often do you perform security audits?

- **Internal Audits**: Monthly
- **Third-Party Audits**: Quarterly
- **Penetration Testing**: Bi-annually
- **Vulnerability Scanning**: Continuous

Results available to enterprise customers under NDA.

### Can I deploy on-premise for maximum security?

Yes! On-premise deployment options include:
- **Single-tenant Docker deployment** – Full data control
- **Custom security** – Your infrastructure, your rules
- **No data in cloud** – All processing and storage on-premise
- **Dedicated support** – Enterprise support included

Contact sales for on-premise options.

### What encryption standards do you use?

- **Transit**: TLS 1.3 (modern, secure)
- **At Rest**: AES-256-GCM (government-grade)
- **Key Management**: AWS KMS or customer-managed keys (enterprise)
- **Future**: Post-quantum cryptography (roadmap)

---

## Integration & API

### How do I integrate with the API?

See our [Developer Guide](./developer_guide.md) for:
- Quick start guide
- Integration patterns (sync, webhooks, batch)
- 3 real-world use case examples
- Error handling with retry logic
- Rate limiting best practices

**Quickstart:**
1. Get API key from dashboard
2. Upload document: `POST /documents/upload`
3. Trigger analysis: `POST /documents/{id}/analyze`
4. Retrieve results: `GET /documents/{id}/insights`

### Are webhooks supported?

Yes! Webhooks notify you when:
- Document processing completes
- Analysis finishes
- Errors occur

Register webhooks via API. See [API Reference](./api_reference.md) for details.

### What SDKs are available?

**Official SDKs:**
- Python: `pip install doc-intelligence`
- JavaScript/Node.js: `npm install @doc-intelligence/sdk`
- Go: `go get github.com/docint/go-sdk`

Community SDKs and libraries available. Contribute your own!

### Can I integrate with Zapier or Make?

Yes! Both platforms are supported:
- **Zapier** – 1-click workflow automation (available in Zapier app store)
- **Make.com** – Visual workflow builder with AI Document Intelligence module

Contact us to add your integration.

### How do I handle rate limiting?

Implement exponential backoff:
- Check `X-RateLimit-Remaining` header
- On 429 response, wait: 2^attempt seconds (1s, 2s, 4s, 8s, ...)
- Maximum 3 retries recommended

See [API Reference](./api_reference.md) for details.

### Can I batch upload documents?

Yes! Use batch endpoints:
```
POST /documents/batch-upload
- Upload 100+ documents in single request
- Get back array of document IDs
- Automatic parallel processing
```

See [Developer Guide](./developer_guide.md) for batch patterns.

### How do I get results from the API?

Two approaches:

**Polling:**
```
1. POST /documents/{id}/analyze
2. Loop: GET /documents/{id}/insights every 2-5 seconds
3. When status=completed, process results
```

**Webhooks (Recommended):**
```
1. POST /documents/{id}/analyze
2. Listen for webhook at your endpoint
3. Webhook delivers results automatically
```

See [Developer Guide](./developer_guide.md) for implementation examples.

### What is the API SLA?

- **Standard Plans**: 99.5% uptime
- **Professional Plans**: 99.9% uptime
- **Enterprise Plans**: 99.95% SLA + credits for downtime

See [SECURITY.md](../SECURITY.md) for full SLA terms.

---

## Performance & Scalability

### How many documents can I process per day?

Depends on your plan:
- **Free**: 5 documents/month (~0.2/day)
- **Starter**: 50 documents/month (~1.7/day)
- **Professional**: 200 documents/month (~6.7/day)
- **Enterprise**: Unlimited documents/day

You can purchase additional credits for burst processing.

### What's your maximum concurrent processing?

Depends on your plan:
- **Free**: 1 concurrent document
- **Starter**: 2 concurrent
- **Professional**: 5 concurrent
- **Enterprise**: Custom concurrency (10-100+)

Upgrade anytime to increase concurrency.

### Can you handle 1000s of documents at once?

Yes! Strategies:
1. **Batch Upload** – Upload many documents (queueing)
2. **Increase Concurrency** – Professional/Enterprise plans
3. **Stagger Uploads** – Spread processing over time
4. **Enterprise Scale** – Custom infrastructure for massive volume

Contact sales for high-volume requirements.

### How do you scale to high traffic?

- **Horizontal Scaling** – Multiple processing servers
- **Load Balancing** – Distributes API requests
- **Database Clustering** – PostgreSQL replication
- **CDN** – Edge caching for static content
- **Queue Management** – Intelligent job queuing

Enterprise customers can enable advanced scaling.

### What are the data retention policies?

By plan:
- **Free/Starter**: 30 days (auto-delete)
- **Professional**: 90 days (configurable)
- **Enterprise**: Unlimited (or per-contract terms)

You can delete documents anytime via API. On-premise: you control retention.

### Can I achieve sub-second performance?

Yes, for small documents and cached results:
- **API Response**: <100ms (after processing)
- **Vector Search**: <50ms (with index)
- **Cached Results**: <10ms

Processing time (text extraction, embeddings, LLM) is 30-60 seconds minimum due to ML computation.

### What is your database capacity?

- **Multi-tenant SaaS**: 1TB+ documents per customer
- **Professional**: 500GB included
- **Enterprise**: Unlimited (custom infrastructure)

Contact support for large-scale deployments.

---

## Roadmap & Future

### What's coming in Q2 2025?

- Multi-language support (Spanish, French, German, Mandarin)
- Custom document templates
- Workflow automation (Zapier, Make.com, Slack)
- Advanced analytics dashboard
- GraphQL API endpoint

See [Roadmap](./roadmap.md) for complete Q2-2026+ plans.

### When will you support [my language]?

Q2 2025 includes Spanish, French, German, Mandarin Chinese. Additional languages available on request. Vote on language priority at https://github.com/MrSpecks/AI-Document-Intelligence-Platform-v1/discussions

### Can I request a custom feature?

Absolutely! Options:
1. **Community Vote** – Vote on public roadmap (free)
2. **Enterprise Custom Features** – Available for enterprise customers
3. **Feature Requests** – Submit via GitHub Issues
4. **Custom Development** – Available via partnerships

Contact partnerships@documentintelligence.com for custom development.

### Will you open-source the platform?

Currently: **No**. The platform is proprietary SaaS. However:
- SDKs are open-source (Python, Node.js, Go)
- Community contributions welcome
- See [CONTRIBUTING.md](../CONTRIBUTING.md) for how to contribute

Open-source discussions underway – stay tuned!

### What models do you use for AI?

- **LLM**: Claude 3 Sonnet (or Llama 3.3 8B for cost efficiency)
- **Embeddings**: Advanced embedding models (1536-dim)
- **NER**: ML-based entity recognition
- **Classification**: Multi-domain document classifier

Enterprise: You can specify preferred models.

### Can I fine-tune models on my data?

Currently: **No** (by default, to protect privacy)

**Enterprise Option**: Custom fine-tuning available (Q3 2025). You retain ownership of custom models. Contact partnerships@documentintelligence.com

---

## Support & Help

### How do I get support?

Support channels by plan:

- **Free Tier**: Community forum and documentation
- **Starter/Professional**: Email support (24-hour response)
- **Enterprise**: Dedicated support (4-hour response, phone/Slack)

Email: support@documentintelligence.com

### Where can I find documentation?

- **Getting Started**: https://docs.documentintelligence.com
- **API Reference**: [API Reference](./api_reference.md)
- **Developer Guide**: [Developer Guide](./developer_guide.md)
- **Architecture**: [Architecture Guide](./architecture.md)
- **Video Tutorials**: https://youtube.com/documentintelligence
- **Blog**: https://blog.documentintelligence.com

### How do I report a bug?

1. **GitHub Issues**: https://github.com/MrSpecks/AI-Document-Intelligence-Platform-v1/issues
2. **Email**: bugs@documentintelligence.com (with reproduction steps)
3. **Dashboard**: In-app bug reporting tool
4. **Chat**: Community Slack channel

Include:
- Document type and file (sanitized)
- Steps to reproduce
- Expected vs. actual behavior
- Screenshots/logs (if applicable)

### What's your response time?

- **Critical Issues**: 1 hour (enterprise) / 4 hours (pro)
- **High Priority**: 4 hours / 24 hours
- **Medium**: 24 hours / 2 business days
- **Low**: 2-5 business days

Enterprise has guaranteed SLA response times.

### How do I get trained on the platform?

- **Self-Paced Training**: https://learn.documentintelligence.com
- **Live Webinars**: Monthly training sessions
- **Enterprise Training**: Dedicated onboarding and training
- **Customer Success Manager**: Assigned to professional/enterprise

Contact support@documentintelligence.com to schedule training.

### Do you offer custom integrations?

Yes! Available options:
- **API Integration** – Build your own integration
- **Partner Integrations** – Pre-built connectors (Zapier, Make.com, etc.)
- **Custom Development** – Available for enterprise customers

Contact partnerships@documentintelligence.com for integration services.

---

## Troubleshooting

### My document analysis isn't completing

**Possible causes:**
1. Network timeout – Retry with exponential backoff
2. Large file size – Document may exceed limits
3. Processing error – Check error message via API

**Solution:**
- Check document status: `GET /documents/{id}`
- Verify error_message field
- Retry with fresh upload
- Contact support with document ID

### I'm getting rate limited (429 errors)

**Cause**: Too many requests per minute

**Solution**:
1. Implement exponential backoff (see [Developer Guide](./developer_guide.md))
2. Check `X-RateLimit-Remaining` header
3. Wait until `X-RateLimit-Reset` time
4. Upgrade plan for higher limits

### File upload is failing

**Possible causes:**
1. Unsupported file type – Use PDF, DOCX, TXT, PNG, JPG
2. File too large – Max 10 MB
3. Corrupted file – Try re-exporting
4. Network error – Retry upload

**Solution:**
```
POST /documents/upload
- Verify file is valid and not corrupted
- Check file type is supported
- Ensure file < 10 MB
- Retry with exponential backoff
```

### API key isn't working

**Possible causes:**
1. Invalid key format – Use bearer token auth
2. Expired key – Regenerate from dashboard
3. Insufficient permissions – Key may lack required scope
4. Wrong environment – Staging key won't work on production

**Solution**:
1. Get new key from dashboard: Settings → API Keys
2. Verify header format: `Authorization: Bearer YOUR_KEY`
3. Check key isn't expired
4. Use correct environment (staging vs. production)

### Insights results are inaccurate

**Possible causes:**
1. Poor document quality – Blurry/low-res scans
2. Unsupported format – Custom templates not configured
3. LLM confusion – Similar entities in document
4. OCR errors – Scanned PDF may have extraction errors

**Solution**:
1. Verify document is clear and readable
2. Use document_type hint (invoice, contract, cv, etc.)
3. Provide feedback via dashboard
4. For enterprise: fine-tune model on similar documents

### High latency on analysis

**Possible causes:**
1. Large document – More chunks = longer processing
2. Network congestion – Retry
3. High server load – Peak hours slower
4. Complex document – Tables and images take longer

**Solution**:
- Large documents: Use batch processing
- Peak hours: Schedule during off-hours
- Monitor metrics: API response time stats
- Contact support for performance optimization

### I'm running out of credits

**Possible causes:**
1. High document volume – Using more than expected
2. Document size – Large documents use more credits
3. Overage charges – Exceeded plan limits

**Solution**:
1. Monitor usage dashboard
2. Set up alerts for low balance
3. Purchase additional credits
4. Upgrade to higher plan
5. Implement cost optimization (batching, caching results)

---

## Still Have Questions?

- **Contact Us**: support@documentintelligence.com
- **Community Forum**: https://community.documentintelligence.com
- **GitHub Discussions**: https://github.com/MrSpecks/AI-Document-Intelligence-Platform-v1/discussions
- **Live Chat**: Available on dashboard (pro/enterprise)

---

**Last Updated**: January 2025
