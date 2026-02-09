# AI Career Counselor - Requirements Document

## 1. Project Overview

### 1.1 Project Name
AI Career Counselor - Intelligent Career Guidance Platform for Indian Students

### 1.2 Project Vision
Democratize career guidance for every Indian student, regardless of their location, language, or economic background. Empower the next generation to make informed, confident career decisions that align with their unique strengths, interests, and aspirations.

### 1.3 Target Audience
- **Primary Users**: Indian students from Class 10 onwards (Age 15-18+)
- **Secondary Users**: Parents seeking career guidance information
- **Tertiary Users**: Educational institutions and career counselors

### 1.4 Core Problem Statement
95% of Indian students lack access to professional career counseling, leading to uninformed career decisions at critical junctures (Class 10 stream selection, Class 12 college/course selection). The platform addresses information fragmentation, accessibility barriers, and decision paralysis through free, AI-powered guidance.

---

## 2. Functional Requirements

### 2.1 User Interface Requirements

#### 2.1.1 Homepage
- **FR-UI-001**: Display hero section with clear value proposition and call-to-action
- **FR-UI-002**: Provide stream selection cards (Science PCM/PCB, Commerce, Arts/Humanities)
- **FR-UI-003**: Showcase platform statistics (career options, resources, success metrics)
- **FR-UI-004**: Feature highlights section (AI Chat, Exam Info, Career Database)
- **FR-UI-005**: Mobile-first responsive design optimized for 4G smartphones
- **FR-UI-006**: Navigation menu with links to all major sections

#### 2.1.2 Career Pages
- **FR-CP-001**: Minimum 43 detailed career pages covering Engineering, Medical, Commerce, Arts, and Emerging careers
- **FR-CP-002**: Each career page must include:
  - Career overview and job description
  - Average and top salary ranges (Indian context)
  - Required skills and aptitudes
  - Relevant entrance exams and eligibility criteria
  - Top colleges/institutions offering the program
  - Job roles and hiring companies
  - Work environment and daily activities
  - Honest pros and cons assessment
  - AI impact analysis on the career field
  - Future outlook (5-10 years)
- **FR-CP-003**: Clickable navigation between related career pages
- **FR-CP-004**: Consistent page structure and formatting across all careers

#### 2.1.3 Stream Selection Guide
- **FR-SS-001**: Dedicated pages for Science PCM, Science PCB, Commerce, and Arts streams
- **FR-SS-002**: Comparison matrix of streams with career options
- **FR-SS-003**: Entrance exam mapping to each stream
- **FR-SS-004**: Salary expectations by stream
- **FR-SS-005**: Personality traits and aptitudes suitable for each stream

#### 2.1.4 Entrance Exam Hub
- **FR-EX-001**: Comprehensive information for 10+ major entrance exams (JEE Main/Advanced, NEET, CAT, CLAT, UCEED, NID, NIFT, etc.)
- **FR-EX-002**: Each exam page must include:
  - Exam pattern and syllabus
  - Eligibility criteria
  - Application process and fees
  - Important dates and deadlines
  - Preparation tips and resources
  - Colleges accepting the exam score
  - Counseling process details
- **FR-EX-003**: Exam calendar view with timeline visualization

#### 2.1.5 Resources Section
- **FR-RS-001**: Study material recommendations
- **FR-RS-002**: College application guides
- **FR-RS-003**: Scholarship information database
- **FR-RS-004**: External resource links (curated and verified)
- **FR-RS-005**: Exam preparation strategies

### 2.2 AI Chatbot Requirements

#### 2.2.1 Core Chatbot Functionality
- **FR-AI-001**: Persistent chat interface accessible from every page
- **FR-AI-002**: Integration with Groq API using Llama 3.3 70B model
- **FR-AI-003**: Real-time conversational responses (<3 seconds latency)
- **FR-AI-004**: Context-aware responses based on current page user is viewing
- **FR-AI-005**: Conversation history maintenance (minimum 10 previous messages)
- **FR-AI-006**: Graceful error handling with user-friendly messages

#### 2.2.2 AI Knowledge Base
- **FR-AI-007**: Pre-trained with all 43+ career pages content
- **FR-AI-008**: Knowledge of Indian education system (streams, entrance exams, reservation policies)
- **FR-AI-009**: Understanding of entrance exam ecosystem and eligibility criteria
- **FR-AI-010**: Awareness of realistic salary ranges and job market trends
- **FR-AI-011**: Information about emerging careers and AI impact on jobs

