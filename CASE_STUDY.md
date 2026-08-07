# Workout Tracker Technical Case Study

## 1. Executive summary

The CIS 440 Workout Tracker was a six-person academic team project exploring how a browser application could help beginners plan activity, log completed exercises, maintain a schedule, and review progress. The documented prototype connected a JavaScript interface to a Node.js/Express application and relational MySQL data, with an optional Google Calendar boundary.

My role was **Frontend / Routing Contributor**. I contributed interface and internal-page foundations; routing and client-side navigation behavior; workout-entry and calendar prototypes; responsive exercise-catalog behavior; beginner routine planning; recovery/reminder heuristics; configurable history analytics and chart views; and selected naming, browser, routing, and server-startup maintenance. Shared backend, database, authentication, design, testing, integration, and product work were team-developed.

This code-free presentation focuses on systems analysis, workflow design, dashboard thinking, implementation tradeoffs, troubleshooting, and bounded validation. It does not claim production deployment, active users, business impact, clinical effectiveness, complete security, or formal accessibility conformance.

## 2. Problem and intended users

Starting a workout routine creates several connected decisions: what to train, which exercise fits available equipment, how to record a session, when to rest, and how to interpret progress. A beginner can lose continuity when planning, logging, scheduling, and reporting are separated.

The team framed a single browser workflow around that problem. The intended user could browse exercises, record sets and repetitions, schedule activity, preview a beginner routine, receive transparent recovery prompts, and review summaries of recorded history. The prototype was educational software, not medical guidance or a validated fitness service.

## 3. Project scope and six-person team context

The academic team used Scrum-oriented roles spanning product coordination, database/ERD work, frontend/UI work, frontend/routing work, and backend/API work. Role labels explain the collaboration structure; they do not establish exclusive ownership of every feature or file.

The documented prototype included:

- account access and profile management;
- exercise browsing and custom exercise creation;
- workout logging, history, and selected entry editing;
- schedule and calendar views;
- rule-based recommendations and beginner planning;
- dashboard summaries and configurable charts;
- reminder, activity, and motivational-quote surfaces;
- privacy-aware community presentation;
- a static/offline-capable application shell; and
- optional Google Calendar connection and event creation.

Those product areas describe the team-developed application. The personal contribution sections below are intentionally narrower.

## 4. Requirements and workflow framing

The project translated the beginner-user problem into a sequence of observable states rather than one large feature list.

| User need | Workflow | System response |
|---|---|---|
| Find an appropriate exercise | Filter or browse by muscle group and equipment | Present grouped catalog results, loading states, bounded lists, and expansion controls |
| Record completed work | Select a date and enter exercises, sets, repetitions, weight, and notes | Validate input, submit user-scoped data, and expose reusable entry behavior |
| Plan future activity | Choose training days, equipment, and a start date | Generate a deterministic multi-week preview and connect it to entry/schedule interfaces |
| Avoid repetitive patterns | Record or plan activity for muscle groups | Apply simple repeated-muscle-group checks and suppress duplicate notices |
| Understand history | Select a period, measure, and chart mode | Convert workout history into summaries, chart-ready series, empty states, and saved view settings |

Important failure states included loading, empty results, invalid input, unauthorized access, offline/static-shell limits, sparse history, and unavailable provider configuration. Making those states explicit supported both interface behavior and troubleshooting.

## 5. Jonathan's verified contribution areas

Public contribution statements were bounded through an evidence review of the private project history. Private commits and evidence are not reproduced here.

### Interface foundations and routing behavior

**Need:** Internal pages required consistent structure and dependable navigation between authenticated views.

**Contribution:** I worked on dashboard/internal-page foundations, navigation wiring, logout and refresh behavior, page-entry checks, and client-side token-presence routing behavior using HTML, CSS, and browser JavaScript.

**Tradeoff:** A browser guard can support navigation and feedback but cannot authorize protected data. The documented design therefore treats server-side API checks as the authorization boundary.

