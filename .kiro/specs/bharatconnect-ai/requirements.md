# Requirements Document: BharatConnect AI

## Introduction

BharatConnect AI is an inclusive, AI-powered conversational application designed to help rural and semi-urban youth, elderly citizens, and low digitally literate users in India discover and understand government schemes, skill programs, and public opportunities through simple conversational interaction. The system addresses the critical problem of accessibility barriers—including language constraints and complex portals—that prevent millions of Indians from accessing beneficial government and private schemes.

## Glossary

- **System**: The BharatConnect AI application
- **User**: Rural/semi-urban youth, elderly citizens, or low digitally literate individuals seeking scheme information
- **Scheme**: Government, skill, education, or verified private/NGO programs and opportunities
- **Profile**: User's age and education level collected during onboarding
- **RAG**: Retrieval-Augmented Generation, a hybrid approach combining retrieval and AI generation
- **LLM**: Large Language Model used for generating conversational responses
- **Embedding**: Vector representation of text used for semantic search
- **Cache**: Stored fetched content to reduce redundant external requests
- **Verified_Source**: Official government portals or trusted NGO/private organization websites

## Requirements

### Requirement 1: Language Selection

**User Story:** As a first-time user, I want to select my preferred language before any other interaction, so that I can use the application in a language I understand.

#### Acceptance Criteria

1. WHEN a user opens the application for the first time, THE System SHALL display a language selection screen before any other input
2. THE System SHALL support a minimum of 1 and maximum of 2 languages for the MVP
3. WHEN a user attempts to access any other feature, THE System SHALL prevent access until language selection is complete
4. WHEN a user selects a language, THE System SHALL persist that selection across all screens and AI responses
5. THE System SHALL display all UI elements and AI responses in the selected language

### Requirement 2: User Profile Collection

**User Story:** As a user, I want to provide my age and education level once, so that the system can personalize scheme recommendations without repeatedly asking for the same information.

#### Acceptance Criteria

1. WHEN a user completes language selection, THE System SHALL prompt for age and education level
2. THE System SHALL require age as a mandatory numeric input
3. THE System SHALL provide education level as a mandatory dropdown with exactly these options: None, Below 10th, 10th Pass, 12th Pass, Undergraduate, Postgraduate
4. WHEN a user submits the profile, THE System SHALL store the profile data persistently
5. WHEN a user returns to the application, THE System SHALL reuse the stored profile without re-prompting
6. THE System SHALL collect profile information exactly once per user

### Requirement 3: Conversational Interaction

**User Story:** As a low digitally literate user, I want to type keywords or incomplete sentences, so that I can find schemes without needing to form perfect questions.

#### Acceptance Criteria

1. WHEN a user enters keywords or incomplete sentences, THE System SHALL interpret the intent and provide relevant scheme information
2. THE System SHALL accept text input of any length and structure
3. WHEN a user submits a query, THE System SHALL respond within a reasonable time frame (under 5 seconds)
4. THE System SHALL maintain conversation context across multiple user queries in a session
5. WHEN a user's query is ambiguous, THE System SHALL ask clarifying questions in simple language

### Requirement 4: Personalized Scheme Discovery

**User Story:** As a user, I want to receive scheme recommendations based on my age and education level, so that I only see opportunities relevant to me.

#### Acceptance Criteria

1. WHEN the System retrieves schemes, THE System SHALL filter results based on the user's stored age and education level
2. WHEN a scheme has age eligibility criteria, THE System SHALL only include it if the user's age falls within the range
3. WHEN a scheme has education eligibility criteria, THE System SHALL only include it if the user's education level meets the requirement
4. THE System SHALL prioritize schemes with the highest relevance to the user's profile
5. WHEN no schemes match the user's profile, THE System SHALL inform the user and suggest broadening the search

### Requirement 5: Scheme Data Management

**User Story:** As a system administrator, I want the system to be pre-populated with verified schemes, so that users can immediately access accurate information.

#### Acceptance Criteria

1. THE System SHALL include PMKVY (Pradhan Mantri Kaushal Vikas Yojana) scheme data at launch
2. THE System SHALL include Skill India Mission scheme data at launch
3. THE System SHALL include at least one education or scholarship scheme at launch
4. WHEN storing scheme data, THE System SHALL include scheme name, description, eligibility criteria, benefits, and official source link
5. THE System SHALL store schemes in a structured database format

