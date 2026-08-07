# CIS 440 Workout Tracker — technical case study

## 1. Project at a glance

The CIS 440 Workout Tracker is a beginner-oriented academic web application for planning workouts, logging completed exercises, reviewing progress, and maintaining a simple schedule.

| Item | Summary |
|---|---|
| Context | Six-person CIS 440 academic team capstone |
| Intended users | Beginners who need a clearer path from planning to consistent workout logging |
| Jonathan's role | Public-release maintainer and verified frontend/routing contributor |
| Team roles | Scrum Master; Product Owner; Database / ERD Developer; Frontend / UI Developer; Frontend / Routing Developer; Backend / API Developer |
| Application stack | Node.js, Express, MySQL, vanilla browser JavaScript, HTML, and CSS |
| Case-study format | Code-free static site with synthetic diagrams and generated artwork |
| Publication status | Public code-free repository and GitHub Pages site created after exact Stage 1 authorization and anonymously verified |

Other contributor names are omitted pending their individual publication preferences. Jonathan Rodriguez maintains this portfolio presentation and is identified only for contribution areas supported by the private project history; he is not the sole author.

Direct team authorization has not yet been obtained. This is not an official publication or endorsement by the full team, the instructor, Arizona State University, or W. P. Carey.

## 2. The problem

Beginning a workout routine creates several small but compounding decisions: what to train, which exercise fits available equipment, how to record a session, when to rest, and how to interpret progress. The team project explored how one browser-based workflow could reduce that friction without presenting itself as medical guidance or a production fitness platform.

The resulting prototype joined account access, exercise browsing, workout logging, schedules, beginner planning, progress views, privacy controls, and optional calendar integration. The product goal was clarity and continuity, not invented engagement metrics or claims of real-world adoption.

## 3. Users and use cases

The intended user can:

1. create a local account and sign in;
2. browse exercises by muscle group and equipment;
3. log sets, repetitions, weight, date, and notes;
4. review workout history and progress summaries;
5. schedule exercises and preview a beginner routine;
6. receive rule-based recovery reminders;
7. choose how activity appears in community views; and
8. optionally connect a development Google Calendar account.

These are source-visible prototype capabilities. They are not evidence of public deployment, clinical validation, production scale, or actual user adoption.

## 4. Team and contribution boundary

The application was team work. The public case study uses role-only context:

- Scrum Master
- Product Owner
- Database / ERD Developer
- Frontend / UI Developer
- Frontend / Routing Developer
- Backend / API Developer

Names other than Jonathan Rodriguez are omitted pending their individual publication preferences. Roles describe collaboration context; they do not assign exclusive ownership of every file or feature. Architecture, server behavior, database design, authentication, testing, interface design, and integration include shared and other-contributor work.

Private diff review supports only the bounded contribution statements below. Commit counts and private revisions are excluded because they do not measure contribution value and do not belong in this fresh public narrative.

## 5. My verified contributions

### Interface foundations and routing behavior

- **Context:** The growing prototype needed consistent internal-page structure and dependable movement between authenticated views.
- **My responsibility:** Contribute dashboard/internal-page foundations, navigation wiring, and client-side token-presence guard behavior.
- **Implementation:** Vanilla HTML, CSS, and browser JavaScript organized shared layout, navigation, logout/refresh behavior, and page-entry checks.
- **Challenge:** Client-side page guards can improve navigation flow but cannot replace server-side authorization.
- **Action:** Kept the browser behavior focused on routing and user feedback while treating protected APIs as the real authorization boundary.
- **Outcome:** The interface gained a more consistent path across internal views without overstating the security value of client-side checks.
- **Evidence boundary:** Supported by private parent-to-commit diff review; private revisions are not published.

### Workout entry, calendar, and catalog interaction

- **Context:** Workout logging requires several related inputs, and a large exercise list can become difficult to scan on smaller screens.
- **My responsibility:** Prototype workout-entry/calendar experiences and improve workout-catalog presentation.
- **Implementation:** Date selection, form validation, reusable prior-entry behavior, repeated entry creation, responsive grouping, skeleton loading states, bounded initial lists, and expand/collapse controls.
- **Challenge:** The UI had to remain understandable across loading, empty, validation, and narrow-screen states.
- **Action:** Broke the interactions into explicit states and progressive controls rather than presenting one dense surface.
- **Outcome:** The prototype provided clearer paths for logging work and exploring available exercises.
- **Evidence boundary:** These are interface contributions; final data behavior and integrated product areas remain team work.

### Beginner planning and recovery reminders

