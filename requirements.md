# Visual Tutor AI - Software Requirements Specification

## 1. Project Overview

### 1.1 Purpose

Visual Tutor AI is a web-based educational platform that combines AI-powered concept explanations with interactive visual simulations to enhance learning outcomes. The system provides structured, guided learning experiences by pairing LLM-generated explanations with relevant interactive simulations.

### 1.2 Target Users

- Students (high school to undergraduate level)
- Self-learners seeking interactive concept understanding
- Educators looking for supplementary teaching tools
- STEM learners requiring visual reinforcement of abstract concepts

### 1.3 Objectives

- Provide clear, structured explanations for complex concepts
- Integrate interactive simulations to reinforce theoretical understanding
- Offer guided learning steps to facilitate active engagement
- Minimize operational costs through intelligent caching
- Ensure domain accuracy through curated simulation sources

### 1.4 Development Roadmap

**Phase 1 (MVP):**
- External simulation integration via iframe/link mapping
- LLM-powered explanation generation
- Response caching mechanism
- Basic user interaction flow

**Phase 2 (Future):**
- In-house simulation engine development
- Dynamic simulation generation
- Advanced caching with semantic search
- Adaptive difficulty and personalization

## 2. Functional Requirements

### 2.1 User Interaction

**REQ-UI-001:** Users shall be able to submit concept-based questions through a text input interface.

**REQ-UI-002:** The system shall return structured explanations in response to user queries.

**REQ-UI-003:** The system shall suggest a relevant simulation for each concept explanation.

**REQ-UI-004:** The system shall provide 3-5 guided activity steps to help users interact with the simulation.

**REQ-UI-005:** The simulation shall auto-load within the user interface (embedded or linked).

**REQ-UI-006:** The interface shall display all components (explanation, simulation, guided steps) in a cohesive layout.

### 2.2 LLM Integration

**REQ-LLM-001:** The backend shall securely call an external LLM API with appropriate authentication.

**REQ-LLM-002:** The system shall request structured JSON responses from the LLM.

**REQ-LLM-003:** LLM output shall include the following fields:
- `explanation`: Clear concept explanation (string)
- `concept`: Normalized concept identifier (string)
- `simulation_identifier`: Reference to approved simulation (string)
- `guided_steps`: Array of 3-5 actionable learning steps (array of strings)

**REQ-LLM-004:** The system shall validate LLM responses against expected schema before processing.

**REQ-LLM-005:** The system shall handle LLM API failures gracefully with appropriate error messages.

### 2.3 Simulation Integration (Phase 1)

**REQ-SIM-001:** Simulations shall be sourced from external open-source platforms (e.g., PhET Interactive Simulations, Geogebra,Ophysics etc.).

**REQ-SIM-002:** Simulations shall be embedded via iframe when permitted by the source platform.

**REQ-SIM-003:** The system shall maintain a deterministic mapping between concept identifiers and simulation URLs.

**REQ-SIM-004:** The system shall validate simulation identifiers against an approved whitelist before embedding.

**REQ-SIM-005:** The system shall NOT allow LLM to generate arbitrary or unverified URLs.

**REQ-SIM-006:** The system shall provide fallback behavior (external link) when iframe embedding is not supported.

### 2.4 Future Simulation Engine (Phase 2)

**REQ-SIM-FUT-001:** The system architecture shall support modular integration of an in-house simulation engine.

**REQ-SIM-FUT-002:** The simulation engine shall support dynamic generation based on concept parameters.

**REQ-SIM-FUT-003:** The system shall integrate physics/education-specific modeling engines (e.g., Matter.js, D3.js).

**REQ-SIM-FUT-004:** The simulation engine shall provide a consistent API interface for frontend integration.

### 2.5 Intelligent Response Caching

**REQ-CACHE-001:** The system shall check the cache for existing responses before making LLM API calls.

**REQ-CACHE-002:** Cache lookup shall be based on normalized concept identifiers.

**REQ-CACHE-003:** The system shall store complete LLM responses (explanation, simulation_id, guided_steps) in the cache.

**REQ-CACHE-004:** Cached responses shall include timestamp metadata for potential expiration logic.