#### 2.2.3 AI Response Features
- **FR-AI-012**: Generate clickable career path links in responses (e.g., `/career/computer-science`)
- **FR-AI-013**: Suggest 2-3 relevant career options based on user interests
- **FR-AI-014**: Provide entrance exam recommendations aligned with career goals
- **FR-AI-015**: Offer stream selection guidance for Class 10 students
- **FR-AI-016**: Multi-turn conversation support for deeper counseling

#### 2.2.4 Rate Limiting & Security
- **FR-AI-017**: Implement rate limiting (10 requests per hour per user)
- **FR-AI-018**: IP-based throttling to prevent API abuse
- **FR-AI-019**: Input sanitization (remove HTML/script tags, enforce length limits)
- **FR-AI-020**: Display remaining request count to users
- **FR-AI-021**: Clear error messages when rate limit exceeded

### 2.3 Backend Requirements

#### 2.3.1 API Architecture
- **FR-BE-001**: Serverless backend using Netlify Functions
- **FR-BE-002**: RESTful API endpoint for chatbot interactions
- **FR-BE-003**: Environment variable management for API keys
- **FR-BE-004**: CORS configuration for secure cross-origin requests
- **FR-BE-005**: Request/response logging for debugging

#### 2.3.2 Data Storage
- **FR-BE-006**: Netlify Blobs for rate limiting data storage
- **FR-BE-007**: Key-value storage with TTL (time-to-live) support
- **FR-BE-008**: Efficient data retrieval (<100ms latency)

#### 2.3.3 External Integrations
- **FR-BE-009**: Groq API integration with error handling
- **FR-BE-010**: Fallback mechanisms for API failures
- **FR-BE-011**: API response caching where applicable

### 2.4 Performance Requirements

#### 2.4.1 Load Time
- **FR-PF-001**: Homepage load time <2 seconds on 4G networks
- **FR-PF-002**: Career page load time <2.5 seconds
- **FR-PF-003**: AI chatbot response time <3 seconds
- **FR-PF-004**: Lazy loading for images and non-critical content

#### 2.4.2 Optimization
- **FR-PF-005**: Code splitting for route-based bundles
- **FR-PF-006**: Minified CSS and JavaScript in production
- **FR-PF-007**: Image optimization (WebP format, responsive sizes)
- **FR-PF-008**: CDN delivery for static assets

### 2.5 Security Requirements

#### 2.5.1 Data Protection
- **FR-SC-001**: HTTPS enforcement for all connections
- **FR-SC-002**: Server-side API key storage (never exposed to client)
- **FR-SC-003**: Input validation and sanitization on all user inputs
- **FR-SC-004**: XSS attack prevention
- **FR-SC-005**: CSRF protection for form submissions

#### 2.5.2 Privacy
- **FR-SC-006**: No collection of personally identifiable information (PII) without consent
- **FR-SC-007**: Clear privacy policy and terms of service
- **FR-SC-008**: Conversation data not stored permanently (session-based only)

### 2.6 Accessibility Requirements

#### 2.6.1 Responsive Design
- **FR-AC-001**: Mobile-first design approach
- **FR-AC-002**: Support for screen sizes from 320px to 2560px width
- **FR-AC-003**: Touch-friendly interface elements (minimum 44x44px tap targets)
- **FR-AC-004**: Readable font sizes (minimum 16px base)

#### 2.6.2 Browser Compatibility
- **FR-AC-005**: Support for Chrome, Firefox, Safari, Edge (latest 2 versions)
- **FR-AC-006**: Graceful degradation for older browsers
- **FR-AC-007**: Progressive enhancement approach

#### 2.6.3 Web Accessibility
- **FR-AC-008**: Semantic HTML structure
- **FR-AC-009**: Keyboard navigation support
- **FR-AC-010**: Sufficient color contrast ratios (WCAG AA compliance)
- **FR-AC-011**: Alt text for all images

---

## 3. Non-Functional Requirements

### 3.1 Scalability
- **NFR-SC-001**: Support 1,000-2,000 concurrent users initially
- **NFR-SC-002**: Architecture scalable to 10,000+ daily active users
- **NFR-SC-003**: Horizontal scaling capability for future growth
- **NFR-SC-004**: Database-ready architecture for user accounts (future)

