# Implementation Plan: BharatConnect AI

## Overview

This implementation plan breaks down the BharatConnect AI system into discrete, actionable coding tasks. The system follows a hybrid RAG architecture with a language-first UX approach. Implementation will proceed incrementally, building core infrastructure first, then adding retrieval capabilities, external fetching, caching, and finally the LLM response generation layer.

The plan prioritizes getting a working end-to-end flow early, then enhancing with property-based tests and additional features. Each task builds on previous work, ensuring no orphaned code.

## Tasks

- [ ] 1. Set up project structure and database schema
  - Create Python project with FastAPI, initialize virtual environment
  - Set up Supabase connection and create database tables (Users, Schemes, Embeddings, Cache, Sessions)
  - Add pgvector extension for vector operations
  - Create database migration scripts
  - Add initial configuration files (environment variables, database connection)
  - _Requirements: 13.1, 13.2, 13.3, 13.4, 13.5_

- [ ] 2. Implement user profile management
  - [ ] 2.1 Create User model and profile validation logic
    - Implement User data model with language, age, education fields
    - Add validation for age (1-120) and education (enum values)
    - Create database CRUD operations for user profiles
    - _Requirements: 2.2, 2.3, 2.4_

  - [ ]* 2.2 Write property test for profile validation
    - **Property 3: Age Input Validation**
    - **Property 4: Education Level Validation**
    - **Validates: Requirements 2.2, 2.3**

  - [ ]* 2.3 Write property test for profile persistence
    - **Property 5: Profile Persistence Round Trip**
    - **Validates: Requirements 2.4**

  - [ ] 2.4 Implement profile API endpoints
    - Create POST /api/user/profile endpoint for profile submission
    - Create GET /api/user/profile/{user_id} endpoint for profile retrieval
    - Add request validation and error handling
    - _Requirements: 14.2, 14.4_

  - [ ]* 2.5 Write unit tests for profile API endpoints
    - Test profile submission with valid data
    - Test profile retrieval
    - Test validation errors for invalid inputs
    - _Requirements: 14.2, 14.4_

- [ ] 3. Implement language selection module
  - [ ] 3.1 Create language selection logic and persistence
    - Implement language validation (support English for MVP)
    - Create POST /api/user/language endpoint
    - Store language preference in user profile
    - _Requirements: 1.1, 1.4_

  - [ ]* 3.2 Write property test for language persistence
    - **Property 2: Language Persistence**
    - **Validates: Requirements 1.4, 1.5**

  - [ ] 3.3 Implement language selection enforcement middleware
    - Create middleware to check language selection before allowing other operations
    - Return 403 error if language not selected
    - _Requirements: 1.3_

  - [ ]* 3.4 Write property test for language enforcement
    - **Property 1: Language Selection Enforcement**
    - **Validates: Requirements 1.3**

- [ ] 4. Implement scheme data models and initial data population
  - [ ] 4.1 Create Scheme model and database operations
    - Implement Scheme data model with all required fields
    - Create database CRUD operations for schemes
    - Add eligibility filtering helper functions
    - _Requirements: 5.4_

  - [ ]* 4.2 Write property test for scheme data completeness
    - **Property 11: Scheme Data Completeness**
    - **Validates: Requirements 5.4**

  - [ ] 4.3 Populate database with initial scheme data
    - Add PMKVY scheme data
    - Add Skill India Mission scheme data
    - Add at least one education/scholarship scheme
    - _Requirements: 5.1, 5.2, 5.3_

- [ ] 5. Implement embedding service
  - [ ] 5.1 Create embedding generation module
    - Load Sentence-Transformers model (all-MiniLM-L6-v2)
    - Implement generate_embedding() function for text
    - Implement batch_generate_embeddings() for multiple texts
    - Cache model in memory for performance
    - _Requirements: 9.1, 9.2_

  - [ ] 5.2 Generate and store embeddings for schemes
    - Generate embeddings for all scheme descriptions
    - Store embeddings in Embeddings table with scheme_id reference
    - Create index on embedding_vector for efficient search
    - _Requirements: 9.5_

  - [ ]* 5.3 Write unit tests for embedding generation
    - Test embedding generation for sample text
    - Test batch embedding generation
    - Test embedding vector dimensions
    - _Requirements: 9.1, 9.2_