- **Context:** New users needed help turning broad intent into a manageable schedule while avoiding repetitive training patterns.
- **My responsibility:** Build a beginner-planning flow and product-level reminder heuristics.
- **Implementation:** Training-day, equipment, and start-date controls; a multi-week preview; integration with workout-entry interfaces; repeated-muscle-group checks; and duplicate-notice suppression.
- **Challenge:** Helpful guidance needed to remain deterministic and transparent rather than masquerading as medical or personalized clinical advice.
- **Action:** Used simple rules, explicit inputs, and conservative language.
- **Outcome:** Users could preview a routine and receive general recovery-oriented prompts within the prototype.
- **Evidence boundary:** The reminders are interface guidance, not health advice.

### Progress analytics and selected maintenance

- **Context:** Raw workout history is difficult to interpret without summaries and adjustable views.
- **My responsibility:** Build configurable history analytics and contribute selected integration maintenance.
- **Implementation:** Summary metrics, line/bar chart views, presets, saved dashboard configuration, insight cards, and bounded naming/routing/browser/server-startup fixes.
- **Challenge:** Visual summaries needed meaningful empty states and honest limits when data was sparse.
- **Action:** Paired synthetic chart views with readable labels and kept maintenance claims limited to the reviewed changes.
- **Outcome:** The dashboard made recorded activity easier to inspect and customize.
- **Evidence boundary:** No production performance, adoption, or business-impact metric is claimed.

## 6. Product functionality

The final team prototype includes these source-visible areas:

- account access and profile management;
- exercise browsing and custom exercise creation;
- workout logging, history, and entry editing;
- schedule and calendar views;
- deterministic routine recommendations and beginner planning;
- dashboard summaries and configurable charts;
- activity/reminder and motivational-quote surfaces;
- privacy-aware community views;
- a static/offline-capable PWA shell; and
- optional Google Calendar connection and event creation.

This list describes team-owned application functionality. It is intentionally separate from Jonathan's verified contribution list.

## 7. Architecture and data flow

The application follows a direct browser/server/database structure:

```text
Browser pages and client state
        |
        | HTTP + bearer token for protected requests
        v
Node.js / Express routes and business rules
        |
        | parameterized SQL
        v
MySQL users, exercises, workouts, entries, schedules, quotes, and OAuth tokens

Optional boundary: Express <-> Google OAuth and Calendar APIs
```

A representative workout flow is:

1. the signed-in browser submits a workout and entries;
2. the server verifies the application token and validates input;
3. parameterized queries persist data under the authenticated user;
4. history endpoints return owned data;
5. dashboard code aggregates the response into synthetic metrics and chart-ready series; and
6. optional calendar synchronization is attempted only when separately configured.

The codebase is an academic monolith rather than a service-oriented production architecture. That kept the capstone understandable but increased coupling among startup, routing, SQL, authentication, business rules, and provider integration.

## 8. Technical decisions

### Node.js and Express

A single Express application served static pages and JSON routes. The compact structure helped a student team trace end-to-end behavior, while the large server module created maintenance and test costs.

### Relational MySQL data

Users, workouts, entries, exercises, and schedules have relationships that benefit from foreign keys and user-scoped queries. A clean public schema and synthetic reference seed are prepared separately from this code-free case study.

### JWT and password hashing

The audited prototype used bearer tokens and bcrypt-based password hashing. Protected APIs, rather than client navigation guards, are the authorization boundary. The reconstructed source candidate narrows token persistence to tab-scoped session storage, checks a server-side session version, rotates that version on login and sensitive changes, adds current-password step-up for destructive account actions, and applies bounded login/registration throttles. Residual limitations include bearer-token exposure to same-origin script, single-process rate-limit state, no new-mailbox verification, and no shared multi-instance session store.

### Browser analytics

Dashboard summaries were calculated in the browser to keep visual experimentation close to the interface. This made presets and chart changes approachable, but an unbounded history flow can create extra requests and client work.

### PWA and calendar boundaries

The service worker supports a static shell rather than complete offline workout tracking. Google Calendar is optional and depends on developer-supplied configuration. The reconstructed source candidate adds application-layer AES-256-GCM protection for stored provider grants, a browser-bound one-time authorization state, and best-effort provider revocation on disconnect. The live provider flow remains untested, and production key rotation, multi-instance state, consent, and event-lifecycle controls still need design work.

## 9. Security, privacy, and responsible release

The publication review treats the original team source and this case study as separate artifacts.

- This repository contains no application source, private Git history, environment file, credential, SQL data, private report, or real account/workout record.
- Interface examples, values, and diagrams are synthetic.
- Other contributor names are omitted pending their individual publication preferences while six-person team context and roles are retained.
- Private screenshots, the uncertain background photograph, original project icons/logo, and unclear media are excluded.
- The generated social card and HTML/CSS diagrams were created for this portfolio release without private media input.
- The separate source candidate uses placeholder configuration and remains local, Git-free, and unpublished throughout Stage 1 pending direct team review and a separate Phase 2 authorization.
- Credential material historically tracked in private source must be treated as compromised and remediated by its owners; omission does not make an old credential safe.

