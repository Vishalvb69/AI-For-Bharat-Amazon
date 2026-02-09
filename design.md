# AI Career Counselor - Design Document

## 1. System Architecture Overview

### 1.1 High-Level Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                        CLIENT LAYER                          │
│  ┌──────────────────────────────────────────────────────┐  │
│  │   React 18 SPA (Vite + Tailwind CSS)                 │  │
│  │   - Homepage, Career Pages, Stream Guides            │  │
│  │   - AI Chat Interface, Exam Hub, Resources           │  │
│  └──────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────┘
                            ↕ HTTPS
┌─────────────────────────────────────────────────────────────┐
│                    CDN LAYER (Netlify)                       │
│  - Global edge caching                                       │
│  - SSL/TLS termination                                       │
│  - Static asset delivery                                     │
└─────────────────────────────────────────────────────────────┘
                            ↕
┌─────────────────────────────────────────────────────────────┐
│                  BACKEND LAYER (Serverless)                  │
│  ┌──────────────────────────────────────────────────────┐  │
│  │   Netlify Functions (Node.js)                        │  │
│  │   - /api/chat endpoint                               │  │
│  │   - Rate limiting logic                              │  │
│  │   - Input sanitization                               │  │
│  └──────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────┘
                            ↕
┌─────────────────────────────────────────────────────────────┐
│                   EXTERNAL SERVICES                          │
│  ┌─────────────────────┐    ┌──────────────────────────┐   │
│  │   Groq API          │    │   Netlify Blobs          │   │
│  │   (Llama 3.3 70B)   │    │   (Rate Limit Storage)   │   │
│  └─────────────────────┘    └──────────────────────────┘   │
└─────────────────────────────────────────────────────────────┘
```

### 1.2 Technology Stack

#### Frontend
- **Framework**: React 18.3+
- **Build Tool**: Vite 5+
- **Styling**: Tailwind CSS 3+
- **Routing**: React Router v6
- **Icons**: Lucide React
- **State Management**: React Hooks (useState, useEffect, useContext)

#### Backend
- **Runtime**: Node.js 18+ (Netlify Functions)
- **API Framework**: Serverless functions
- **Storage**: Netlify Blobs (key-value store)

#### External Services
- **AI Model**: Groq API (Llama 3.3 70B)
- **Hosting**: Netlify
- **CDN**: Netlify Edge Network
- **Version Control**: GitHub

---

## 2. Frontend Architecture

### 2.1 Component Hierarchy

```
App
├── Layout
│   ├── Header
│   │   ├── Logo
│   │   ├── Navigation
│   │   └── MobileMenu
│   ├── Main Content (Route-based)
│   └── Footer
│       ├── FooterLinks
│       └── SocialMedia
├── Pages
│   ├── HomePage
│   │   ├── HeroSection
│   │   ├── StreamCards
│   │   ├── FeaturesSection
│   │   └── StatsSection
│   ├── CareerPages (43 routes)
│   │   ├── CareerHeader
│   │   ├── OverviewSection
│   │   ├── SalarySection
│   │   ├── SkillsSection
│   │   ├── ExamsSection
│   │   ├── CollegesSection
│   │   ├── JobRolesSection
│   │   ├── ProsConsSection
│   │   └── AIImpactSection
│   ├── StreamPages (4 routes)
│   │   ├── StreamOverview
│   │   ├── CareerOptions
│   │   ├── ExamMapping
│   │   └── ComparisonTable
│   ├── ExamPages (10+ routes)
│   │   ├── ExamOverview
│   │   ├── PatternSection
│   │   ├── EligibilitySection
│   │   ├── DatesSection
│   │   └── PreparationTips
│   └── ResourcesPage
│       ├── StudyMaterials
│       ├── ApplicationGuides
│       └── ScholarshipInfo
└── Components
    ├── ChatBot
    │   ├── ChatButton
    │   ├── ChatWindow
    │   ├── MessageList
    │   ├── MessageInput
    │   └── RateLimitIndicator
    ├── Cards
    │   ├── CareerCard
    │   ├── StreamCard
    │   └── ExamCard
    └── Common
        ├── Button
        ├── Badge
        ├── LoadingSpinner
        └── ErrorMessage
```


### 2.2 Routing Structure

```javascript
// Route Configuration
const routes = [
  { path: '/', component: HomePage },
  
  // Stream Selection Routes
  { path: '/stream/science-pcm', component: SciencePCMPage },
  { path: '/stream/science-pcb', component: SciencePCBPage },
  { path: '/stream/commerce', component: CommercePage },
  { path: '/stream/arts', component: ArtsPage },
  
  // Engineering Career Routes
  { path: '/career/computer-science', component: ComputerSciencePage },
  { path: '/career/mechanical-engineering', component: MechanicalPage },
  { path: '/career/civil-engineering', component: CivilPage },
  { path: '/career/electrical-engineering', component: ElectricalPage },
  { path: '/career/chemical-engineering', component: ChemicalPage },
  { path: '/career/aerospace-engineering', component: AerospacePage },
  { path: '/career/biotechnology', component: BiotechnologyPage },
  
  // Medical Career Routes
  { path: '/career/mbbs', component: MBBSPage },
  { path: '/career/dentistry', component: DentistryPage },
  { path: '/career/pharmacy', component: PharmacyPage },
  { path: '/career/nursing', component: NursingPage },
  { path: '/career/physiotherapy', component: PhysiotherapyPage },
  
  // Commerce Career Routes
  { path: '/career/chartered-accountant', component: CAPage },
  { path: '/career/company-secretary', component: CSPage },
  { path: '/career/mba', component: MBAPage },
  { path: '/career/investment-banking', component: InvestmentBankingPage },
  
  // Arts Career Routes
  { path: '/career/psychology', component: PsychologyPage },
  { path: '/career/journalism', component: JournalismPage },
  { path: '/career/design', component: DesignPage },
  { path: '/career/architecture', component: ArchitecturePage },
  { path: '/career/fashion-design', component: FashionDesignPage },
  { path: '/career/teaching', component: TeachingPage },
  
  // Emerging Career Routes (21+ routes)
  { path: '/career/content-creator', component: ContentCreatorPage },
  { path: '/career/esports', component: EsportsPage },
  // ... additional emerging careers
  
  // Entrance Exam Routes
  { path: '/exam/jee-main', component: JEEMainPage },
  { path: '/exam/jee-advanced', component: JEEAdvancedPage },
  { path: '/exam/neet', component: NEETPage },
  { path: '/exam/cat', component: CATPage },
  { path: '/exam/clat', component: CLATPage },
  // ... additional exams
  
  // Resources
  { path: '/resources', component: ResourcesPage },
  
  // Error handling
  { path: '*', component: NotFoundPage }
];
```


### 2.3 State Management Strategy

#### Local Component State
- Form inputs (chat messages, search queries)
- UI toggles (mobile menu, chat window visibility)
- Loading states for async operations

#### Context API Usage
```javascript
// ChatContext - Global chat state
const ChatContext = {
  messages: [],
  isOpen: false,
  rateLimitRemaining: 10,
  addMessage: (message) => {},
  toggleChat: () => {},
  updateRateLimit: (count) => {}
};