### Requirement 6: Scheme Output Format

**User Story:** As a user, I want to see scheme information in a simple, brochure-style format with clear eligibility highlights, so that I can quickly understand if a scheme is right for me.

#### Acceptance Criteria

1. WHEN displaying a scheme, THE System SHALL present information in a simple, brochure-style format
2. THE System SHALL highlight eligibility criteria prominently in the output
3. WHEN displaying a scheme, THE System SHALL include the scheme name, brief description, key benefits, and eligibility requirements
4. THE System SHALL provide an official or trusted source link for each scheme
5. THE System SHALL use simple language appropriate for low literacy users in all scheme descriptions

### Requirement 7: Grounded AI Responses

**User Story:** As a user, I want to receive accurate information based on real data, so that I can trust the schemes recommended to me.

#### Acceptance Criteria

1. WHEN generating responses, THE System SHALL base all scheme information on retrieved data from the database or verified sources
2. THE System SHALL NOT generate fictional or hallucinated scheme information
3. WHEN the System lacks information to answer a query, THE System SHALL explicitly state the limitation rather than fabricate information
4. THE System SHALL cite the source for each scheme mentioned in responses
5. WHEN fetching external data, THE System SHALL only retrieve from verified government or trusted organization websites

### Requirement 8: Retrieval-Augmented Generation Pipeline

**User Story:** As a system architect, I want a hybrid RAG pipeline, so that the system can efficiently retrieve relevant schemes and generate personalized responses.

#### Acceptance Criteria

1. WHEN a user submits a query, THE System SHALL execute the pipeline in this order: Retrieve → Fetch → Cache → Respond
2. THE System SHALL use semantic embeddings to retrieve relevant schemes from the database
3. WHEN retrieved data is insufficient, THE System SHALL fetch additional information from verified sources
4. WHEN fetching external data, THE System SHALL cache the content to avoid redundant requests
5. THE System SHALL generate responses using the LLM based on retrieved and fetched data

### Requirement 9: Semantic Search with Embeddings

**User Story:** As a developer, I want to use sentence embeddings for semantic search, so that users can find relevant schemes even with imperfect queries.

#### Acceptance Criteria

1. THE System SHALL use Sentence-Transformers to generate embeddings for scheme descriptions
2. WHEN a user query is received, THE System SHALL convert it to an embedding vector
3. THE System SHALL perform similarity search between query embeddings and scheme embeddings
4. THE System SHALL return the top-k most similar schemes based on cosine similarity or equivalent metric
5. THE System SHALL store embeddings in the database for efficient retrieval

### Requirement 10: External Data Fetching

**User Story:** As a system, I want to fetch fresh scheme information from official sources when needed, so that users receive up-to-date information.

#### Acceptance Criteria

1. WHEN the System needs additional scheme information, THE System SHALL fetch data from verified public sources using HTTP requests
2. THE System SHALL use BeautifulSoup or equivalent library to parse HTML content from official websites
3. WHEN fetching fails, THE System SHALL handle errors gracefully and inform the user
4. THE System SHALL validate that fetched content comes from whitelisted verified sources
5. THE System SHALL extract structured information (scheme name, eligibility, benefits) from fetched content

### Requirement 11: Content Caching

**User Story:** As a system administrator, I want fetched content to be cached, so that the system reduces redundant external requests and improves response time.

#### Acceptance Criteria

1. WHEN the System fetches external content, THE System SHALL store it in the cache database table
2. WHEN a query requires external data, THE System SHALL check the cache before making new fetch requests
3. THE System SHALL include a timestamp with each cached entry
4. THE System SHALL serve cached content if it exists and is within a reasonable freshness window
5. THE System SHALL log cache hits and misses for monitoring purposes

### Requirement 12: LLM Integration with Swappable Architecture

**User Story:** As a developer, I want the LLM layer to be swappable, so that I can replace Gemini with AWS Bedrock without major code changes.

#### Acceptance Criteria

1. THE System SHALL implement an abstraction layer for LLM interactions
2. THE System SHALL use Gemini as the default LLM for the MVP
3. WHEN integrating an LLM, THE System SHALL define a common interface for prompt submission and response retrieval
4. THE System SHALL design the LLM integration to allow AWS Bedrock as a drop-in replacement
5. THE System SHALL isolate LLM-specific configuration in a separate module or configuration file

### Requirement 13: Database Schema

