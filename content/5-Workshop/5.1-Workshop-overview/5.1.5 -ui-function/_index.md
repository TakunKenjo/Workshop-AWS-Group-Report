---
title : "UI specification and system functionality"
date : 2024-01-01 
weight : 5
chapter : false
pre : " <b> 5.1.5 </b> "
---

### 1. Login Interface


![image56.png](/images/5-Workshop/5.1-Workshop-overview/5.1.5-ui-function/image56.png)


#### 1.1. Interface Overview

Allows registered users to authenticate identity to access the SmartDocAI system. Supports 2 main authentication methods:

- **Native Authentication**: Uses Email and Password.
- **SSO (Social Login)**: Uses Google account via AWS Cognito Hosted UI (OAuth2 / OIDC).

#### 1.2. Layout Design and Interface Components


| No. | Component Name | Component Type | Description / Display Spec |
| --- | --- | --- | --- |
| 1 | Form Title | `<h2>`, `<p>` | Displays "Login" title with link "Register now!" navigating to `/register`. |
| 2 | Email | Input (Email) | Personal Email input field. Integrates Mail Icon on the left. |
| 3 | Password | Input (Password) | Password input field. Integrates Lock Icon on left and show/hide password toggle (Eye/EyeOff Icon) on right. |
| 4 | "Login" Button | Button | Primary Action Button. Displays spinner "Logging in..." while request is being processed. |
| 5 | Divider | Divider | Text "or continue with" separating authentication methods. |
| 6 | "Sign in with Google" Button | Button (Social) | Button containing official Google SVG logo with shimmer hover effect. Redirects to Cognito Hosted UI on click. |


#### 1.3. Input Data Validation Rules


| Data Field | Validation Rules | Displayed Error Message |
| --- | --- | --- |
| Email | - Cannot be empty. Must match standard Email format (`user@domain.com`). | - "Please enter email.", "Invalid email." |
| Password | Cannot be empty. | "Please enter password." |


#### 1.4. Exception Handling & System Safety Mechanisms

- **Automatic Username Resolution**:
  When users initially sign up via Google OAuth, Cognito generates an internal username like `Google_10293847...`. When attempting manual login with Email/Password after setting a native password, Cognito throws `UserNotFoundException` because the internal username is not the email address. The system automatically intercepts `UserNotFoundException`, calls `/api/auth/resolve-login-username` via Redux Thunk to look up the real username tied to that email, and retries automatically without requiring user interaction.

- **Secure Session & Token Storage**:
  JWT Tokens and session info (`id_token`, `access_token`, `refresh_token`, `auth_user`) are stored in `sessionStorage`, automatically clearing when the tab closes to prevent information security risks on public computers.

- **Status Feedback & Form Spam Prevention**:
  The "Login" button immediately disables and activates a Spinner while exchanging data with AWS Cognito, preventing double-clicks or duplicate request submissions.

### 2. Registration Interface

Designed as a 2-step process on a single interface:

##### Step 1 (Register): Enter personal info and password


![image57.png](/images/5-Workshop/5.1-Workshop-overview/5.1.5-ui-function/image57.png)


##### Step 2 (Confirm OTP): Enter 6-digit verification code automatically sent via Email to activate account.


![image58.png](/images/5-Workshop/5.1-Workshop-overview/5.1.5-ui-function/image58.png)


#### 2.1. Interface Overview

The Registration interface (`/register`) serves as the onboarding point for new users into SmartDocAI. Designed in a modern Dark Mode theme, adhering to UX/UI guidelines for optimized user experience.

#### 2.2. Layout Design and Interface Components

##### 1. Step 1 Interface: Enter registration info