**REQ-CACHE-005:** Cache hits shall return responses with sub-second latency.

**REQ-CACHE-006:** The system shall log cache hit/miss rates for monitoring.

### 2.6 Cost Optimization

**REQ-COST-001:** The system shall implement token limits for LLM requests to control costs.

**REQ-COST-002:** The system shall support configurable model selection (e.g., GPT-4 vs GPT-3.5) based on query complexity.

**REQ-COST-003:** The system shall log all API calls with token usage for cost tracking.

**REQ-COST-004:** The system shall implement rate limiting to prevent excessive API usage.

**REQ-COST-005:** Cache utilization shall reduce redundant API calls by at least 40%.

## 3. Non-Functional Requirements

### 3.1 Performance

**NFR-PERF-001:** The system shall respond to user queries within 3 seconds (cache hit) or 8 seconds (cache miss).

**NFR-PERF-002:** The system shall support concurrent users with minimal performance degradation.

**NFR-PERF-003:** Simulation loading shall complete within 5 seconds on standard broadband connections.

### 3.2 Scalability

**NFR-SCALE-001:** The system architecture shall support horizontal scaling of backend services.

**NFR-SCALE-002:** The cache layer shall be designed to handle growing data volumes efficiently.

**NFR-SCALE-003:** The system shall support at least 100 concurrent users in Phase 1.

### 3.3 Security

**NFR-SEC-001:** API keys and credentials shall be stored securely using environment variables or secret management services.

**NFR-SEC-002:** User inputs shall be sanitized to prevent injection attacks.

**NFR-SEC-003:** The system shall implement HTTPS for all client-server communication.

**NFR-SEC-004:** Simulation URLs shall be validated against a whitelist to prevent malicious content injection.

### 3.4 Maintainability

**NFR-MAINT-001:** Code shall follow consistent style guidelines and be well-documented.

**NFR-MAINT-002:** The system shall use modular architecture to facilitate future enhancements.

**NFR-MAINT-003:** Configuration parameters shall be externalized for easy modification.

### 3.5 Cost-Efficiency

**NFR-COST-001:** Monthly LLM API costs shall remain under defined budget thresholds through caching and optimization.

**NFR-COST-002:** Infrastructure costs shall be minimized through efficient resource utilization.

### 3.6 Reliability

**NFR-REL-001:** The system shall have 99% uptime during operational hours.

**NFR-REL-002:** The system shall implement error handling and logging for debugging.

**NFR-REL-003:** The system shall gracefully degrade when external services (LLM, simulations) are unavailable.

## 4. System Architecture Overview

### 4.1 Layered Architecture

The system follows a multi-tier architecture:

**Frontend Layer:**
- User interface for query input and content display
- Simulation embedding/rendering
- Responsive design for multiple devices

**Backend API Layer:**
- Request routing and validation
- Business logic orchestration
- Response formatting

**Cache Layer:**
- Concept-based response storage
- Fast lookup mechanism
- Cache invalidation logic

**LLM Service Layer:**
- API integration with external LLM provider
- Prompt engineering and formatting
- Response parsing and validation

**External Simulation Layer (Phase 1):**
- Curated simulation repository mapping
- URL validation and whitelisting
- Iframe embedding logic

**Internal Simulation Engine (Phase 2):**
- Dynamic simulation generation
- Physics/math modeling
- Custom visualization rendering

### 4.2 Request Flow

1. User submits concept query via frontend
2. Backend receives request and normalizes concept identifier
3. System checks cache for existing response
4. If cache miss: Backend calls LLM API with structured prompt
5. LLM returns JSON response with explanation and simulation_identifier
6. Backend validates simulation_identifier against whitelist
7. Backend stores response in cache
8. Backend returns complete response to frontend
9. Frontend displays explanation, loads simulation, and shows guided steps

## 5. Technical Stack

### 5.1 Frontend
- HTML5, CSS3, JavaScript (ES6+)
- Framework: React.js or Vue.js (recommended)
- UI Library: Tailwind CSS or Material-UI
- HTTP Client: Axios or Fetch API

### 5.2 Backend
- Runtime: Node.js with Express.js
- Alternative: Python with Flask/FastAPI
- API Design: RESTful architecture
- Validation: Joi or Zod for schema validation