- [ ] 6. Checkpoint - Verify database and embeddings
  - Ensure all tests pass, verify database schema is correct, verify embeddings are generated for all schemes, ask the user if questions arise.

- [ ] 7. Implement retrieval module
  - [ ] 7.1 Create semantic search functionality
    - Implement query embedding generation
    - Implement cosine similarity search against scheme embeddings
    - Return top-k most similar schemes (k=5 default)
    - _Requirements: 8.2, 9.3_

  - [ ]* 7.2 Write property test for similarity search ordering
    - **Property 18: Similarity Search Ordering**
    - **Validates: Requirements 9.3**

  - [ ]* 7.3 Write property test for semantic retrieval relevance
    - **Property 16: Semantic Retrieval Relevance**
    - **Validates: Requirements 8.2**

  - [ ] 7.4 Implement eligibility filtering
    - Create filter_by_eligibility() function
    - Filter schemes by age range (if specified)
    - Filter schemes by education level (if specified)
    - _Requirements: 4.1, 4.2, 4.3_

  - [ ]* 7.5 Write property test for eligibility filtering
    - **Property 9: Eligibility Filtering Correctness**
    - **Validates: Requirements 4.1, 4.2, 4.3**

  - [ ] 7.6 Implement relevance ranking
    - Sort filtered schemes by similarity score
    - Return schemes in descending order of relevance
    - _Requirements: 4.4_

  - [ ]* 7.7 Write property test for relevance ranking
    - **Property 10: Relevance Ranking**
    - **Validates: Requirements 4.4**

- [ ] 8. Implement cache module
  - [ ] 8.1 Create cache storage and retrieval functions
    - Implement get_cached_content(url) function
    - Implement store_cached_content(url, content) function
    - Implement is_cache_fresh() function with 24-hour freshness window
    - Create database operations for Cache table
    - _Requirements: 11.1, 11.2, 11.3, 11.4_

  - [ ]* 8.2 Write property test for cache storage
    - **Property 20: Cache Storage After Fetch**
    - **Validates: Requirements 11.1, 11.3**

  - [ ]* 8.3 Write property test for cache-first behavior
    - **Property 17: Cache-First Behavior**
    - **Validates: Requirements 8.4, 11.2, 11.4**

  - [ ]* 8.4 Write unit tests for cache operations
    - Test cache storage and retrieval
    - Test cache freshness logic
    - Test cache miss scenario
    - _Requirements: 11.1, 11.2, 11.4_

- [ ] 9. Implement fetch module
  - [ ] 9.1 Create verified source whitelist and validation
    - Define whitelist of verified domains (pmkvyofficial.org, skillindia.gov.in, etc.)
    - Implement validate_source(url) function
    - _Requirements: 7.5_

  - [ ]* 9.2 Write property test for verified source validation
    - **Property 15: Verified Source Validation**
    - **Validates: Requirements 7.5**

  - [ ] 9.3 Implement external content fetching
    - Implement fetch_from_source(url) with requests library
    - Add timeout (5 seconds) and retry logic
    - Implement error handling for network failures
    - _Requirements: 10.1, 10.3_

  - [ ] 9.4 Implement HTML parsing and content extraction
    - Use BeautifulSoup to parse HTML content
    - Implement extract_scheme_info() to extract name, eligibility, benefits
    - Handle parsing errors gracefully
    - _Requirements: 10.2, 10.5_

  - [ ]* 9.5 Write property test for content parsing
    - **Property 19: Fetch Content Parsing**
    - **Validates: Requirements 10.5**

  - [ ] 9.6 Integrate fetch with cache module
    - Check cache before fetching
    - Store fetched content in cache after successful fetch
    - Return cached content if fresh
    - _Requirements: 8.4_

  - [ ]* 9.7 Write unit tests for fetch module
    - Test successful fetch and parse
    - Test fetch failure handling
    - Test cache integration
    - _Requirements: 10.1, 10.3_