// ThemeContext - Future dark mode support
const ThemeContext = {
  theme: 'light',
  toggleTheme: () => {}
};
```

#### URL State
- Current page/route
- Query parameters for filtering (future)


### 2.4 Design System

#### Color Palette
```css
/* Primary Colors */
--primary-blue: #2563eb;      /* Main brand color */
--primary-blue-dark: #1e40af; /* Hover states */
--primary-blue-light: #dbeafe; /* Backgrounds */

/* Secondary Colors */
--secondary-purple: #7c3aed;
--secondary-green: #10b981;
--secondary-orange: #f59e0b;

/* Neutral Colors */
--gray-50: #f9fafb;
--gray-100: #f3f4f6;
--gray-200: #e5e7eb;
--gray-300: #d1d5db;
--gray-600: #4b5563;
--gray-900: #111827;

/* Semantic Colors */
--success: #10b981;
--warning: #f59e0b;
--error: #ef4444;
--info: #3b82f6;
```

#### Typography
```css
/* Font Family */
font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;

/* Font Sizes */
--text-xs: 0.75rem;    /* 12px */
--text-sm: 0.875rem;   /* 14px */
--text-base: 1rem;     /* 16px */
--text-lg: 1.125rem;   /* 18px */
--text-xl: 1.25rem;    /* 20px */
--text-2xl: 1.5rem;    /* 24px */
--text-3xl: 1.875rem;  /* 30px */
--text-4xl: 2.25rem;   /* 36px */

/* Font Weights */
--font-normal: 400;
--font-medium: 500;
--font-semibold: 600;
--font-bold: 700;
```

#### Spacing System
```css
/* Tailwind default spacing scale */
0.5 = 2px
1 = 4px
2 = 8px
3 = 12px
4 = 16px
6 = 24px
8 = 32px
12 = 48px
16 = 64px
```


#### Component Patterns

**Button Variants**
```jsx
// Primary Button
<button className="bg-blue-600 hover:bg-blue-700 text-white px-6 py-3 rounded-lg">

// Secondary Button
<button className="bg-gray-200 hover:bg-gray-300 text-gray-900 px-6 py-3 rounded-lg">

// Outline Button
<button className="border-2 border-blue-600 text-blue-600 hover:bg-blue-50 px-6 py-3 rounded-lg">
```

**Card Component**
```jsx
<div className="bg-white rounded-xl shadow-lg p-6 hover:shadow-xl transition-shadow">
  {/* Card content */}
</div>
```

**Badge Component**
```jsx
<span className="inline-flex items-center px-3 py-1 rounded-full text-sm font-medium bg-blue-100 text-blue-800">
  {/* Badge text */}
</span>
```


### 2.5 Responsive Design Breakpoints

```css
/* Mobile First Approach */
/* Default: Mobile (320px - 639px) */

/* Small tablets */
@media (min-width: 640px) { /* sm */ }

/* Tablets */
@media (min-width: 768px) { /* md */ }

/* Small laptops */
@media (min-width: 1024px) { /* lg */ }

/* Desktops */
@media (min-width: 1280px) { /* xl */ }

/* Large desktops */
@media (min-width: 1536px) { /* 2xl */ }
```

**Layout Adaptations**
- Mobile: Single column, hamburger menu, stacked cards
- Tablet: 2-column grid, expanded menu
- Desktop: 3-column grid, full navigation, sidebar chat

---

## 3. Backend Architecture

### 3.1 Serverless Function Structure

```
netlify/functions/
└── chat.js
    ├── Handler function
    ├── Rate limiting logic
    ├── Input sanitization
    ├── Groq API integration
    └── Error handling
```


### 3.2 API Endpoint Design

#### POST /api/chat

**Request Format**
```json
{
  "message": "What should I study after Class 10 if I like computers?",
  "conversationHistory": [
    {
      "role": "user",
      "content": "Previous message"
    },
    {
      "role": "assistant",
      "content": "Previous response"
    }
  ],
  "currentPage": "/career/computer-science"
}
```

**Response Format (Success)**
```json
{
  "success": true,
  "response": "Based on your interest in computers, I recommend exploring Computer Science Engineering...",
  "rateLimitRemaining": 9,
  "timestamp": "2026-02-03T10:30:00Z"
}
```

**Response Format (Rate Limit Exceeded)**
```json
{
  "success": false,
  "error": "Rate limit exceeded. Please try again in 1 hour.",
  "rateLimitRemaining": 0,
  "resetTime": "2026-02-03T11:30:00Z"
}
```

**Response Format (Error)**
```json
{
  "success": false,
  "error": "An error occurred processing your request. Please try again.",
  "errorCode": "GROQ_API_ERROR"
}
```


### 3.3 Rate Limiting Implementation

**Strategy**: IP-based token bucket algorithm

```javascript
// Rate Limit Configuration
const RATE_LIMIT = {
  maxRequests: 10,
  windowMs: 3600000, // 1 hour
  keyPrefix: 'ratelimit:'
};