### 5.3 AI Layer
- LLM Provider: OpenAI GPT-4/GPT-3.5 or Anthropic Claude
- Prompt Management: Custom prompt templates
- Response Parsing: JSON schema validation

### 5.4 Database
- Cache Storage: Redis (in-memory) or MongoDB
- Persistent Storage: PostgreSQL or MongoDB (for analytics)
- Schema: Document-based for flexible response storage

### 5.5 Cloud Infrastructure
- Hosting: AWS, Google Cloud, or Azure
- Containerization: Docker (optional)
- CDN: CloudFlare or AWS CloudFront (for static assets)

## 6. API Design

### 6.1 Endpoint: POST /api/chat

**Request Format:**
```json
{
  "query": "Explain Newton's second law of motion",
  "user_id": "optional_user_identifier"
}
```

**Response Format (Success):**
```json
{
  "status": "success",
  "data": {
    "explanation": "Newton's second law states that the acceleration of an object is directly proportional to the net force acting on it and inversely proportional to its mass. Mathematically: F = ma.",
    "concept": "newtons_second_law",
    "simulation_identifier": "phet_forces_and_motion",
    "simulation_url": "https://phet.colorado.edu/sims/html/forces-and-motion-basics/latest/forces-and-motion-basics_en.html",
    "guided_steps": [
      "Observe the relationship between force and acceleration by adjusting the applied force slider",
      "Change the mass of the object and notice how acceleration changes for the same force",
      "Try different combinations and verify that F = ma holds true",
      "Experiment with friction and observe its effect on net force"
    ],
    "cached": false,
    "timestamp": "2026-02-13T10:30:00Z"
  }
}
```

**Response Format (Error):**
```json
{
  "status": "error",
  "message": "Unable to process request",
  "error_code": "LLM_API_FAILURE"
}
```

## 7. Database Requirements

### 7.1 Response Cache Schema

**Collection/Table: response_cache**

```json
{
  "concept_id": "newtons_second_law",
  "explanation": "Newton's second law states that...",
  "simulation_identifier": "phet_forces_and_motion",
  "simulation_url": "https://phet.colorado.edu/...",
  "guided_steps": [
    "Step 1...",
    "Step 2...",
    "Step 3..."
  ],
  "created_at": "2026-02-13T10:30:00Z",
  "access_count": 15,
  "last_accessed": "2026-02-13T14:20:00Z"
}
```

**Indexes:**
- Primary: `concept_id` (unique)
- Secondary: `created_at`, `access_count`

### 7.2 Simulation Mapping Schema

**Collection/Table: simulation_registry**

```json
{
  "simulation_id": "phet_forces_and_motion",
  "title": "Forces and Motion Basics",
  "source": "PhET Interactive Simulations",
  "url": "https://phet.colorado.edu/sims/html/forces-and-motion-basics/latest/forces-and-motion-basics_en.html",
  "embed_allowed": true,
  "topics": ["physics", "forces", "motion", "newtons_laws"],
  "verified": true
}
```

## 8. Future Enhancements

### 8.1 Adaptive Difficulty
- Multi-level explanations (beginner, intermediate, advanced)
- Dynamic complexity adjustment based on user interaction

### 8.2 Student Profiling
- User progress tracking
- Personalized learning paths
- Concept mastery assessment

### 8.3 Embedding-Based Semantic Cache
- Vector embeddings for concept similarity
- Semantic search for related cached responses
- Improved cache hit rates for paraphrased queries

### 8.4 In-House Simulation Generator
- Custom physics engine integration
- Procedural simulation generation
- Domain-specific modeling tools

### 8.5 Analytics Dashboard
- Usage statistics and metrics
- Cost tracking and optimization insights
- Popular concepts and simulation analytics
- User engagement metrics

### 8.6 Multi-Language Support
- Internationalization of explanations
- Localized simulation content
- Language detection and routing

### 8.7 Collaborative Features
- Shared learning sessions
- Teacher-student interaction modes
- Classroom integration tools

---

**Document Version:** 1.0  
**Last Updated:** February 13, 2026  
**Status:** Draft - Pending Review