- [ ] 10. Checkpoint - Verify retrieval and fetch pipeline
  - Ensure all tests pass, verify semantic search returns relevant schemes, verify fetch and cache work correctly, ask the user if questions arise.

- [ ] 11. Implement LLM integration with abstraction layer
  - [ ] 11.1 Create LLM provider abstraction
    - Define LLMProvider abstract base class with generate_response() method
    - Implement GeminiProvider class
    - Add placeholder BedrockProvider class for future use
    - Add LLM configuration management
    - _Requirements: 12.1, 12.3, 12.4, 12.5_

  - [ ] 11.2 Implement prompt construction
    - Create construct_prompt() function
    - Include user profile, retrieved schemes, and query in prompt
    - Add instruction to respond in user's selected language
    - Add instruction to not hallucinate information
    - _Requirements: 7.1, 1.5_

  - [ ] 11.3 Implement response generation and formatting
    - Implement call_llm() function using Gemini API
    - Implement format_response() to create brochure-style output
    - Include scheme name, description, benefits, eligibility, and source link
    - Highlight eligibility criteria in output
    - _Requirements: 6.1, 6.2, 6.3, 6.4_

  - [ ]* 11.4 Write property test for response grounding
    - **Property 13: Response Grounding**
    - **Validates: Requirements 7.1**

  - [ ]* 11.5 Write property test for source citation
    - **Property 14: Source Citation**
    - **Validates: Requirements 7.4**

  - [ ]* 11.6 Write property test for scheme output completeness
    - **Property 12: Scheme Output Completeness**
    - **Validates: Requirements 6.2, 6.3, 6.4**

  - [ ] 11.7 Implement LLM error handling and fallback
    - Add timeout (10 seconds) for LLM calls
    - Implement retry logic with exponential backoff
    - Create template-based fallback responses
    - _Requirements: 18.2_

  - [ ]* 11.8 Write unit tests for LLM integration
    - Test prompt construction
    - Test response formatting
    - Test fallback to template responses
    - _Requirements: 7.1, 18.2_

- [ ] 12. Implement session management
  - [ ] 12.1 Create session creation and storage
    - Implement create_session() function with unique ID generation
    - Store session in Sessions table with user_id and expiration
    - Set TTL to 24 hours
    - _Requirements: 17.1, 17.4_

  - [ ]* 12.2 Write property test for session ID uniqueness
    - **Property 22: Session ID Uniqueness**
    - **Validates: Requirements 17.1**

  - [ ]* 12.3 Write property test for session TTL enforcement
    - **Property 23: Session TTL Enforcement**
    - **Validates: Requirements 17.4**

  - [ ] 12.4 Implement conversation history management
    - Store conversation messages in session JSONB field
    - Implement add_message_to_session() function
    - Implement get_conversation_history() function
    - _Requirements: 3.4, 17.2_

  - [ ]* 12.5 Write property test for session context preservation
    - **Property 8: Session Context Preservation**
    - **Validates: Requirements 3.4**

  - [ ]* 12.6 Write unit tests for session management
    - Test session creation
    - Test conversation history storage and retrieval
    - Test session expiration
    - _Requirements: 17.1, 17.4_