// Rate Limit Flow
1. Extract client IP from request headers
2. Generate rate limit key: `ratelimit:${ip}`
3. Check Netlify Blobs for existing count
4. If count >= maxRequests, reject with 429 status
5. If count < maxRequests, increment and process request
6. Set TTL on blob entry to auto-expire after 1 hour
```

**Storage Schema (Netlify Blobs)**
```javascript
{
  key: 'ratelimit:192.168.1.1',
  value: {
    count: 7,
    resetTime: 1738584600000
  },
  ttl: 3600 // seconds
}
```


### 3.4 Input Sanitization

**Security Measures**
```javascript
// Input validation rules
const sanitizeInput = (message) => {
  // 1. Length validation
  if (message.length > 500) {
    throw new Error('Message too long (max 500 characters)');
  }
  
  // 2. Remove HTML tags
  message = message.replace(/<[^>]*>/g, '');
  
  // 3. Remove script tags
  message = message.replace(/<script\b[^<]*(?:(?!<\/script>)<[^<]*)*<\/script>/gi, '');
  
  // 4. Trim whitespace
  message = message.trim();
  
  // 5. Check for empty message
  if (!message) {
    throw new Error('Message cannot be empty');
  }
  
  return message;
};
```


### 3.5 Groq API Integration

**Configuration**
```javascript
const groqConfig = {
  apiKey: process.env.GROQ_API_KEY,
  model: 'llama-3.3-70b-versatile',
  temperature: 0.7,
  maxTokens: 1024,
  topP: 1,
  stream: false
};
```

**System Prompt Design**
```javascript
const systemPrompt = `You are an expert Indian career counselor helping students from Class 10 onwards make informed career decisions.

CONTEXT:
- You have access to information about 43+ career paths
- You understand the Indian education system (streams, entrance exams, colleges)
- You provide unbiased, realistic guidance

RESPONSE GUIDELINES:
1. Be encouraging and supportive
2. Suggest 2-3 relevant career options, not just one
3. Include clickable links like [Computer Science](/career/computer-science)
4. Mention relevant entrance exams (JEE, NEET, CAT, etc.)
5. Provide realistic salary expectations
6. Consider student's interests, strengths, and background
7. Keep responses concise (200-300 words)

AVAILABLE CAREER PATHS:
Engineering: Computer Science, Mechanical, Civil, Electrical, Chemical, Aerospace, Biotechnology
Medical: MBBS, Dentistry, Pharmacy, Nursing, Physiotherapy
Commerce: CA, CS, MBA, Investment Banking
Arts: Psychology, Journalism, Design, Architecture, Fashion Design, Teaching
Emerging: Content Creator, Esports, Chef, and 18+ more

Current page context: {currentPage}`;
```


### 3.6 Error Handling Strategy

**Error Categories**
```javascript
const ErrorTypes = {
  RATE_LIMIT_EXCEEDED: {
    status: 429,
    message: 'Rate limit exceeded. Please try again in 1 hour.',
    userFriendly: true
  },
  INVALID_INPUT: {
    status: 400,
    message: 'Invalid input provided',
    userFriendly: true
  },
  GROQ_API_ERROR: {
    status: 502,
    message: 'AI service temporarily unavailable. Please try again.',
    userFriendly: true
  },
  INTERNAL_ERROR: {
    status: 500,
    message: 'An unexpected error occurred. Please try again later.',
    userFriendly: true
  }
};
```

**Error Response Flow**
1. Catch error in function handler
2. Log error details (for debugging)
3. Map to user-friendly error type
4. Return appropriate HTTP status code
5. Include helpful recovery suggestions

---

## 4. AI Chatbot Design

### 4.1 Chat Interface UX

**Visual Design**
- Floating chat button (bottom-right corner)
- Slide-up chat window (mobile) or sidebar (desktop)
- Message bubbles (user: right-aligned blue, AI: left-aligned gray)
- Typing indicator during AI response
- Rate limit counter display


**Interaction Flow**
```
1. User clicks chat button
   ↓
2. Chat window opens with welcome message
   ↓
3. User types message and hits send
   ↓
4. Frontend validates input (length, not empty)
   ↓
5. Show typing indicator
   ↓
6. Send request to /api/chat with conversation history
   ↓
7. Backend processes (rate limit check → sanitize → Groq API)
   ↓
8. Receive response
   ↓
9. Parse response for clickable links
   ↓
10. Display AI message with formatted links
    ↓
11. Update rate limit counter
    ↓
12. User can continue conversation or close chat
```

### 4.2 Conversation Context Management

**Context Window**: Last 10 messages (5 user + 5 assistant)

**Context Structure**
```javascript
const conversationHistory = [
  { role: 'user', content: 'I like math and physics' },
  { role: 'assistant', content: 'Great! Engineering could be perfect...' },
  { role: 'user', content: 'What about computer science?' },
  { role: 'assistant', content: 'Computer Science is excellent...' }
  // ... up to 10 messages
];
```


**Context Pruning Strategy**
- Keep only last 10 messages to manage token costs
- Preserve system prompt in every request
- Include current page URL for context-aware responses

### 4.3 Link Generation & Parsing

**AI Response Format**
```
AI generates: "You should explore [Computer Science](/career/computer-science) or [Mechanical Engineering](/career/mechanical-engineering)."

Frontend parses and converts to:
"You should explore <Link to="/career/computer-science">Computer Science</Link> or <Link to="/career/mechanical-engineering">Mechanical Engineering</Link>."
```

**Link Parsing Logic**
```javascript
const parseLinks = (text) => {
  const linkRegex = /\[([^\]]+)\]\(([^)]+)\)/g;
  return text.replace(linkRegex, (match, label, url) => {
    return `<a href="${url}" class="text-blue-600 underline hover:text-blue-800">${label}</a>`;
  });
};
```


### 4.4 Sample Conversation Flows

**Flow 1: Stream Selection (Class 10 Student)**
```
User: "I'm in Class 10. Should I take Science or Commerce?"

AI: "Great question! Let me help you decide:

If you enjoy Math, Physics, and problem-solving → Consider [Science PCM](/stream/science-pcm)
- Opens doors to Engineering, Architecture, Data Science
- Entrance exams: JEE Main, JEE Advanced, BITSAT

If you're interested in Biology and healthcare → Consider [Science PCB](/stream/science-pcb)
- Leads to Medical, Pharmacy, Biotechnology
- Entrance exams: NEET, AIIMS

If you like Business, Economics, and Finance → Consider [Commerce](/stream/commerce)
- Careers: CA, CS, MBA, Banking
- Entrance exams: CAT, CLAT (for law)

What subjects do you enjoy most?"
```

**Flow 2: Career Exploration**
```
User: "Tell me about computer science"

AI: "Computer Science is one of the most in-demand fields! Here's what you should know:

📚 [Computer Science Engineering](/career/computer-science) offers:
- Average Salary: ₹6-8 LPA (freshers), ₹15-25 LPA (experienced)
- Top Companies: Google, Microsoft, Amazon, TCS, Infosys
- Job Roles: Software Developer, Data Scientist, AI Engineer

🎓 Entrance Exams:
- [JEE Main](/exam/jee-main) - For NITs, IIITs
- [JEE Advanced](/exam/jee-advanced) - For IITs

💡 Skills Needed: Programming, Problem-solving, Mathematics