**Implemented output:** Consistent internal-page structure and explicit routing states across the contributed views.

### Workout entry, calendar, and catalog interaction

**Need:** Logging requires related inputs, while a large exercise list can become difficult to scan on a narrow screen.

**Contribution:** I prototyped date selection, input validation, reusable prior-entry behavior, repeated entry creation, calendar interactions, responsive catalog grouping, skeleton loading, bounded initial lists, and expand/collapse controls.

**Tradeoff:** Progressive disclosure reduced initial density, but every loading, empty, validation, and expanded state still required clear behavior.

**Implemented output:** Explicit workout-entry and catalog states designed for both desktop and narrow layouts.

### Beginner planning and recovery reminders

**Need:** New users needed a structured way to turn broad intent into a manageable schedule.

**Contribution:** I built training-day, equipment, and start-date controls; a multi-week routine preview; connections to workout-entry interfaces; repeated-muscle-group checks; and duplicate-notice suppression.

**Tradeoff:** The guidance needed to remain deterministic and transparent rather than resemble personalized clinical advice.

**Implemented output:** A rule-based planning flow and general recovery prompts whose inputs and limits can be explained.

### Progress analytics and selected maintenance

**Need:** Raw workout history is hard to inspect without summaries and adjustable views.

**Contribution:** I built summary metrics, line and bar chart modes, presets, saved dashboard configuration, insight cards, and bounded naming/routing/browser/server-startup fixes.

**Tradeoff:** Browser-side aggregation kept chart calculations close to the display layer but raised performance questions for long histories and required meaningful sparse-data states.

**Implemented output:** Configurable progress views and documented maintenance changes without claiming user or business impact.

## 6. Architecture and data flow

The documented application used a straightforward browser/server/database structure:

```text
Browser pages and client state
        |
        | HTTP requests; bearer token for protected routes
        v
Node.js / Express routes and business rules
        |
        | Parameterized SQL
        v
MySQL users, exercises, workouts, entries, schedules, quotes, and provider grants

Optional external boundary: Express <-> Google OAuth and Calendar APIs
```

A representative workout-reporting flow was:

1. The signed-in browser collected a workout and its entries.
2. The Express application validated the request and checked the protected-route context.
3. Parameterized queries persisted data under the authenticated user.
4. History endpoints returned user-scoped records.
5. Browser logic transformed the response into summary metrics and chart-ready series.
6. Optional calendar synchronization was attempted only when separately configured.

The academic monolith made the full request path approachable for a student team. It also coupled startup, routing, SQL, authentication, business rules, and provider integration more tightly than a production-oriented system would.

## 7. Technical decisions and tradeoffs

### JavaScript, Node.js, and Express

A single Express application served static pages and JSON routes. This made browser-to-route tracing direct, but a large server surface increased maintenance and testing costs.

### Relational MySQL data

Users, exercises, workouts, entries, and schedules have relationships that benefit from foreign keys and user-scoped queries. Relational modeling fit the workflow, while local database availability and migration discipline remained validation constraints.

### Browser routing and protected APIs

Client-side token-presence checks supported page flow. They were not treated as a security control: protected APIs remained responsible for authorization. This distinction is important in both implementation and support documentation.

### Browser-side reporting

Calculating summaries and chart series in the browser kept chart calculations close to the display layer. The tradeoff was additional client work and possible extra requests as history grew.

### Static shell and optional provider integration

The application shell supported a limited offline/static experience rather than complete offline workout synchronization. Google Calendar remained optional and configuration-dependent; the live provider flow was not validated for this case study.

## 8. Dashboard and reporting logic

The reporting work converted history records into understandable interface states rather than claiming a formal enterprise BI implementation.

The configurable views included:

- summary totals and active-period measures;
- exercise or period-based series;
- line and bar chart modes;
- presets and saved display configuration;
- insight cards and readable labels; and
- empty and sparse-history states.

