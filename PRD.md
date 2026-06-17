# PRD: Waxis Enterprise Knowledge & Domain AI

Document status: Draft v1.0  
Owner: Waxis Inc  
Research snapshot: 2026-06-16 EDT  
Repository: `waxis-enterprise-knowledge-domain-ai`

## 1. Problem Statement

Enterprise knowledge is buried across SOPs, contracts, policies, product docs, support tickets, training decks, PDFs, shared drives, Confluence/Notion pages, CRM notes, email attachments, and expert memory. Generic chatbots fail because they retrieve the wrong context, ignore permissions, provide weak citations, hallucinate over stale information, and cannot adapt to company terminology.

The problem is especially acute for teams with proprietary domain knowledge:

- Employees cannot find trusted answers quickly.
- New hires repeat questions that experts have answered many times.
- Customer-facing teams use inconsistent policy or product information.
- AI pilots produce uncited answers and cannot prove where claims came from.
- Permissioned knowledge is difficult to expose safely to agents.
- Domain terminology, acronyms, and business rules are not represented in generic models.

## 2. Solution Summary

Waxis Enterprise Knowledge & Domain AI turns internal documents, SOPs, customer history, product data, and expert knowledge into a permissioned, citation-backed, agent-ready knowledge system.

The solution combines:

- Source connectors and ingestion pipeline
- Document parsing, OCR, chunking, metadata extraction, and freshness tracking
- Permission-aware hybrid retrieval
- RAG answer engine with citations
- GraphRAG-style relationship layer for complex domains
- Domain terminology and policy layer
- Evaluation and feedback workflow
- Knowledge admin console
- Agent and API access for Waxis business workflows

## 3. Research-Backed Product Principles

1. RAG must be permission-aware before retrieval, not only filtered after generation.
2. Answers should be grounded in sources and provide citations that users can verify.
3. Hybrid retrieval is usually necessary because enterprise questions combine semantic meaning, exact identifiers, metadata, and business relationships.
4. Graph retrieval is useful for complex narrative or relational knowledge where plain vector snippets lose structure.
5. Freshness, ownership, and approval state are core knowledge attributes.
6. Domain AI should include terminology, policy, examples, and evaluation, not only embeddings.
7. Knowledge systems should expose reusable APIs and connectors so agents can use them safely.

## 4. Market And Technology Signals

- OpenAI File Search requires vector stores and supports configurable retrieval result counts, showing hosted RAG patterns are becoming standard for agent applications.
- Amazon Bedrock Knowledge Bases describe RAG as a way to use proprietary data to improve relevance and accuracy, including citations in generated responses.
- Google Gemini Enterprise Agent Search positions enterprise search as an out-of-the-box grounding and RAG component with citations, connectors, and control over data sources.
- Microsoft Research GraphRAG combines text extraction, network analysis, and LLM summarization; Microsoft GraphRAG docs describe a structured, hierarchical RAG approach that uses knowledge graphs instead of naive plain-text snippets.
- MCP provides a standard pattern for AI applications to connect to external tools, data sources, and workflows.
- NIST and OWASP guidance require risk controls for hallucination, privacy, prompt injection, data poisoning, insecure output handling, and excessive agency.

## 5. Target Customers

### Primary

- Knowledge-heavy B2B companies with SOPs, policies, product docs, contracts, training material, and expert workflows.
- Teams that need reliable internal search, support answers, onboarding assistance, compliance guidance, or domain-specific assistants.
- Companies with proprietary terminology and access-controlled knowledge.

### Secondary

- Professional services, manufacturing, healthcare-adjacent operations, legal-adjacent operations, education, SaaS support, and technical documentation teams.
- Companies preparing internal AI agents that need trusted context.

## 6. Personas

### Knowledge Owner

Needs ingestion, approval, freshness, ownership, and quality controls for enterprise knowledge.

### Employee Or Frontline User

Needs accurate answers with citations and clear confidence.

### Subject Matter Expert

Needs fewer repeated questions and a workflow to review knowledge gaps and improve answers.

### Compliance Or Security Reviewer

Needs permission controls, audit logs, data retention, and safe handling of sensitive knowledge.

### AI Workflow Developer

Needs API access to reliable retrieval, citations, and domain context for agents.

### Customer Support Or Sales User

Needs consistent answers that match approved policy and product information.

## 7. Goals

1. Ingest and normalize enterprise knowledge from common document and application sources.
2. Enforce permissions throughout retrieval and answer generation.
3. Provide accurate, citation-backed answers over enterprise knowledge.
4. Support domain terminology, acronyms, policies, and source authority rules.
5. Identify stale, conflicting, missing, or low-quality knowledge.
6. Provide evaluation workflows that measure retrieval and answer quality.
7. Expose knowledge APIs and connectors for Waxis agents and client applications.