Would you like to know about related careers like [Data Science](/career/data-science)?"
```


---

## 5. Page Templates & Content Structure

### 5.1 Career Page Template

**Standard Structure** (Applied to all 43 career pages)

```jsx
<CareerPage>
  {/* Hero Section */}
  <section className="bg-gradient-to-r from-blue-600 to-purple-600 text-white py-16">
    <h1 className="text-4xl font-bold">{careerTitle}</h1>
    <p className="text-xl mt-4">{tagline}</p>
  </section>

  {/* Overview Section */}
  <section className="py-12">
    <h2>What is {careerTitle}?</h2>
    <p>{detailedDescription}</p>
  </section>

  {/* Salary Section */}
  <section className="bg-gray-50 py-12">
    <h2>💰 Salary Expectations</h2>
    <div className="grid md:grid-cols-3 gap-6">
      <Card>
        <h3>Fresher</h3>
        <p className="text-3xl font-bold">{fresherSalary}</p>
      </Card>
      <Card>
        <h3>Mid-Level (5-10 years)</h3>
        <p className="text-3xl font-bold">{midLevelSalary}</p>
      </Card>
      <Card>
        <h3>Senior (10+ years)</h3>
        <p className="text-3xl font-bold">{seniorSalary}</p>
      </Card>
    </div>
  </section>

  {/* Skills Section */}
  <section className="py-12">
    <h2>🎯 Required Skills</h2>
    <ul className="grid md:grid-cols-2 gap-4">
      {skills.map(skill => (
        <li className="flex items-center">
          <CheckIcon /> {skill}
        </li>
      ))}
    </ul>
  </section>

  {/* Entrance Exams Section */}
  <section className="bg-blue-50 py-12">
    <h2>📝 Entrance Exams</h2>
    <div className="grid md:grid-cols-2 gap-6">
      {exams.map(exam => (
        <ExamCard exam={exam} />
      ))}
    </div>
  </section>

  {/* Top Colleges Section */}
  <section className="py-12">
    <h2>🏛️ Top Institutions</h2>
    <ul>{colleges.map(college => <li>{college}</li>)}</ul>
  </section>

  {/* Job Roles Section */}
  <section className="bg-gray-50 py-12">
    <h2>💼 Career Opportunities</h2>
    <div className="grid md:grid-cols-3 gap-6">
      {jobRoles.map(role => (
        <Card>
          <h3>{role.title}</h3>
          <p>{role.description}</p>
        </Card>
      ))}
    </div>
  </section>

  {/* Pros & Cons Section */}
  <section className="py-12">
    <div className="grid md:grid-cols-2 gap-8">
      <div>
        <h2>✅ Pros</h2>
        <ul>{pros.map(pro => <li>{pro}</li>)}</ul>
      </div>
      <div>
        <h2>⚠️ Cons</h2>
        <ul>{cons.map(con => <li>{con}</li>)}</ul>
      </div>
    </div>
  </section>

  {/* AI Impact Section */}
  <section className="bg-purple-50 py-12">
    <h2>🤖 AI Impact Analysis</h2>
    <p>{aiImpactDescription}</p>
    <div className="mt-6">
      <Badge color={impactLevel}>{impactLevel}</Badge>
    </div>
  </section>

  {/* Future Outlook Section */}
  <section className="py-12">
    <h2>🔮 Future Outlook (2030+)</h2>
    <p>{futureOutlook}</p>
  </section>

  {/* CTA Section */}
  <section className="bg-blue-600 text-white py-12 text-center">
    <h2>Ready to explore this career?</h2>
    <Button onClick={openChat}>Chat with AI Counselor</Button>
  </section>
</CareerPage>
```


### 5.2 Stream Selection Page Template

```jsx
<StreamPage>
  {/* Hero Section */}
  <section className="hero">
    <h1>{streamName} Stream</h1>
    <p>{streamDescription}</p>
  </section>

  {/* Subject Combination */}
  <section>
    <h2>📚 Subjects You'll Study</h2>
    <div className="grid md:grid-cols-3 gap-4">
      {subjects.map(subject => (
        <Card>
          <h3>{subject.name}</h3>
          <p>{subject.description}</p>
        </Card>
      ))}
    </div>
  </section>

  {/* Career Options */}
  <section>
    <h2>🎯 Career Paths Available</h2>
    <div className="grid md:grid-cols-3 gap-6">
      {careers.map(career => (
        <CareerCard career={career} />
      ))}
    </div>
  </section>

  {/* Entrance Exams */}
  <section>
    <h2>📝 Major Entrance Exams</h2>
    <div className="grid md:grid-cols-2 gap-6">
      {exams.map(exam => (
        <ExamCard exam={exam} />
      ))}
    </div>
  </section>

  {/* Who Should Choose */}
  <section>
    <h2>✨ This Stream is Perfect For You If:</h2>
    <ul>
      {idealFor.map(trait => <li>{trait}</li>)}
    </ul>
  </section>

  {/* Comparison with Other Streams */}
  <section>
    <h2>⚖️ Compare with Other Streams</h2>
    <ComparisonTable streams={allStreams} />
  </section>
</StreamPage>
```


### 5.3 Entrance Exam Page Template

```jsx
<ExamPage>
  {/* Hero Section */}
  <section className="hero">
    <h1>{examName}</h1>
    <p>{examFullName}</p>
    <Badge>{examLevel}</Badge>
  </section>

  {/* Quick Facts */}
  <section>
    <h2>📊 Quick Facts</h2>
    <div className="grid md:grid-cols-4 gap-4">
      <Card>
        <h3>Exam Date</h3>
        <p>{examDate}</p>
      </Card>
      <Card>
        <h3>Application Fee</h3>
        <p>{applicationFee}</p>
      </Card>
      <Card>
        <h3>Exam Mode</h3>
        <p>{examMode}</p>
      </Card>
      <Card>
        <h3>Duration</h3>
        <p>{duration}</p>
      </Card>
    </div>
  </section>

  {/* Eligibility Criteria */}
  <section>
    <h2>✅ Eligibility Criteria</h2>
    <ul>
      <li>Age Limit: {ageLimit}</li>
      <li>Educational Qualification: {qualification}</li>
      <li>Minimum Marks: {minimumMarks}</li>
      <li>Attempts Allowed: {attemptsAllowed}</li>
    </ul>
  </section>

  {/* Exam Pattern */}
  <section>
    <h2>📝 Exam Pattern</h2>
    <table>
      <thead>
        <tr>
          <th>Section</th>
          <th>Questions</th>
          <th>Marks</th>
          <th>Duration</th>
        </tr>
      </thead>
      <tbody>
        {examPattern.map(section => (
          <tr>
            <td>{section.name}</td>
            <td>{section.questions}</td>
            <td>{section.marks}</td>
            <td>{section.duration}</td>
          </tr>
        ))}
      </tbody>
    </table>
  </section>

  {/* Syllabus */}
  <section>
    <h2>📚 Syllabus Overview</h2>
    {syllabus.map(subject => (
      <div>
        <h3>{subject.name}</h3>
        <ul>
          {subject.topics.map(topic => <li>{topic}</li>)}
        </ul>
      </div>
    ))}
  </section>

  {/* Colleges Accepting */}
  <section>
    <h2>🏛️ Colleges Accepting This Exam</h2>
    <ul>
      {colleges.map(college => <li>{college}</li>)}
    </ul>
  </section>

  {/* Preparation Tips */}
  <section>
    <h2>💡 Preparation Strategy</h2>
    <ul>
      {preparationTips.map(tip => <li>{tip}</li>)}
    </ul>
  </section>

  {/* Important Dates */}
  <section>
    <h2>📅 Important Dates</h2>
    <Timeline events={importantDates} />
  </section>