**User Story:** As a developer, I want a well-structured database schema, so that the system can efficiently store and retrieve user data, schemes, embeddings, and cache logs.

#### Acceptance Criteria

1. THE System SHALL include a Users table with fields for user ID, language preference, age, and education level
2. THE System SHALL include a Schemes table with fields for scheme ID, name, description, eligibility criteria, benefits, and source link
3. THE System SHALL include an Embeddings table with fields for scheme ID and embedding vector
4. THE System SHALL include a Cache table with fields for URL, fetched content, and timestamp
5. THE System SHALL use Supabase (PostgreSQL) as the database backend

### Requirement 14: Backend API

**User Story:** As a frontend developer, I want a RESTful API, so that I can integrate the conversational interface with the backend logic.

#### Acceptance Criteria

1. THE System SHALL implement a FastAPI backend with RESTful endpoints
2. THE System SHALL provide an endpoint for language selection and profile submission
3. THE System SHALL provide an endpoint for conversational query submission and response retrieval
4. THE System SHALL provide an endpoint for retrieving user profile information
5. WHEN an API request fails, THE System SHALL return appropriate HTTP status codes and error messages

### Requirement 15: Accessibility-First UI

**User Story:** As a low digitally literate user, I want a simple and accessible interface, so that I can easily navigate and use the application.

#### Acceptance Criteria

1. THE System SHALL use large, readable fonts appropriate for users with low digital literacy
2. THE System SHALL provide clear visual indicators for interactive elements
3. THE System SHALL minimize the number of steps required to complete any task
4. THE System SHALL use simple, jargon-free language in all UI text
5. THE System SHALL ensure the interface is usable on mobile devices with small screens

### Requirement 16: Multi-Language Extensibility

**User Story:** As a product manager, I want the system to support multi-language extensibility, so that we can add more Indian languages in future iterations.

#### Acceptance Criteria

1. THE System SHALL implement a language management module that supports adding new languages
2. THE System SHALL store UI text strings in a format that supports internationalization (i18n)
3. WHEN a new language is added, THE System SHALL translate all UI elements and system messages
4. THE System SHALL ensure LLM responses are generated in the user's selected language
5. THE System SHALL maintain English as the MVP language with extensibility for Hindi and other regional languages

### Requirement 17: Session Management

**User Story:** As a user, I want my conversation context to be maintained during a session, so that I don't have to repeat information.

#### Acceptance Criteria

1. WHEN a user starts a conversation, THE System SHALL create a session with a unique identifier
2. THE System SHALL maintain conversation history within a session
3. WHEN a user submits a follow-up query, THE System SHALL use previous conversation context to improve response relevance
4. THE System SHALL persist session data for a reasonable duration (e.g., 24 hours)
5. WHEN a session expires, THE System SHALL clear conversation history but retain user profile data

### Requirement 18: Error Handling and Resilience

**User Story:** As a user, I want the system to handle errors gracefully, so that I receive helpful messages instead of technical errors.

#### Acceptance Criteria

1. WHEN an error occurs, THE System SHALL display a user-friendly error message in the selected language
2. WHEN the LLM service is unavailable, THE System SHALL inform the user and suggest trying again later
3. WHEN the database connection fails, THE System SHALL handle the error without crashing
4. WHEN external data fetching fails, THE System SHALL fall back to cached or database content
5. THE System SHALL log all errors for debugging and monitoring purposes

### Requirement 19: Performance and Scalability

**User Story:** As a system architect, I want the system to be designed for scalability, so that it can handle increased user load and be deployed on cloud infrastructure.

#### Acceptance Criteria

1. THE System SHALL respond to user queries within 5 seconds under normal load
2. THE System SHALL be designed to scale horizontally by adding more backend instances
3. THE System SHALL use connection pooling for database connections
4. THE System SHALL implement caching strategies to reduce database and external API load
5. THE System SHALL be deployable on AWS or equivalent cloud infrastructure with minimal configuration changes

### Requirement 20: Data Privacy and Security

**User Story:** As a user, I want my personal information to be stored securely, so that my privacy is protected.

#### Acceptance Criteria

1. THE System SHALL encrypt user profile data at rest in the database
2. THE System SHALL use HTTPS for all API communications
3. THE System SHALL NOT store or log sensitive personal information beyond age and education level
4. THE System SHALL implement authentication for administrative endpoints
5. THE System SHALL comply with basic data protection principles for user data handling