## 8. Non-Goals

- The product will not replace source-of-record document management in MVP.
- The product will not fine-tune foundation models in MVP.
- The product will not guarantee answers for unsupported or unapproved knowledge.
- The product will not bypass source-system permissions.
- The product will not support every file type and enterprise app on day one.

## 9. Product Scope

### MVP

- Connectors for file upload, website/docs crawl, Google Drive or SharePoint-style folders, Notion/Confluence-style docs through configurable connectors where available, and CSV metadata import.
- Document parsing for PDF, DOCX, HTML, Markdown, TXT, CSV, and common slides where practical.
- OCR support for scanned documents through pluggable processor.
- Metadata model: source, owner, department, access group, document type, effective date, review date, status, language, version.
- Permission-aware hybrid retrieval using keyword, vector, and metadata filters.
- Answer engine with citations and source preview.
- Admin console for source management, ingestion status, and stale document review.
- Feedback capture: helpful, incorrect, missing source, stale, permission issue.
- Evaluation dataset and regression runner.

### V1

- GraphRAG-style entity and relationship extraction for complex domains.
- Conflict detection across sources.
- Domain glossary and synonym management.
- Policy-aware answer modes such as "strict policy", "draft", "explain", and "compare sources".
- Agent API for retrieval, answer, source lookup, glossary lookup, and citation validation.
- Knowledge gap dashboard from failed searches and support/sales workflows.
- Multi-language retrieval and answer support.

### Future

- Domain model adaptation and fine-tuning workflow.
- Expert review queue with auto-generated suggested article updates.
- Knowledge graph editor.
- Personalized knowledge recommendations by role.
- External customer portal search.
- Federated search across private client environments.

## 10. Core User Stories

1. As an employee, I want to ask a natural-language question and get a cited answer, so that I can trust the result.
2. As an employee, I want to see the source passages behind an answer, so that I can verify important claims.
3. As a knowledge owner, I want to ingest documents with metadata and access groups, so that search respects organizational boundaries.
4. As a security reviewer, I want retrieval to enforce permissions before answer generation, so that sensitive documents are not leaked.
5. As a subject matter expert, I want to review unanswered questions, so that I can identify knowledge gaps.
6. As a support agent, I want answers grounded in approved policy, so that customer responses are consistent.
7. As a new employee, I want acronyms and internal terminology explained, so that onboarding is faster.
8. As an admin, I want stale document alerts, so that outdated knowledge does not drive AI answers.
9. As a developer, I want a retrieval API with citations, so that business agents can use trusted knowledge.
10. As a compliance owner, I want audit logs of questions, sources used, and answer mode, so that sensitive use is reviewable.
11. As a product manager, I want answer quality metrics, so that I know which knowledge areas need improvement.
12. As a knowledge owner, I want conflicting source detection, so that inconsistent policy can be resolved.
13. As an employee, I want AI to say when it does not know, so that I do not act on fabricated answers.
14. As an admin, I want source authority ranking, so that approved policies outrank outdated notes.
15. As an AI workflow developer, I want glossary and policy APIs, so that agents can use domain language correctly.

## 11. Functional Requirements

### 11.1 Source Connectors And Ingestion

- FR-001: Support manual file upload and batch upload.
- FR-002: Support configurable connectors for enterprise document sources and websites.
- FR-003: Capture metadata including source, owner, department, access group, document type, effective date, review date, version, language, and status.
- FR-004: Detect duplicate and near-duplicate documents.
- FR-005: Track ingestion status, parsing errors, chunk count, index status, and freshness.
- FR-006: Support incremental sync where connectors provide change events or timestamps.

### 11.2 Document Processing

- FR-010: Parse text from PDF, DOCX, HTML, Markdown, TXT, CSV, and common presentation formats where available.
- FR-011: Support OCR for scanned documents through pluggable processing.
- FR-012: Preserve structure such as headings, tables, lists, page numbers, and section hierarchy where possible.
- FR-013: Generate chunks with source references, hierarchy, metadata, and stable IDs.
- FR-014: Support document-level and chunk-level access control metadata.

### 11.3 Retrieval

- FR-020: Support hybrid retrieval using vector similarity, keyword search, metadata filters, recency, source authority, and access groups.
- FR-021: Apply permissions before returning candidate context to the model.
- FR-022: Support query rewriting and synonym expansion using approved glossary terms.
- FR-023: Support reranking for top candidate passages.
- FR-024: Return retrieval diagnostics: query, filters, source count, confidence, and why sources were selected.

### 11.4 Answer Engine