</ExamPage>
```


---

## 6. Data Models & Content Schema

### 6.1 Career Data Model

```typescript
interface Career {
  id: string;
  slug: string; // URL-friendly identifier
  title: string;
  category: 'engineering' | 'medical' | 'commerce' | 'arts' | 'emerging';
  tagline: string;
  description: string;
  
  salary: {
    fresher: { min: number; max: number; currency: 'INR' };
    midLevel: { min: number; max: number; currency: 'INR' };
    senior: { min: number; max: number; currency: 'INR' };
  };
  
  skills: string[];
  
  entranceExams: {
    examId: string;
    examName: string;
    importance: 'primary' | 'secondary';
  }[];
  
  topColleges: string[];
  
  jobRoles: {
    title: string;
    description: string;
    averageSalary: string;
  }[];
  
  workEnvironment: string;
  
  pros: string[];
  cons: string[];
  
  aiImpact: {
    level: 'low' | 'medium' | 'high';
    description: string;
  };
  
  futureOutlook: string;
  
  relatedCareers: string[]; // Array of career IDs
}
```


### 6.2 Entrance Exam Data Model

```typescript
interface EntranceExam {
  id: string;
  slug: string;
  name: string;
  fullName: string;
  conductedBy: string;
  level: 'national' | 'state' | 'university';
  
  dates: {
    applicationStart: string;
    applicationEnd: string;
    examDate: string;
    resultDate: string;
  };
  
  eligibility: {
    ageLimit: string;
    qualification: string;
    minimumMarks: string;
    attemptsAllowed: number;
  };
  
  examPattern: {
    mode: 'online' | 'offline' | 'both';
    duration: string;
    sections: {
      name: string;
      questions: number;
      marks: number;
      duration: string;
    }[];
  };
  
  syllabus: {
    subject: string;
    topics: string[];
  }[];
  
  fees: {
    general: number;
    obc: number;
    scst: number;
  };
  
  acceptingColleges: string[];
  
  preparationTips: string[];
  
  officialWebsite: string;
}
```


### 6.3 Stream Data Model

```typescript
interface Stream {
  id: string;
  slug: string;
  name: string;
  description: string;
  
  subjects: {
    name: string;
    description: string;
    isCompulsory: boolean;
  }[];
  
  careers: string[]; // Array of career IDs
  
  entranceExams: string[]; // Array of exam IDs
  
  idealFor: string[]; // Personality traits
  
  averageSalaryRange: {
    min: number;
    max: number;
  };
  
  popularityRank: number;
}
```

---

## 7. Performance Optimization

### 7.1 Code Splitting Strategy

```javascript
// Route-based code splitting
const HomePage = lazy(() => import('./pages/HomePage'));
const CareerPage = lazy(() => import('./pages/CareerPage'));
const StreamPage = lazy(() => import('./pages/StreamPage'));
const ExamPage = lazy(() => import('./pages/ExamPage'));

// Component lazy loading
<Suspense fallback={<LoadingSpinner />}>
  <Routes>
    <Route path="/" element={<HomePage />} />
    <Route path="/career/:slug" element={<CareerPage />} />
    {/* ... other routes */}
  </Routes>
</Suspense>
```


### 7.2 Image Optimization

```javascript
// Responsive images
<img 
  src="/images/career-hero.webp"
  srcSet="
    /images/career-hero-320w.webp 320w,
    /images/career-hero-640w.webp 640w,
    /images/career-hero-1024w.webp 1024w
  "
  sizes="(max-width: 640px) 100vw, (max-width: 1024px) 50vw, 33vw"
  alt="Career illustration"
  loading="lazy"
/>
```

**Image Guidelines**
- Use WebP format for modern browsers
- Provide fallback to JPEG/PNG
- Implement lazy loading for below-fold images
- Compress images to <100KB where possible
- Use CDN for image delivery

### 7.3 Caching Strategy

**Browser Caching**
```javascript
// netlify.toml
[[headers]]
  for = "/*.js"
  [headers.values]
    Cache-Control = "public, max-age=31536000, immutable"

[[headers]]
  for = "/*.css"
  [headers.values]
    Cache-Control = "public, max-age=31536000, immutable"

[[headers]]
  for = "/images/*"
  [headers.values]
    Cache-Control = "public, max-age=604800"
```

**API Response Caching**
- Cache common AI responses (future enhancement)
- Use service worker for offline support (PWA)


### 7.4 Bundle Size Optimization

**Target Metrics**
- Initial bundle: <200KB (gzipped)
- Per-route chunk: <50KB (gzipped)
- Total JavaScript: <500KB (gzipped)

**Optimization Techniques**
```javascript
// Tree shaking - import only what's needed
import { useState, useEffect } from 'react'; // ✅
// import * as React from 'react'; // ❌

// Use lightweight alternatives
import { MessageCircle } from 'lucide-react'; // ✅ 1KB
// import { FaComments } from 'react-icons/fa'; // ❌ 50KB+

// Dynamic imports for heavy components
const HeavyChart = lazy(() => import('./HeavyChart'));
```

---

## 8. Security Architecture

### 8.1 Security Layers

```
┌─────────────────────────────────────────┐
│   Layer 1: Network Security             │
│   - HTTPS enforcement                   │
│   - CORS policies                       │
│   - DDoS protection (Netlify)           │
└─────────────────────────────────────────┘
                  ↓
