# Requirements Document: Adhikar Setu AI

## Introduction

Adhikar Setu AI is a mobile-first web application designed to provide multilingual, voice-enabled legal awareness assistance for Indian citizens, particularly targeting underserved communities. The system simplifies complex legal rights into easy-to-understand language, generates personalized complaint guidance, and operates efficiently in low-bandwidth environments.

## Glossary

- **System**: The Adhikar Setu AI web application
- **User**: An Indian citizen seeking legal awareness assistance
- **Legal_Category**: A classification of legal issues (e.g., consumer rights, labor rights, property disputes)
- **Intent_Detector**: The AI component that analyzes user queries to determine legal intent
- **Complaint_Generator**: The component that creates step-by-step guidance for filing complaints
- **Document_Checklist**: A list of required documents for a specific legal procedure
- **Scam_Alert_System**: The component that identifies potential scam indicators
- **Voice_Interface**: The speech-to-text and text-to-speech interaction system
- **Low_Literacy_Mode**: A simplified explanation mode for users with limited reading ability
- **LLM_Service**: AWS Bedrock service for language model processing
- **Transcription_Service**: Amazon Transcribe for converting speech to text
- **Speech_Service**: Amazon Polly for converting text to speech
- **Backend**: AWS Lambda functions handling business logic
- **Storage**: AWS S3 for storing application data and assets

## Requirements

### Requirement 1: Multilingual Voice Interaction

**User Story:** As a user with limited literacy or language barriers, I want to interact with the system using voice in my preferred Indian language, so that I can access legal information without typing.

#### Acceptance Criteria

1. WHEN a user selects a language from supported Indian languages, THE System SHALL configure all interactions in that language
2. WHEN a user speaks a query, THE Transcription_Service SHALL convert the speech to text within 3 seconds
3. WHEN the system generates a response, THE Speech_Service SHALL convert the text response to speech in the selected language
4. THE System SHALL support at least Hindi, English, Tamil, Telugu, Bengali, Marathi, Gujarati, Kannada, Malayalam, and Punjabi
5. WHEN voice input is unclear or has low confidence, THE System SHALL request the user to repeat the query
6. WHEN network conditions are poor, THE System SHALL provide visual feedback indicating processing status

### Requirement 2: Legal Intent Detection and Category Matching

**User Story:** As a user describing my legal issue, I want the system to understand my problem and categorize it correctly, so that I receive relevant legal guidance.

#### Acceptance Criteria

1. WHEN a user submits a query in natural language, THE Intent_Detector SHALL analyze the query using the LLM_Service
2. WHEN the intent is detected, THE System SHALL map the query to one or more Legal_Categories
3. THE System SHALL support legal categories including consumer rights, labor rights, property disputes, family law, criminal law, civil rights, and government schemes
4. WHEN multiple categories are possible, THE System SHALL present options to the user for clarification
5. WHEN the intent cannot be determined with confidence above 70%, THE System SHALL ask clarifying questions
6. THE Intent_Detector SHALL process queries within 5 seconds under normal network conditions

### Requirement 3: Simplified Legal Rights Explanation

**User Story:** As a user unfamiliar with legal terminology, I want complex legal rights explained in simple language, so that I can understand my rights and options.

#### Acceptance Criteria

1. WHEN a Legal_Category is identified, THE System SHALL retrieve relevant legal rights information
2. THE System SHALL use the LLM_Service to simplify legal language into grade 6-8 reading level
3. WHEN Low_Literacy_Mode is enabled, THE System SHALL further simplify explanations to grade 3-5 reading level
4. THE System SHALL provide explanations in the user's selected language
5. WHEN explaining rights, THE System SHALL include practical examples relevant to Indian context
6. THE System SHALL break down complex legal concepts into numbered steps or bullet points

### Requirement 4: Personalized Complaint Guidance Generation

**User Story:** As a user who needs to file a complaint, I want step-by-step personalized guidance, so that I know exactly what actions to take.

#### Acceptance Criteria

1. WHEN a user requests complaint guidance for a Legal_Category, THE Complaint_Generator SHALL create a personalized action plan
2. THE Complaint_Generator SHALL use the LLM_Service to generate context-specific steps based on user's situation
3. THE System SHALL present guidance as a numbered sequence of actionable steps
4. WHEN generating guidance, THE System SHALL include relevant authority names, contact information, and deadlines
5. THE System SHALL specify which steps can be completed online versus in-person
6. WHEN guidance is generated, THE System SHALL provide estimated timelines for each step
7. THE System SHALL allow users to save generated guidance for offline access

### Requirement 5: Document Checklist Generation

**User Story:** As a user preparing to file a complaint or application, I want a checklist of required documents, so that I can gather everything needed before proceeding.

#### Acceptance Criteria

1. WHEN complaint guidance is generated, THE System SHALL automatically create a Document_Checklist
2. THE Document_Checklist SHALL list all required documents specific to the legal procedure
3. WHEN listing documents, THE System SHALL provide brief descriptions of each document in simple language
4. THE System SHALL indicate which documents are mandatory versus optional
5. THE System SHALL specify acceptable formats for each document (original, photocopy, notarized)
6. WHEN a document is commonly difficult to obtain, THE System SHALL provide guidance on how to acquire it
7. THE System SHALL allow users to mark documents as collected for tracking progress

### Requirement 6: Scam Risk Alert Mode

**User Story:** As a user vulnerable to legal scams, I want the system to alert me about potential scam indicators, so that I can protect myself from fraud.