### 3.2 Reliability
- **NFR-RL-001**: 99.9% uptime SLA (leveraging Netlify infrastructure)
- **NFR-RL-002**: Automatic failover for critical services
- **NFR-RL-003**: Graceful degradation when AI service unavailable
- **NFR-RL-004**: Error monitoring and alerting system

### 3.3 Maintainability
- **NFR-MT-001**: Modular component architecture
- **NFR-MT-002**: Comprehensive code documentation
- **NFR-MT-003**: Version control using Git/GitHub
- **NFR-MT-004**: Automated deployment pipeline (CI/CD)
- **NFR-MT-005**: Environment-based configuration management

### 3.4 Usability
- **NFR-US-001**: Intuitive navigation requiring no training
- **NFR-US-002**: Consistent UI patterns across all pages
- **NFR-US-003**: Clear call-to-action buttons
- **NFR-US-004**: Helpful error messages with recovery suggestions
- **NFR-US-005**: Maximum 3 clicks to reach any career page

### 3.5 Cost Efficiency
- **NFR-CE-001**: Operate within free tiers initially (Groq, Netlify)
- **NFR-CE-002**: Cost monitoring and budget alerts
- **NFR-CE-003**: Efficient API usage through caching and rate limiting
- **NFR-CE-004**: Scalable cost structure (<₹10,000/month for 60,000 users)

---

## 4. Content Requirements

### 4.1 Career Coverage
- **CR-001**: Minimum 7 Engineering careers (Computer Science, Mechanical, Civil, Electrical, Chemical, Aerospace, Biotechnology)
- **CR-002**: Minimum 5 Medical careers (MBBS, Dentistry, Pharmacy, Nursing, Physiotherapy)
- **CR-003**: Minimum 4 Commerce careers (CA, CS, BBA/MBA, Investment Banking)
- **CR-004**: Minimum 6 Arts careers (Psychology, Journalism, Design, Architecture, Fashion Design, Teaching)
- **CR-005**: Minimum 21 Emerging careers (Content Creator, Esports, Athlete, Chef, etc.)

### 4.2 Content Quality
- **CR-006**: Accurate salary data based on verified sources (Naukri, LinkedIn, Government surveys)
- **CR-007**: Updated entrance exam information (annual review)
- **CR-008**: Honest pros and cons for each career (no bias)
- **CR-009**: AI impact analysis for every career field
- **CR-010**: Future outlook assessment (5-10 year horizon)

### 4.3 Content Tone
- **CR-011**: Student-friendly language (avoid jargon where possible)
- **CR-012**: Encouraging and supportive tone
- **CR-013**: Unbiased presentation (not promoting specific colleges/courses)
- **CR-014**: Realistic expectations (no inflated promises)

---

## 5. Future Enhancement Requirements (Roadmap)

### 5.1 Phase 2 (Q2-Q3 2026)
- **FE-P2-001**: Multi-language support (Hindi + 1 regional language)
- **FE-P2-002**: User account system with Google OAuth
- **FE-P2-003**: Conversation history persistence
- **FE-P2-004**: Career page bookmarking
- **FE-P2-005**: Analytics dashboard (Google Analytics, Hotjar)
- **FE-P2-006**: 10+ additional career pages

### 5.2 Phase 3 (Q4 2026 - Q1 2027)
- **FE-P3-001**: Mobile app (React Native) for iOS and Android
- **FE-P3-002**: Psychometric assessment integration
- **FE-P3-003**: Video testimonials from professionals
- **FE-P3-004**: WhatsApp bot MVP
- **FE-P3-005**: Voice input support
- **FE-P3-006**: Offline mode (PWA)

### 5.3 Phase 4 (Q2 2027+)
- **FE-P4-001**: Mock test platform for entrance exams
- **FE-P4-002**: College database (500+ institutions)
- **FE-P4-003**: Mentorship matching system
- **FE-P4-004**: Scholarship finder
- **FE-P4-005**: Virtual career fairs
- **FE-P4-006**: Alumni network
- **FE-P4-007**: Premium features (unlimited AI, expert counseling)

---

## 6. Technical Constraints

### 6.1 Technology Stack
- **TC-001**: Frontend must use React 18+ with Vite
- **TC-002**: Styling must use Tailwind CSS
- **TC-003**: Routing must use React Router v6
- **TC-004**: Backend must use Netlify Serverless Functions
- **TC-005**: AI must use Groq API (Llama 3.3 70B model)