┌─────────────────────────────────────────┐
│   Layer 2: Input Validation             │
│   - Length limits                       │
│   - HTML/script tag removal             │
│   - Character encoding validation       │
└─────────────────────────────────────────┘
                  ↓
┌─────────────────────────────────────────┐
│   Layer 3: Rate Limiting                │
│   - IP-based throttling                 │
│   - Token bucket algorithm              │
│   - Automatic cooldown periods          │
└─────────────────────────────────────────┘
                  ↓
┌─────────────────────────────────────────┐
│   Layer 4: API Security                 │
│   - Server-side API keys                │
│   - Environment variable protection     │
│   - Request signing (future)            │
└─────────────────────────────────────────┘
```


### 8.2 CORS Configuration

```javascript
// netlify/functions/chat.js
export const handler = async (event) => {
  const headers = {
    'Access-Control-Allow-Origin': 'https://hilarious-stroopwafel-f4a49b.netlify.app',
    'Access-Control-Allow-Headers': 'Content-Type',
    'Access-Control-Allow-Methods': 'POST, OPTIONS',
  };

  // Handle preflight requests
  if (event.httpMethod === 'OPTIONS') {
    return {
      statusCode: 200,
      headers,
      body: ''
    };
  }

  // ... rest of handler logic
};
```

### 8.3 Content Security Policy

```html
<!-- index.html -->
<meta http-equiv="Content-Security-Policy" content="
  default-src 'self';
  script-src 'self' 'unsafe-inline';
  style-src 'self' 'unsafe-inline';
  img-src 'self' data: https:;
  font-src 'self' data:;
  connect-src 'self' https://api.groq.com;
">
```

---

## 9. Deployment Architecture

### 9.1 CI/CD Pipeline

```
┌─────────────────────────────────────────┐
│   Developer pushes to GitHub            │
└─────────────────────────────────────────┘
                  ↓
┌─────────────────────────────────────────┐
│   GitHub triggers Netlify webhook       │
└─────────────────────────────────────────┘
                  ↓
┌─────────────────────────────────────────┐
│   Netlify Build Process                 │
│   1. Install dependencies (npm ci)      │
│   2. Run build (npm run build)          │
│   3. Run tests (optional)               │
│   4. Generate static files              │
└─────────────────────────────────────────┘
                  ↓
┌─────────────────────────────────────────┐
│   Deploy to Netlify CDN                 │
│   - Atomic deployments                  │
│   - Instant rollback capability         │
│   - Preview deployments for PRs         │
└─────────────────────────────────────────┘
                  ↓
┌─────────────────────────────────────────┐
│   Live on Production                    │
│   https://hilarious-stroopwafel...      │
└─────────────────────────────────────────┘
```


### 9.2 Environment Configuration

```javascript
// .env.development
VITE_API_URL=http://localhost:8888/.netlify/functions
VITE_ENVIRONMENT=development

// .env.production
VITE_API_URL=https://hilarious-stroopwafel-f4a49b.netlify.app/.netlify/functions
VITE_ENVIRONMENT=production

// Server-side (Netlify)
GROQ_API_KEY=gsk_xxxxxxxxxxxxx (secret)
```

### 9.3 Monitoring & Logging

**Netlify Analytics**
- Page views and unique visitors
- Top pages and referrers
- Bandwidth usage

**Function Logs**
- Request/response logging
- Error tracking
- Performance metrics (execution time)

**Future Enhancements**
- Sentry for error tracking
- Google Analytics for user behavior
- Hotjar for heatmaps and session recordings

---

## 10. Accessibility Design

### 10.1 WCAG 2.1 AA Compliance

**Color Contrast**
- Text: Minimum 4.5:1 ratio
- Large text (18pt+): Minimum 3:1 ratio
- Interactive elements: Minimum 3:1 ratio

**Keyboard Navigation**
- All interactive elements accessible via Tab
- Skip to main content link
- Focus indicators visible
- Logical tab order


**Screen Reader Support**
```jsx
// Semantic HTML
<nav aria-label="Main navigation">
  <ul>
    <li><a href="/">Home</a></li>
  </ul>
</nav>

// ARIA labels
<button aria-label="Open chat" onClick={openChat}>
  <MessageCircle />
</button>

// Alt text for images
<img src="/career.jpg" alt="Students discussing career options" />

// Skip link
<a href="#main-content" className="sr-only focus:not-sr-only">
  Skip to main content
</a>
```

### 10.2 Mobile Accessibility

- Minimum touch target size: 44x44px
- Sufficient spacing between interactive elements
- Readable font sizes (minimum 16px)
- Zoom support (no maximum-scale restriction)

---

## 11. Future Architecture Enhancements

### 11.1 Phase 2 Architecture (User Accounts)

```
┌─────────────────────────────────────────┐
│   Frontend (React)                      │
└─────────────────────────────────────────┘
                  ↓
┌─────────────────────────────────────────┐
│   Authentication Layer                  │
│   - Google OAuth                        │
│   - JWT tokens                          │
│   - Session management                  │
└─────────────────────────────────────────┘
                  ↓
┌─────────────────────────────────────────┐
│   API Gateway (Netlify Functions)       │
│   - User profile endpoints              │
│   - Conversation history endpoints      │
│   - Bookmark endpoints                  │
└─────────────────────────────────────────┘
                  ↓
┌─────────────────────────────────────────┐
│   Database (PostgreSQL/Supabase)        │
│   - Users table                         │
│   - Conversations table                 │
│   - Bookmarks table                     │
└─────────────────────────────────────────┘
```


### 11.2 Phase 3 Architecture (Mobile App)

```
┌─────────────────────────────────────────┐
│   React Native App (iOS/Android)        │
│   - Shared components with web          │
│   - Native navigation                   │
│   - Offline support                     │
└─────────────────────────────────────────┘
                  ↓
┌─────────────────────────────────────────┐
│   Shared API Layer                      │
│   - Same backend as web                 │
│   - Mobile-optimized responses          │
└─────────────────────────────────────────┘
```

### 11.3 Phase 4 Architecture (Scale)

```
┌─────────────────────────────────────────┐
│   Load Balancer                         │
└─────────────────────────────────────────┘
                  ↓
┌─────────────────────────────────────────┐
│   Microservices                         │
│   ├── Chat Service                      │
│   ├── Content Service                   │
│   ├── User Service                      │
│   └── Analytics Service                 │
└─────────────────────────────────────────┘
                  ↓