#### Acceptance Criteria

1. WHEN a user describes a situation, THE Scam_Alert_System SHALL analyze the description for common scam patterns
2. WHEN scam indicators are detected with confidence above 60%, THE System SHALL display a prominent warning message
3. THE Scam_Alert_System SHALL identify red flags including requests for upfront payment, unofficial communication channels, pressure tactics, and guaranteed outcomes
4. WHEN an alert is triggered, THE System SHALL provide specific reasons why the situation may be a scam
5. THE System SHALL provide guidance on how to verify legitimacy of legal services or notices
6. THE System SHALL include contact information for reporting scams to authorities
7. THE Scam_Alert_System SHALL operate without requiring explicit user activation

### Requirement 7: Low-Literacy Explanation Mode

**User Story:** As a user with limited reading ability, I want explanations optimized for low literacy, so that I can understand legal information despite my reading level.

#### Acceptance Criteria

1. WHEN Low_Literacy_Mode is enabled, THE System SHALL simplify all text to grade 3-5 reading level
2. THE System SHALL use shorter sentences with maximum 15 words per sentence
3. THE System SHALL replace complex legal terms with everyday language equivalents
4. WHEN technical terms are unavoidable, THE System SHALL provide inline definitions
5. THE System SHALL use visual icons and symbols to supplement text explanations
6. THE System SHALL prioritize voice output over text display in Low_Literacy_Mode
7. THE System SHALL provide an option to enable Low_Literacy_Mode from the main interface

### Requirement 8: Low-Bandwidth Optimization

**User Story:** As a user in an area with poor internet connectivity, I want the application to work efficiently on slow networks, so that I can access legal assistance despite connectivity challenges.

#### Acceptance Criteria

1. THE System SHALL compress all text responses to minimize data transfer
2. WHEN network speed is below 2G levels, THE System SHALL disable non-essential features automatically
3. THE System SHALL cache frequently accessed legal information in the browser
4. THE System SHALL provide text-only mode as an alternative to voice interaction
5. WHEN loading resources, THE System SHALL prioritize critical content over decorative elements
6. THE System SHALL display loading progress indicators for operations exceeding 2 seconds
7. THE System SHALL allow users to download generated guidance as lightweight text files for offline access
8. THE System SHALL limit audio file sizes by using efficient compression for voice responses

### Requirement 9: User Session and Data Management

**User Story:** As a user returning to the application, I want my previous queries and generated guidance saved, so that I can continue from where I left off.

#### Acceptance Criteria

1. WHEN a user generates complaint guidance, THE System SHALL store the guidance in Storage
2. THE System SHALL associate saved data with a unique session identifier
3. WHEN a user returns with the same session identifier, THE System SHALL retrieve previous interactions
4. THE System SHALL allow users to access their saved guidance without requiring account creation
5. THE System SHALL retain user data for 90 days from last access
6. WHEN storing user data, THE System SHALL encrypt sensitive information
7. THE System SHALL provide an option for users to delete their data immediately

### Requirement 10: Backend Processing and API Integration

**User Story:** As a system administrator, I want the backend to efficiently process requests and integrate with AWS services, so that the application scales reliably.

#### Acceptance Criteria

1. THE Backend SHALL use AWS Lambda functions to process all user requests
2. WHEN a request is received, THE Backend SHALL route it to the appropriate Lambda function based on request type
3. THE Backend SHALL integrate with LLM_Service for natural language processing tasks
4. THE Backend SHALL integrate with Transcription_Service for speech-to-text conversion
5. THE Backend SHALL integrate with Speech_Service for text-to-speech conversion
6. WHEN processing requests, THE Backend SHALL implement timeout limits of 30 seconds for Lambda functions
7. WHEN errors occur, THE Backend SHALL log error details and return user-friendly error messages
8. THE Backend SHALL implement rate limiting to prevent abuse (maximum 50 requests per user per hour)

### Requirement 11: Mobile-First User Interface

**User Story:** As a mobile user, I want an interface optimized for small screens and touch interaction, so that I can easily navigate and use the application on my phone.

#### Acceptance Criteria

1. THE System SHALL render all interfaces responsive to screen sizes from 320px to 1920px width
2. WHEN displaying on mobile devices, THE System SHALL use touch-friendly button sizes (minimum 44x44 pixels)
3. THE System SHALL position primary actions within easy thumb reach on mobile screens
4. THE System SHALL use a single-column layout on screens below 768px width
5. WHEN users interact with forms, THE System SHALL display appropriate mobile keyboards (numeric, text, etc.)
6. THE System SHALL minimize scrolling by using collapsible sections for long content
7. THE System SHALL support both portrait and landscape orientations

### Requirement 12: Accessibility and Inclusive Design

**User Story:** As a user with disabilities, I want the application to be accessible, so that I can use legal assistance features regardless of my abilities.

#### Acceptance Criteria

1. THE System SHALL support screen reader navigation with proper ARIA labels
2. THE System SHALL provide sufficient color contrast (minimum 4.5:1 ratio) for all text
3. THE System SHALL allow keyboard-only navigation for all interactive elements
4. WHEN displaying important information, THE System SHALL use multiple sensory channels (visual, auditory, text)
5. THE System SHALL provide text alternatives for all non-text content
6. THE System SHALL allow users to adjust text size without breaking layout
7. THE System SHALL avoid using color as the only means of conveying information