- [ ] 13. Implement conversation handler and query API
  - [ ] 13.1 Create conversation handler orchestration
    - Implement handle_query() function that orchestrates Retrieve → Fetch → Cache → Respond pipeline
    - Retrieve user profile for personalization
    - Call retrieval module for semantic search
    - Conditionally call fetch module if data insufficient
    - Call LLM module for response generation
    - Store message in session history
    - _Requirements: 8.1, 3.1_

  - [ ]* 13.2 Write property test for query acceptance
    - **Property 7: Query Acceptance**
    - **Validates: Requirements 3.2**

  - [ ] 13.3 Create POST /api/chat/query endpoint
    - Accept user_id, query, and optional session_id
    - Call conversation handler
    - Return formatted response with schemes and session_id
    - _Requirements: 14.3_

  - [ ] 13.4 Create GET /api/chat/history endpoint
    - Accept session_id
    - Return conversation history
    - _Requirements: 14.3_

  - [ ]* 13.5 Write unit tests for conversation API
    - Test query submission and response
    - Test conversation history retrieval
    - Test follow-up queries with context
    - _Requirements: 14.3_

- [ ] 14. Implement error handling and API error responses
  - [ ] 14.1 Create standardized error response format
    - Define error response JSON structure
    - Include error code, message, and details
    - _Requirements: 14.5, 18.1_

  - [ ] 14.2 Implement error handling middleware
    - Catch all exceptions and format as error responses
    - Return appropriate HTTP status codes
    - Translate error messages to user's selected language
    - _Requirements: 14.5, 18.1_

  - [ ]* 14.3 Write property test for API error response format
    - **Property 21: API Error Response Format**
    - **Validates: Requirements 14.5, 18.1**

  - [ ] 14.4 Implement specific error handlers
    - Database connection failure handler
    - LLM service unavailability handler
    - External fetch failure handler
    - Validation error handler
    - _Requirements: 18.2, 18.3, 18.4_

  - [ ]* 14.5 Write unit tests for error handling
    - Test database error handling
    - Test LLM error handling
    - Test fetch error handling
    - Test validation error handling
    - _Requirements: 18.2, 18.3, 18.4_

- [ ] 15. Checkpoint - Verify complete backend functionality
  - Ensure all tests pass, verify end-to-end query flow works, verify error handling works correctly, ask the user if questions arise.

- [ ] 16. Implement frontend language selection screen
  - [ ] 16.1 Create language selection UI component
    - Build language selection screen with large, readable buttons
    - Support English for MVP (extensible for Hindi and other languages)
    - Prevent access to other screens until language selected
    - _Requirements: 1.1, 1.3, 15.1, 15.2_

  - [ ] 16.2 Integrate language selection with backend API
    - Call POST /api/user/language on language selection
    - Store language preference locally
    - Navigate to profile screen after successful selection
    - _Requirements: 1.4_

  - [ ]* 16.3 Write unit tests for language selection component
    - Test language selection UI rendering
    - Test API integration
    - Test navigation after selection
    - _Requirements: 1.1, 1.4_

- [ ] 17. Implement frontend profile collection form
  - [ ] 17.1 Create profile collection UI component
    - Build form with age input (numeric) and education dropdown
    - Use large, readable fonts and clear labels
    - Display validation errors clearly
    - _Requirements: 2.1, 15.1, 15.2_

  - [ ] 17.2 Integrate profile form with backend API
    - Call POST /api/user/profile on form submission
    - Handle validation errors from backend
    - Navigate to chat interface after successful submission
    - _Requirements: 2.4_

  - [ ] 17.3 Implement profile reuse logic
    - Check if user profile exists on app load
    - Skip profile collection if profile exists
    - Navigate directly to chat interface
    - _Requirements: 2.5_

  - [ ]* 17.4 Write property test for profile reuse
    - **Property 6: Profile Reuse**
    - **Validates: Requirements 2.5**

  - [ ]* 17.5 Write unit tests for profile form component
    - Test form rendering
    - Test validation
    - Test API integration
    - Test profile reuse logic
    - _Requirements: 2.1, 2.4, 2.5_