┌─────────────────────────────────────────┐
│   Data Layer                            │
│   ├── PostgreSQL (user data)            │
│   ├── Redis (caching)                   │
│   └── S3 (media storage)                │
└─────────────────────────────────────────┘
```

---

## 12. Testing Strategy

### 12.1 Testing Pyramid

```
                    ┌─────────────┐
                    │   E2E Tests │  (5%)
                    │   Cypress   │
                    └─────────────┘
                  ┌─────────────────┐
                  │ Integration Tests│ (15%)
                  │   React Testing  │
                  │     Library      │
                  └─────────────────┘
              ┌───────────────────────┐
              │    Unit Tests         │ (80%)
              │    Jest + Vitest      │
              └───────────────────────┘
```


### 12.2 Test Coverage Goals

**Unit Tests**
- Utility functions (input sanitization, link parsing): 100%
- React components: 80%
- API handlers: 90%

**Integration Tests**
- User flows (chat interaction, navigation): 70%
- API integration: 80%

**E2E Tests**
- Critical user journeys: 100%
  - Homepage → Career page → Chat
  - Stream selection → Career exploration
  - Entrance exam information lookup

### 12.3 Performance Testing

**Lighthouse Targets**
- Performance: 90+
- Accessibility: 95+
- Best Practices: 95+
- SEO: 90+

**Load Testing**
- Simulate 1,000 concurrent users
- API response time <3 seconds under load
- Zero errors at target load

---

## 13. SEO Strategy

### 13.1 On-Page SEO

```jsx
// Meta tags for each page
<Helmet>
  <title>Computer Science Engineering Career Guide | AI Career Counselor</title>
  <meta name="description" content="Complete guide to Computer Science Engineering in India. Salary, entrance exams (JEE), top colleges, job opportunities, and AI impact analysis." />
  <meta name="keywords" content="computer science, engineering, JEE, IIT, career guidance, India" />
  
  {/* Open Graph */}
  <meta property="og:title" content="Computer Science Engineering Career Guide" />
  <meta property="og:description" content="Complete career guide for aspiring computer science engineers" />
  <meta property="og:image" content="/images/cs-og-image.jpg" />
  <meta property="og:url" content="https://hilarious-stroopwafel-f4a49b.netlify.app/career/computer-science" />
  
  {/* Twitter Card */}
  <meta name="twitter:card" content="summary_large_image" />
  <meta name="twitter:title" content="Computer Science Engineering Career Guide" />
  <meta name="twitter:description" content="Complete career guide for aspiring computer science engineers" />
  <meta name="twitter:image" content="/images/cs-twitter-image.jpg" />
</Helmet>
```


### 13.2 Technical SEO

```xml
<!-- sitemap.xml -->
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  <url>
    <loc>https://hilarious-stroopwafel-f4a49b.netlify.app/</loc>
    <changefreq>weekly</changefreq>
    <priority>1.0</priority>
  </url>
  <url>
    <loc>https://hilarious-stroopwafel-f4a49b.netlify.app/career/computer-science</loc>
    <changefreq>monthly</changefreq>
    <priority>0.8</priority>
  </url>
  <!-- ... all career pages -->
</urlset>
```

```txt
# robots.txt
User-agent: *
Allow: /
Sitemap: https://hilarious-stroopwafel-f4a49b.netlify.app/sitemap.xml
```

### 13.3 Content SEO Strategy

**Target Keywords**
- Primary: "career counseling India", "career guidance after 10th", "engineering careers"
- Long-tail: "computer science salary in India", "JEE exam preparation tips", "best careers after 12th science"

**Content Optimization**
- H1 tag on every page with primary keyword
- Structured data (Schema.org) for career pages
- Internal linking between related careers
- Regular content updates (quarterly)

---

## 14. Analytics & Metrics

### 14.1 Key Performance Indicators (KPIs)

**User Engagement**
```javascript
// Track with Google Analytics
gtag('event', 'page_view', {
  page_title: 'Computer Science Career',
  page_location: window.location.href,
  page_path: '/career/computer-science'
});

gtag('event', 'chat_interaction', {
  event_category: 'engagement',
  event_label: 'message_sent'
});

gtag('event', 'career_exploration', {
  event_category: 'engagement',
  event_label: 'career_page_view',
  value: careerName
});
```


**Conversion Tracking**
- Chat initiation rate
- Career page depth (pages per session)
- Return visitor rate
- Time on site
- Bounce rate by page type

### 14.2 Dashboard Metrics

```
┌─────────────────────────────────────────┐
│   Real-time Metrics                     │
│   - Active users                        │
│   - Current page views                  │
│   - Chat conversations in progress      │
└─────────────────────────────────────────┘

┌─────────────────────────────────────────┐
│   Daily Metrics                         │
│   - Total users                         │
│   - New vs returning                    │
│   - Top career pages                    │
│   - Chat engagement rate                │
│   - Average session duration            │
└─────────────────────────────────────────┘

┌─────────────────────────────────────────┐
│   Weekly/Monthly Trends                 │
│   - User growth rate                    │
│   - Geographic distribution             │
│   - Device breakdown                    │
│   - Traffic sources                     │
└─────────────────────────────────────────┘
```

---

## 15. Error Handling & User Feedback

### 15.1 Error States

**Network Error**
```jsx
<ErrorMessage>
  <AlertCircle className="text-red-500" />
  <h3>Connection Error</h3>
  <p>Unable to reach the server. Please check your internet connection.</p>
  <Button onClick={retry}>Try Again</Button>
</ErrorMessage>
```

**Rate Limit Error**
```jsx
<ErrorMessage>
  <Clock className="text-orange-500" />
  <h3>Rate Limit Reached</h3>
  <p>You've used all 10 free messages this hour. Please try again at {resetTime}.</p>
  <p className="text-sm">Tip: Explore our career pages while you wait!</p>
</ErrorMessage>
```

**AI Service Error**
```jsx
<ErrorMessage>
  <AlertTriangle className="text-yellow-500" />
  <h3>AI Temporarily Unavailable</h3>
  <p>Our AI counselor is taking a short break. Please try again in a few minutes.</p>
  <Button onClick={browseCareers}>Browse Career Pages</Button>
</ErrorMessage>
```


### 15.2 Loading States

**Page Loading**
```jsx
<div className="flex items-center justify-center min-h-screen">
  <div className="animate-spin rounded-full h-12 w-12 border-b-2 border-blue-600"></div>
  <p className="ml-4 text-gray-600">Loading career information...</p>
