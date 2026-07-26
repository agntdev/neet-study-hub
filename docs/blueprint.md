# Study Hub NEET — Bot specification

**Archetype:** education

**Voice:** professional and encouraging — write every user-facing message, button label, error, and empty state in this voice.

A mobile-first NEET UG study manager that organizes NCERT, notes, PDFs, lectures, PYQs, DPPs, formulas, mind maps and personalized recommendations for all NEET aspirants in 1:1 Telegram chats. Free to use with bookmarking, search, progress tracking and curated recommendations.

> This is the complete contract for the bot. Implement EVERY entry point, flow, feature, integration, and edge case below. The completeness review checks the bot against this document after each build pass.

## Primary audience

- NEET aspirants

## Success criteria

- User completes onboarding profile setup
- User accesses at least 3 different resource types weekly
- User engages with weekly progress summary and recommendations

## Entry points

Every feature must be reachable from the bot's command/button surface (button-first; only /start and /help are slash commands).

- **/start** (command, actor: user, command: /start) — Open the main menu
- **NCERT Hub** (button, actor: user, callback: menu:ncert) — Access NCERT resources with filters and search
  - inputs: subject, class, chapter, topic
  - outputs: filtered NCERT resources
- **Notes** (button, actor: user, callback: menu:notes) — Access study notes with filters
  - inputs: subject, class, topic
  - outputs: filtered notes
- **PDFs** (button, actor: user, callback: menu:pdfs) — Access PDF resources with filters
  - inputs: subject, class, chapter
  - outputs: filtered PDFs
- **Lectures** (button, actor: user, callback: menu:lectures) — Access video lectures with filters
  - inputs: subject, class, topic, institute
  - outputs: filtered lectures
- **PYQs** (button, actor: user, callback: menu:pyqs) — Access previous year questions with practice mode
  - inputs: subject, class, difficulty
  - outputs: PYQ practice session
- **DPP** (button, actor: user, callback: menu:dpp) — Access daily practice problems with instant feedback
  - inputs: subject, class, topic
  - outputs: DPP practice session
- **Formulas** (button, actor: user, callback: menu:formulas) — Access formula collections with filters
  - inputs: subject, class, topic
  - outputs: filtered formulas
- **Mind Maps** (button, actor: user, callback: menu:mindmaps) — Access mind maps with filters
  - inputs: subject, class, topic
  - outputs: filtered mind maps
- **Search** (button, actor: user, callback: menu:search) — Free-text search with filters
  - inputs: search query, subject, class, topic, difficulty
  - outputs: search results
- **Bookmarks** (button, actor: user, callback: menu:bookmarks) — View and manage saved resources
  - inputs: category, subject
  - outputs: bookmarked resources
- **Downloads** (button, actor: user, callback: menu:downloads) — View download history and status
  - outputs: download records
- **Recommendations** (button, actor: user, callback: menu:recommendations) — View personalized resource suggestions
  - outputs: top 3 recommended resources

## Flows

### Onboarding
_Trigger:_ /start

1. Welcome message
2. Ask for class
3. Ask for preparation level
4. Ask for weak subjects
5. Confirm profile
6. Show main menu

_Data touched:_ User profile

### Resource Access
_Trigger:_ menu:ncert

1. Show filters
2. Apply filters
3. Show resource list
4. Select resource
5. Show resource details
6. Open/download resource
7. Option to bookmark

_Data touched:_ Resource, Bookmark

### PYQ Practice
_Trigger:_ menu:pyqs

1. Show filters
2. Apply filters
3. Show question
4. User answers
5. Provide feedback
6. Track score
7. Show next question or summary

_Data touched:_ Progress

### DPP Practice
_Trigger:_ menu:dpp

1. Show filters
2. Apply filters
3. Show problem
4. User answers
5. Provide feedback
6. Track score
7. Show next problem or summary

_Data touched:_ Progress

### Search
_Trigger:_ menu:search

1. Show search input
2. Apply filters
3. Show results
4. Select resource
5. Show resource details
6. Open/download resource
7. Option to bookmark

_Data touched:_ Resource, Bookmark

### Bookmarks
_Trigger:_ menu:bookmarks

1. Show bookmark categories
2. Select category
3. Show bookmarked resources
4. Select resource
5. Show resource details
6. Open/download resource

_Data touched:_ Bookmark

### Recommendations
_Trigger:_ menu:recommendations

1. Generate weekly summary
2. Show top 3 recommendations
3. Option to access resource
4. Option to provide feedback on recommendations

_Data touched:_ Progress, User profile

## Data entities

Durable data (must survive a restart) uses the toolkit's persistent store, never in-memory maps.

- **User profile** _(retention: persistent)_ — User's NEET preparation profile
  - fields: class, preparation level, weak subjects, strong subjects, recent activity
- **Resource** _(retention: persistent)_ — Study resource metadata
  - fields: type, subject, class, chapter, topic, difficulty, institute, tags, URL/file
- **Bookmark** _(retention: persistent)_ — User's saved resources with notes
  - fields: user ID, resource ID, notes, categories
- **Progress** _(retention: persistent)_ — User's study progress tracking
  - fields: completed lectures, DPP scores, PYQ attempts
- **Collections** _(retention: persistent)_ — Custom modules and saved searches
  - fields: user ID, name, filters, resources

## Integrations

- **Telegram** (required) — Bot API messaging
- **OpenAI** (required) — Content generation for personalized recommendations
Call external APIs against their real contract (correct endpoints, ids, params); credentials from env. Do not fake responses.

## Owner controls

- Admin reporting chat for critical errors and usage summaries
- Seed content management (NCERT, PYQs, lectures)

## Notifications

- Weekly progress summary to user
- Personalized recommendations to user
- Error reports to admin
- Usage summaries to admin

## Permissions & privacy

- User data is stored securely and only used for study tracking and recommendations
- Resources are accessed via Telegram or external URLs without storing user files
- User can delete their data at any time via /delete_profile command

## Edge cases

- User cancels onboarding at any step
- User tries to access resources without completing onboarding
- User attempts to bookmark the same resource multiple times
- User tries to access non-existent resource
- User provides invalid input during practice sessions

## Required tests

- End-to-end onboarding flow test
- Resource filtering and search functionality test
- PYQ/DPP practice session flow test
- Bookmarking and collections management test
- Progress tracking and recommendation generation test
- Error handling and admin notification test

## Assumptions

- Owner will provide initial seed content as described
- Telegram will handle file delivery without bot intervention
- OpenAI API will be available for content generation
- Users will complete onboarding before using core features