This work demonstrates metric definition, report-state design, data-flow documentation, and communication of confidence boundaries. It does not demonstrate Power BI or Tableau deployment, executive adoption, production-scale data processing, or measurable business outcomes.

## 9. Troubleshooting and release lessons

### Trace the complete path

A browser symptom can originate in page state, routing, an API contract, a relational dependency, or environment configuration. The selected routing, browser, naming, and startup maintenance reinforced the value of following the request from interface to data boundary.

### Make failure states actionable

Loading, empty, invalid, offline, unauthorized, and unconfigured-provider states need clear behavior. Those states help the intended user and give a teammate or support analyst a better starting point for diagnosis.

### Separate evidence types

Static review, a no-database smoke test, a database test, a provider test, and a production deployment answer different questions. Recording exactly which checks ran is more useful than a broad claim that everything works.

### Design for handoffs

Consistent structures, bounded ownership statements, documented data flow, and small explainable changes make team integration and later support easier. The public presentation applies the same principle by separating verified personal contributions from shared product context.

## 10. Validation evidence

The current case-study repository is validated as a static presentation: exact public inventory, semantic heading and fragment structure, local links and assets, canonical/Open Graph/Twitter metadata, social-card dimensions, CSS syntax, keyboard focus support, reduced-motion and forced-color rules, responsive overflow checks, privacy scanning, and live Pages readback.

Previously recorded application checks included dependency installation and audit, JavaScript/JSON syntax and import checks, source inventory and reference checks, and six bounded no-database smoke/security-regression tests. The dependency audit reported zero known vulnerabilities at the time it ran. These are historical audit results, not a current security guarantee.

Not run or not established:

- synthetic MySQL end-to-end workflow testing;
- live Google OAuth or Calendar behavior;
- production application deployment, reliability, or scale;
- complete keyboard, screen-reader, zoom, contrast, or formal accessibility-conformance review;
- active-user, adoption, revenue, retention, or clinical-effectiveness evidence.

## 11. Limitations and future improvements

The documented prototype retains academic-project constraints: a tightly coupled server, incomplete database and provider validation, partial offline behavior, client-side reporting costs for long histories, limited operational observability, and privacy edge cases in small-community views.

Reasonable next steps would be to:

1. run representative workflows against a disposable synthetic database;
2. strengthen automated authorization, ownership, and privacy regression tests;
3. test the optional provider lifecycle with development-only accounts and synthetic events;
4. measure and bound long-history reporting performance;
5. add clearer operational logging, migration, rollback, and recovery procedures; and
6. complete keyboard, screen-reader, zoom, reflow, and contrast review without claiming conformance before the evidence exists.

## 12. Transferable skills

### IT / Systems Analysis

- tracing browser, API, database, and provider boundaries;
- interpreting requirements as workflows and failure states;
- troubleshooting routing and environment behavior;
- documenting validation, privacy, and support limitations.

### Business Intelligence / Reporting

- defining summary metrics and configurable chart views;
- turning history data into understandable report states;
- documenting data flow, empty states, and confidence limits;
- communicating technical outputs to non-specialist readers.

### Business / Product Analysis

- framing a beginner-user problem;
- translating needs into features and workflows;
- balancing user experience, technical constraints, privacy, and team scope;
- documenting handoffs and prioritized improvements.

### Technical Implementation

- responsive browser interfaces;
- routing, form, loading, empty, and error states;
- JavaScript integration in a Node/Express/MySQL application;
- selected browser, naming, routing, and startup maintenance.

## 13. Credits and notice

The underlying application was developed by a six-person CIS 440 academic team. Jonathan Rodriguez maintains this independent case-study presentation and contributed only the verified areas described above; this does not imply sole authorship or official team, instructor, university, or school endorsement. Other contributors are not named here because their individual public naming preferences have not been collected for this presentation.

See [NOTICE.md](NOTICE.md) for the durable attribution, privacy, media, evidence, and rights boundary. See [TEAM_SHARING.md](TEAM_SHARING.md) for neutral reuse and correction guidance for original teammates.