</div>
```

**Chat Loading**
```jsx
<div className="flex items-start space-x-2">
  <div className="bg-gray-200 rounded-lg p-4">
    <div className="flex space-x-2">
      <div className="w-2 h-2 bg-gray-400 rounded-full animate-bounce"></div>
      <div className="w-2 h-2 bg-gray-400 rounded-full animate-bounce delay-100"></div>
      <div className="w-2 h-2 bg-gray-400 rounded-full animate-bounce delay-200"></div>
    </div>
  </div>
</div>
```

### 15.3 Success Feedback

**Message Sent**
```jsx
<Toast type="success">
  <CheckCircle className="text-green-500" />
  <p>Message sent! AI is thinking...</p>
</Toast>
```

**Bookmark Added** (Future)
```jsx
<Toast type="success">
  <Bookmark className="text-blue-500" />
  <p>Career saved to your bookmarks!</p>
</Toast>
```

---

## 16. Internationalization (Future)

### 16.1 Multi-language Support Architecture

```javascript
// i18n configuration
import i18n from 'i18next';
import { initReactI18next } from 'react-i18next';

i18n
  .use(initReactI18next)
  .init({
    resources: {
      en: { translation: enTranslations },
      hi: { translation: hiTranslations },
      ta: { translation: taTranslations }
    },
    lng: 'en',
    fallbackLng: 'en',
    interpolation: {
      escapeValue: false
    }
  });
```


### 16.2 Language-Specific Content

```javascript
// Career page in multiple languages
const careerContent = {
  en: {
    title: 'Computer Science Engineering',
    description: 'Build software and solve problems...'
  },
  hi: {
    title: 'कंप्यूटर विज्ञान इंजीनियरिंग',
    description: 'सॉफ्टवेयर बनाएं और समस्याओं को हल करें...'
  }
};
```

**Language Selector**
```jsx
<select onChange={changeLanguage} value={currentLanguage}>
  <option value="en">English</option>
  <option value="hi">हिंदी</option>
  <option value="ta">தமிழ்</option>
  <option value="te">తెలుగు</option>
  <option value="bn">বাংলা</option>
</select>
```

---

## 17. Design Principles & Guidelines

### 17.1 Core Design Principles

1. **Mobile-First**: Design for smallest screen, enhance for larger
2. **Accessibility**: WCAG 2.1 AA compliance minimum
3. **Performance**: <2 second load time on 4G
4. **Simplicity**: Clear hierarchy, minimal cognitive load
5. **Consistency**: Reusable components, unified design language
6. **Trust**: Honest information, transparent limitations

### 17.2 Visual Hierarchy

```
Level 1: Page Title (text-4xl, font-bold)
Level 2: Section Headings (text-3xl, font-semibold)
Level 3: Subsection Headings (text-2xl, font-semibold)
Level 4: Card Titles (text-xl, font-medium)
Body Text: (text-base, font-normal)
Small Text: (text-sm, font-normal)
```


### 17.3 Interaction Patterns

**Hover States**
- Cards: Elevate shadow, slight scale (1.02)
- Buttons: Darken background color
- Links: Underline, color change

**Active States**
- Buttons: Pressed effect (scale 0.98)
- Form inputs: Border color change, focus ring

**Transitions**
```css
/* Smooth transitions for all interactive elements */
.transition-all {
  transition-property: all;
  transition-timing-function: cubic-bezier(0.4, 0, 0.2, 1);
  transition-duration: 150ms;
}
```

---

## 18. Documentation Standards

### 18.1 Code Documentation

```javascript
/**
 * Sanitizes user input to prevent XSS attacks
 * @param {string} input - Raw user input
 * @returns {string} Sanitized input safe for processing
 * @throws {Error} If input exceeds maximum length
 */
function sanitizeInput(input) {
  // Implementation
}
```

### 18.2 Component Documentation

```jsx
/**
 * CareerCard Component
 * 
 * Displays a career option with title, description, and salary range
 * 
 * @component
 * @param {Object} props
 * @param {string} props.title - Career title
 * @param {string} props.description - Brief career description
 * @param {string} props.salary - Salary range
 * @param {string} props.link - URL to detailed career page
 * 
 * @example
 * <CareerCard 
 *   title="Computer Science"
 *   description="Build software..."
 *   salary="₹6-25 LPA"
 *   link="/career/computer-science"
 * />
 */
const CareerCard = ({ title, description, salary, link }) => {
  // Implementation
};
```

---

## 19. Version Control Strategy

### 19.1 Git Workflow

```
main (production)
  ↑
  └── develop (staging)
        ↑
        ├── feature/career-pages
        ├── feature/ai-chatbot
        ├── feature/exam-hub
        └── bugfix/rate-limit-issue
```


### 19.2 Commit Message Convention

```
feat: Add Computer Science career page
fix: Resolve rate limiting bug in chat API
docs: Update README with deployment instructions
style: Format code with Prettier
refactor: Simplify career page component structure
test: Add unit tests for input sanitization
chore: Update dependencies to latest versions
```

### 19.3 Branch Naming

```
feature/feature-name
bugfix/bug-description
hotfix/critical-issue
release/version-number
```

---

## 20. Maintenance & Updates

### 20.1 Content Update Schedule

**Quarterly (Every 3 months)**
- Review and update salary data
- Verify entrance exam dates and patterns
- Add new emerging careers
- Update AI impact analysis

**Annual (Yearly)**
- Comprehensive content audit
- Update college rankings and cutoffs
- Review and refresh all career pages
- Update technology stack dependencies

### 20.2 Technical Maintenance

**Weekly**
- Monitor error logs
- Check API usage and costs
- Review performance metrics
- Security vulnerability scans

**Monthly**
- Dependency updates (npm audit)
- Performance optimization review
- Backup verification
- Analytics review

---

## 21. Conclusion

This design document provides a comprehensive blueprint for the AI Career Counselor platform. The architecture prioritizes:

1. **Scalability**: Serverless architecture ready to grow from 1K to 100K+ users
2. **Performance**: <2 second load times, optimized for Indian 4G networks
3. **Security**: Multi-layer protection with rate limiting and input sanitization
4. **Accessibility**: Mobile-first, WCAG compliant, reaching underserved communities
5. **Maintainability**: Modular components, clear documentation, automated deployment

The platform successfully addresses the critical career guidance gap in India through intelligent AI counseling, comprehensive content coverage, and a user-centric design approach.

---

**Document Version**: 1.0  
**Last Updated**: February 3, 2026  
**Next Review Date**: May 1, 2026  
**Maintained By**: Development Team (Vishal Bhatia, Praduman, Pranjal)