This is release engineering and portfolio documentation, not a security certification or legal opinion.

## 10. Testing and validation

Bounded final local validation is recorded below. The results apply to disposable local copies and remain intentionally narrower than production, database, provider, and accessibility-conformance testing.

| Validation area | Current status |
|---|---|
| Source dependency installation, audit, and import checks | Clean install completed; 149 packages audited with 0 known vulnerabilities; 7 direct dependency loads passed |
| JavaScript syntax and static source checks | Passed 28 JavaScript syntax checks, 3 JSON parses, the 76-file inventory gate, 216 local-reference/service-worker checks, and the 46-route/8-table documentation contract |
| No-database HTTP and authentication-boundary smoke checks | 6 of 6 bounded smoke/security-regression tests passed; required-environment failure also exited safely |
| Synthetic MySQL schema and representative CRUD | Not run; no local MySQL server or usable container daemon was available |
| Google OAuth and Calendar integration | Not tested |
| Case-study HTML/CSS/link validation | 16 of 16 static validator gates passed before the final narrative update; a final rerun is recorded in the private validation report |
| Browser keyboard, responsive, and accessibility review | Desktop, 375 px, and 320 px layouts were inspected without document overflow; static keyboard/focus/reduced-motion/forced-color support passed; keyboard-only, assistive-technology, zoom, and conformance review remain incomplete |
| Production application deployment and GitHub Pages | No production application deployment is claimed; the public case-study repository and static Pages site are anonymously reachable |
| Final identity, secret, privacy, and asset scan | Local gates passed after manual review of false positives and generated images; postpublication scans remain required |

The private final validation report is the controlling evidence record. A missing or skipped check remains labeled as such; no old or source-visible behavior is converted into a test result.

## 11. Challenges and lessons learned

### Follow the entire path

A browser symptom can originate in page state, routing, an API contract, a SQL relationship, or environment configuration. Tracing the complete request path produces better fixes than treating each layer in isolation.

### Make failure actionable

Loading, empty, invalid, offline, and unauthorized states deserve explicit user feedback. Clear states help both the person using the interface and the teammate diagnosing it.

### Bound confidence

Static review, a smoke test, and a synthetic database test answer different questions. Naming what was and was not tested is more useful than a broad “works” claim.

### Design for handoffs

Consistent page structure, documented boundaries, and small explainable changes make team integration easier. Release documentation is part of that handoff, not an afterthought.

## 12. Limitations and future improvements

Current boundaries include:

- no verified production deployment or production-scale evidence;
- no current OAuth integration test;
- no complete keyboard-only, screen-reader, zoom, contrast, or reduced-motion validation;
- tab-scoped bearer sessions rather than hardened secure-cookie sessions;
- in-memory, single-process abuse controls and OAuth state;
- no verification of a replacement login mailbox and no production account-recovery flow;
- operator-managed provider-token encryption without a tested rotation procedure;
- incomplete observability, migration rollback, and operational recovery;
- partial offline behavior rather than offline data synchronization;
- potential performance and privacy edge cases in long-history and small-community scenarios; and
- the code-free case-study repository and GitHub Pages site were created after the exact Stage 1 phrase and anonymously reached; the source candidate remains local and unpublished.

Priority next steps are to extend bounded authorization and privacy regression tests, exercise a disposable synthetic database, replace process-local security state with shared production controls, add mailbox verification and key-rotation procedures, improve observability, and complete keyboard/screen-reader/zoom review.

## 13. Transferable skills

This work demonstrates:

- systems analysis across browser, API, and relational-data layers;
- technical troubleshooting and failure-state design;
- dashboard and data-visualization thinking;
- requirements interpretation and documentation;
- privacy-aware release engineering;
- Git collaboration without equating commit counts with value;
- user-support-oriented interface decisions; and
- cross-functional teamwork with explicit ownership boundaries.

## 14. Credits, license, and origin

This case study describes a six-person CIS 440 academic team capstone. Other contributor names are omitted pending their individual publication preferences. Jonathan Rodriguez maintains the public-release materials and contributed the verified areas described above; this does not imply sole authorship. Direct team authorization has not yet been obtained, and this is not an official publication or endorsement by the full team, the instructor, Arizona State University, or W. P. Carey.

The separate source candidate preserves the source project's applicable license and provenance notices. This code-free case-study candidate does not copy the application source or private history and does not silently relicense team, instructor, or third-party work.

The social card and HTML/CSS diagrams are release-created assets using synthetic content. See [NOTICE.md](NOTICE.md) for the durable attribution and asset boundary.