### 6.2 Deployment
- **TC-006**: Hosting on Netlify with CDN
- **TC-007**: GitHub for version control
- **TC-008**: Automated CI/CD pipeline
- **TC-009**: Environment-based configuration (dev, staging, production)

### 6.3 Budget Constraints
- **TC-010**: Initial operation within free tiers
- **TC-011**: Monthly cost <₹1,000 for first 60,000 users
- **TC-012**: Scalable pricing model for growth phases

---

## 7. Success Metrics

### 7.1 User Engagement (12-month targets)
- **SM-UE-001**: 10,000 monthly active users
- **SM-UE-002**: Average session duration 8+ minutes
- **SM-UE-003**: 3+ career pages visited per session
- **SM-UE-004**: 40% chatbot engagement rate
- **SM-UE-005**: 30% return visit rate within 7 days

### 7.2 Educational Impact
- **SM-EI-001**: 4.5+ star user satisfaction rating
- **SM-EI-002**: 80%+ students report clearer career direction
- **SM-EI-003**: 90%+ find answers to their questions
- **SM-EI-004**: 50%+ explore non-traditional careers
- **SM-EI-005**: 70%+ understand relevant entrance exams

### 7.3 Social Impact
- **SM-SI-001**: Users from 500+ cities (including Tier 2/3)
- **SM-SI-002**: 10,000+ students from underserved communities
- **SM-SI-003**: 45%+ female users exploring STEM
- **SM-SI-004**: 5,000+ first-generation learners
- **SM-SI-005**: ₹5 crore saved in career counseling fees

### 7.4 Technical Performance
- **SM-TP-001**: <2 second page load time (95th percentile)
- **SM-TP-002**: 99.9% uptime
- **SM-TP-003**: <3 second AI response time (95th percentile)
- **SM-TP-004**: Zero critical security vulnerabilities

---

## 8. Compliance & Legal

### 8.1 Data Protection
- **CL-DP-001**: Compliance with IT Act 2000 (India)
- **CL-DP-002**: GDPR-ready architecture for future international users
- **CL-DP-003**: Clear privacy policy and cookie consent
- **CL-DP-004**: Data retention policies

### 8.2 Content Accuracy
- **CL-CA-001**: Disclaimer that AI supplements, not replaces human counselors
- **CL-CA-002**: Salary ranges marked as indicative
- **CL-CA-003**: Last updated dates on all career pages
- **CL-CA-004**: No guarantees of admission/employment

### 8.3 Intellectual Property
- **CL-IP-001**: Proper attribution for external content sources
- **CL-IP-002**: Licensed icons and images
- **CL-IP-003**: Open-source license compliance

---

## 9. Acceptance Criteria

### 9.1 Minimum Viable Product (MVP)
The platform is considered MVP-ready when:
- ✅ All 43 career pages are live with complete information
- ✅ AI chatbot functional with clickable career links
- ✅ Entrance exam hub with 10+ exams covered
- ✅ Stream selection guides for all 4 streams
- ✅ Mobile-responsive design tested on 5+ devices
- ✅ Page load time <2 seconds on 4G
- ✅ Rate limiting implemented and tested
- ✅ Deployed on Netlify with custom domain
- ✅ Zero critical bugs or security vulnerabilities

### 9.2 Quality Assurance
- **QA-001**: Cross-browser testing (Chrome, Firefox, Safari, Edge)
- **QA-002**: Mobile device testing (iOS, Android)
- **QA-003**: Accessibility audit (WCAG AA compliance)
- **QA-004**: Performance testing (Lighthouse score 90+)
- **QA-005**: Security audit (OWASP top 10 vulnerabilities)
- **QA-006**: Content accuracy review by domain experts
- **QA-007**: User acceptance testing with 20+ students

---

## 10. Stakeholder Sign-off

### 10.1 Development Team
- [ ] Lead Developer: Vishal Bhatia
- [ ] Developer: Praduman
- [ ] Developer: Pranjal

### 10.2 Content Review
- [ ] Career counseling expert review
- [ ] Educational institution feedback
- [ ] Student user testing feedback

### 10.3 Technical Review
- [ ] Security audit completed
- [ ] Performance benchmarks met
- [ ] Accessibility standards verified

---

**Document Version**: 1.0  
**Last Updated**: February 3, 2026  
**Next Review Date**: May 1, 2026