- FR-030: Generate answers using retrieved context and configured answer mode.
- FR-031: Include citations for claims derived from sources.
- FR-032: Support answer modes: concise answer, detailed answer, compare sources, policy-safe answer, draft response, and no-answer when support is insufficient.
- FR-033: Refuse or escalate when requested content is outside permissions, unsupported by sources, or policy-prohibited.
- FR-034: Provide source preview with passage, document title, metadata, and link to original source where available.
- FR-035: Support confidence indicators based on retrieval quality, source authority, recency, and answer support.

### 11.5 Graph And Domain Layer

- FR-040: Extract entities such as products, policies, procedures, teams, systems, customers, suppliers, locations, and terms.
- FR-041: Extract relationships where source quality supports it, such as policy-applies-to, product-has-feature, procedure-requires-approval, system-owned-by.
- FR-042: Build topic clusters and community summaries for large document sets.
- FR-043: Allow admins to curate glossary terms, synonyms, deprecated terms, and preferred language.
- FR-044: Use graph context for complex questions that require relationships or multi-document reasoning.

### 11.6 Admin And Governance

- FR-050: Manage sources, owners, access groups, sync schedules, and approval status.
- FR-051: Flag stale documents based on review date, failed sync, source deletion, or owner change.
- FR-052: Flag possible conflicts between authoritative sources.
- FR-053: Support document status: draft, approved, deprecated, archived, and restricted.
- FR-054: Provide audit logs for ingestion, access changes, answer events, and admin actions.

### 11.7 Feedback And Evaluation

- FR-060: Capture user feedback at answer level and source level.
- FR-061: Support labels: correct, incorrect, incomplete, stale, missing source, wrong source, permission issue, hallucination, unclear.
- FR-062: Let reviewed examples become evaluation cases.
- FR-063: Evaluate retrieval precision, citation support, faithfulness, answer completeness, refusal correctness, latency, and permissions.
- FR-064: Run regression checks after ingestion pipeline, prompt, retrieval, model, or glossary changes.

### 11.8 APIs And Agent Access

- FR-070: Provide APIs for search, retrieve, answer, source lookup, glossary lookup, citation validation, and feedback capture.
- FR-071: Support scoped API keys and service accounts.
- FR-072: Support MCP-compatible access pattern for agent integrations where appropriate.
- FR-073: Return structured source metadata for downstream workflow audit.

## 12. Non-Functional Requirements

- NFR-001: Permission filtering must occur before context is passed into model generation.
- NFR-002: Common question responses should return within 5 seconds for indexed sources under normal load.
- NFR-003: Source citations must be stable and traceable to document version and chunk ID.
- NFR-004: Ingestion pipeline must be resumable and report partial failures.
- NFR-005: Admins must be able to remove a source and invalidate its chunks.
- NFR-006: The system must support tenant-specific retention and logging policies.
- NFR-007: Evaluation results must be comparable across model, prompt, and retrieval versions.

## 13. Data Model Concepts

- Source Connector
- Document
- Document Version
- Chunk
- Access Group
- Metadata Field
- Glossary Term
- Entity
- Relationship
- Retrieval Query
- Answer
- Citation
- Feedback Event
- Evaluation Case
- Evaluation Run
- Knowledge Gap
- Audit Event

## 14. Key Workflows

### Knowledge Ingestion

1. Admin connects or uploads a source.
2. System parses documents, extracts metadata, chunks content, and indexes.
3. Admin reviews errors, permissions, and approval state.
4. Source becomes searchable for allowed users and agents.

### Employee Question

1. User asks a question.
2. System applies user permissions and retrieves relevant passages.
3. Answer engine generates a cited response.
4. User opens sources, gives feedback, or escalates to SME.
5. Feedback is reviewed and may become an evaluation case.

### Knowledge Gap Review

1. System groups failed searches, low-confidence answers, and negative feedback.
2. Knowledge owner reviews grouped gaps.
3. Owner updates source documents or adds new approved content.
4. Evaluation set validates improvement.

## 15. Analytics And KPIs

### Adoption

- Active users
- Questions per week
- Source connectors configured
- Indexed documents and chunks
- API calls from agents

### Quality

- Answer helpfulness rate
- Citation coverage
- Citation precision
- Retrieval precision at k
- No-answer correctness
- Hallucination report rate
- Human escalation rate

### Knowledge Operations

- Stale document count
- Knowledge gaps opened/closed
- Conflict count
- Average source freshness
- Ingestion failure rate
- SME review cycle time

### Governance

- Permission-block events
- Restricted-source access attempts
- Audit export completeness
- Data retention policy compliance

## 16. Security And Governance Requirements

- Enforce access controls before retrieval.
- Prevent restricted source snippets from entering prompts for unauthorized users.
- Provide audit logs for questions, sources used, answer mode, and feedback.
- Support source deletion and reindexing.
- Support tenant-specific data retention and redaction.
- Detect prompt injection attempts in retrieved documents and user queries where possible.
- Support source authority ranking so approved policies outrank unapproved notes.
- Escalate unsupported answers rather than hallucinating.