- [ ] 18. Implement frontend chat interface
  - [ ] 18.1 Create chat UI component
    - Build chat-style interface with message history
    - Add text input for queries
    - Display user messages and system responses
    - Use mobile-first responsive design
    - _Requirements: 15.1, 15.2, 15.5_

  - [ ] 18.2 Implement scheme display in brochure-style format
    - Create scheme card component
    - Display scheme name, description, benefits prominently
    - Highlight eligibility criteria
    - Include source link as clickable button
    - _Requirements: 6.1, 6.2, 6.3, 6.4_

  - [ ] 18.3 Integrate chat interface with backend API
    - Call POST /api/chat/query on message submission
    - Display loading indicator during API call
    - Render response with scheme cards
    - Maintain session_id across queries
    - _Requirements: 14.3_

  - [ ] 18.4 Implement conversation history display
    - Call GET /api/chat/history on component mount
    - Display previous messages in chat interface
    - Scroll to latest message
    - _Requirements: 3.4, 17.2_

  - [ ]* 18.5 Write unit tests for chat interface
    - Test message submission
    - Test response rendering
    - Test scheme card display
    - Test conversation history loading
    - _Requirements: 3.4, 6.1, 14.3_

- [ ] 19. Implement frontend error handling
  - [ ] 19.1 Create error display components
    - Build error message component with clear, user-friendly text
    - Display errors in user's selected language
    - Add retry button for recoverable errors
    - _Requirements: 18.1_

  - [ ] 19.2 Integrate error handling in all API calls
    - Catch API errors and display error messages
    - Handle network errors gracefully
    - Show appropriate messages for different error types
    - _Requirements: 18.1, 18.2, 18.3, 18.4_

  - [ ]* 19.3 Write unit tests for error handling
    - Test error message display
    - Test retry functionality
    - Test different error scenarios
    - _Requirements: 18.1_

- [ ] 20. Final integration and end-to-end testing
  - [ ] 20.1 Test complete user journey
    - Test language selection → profile collection → query → response flow
    - Test follow-up queries with context
    - Test profile reuse on return visit
    - Test error scenarios
    - _Requirements: All_

  - [ ] 20.2 Verify all property tests pass
    - Run all property-based tests with 100+ iterations
    - Verify all properties hold across randomized inputs
    - Fix any failures
    - _Requirements: All testable properties_

  - [ ] 20.3 Verify scheme data and embeddings
    - Confirm PMKVY, Skill India, and education scheme data is present
    - Verify embeddings are generated for all schemes
    - Test semantic search returns relevant results
    - _Requirements: 5.1, 5.2, 5.3_

  - [ ] 20.4 Performance and scalability verification
    - Test query response time (should be under 5 seconds)
    - Verify cache improves performance
    - Test with multiple concurrent users
    - _Requirements: 19.1_

- [ ] 21. Deployment preparation
  - [ ] 21.1 Create Docker configuration
    - Write Dockerfile for backend
    - Write docker-compose.yml for local development
    - Configure environment variables
    - _Requirements: 19.5_

  - [ ] 21.2 Set up database migrations
    - Create migration scripts for production deployment
    - Test migrations on clean database
    - Document migration process
    - _Requirements: 13.5_

  - [ ] 21.3 Configure security settings
    - Enable HTTPS for all API communications
    - Implement database encryption at rest
    - Add authentication for admin endpoints
    - _Requirements: 20.1, 20.2, 20.4_

  - [ ] 21.4 Create deployment documentation
    - Document environment variables
    - Document deployment steps
    - Document database setup
    - Document LLM API key configuration
    - _Requirements: 19.5_

- [ ] 22. Final checkpoint - Production readiness
  - Ensure all tests pass, verify deployment configuration is correct, verify security settings are enabled, push code to GitHub repository, ask the user if questions arise.

## Notes

- Tasks marked with `*` are optional and can be skipped for faster MVP development
- Each task references specific requirements for traceability
- Property tests validate universal correctness properties with 100+ iterations
- Unit tests validate specific examples and edge cases
- Checkpoints ensure incremental validation at key milestones
- The implementation follows the Retrieve → Fetch → Cache → Respond pipeline architecture
- Frontend tasks assume React or Vue.js but can be adapted to other frameworks
- All code should be pushed to the GitHub repository: https://github.com/P-SAHANA-BLR/BharatConnect-AI.git