| No. | Component Name | Component Type | Description / Display Spec |
| --- | --- | --- | --- |
| 1 | Form Title | `<h2>`, `<p>` | Displays "Create account" title with link to "Login" if user already has an account. |
| 2 | Full Name | Input (Text) | Input field for user's full name. Integrates User Icon. |
| 3 | Email | Input (Email) | Input field for personal email (used as Cognito authentication username). Integrates Mail Icon. |
| 4 | Phone | Input (Tel) | Contact phone number input. Integrates Phone Icon. Displayed side-by-side with Date of Birth on Desktop. |
| 5 | Date of Birth | Input (Date) | Date of birth picker. Integrates Calendar Icon. |
| 6 | Password | Input (Password) | Password input field. Integrates Lock Icon and show/hide toggle. |
| 7 | Confirm Password | Input (Password) | Re-enter password to check match. Integrates show/hide toggle. |
| 8 | "Create account" Button | Button | Primary Action Button. Displays loading spinner on submit. |


##### 2. Step 2 Interface: OTP Verification


| No. | Component Name | Component Type | Description / Display Spec |
| --- | --- | --- | --- |
| 1 | Email Notification | `<p>` | Displays registered email address (highlighted in Blue) reminding user to check inbox. |
| 2 | Verification Code (OTP) | Input (Text) | 6-digit input field, center-aligned, large tracking-widest font. |
| 3 | "Activate account" Button | Button | Submits OTP code to Cognito/Backend API to complete verification. |
| 4 | "Back to register" Button | Button (Text) | Allows returning to Step 1 if user wants to correct info or email. |


#### 2.3. Input Data Validation Rules


| Data Field | Validation Rules | Displayed Error Message |
| --- | --- | --- |
| Full Name | Required, length 2 to 100 characters, no dangerous special characters or HTML/script tags (XSS protection). | "Full name must be 2-100 characters, no special characters." |
| Email | Required, cannot be empty. Must match RFC 5322 standard format (`user@domain.com`). | "Email cannot be empty.", "Invalid email." |
| Phone | Required. Accepts Vietnam format (`0901234567`) or international E.164 (`+84901234567`), length 10-15 digits. | "Phone number cannot be empty.", "Invalid phone number." |
| Date of Birth | Required. Format `YYYY-MM-DD`, birth year between 1900 and current year. | "Date of birth cannot be empty.", "Date of birth must follow YYYY-MM-DD format (1900-2026)." |
| Avatar | Must be image format: JPG, PNG, WEBP. Maximum file size 5 MB. | "Unsupported image file format. Only JPG, PNG, WEBP accepted.", "Image too large, maximum 5MB." |

### 3. AI Chat & Q&A Area (ChatArea)

#### 3.1. Interface Overview
The core workspace of SmartDocAI, combining `SmartSidebar` (left) and `ChatArea` (right).

#### 3.2. Layout Design and Interface Components


| No. | Component Block | Component Type | Description / Display Spec |
| --- | --- | --- | --- |
| 1 | Header & Logo | BrainCircuit, Toggle | Displays brand name "SmartDocAI", Sidebar collapse/expand toggle, Light/Dark mode theme toggle. |
| 2 | Mobile Topbar | Header Bar | Displayed on small screens (< md), contains Hamburger Menu button to toggle mobile drawer. |
| 3 | Welcome Screen | WelcomeHero | Displayed when no conversation exists. 3-step guide: 1. Upload document ➔ 2. System processes ➔ 3. Ask questions. |
| 4 | Advanced Search Panel | SearchSettings | Expander panel for configuring RAG algorithms: File Filter, Hybrid Search, Re-ranking, Self-RAG, Co-RAG. |
| 5 | Message List | ChatMessage | Container for question (User) and answer (SmartDocAI) bubbles. Supports Markdown, Code Blocks, Math formulas, and Data tables. |
| 6 | Source Citations | Citations Viewer | Displayed under each AI response, pointing out source documents (filename, page number, original content excerpt). |
| 7 | Reasoning Steps | Thinking Process | Displays AI retrieval/reasoning steps for response transparency. |
| 8 | Question Input Bar | ChatInput | Multi-line textarea auto-expanding with text, SendIcon button, Stop generation button, Clear session history button. |


#### 3.3. Search Strategy Configuration Rules

The `SearchSettings` control panel allows users to toggle advanced RAG technologies per search scenario:


| Strategy / Feature | Mechanism | Optimal Use Case |
| --- | --- | --- |
| File Filter | Restricts Vector search space to one or more user-selected files. | Searching data in 1 specified report/contract without mixing with other documents. |
| Hybrid Search | Combines Vector Search (Bedrock Titan Embeddings V2 60%) and Keyword Search (BM25 Algorithm 40%). | When questions contain specialized terminology, identifiers, contract numbers, or exact keywords. |
| Re-ranking | Uses Cross-Encoder model (`ms-marco-MiniLM-L-12-v2`) to re-rank top-K most relevant text chunks. | Filters noise out, increasing precision of context passed to LLM prompt. |
| Self-RAG | 3-step AI self-reflection loop: Query Rewriting, Relevance Filtering, Answer Grading. | Complex questions requiring AI to rewrite query if initial search info is insufficient. |
| Co-RAG (Multi-Agent) | Multi-agent collaboration: Semantic Agent (Vector), Keyword Agent (BM25), Conceptual Agent (LLM Concept). Merges results via voting, union, or intersection strategies. | Demands deep multi-perspective knowledge synthesis across documents. |


#### 3.4. State Handling & Exceptions on Main Page

- **No Document Uploaded**: Chat area shows `WelcomeHero` screen. Advanced RAG toggles in `SearchSettings` are automatically disabled.
- **File Processing Error (File Corrupted / Format Mismatch)**: System fires Toast alert without interrupting layout experience.
- **Per-User Isolation**: All documents and chat histories are tied to `user_id` (`sub`). User A cannot view or query User B's documents on S3/Vector Index.

### 4. Personal Profile Interface

#### 4.1. Interface Overview
Allows users to view and update personal profile info in SmartDocAI:
- **Design Style**: Deep Dark Theme with subtle borders and card layout.
- **Navigation Tabs**: *Personal Info* (Active) and *Security*.
- **2 Main Cards**: Left Card (Avatar upload, drag-and-drop JPG/PNG/WEBP up to 5MB), Right Card (Form for Full Name, Email, Phone, Date of Birth, and "Save changes" button).

#### 4.2. Layout Design and Interface Components


| No. | Component Name | Component Type | Description / Display Spec |
| --- | --- | --- | --- |
| 1 | Back Button | Button / Link | Text "← Back", located top-left, returns to Chat / Dashboard screen. |
| 2 | Header User Info | Avatar & Text | Displays round avatar with initial letter alongside Full Name and Email at the top. |
| 3 | Tab Bar | Navigation Tabs | 2 tabs: Personal Info (default selected) and Security. |
| 4 | Avatar Card Title | Heading 3 | Text "Avatar" at top of left card. |
| 5 | Avatar Display Area | Circle Avatar | Round blue avatar displaying initial letter or uploaded picture. |
| 6 | Drag & Drop Zone | Dropzone Container | Dashed border container: "Drag & Drop image here or click to select - JPG, PNG, WEBP - max 5MB". |
| 7 | Change Avatar Button | Secondary Button | Click opens local file explorer to choose new image. |
| 8 | Remove Avatar Button | Text Button | Deletes current avatar, reverts to default initial letter avatar. |
| 9 | Full Name Input | Input (Text) | Pre-filled with current full name from DynamoDB, editable. |
| 10 | Email Input | Input (Email, Disabled) | Pre-filled with registered email, read-only (grayed out). |
| 11 | Phone Input | Input (Tel) | Pre-filled with phone number, editable. |
| 12 | Date of Birth Input | Input (Date) | Pre-filled with birth date, date picker editable. |
| 13 | "Save changes" Button | Primary Button | Sends PUT request to update profile in DynamoDB. Shows spinner while saving. |


### 4. SPECIAL CASES HANDLING & DATA SAFETY

- **Flexible `password_set` flag**: Ensures users logged in via Google OAuth are not blocked by "Current Password" field, enabling easy native password setup.
- **Input File Validation**: Automatically blocks non-image files or files larger than 5MB before compression.
- **Reactive UI Sync**: Updating Avatar or Full Name in Profile instantly updates Redux Store state, syncing Sidebar and Header without requiring page reload.