## 17. UX Requirements

### Employee Search

- Ask box
- Cited answer
- Source cards
- Confidence and freshness indicators
- Follow-up questions
- Feedback controls
- Escalate to owner

### Knowledge Admin

- Source connector setup
- Ingestion pipeline status
- Permissions mapping
- Stale document queue
- Conflict queue
- Glossary management
- Evaluation dashboard

### Developer/API View

- API keys
- Endpoint docs
- Test console
- Sample requests
- Trace and citation validation

## 18. Implementation Decisions

- Build the system as a knowledge platform, not a one-off chatbot.
- Use hybrid retrieval for MVP and add graph retrieval for complex domains in V1.
- Store document version and chunk IDs for citation traceability.
- Treat permissions, source authority, freshness, and approval state as retrieval inputs.
- Make no-answer behavior an explicit quality feature.
- Capture feedback in labels that can become evaluation cases.
- Expose APIs for other Waxis solutions.

## 19. Testing Decisions

- Test parsing and chunking on representative documents.
- Test retrieval against known questions and expected sources.
- Test citation support and source traceability.
- Test permission boundaries with restricted documents.
- Test stale-source removal and reindexing.
- Test answer refusal when sources are insufficient.
- Test regression after glossary, retrieval, and prompt changes.
- Test prompt-injection examples embedded in documents.

## 20. Rollout Plan

### Phase 0: Knowledge Audit

- Identify source systems and knowledge owners.
- Select first departments and document types.
- Define access groups and source authority.
- Build initial evaluation dataset.

### Phase 1: Permissioned Search And Answers

- Launch ingestion, hybrid retrieval, cited answers, feedback, and admin console.
- Keep graph layer disabled unless needed.
- Review answer quality weekly.

### Phase 2: Domain Intelligence

- Add glossary, source authority tuning, conflict detection, and knowledge gap dashboard.
- Add API usage for customer service and internal agents.

### Phase 3: Graph And Workflow Integration

- Add entity/relationship extraction.
- Add GraphRAG-style retrieval for complex questions.
- Add expert review automation and agent connector expansion.

## 21. Risks And Mitigations

| Risk | Mitigation |
| --- | --- |
| Incorrect answers from weak retrieval | Hybrid retrieval, reranking, citations, no-answer behavior, and evaluations |
| Permission leakage | Pre-retrieval access filters, restricted traces, and negative permission tests |
| Stale documents drive bad answers | Review dates, freshness indicators, stale queue, and source authority |
| Users distrust AI | Show citations, source previews, confidence, and owner information |
| Ingestion complexity slows rollout | Start with limited source types and manual upload fallback |
| Graph extraction creates noise | Make graph layer optional and subject to evaluation before broad use |

## 22. Open Questions

1. Which document sources should be first-class connectors for initial Waxis clients?
2. What permission model is most common: source-level, folder-level, document-level, or chunk-level?
3. Should graph extraction be included in MVP for any design partner?
4. Which languages must be supported first?
5. How should answer confidence be presented without creating false precision?
6. Should external customer-facing knowledge search be separate from internal search?

## 23. Acceptance Criteria

- An admin can ingest documents with metadata and access groups.
- A user can ask a question and receive a cited answer grounded in allowed sources.
- Unauthorized users cannot retrieve or infer restricted document content.
- A knowledge owner can see stale documents and unresolved knowledge gaps.
- Feedback can be captured and converted into evaluation cases.
- Evaluation can compare retrieval and answer quality across versions.
- API consumers can retrieve answer, citations, source metadata, and confidence signals.

## 24. Research References

- OpenAI File Search and vector stores: https://developers.openai.com/api/docs/guides/tools-file-search
- Amazon Bedrock Knowledge Bases for RAG and citations: https://docs.aws.amazon.com/bedrock/latest/userguide/knowledge-base.html
- Google Gemini Enterprise Agent Search: https://cloud.google.com/products/gemini-enterprise-agent-platform/agent-search
- Google grounding overview: https://docs.cloud.google.com/gemini-enterprise-agent-platform/models/grounding/overview
- Microsoft Research GraphRAG: https://www.microsoft.com/en-us/research/project/graphrag/
- Microsoft GraphRAG docs: https://microsoft.github.io/graphrag/
- Model Context Protocol introduction: https://modelcontextprotocol.io/docs/getting-started/intro
- NIST AI Risk Management Framework: https://www.nist.gov/itl/ai-risk-management-framework
- OWASP Top 10 for LLMs and Gen AI Apps 2025: https://genai.owasp.org/llm-top-10/

