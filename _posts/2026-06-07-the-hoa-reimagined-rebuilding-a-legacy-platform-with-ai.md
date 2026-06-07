---
layout: single
title: "The HOA, Reimagined: Rebuilding a Legacy Platform with AI in Under Three Weeks"
date: 2026-06-07 00:00:00 -0500
categories: ai development
tags: ai claude dotnet react hoa proptech
permalink: /:year/:month/:title/
---

I joined my neighborhood HOA board two and a half weeks ago. The "system" I inherited was a
legacy WebForms portal supplied by our third-party management company — a dated, generic product
that the board bent itself around instead of the other way around. So I did what a software
architect does when the off-the-shelf option fits poorly: I built a better one.

What I didn't fully expect was *how fast*. Working alongside Claude, I rebuilt the entire thing —
a state-of-the-art, purpose-built board-management platform — in under three weeks. This is the
story of what I built, why, and what it says about where AI-assisted development is right now.

## From policing to stewardship

The motivation wasn't really "better software." It was a different idea of what an HOA should be.
The thesis I started from — and the one the whole platform is built around — is **stewardship over
policing**. Four ideas carry it:

- **Good-neighbor compliance — help first, never "gotcha."** A flagged lawn or driveway starts
  with a friendly heads-up and an offer to share our group-discount vendors, not a cold form
  letter. A community safety net, not the neighborhood police.
- **Radical transparency — see where every dollar goes.** A real-time dashboard and single-page
  visual summaries replace the dense once-a-year budget. Minutes and action items get published
  within 48 hours, plainly.
- **Open-source committees — low-commitment ways to help.** Instead of a permanent board seat,
  neighbors join a focused micro-committee for a few weeks — vet a fence builder, test the payment
  portal. It taps the talent already living here.
- **An ROI mindset — dues as an investment, not a tax.** Every dollar is framed around protecting
  the value of each homeowner's largest asset: their home.

Good software can't manufacture good governance. But the *wrong* software actively fights it — it
makes transparency expensive and good-neighbor follow-up tedious. I wanted tooling that made the
right behavior the easy one.

## What I built

The platform covers the full operational surface of running a ~80-home community. A quick tour:

- **An interactive neighborhood map** is the centerpiece — every home is clickable and drills into
  its real records. Alongside it: a sortable homeowner table and a community-value overview
  dashboard that tracks aggregate property values and trends over time.
- **Compliance, done the right way.** Violations follow the Texas §209 notice ladder, with a clean
  separation between behavioral issues (close with the current owner) and structural ones (which
  follow the property to resale). Architectural/ACC requests, work orders, projects, vendors, and
  committees all live here too — each with public reference numbers and shareable deep links.
- **Operations and money.** Budgets, a ledger, and finance categories; announcements and bulletins
  with per-recipient delivery tracking; a document library; action items; a Texas resale
  certificate rendered straight to PDF; and a full audit log over everything.

The point isn't the length of the feature list — it's that a board volunteer can now do in a few
clicks what used to mean emails, spreadsheets, and a call to the management company.

## The part that makes it interesting

A few deliberate choices are what make this more than a CRUD app — and what made three weeks
possible.

### A map that's real, not a picture

The neighborhood map isn't a screenshot with hotspots. It's rendered from **real OpenStreetMap
building footprints** — the actual outline of every house — drawn schematically with Leaflet (no
map tiles, no API key, no rate limits). Homes recolor live based on dues status, open violations,
or estimated value. Geometry and records are two separate sources of truth joined by a stable lot
id, so the visual layer and the data layer evolve independently.

### An AI-native back office

Here's the trick that collapsed the timeline. The platform exposes a custom **MCP (Model Context
Protocol) server**, which means I can point Claude directly at the system and have it safely
**backfill years of history** — reading old email threads and statements and turning them into
properly dated violations, requests, action items, and ledger entries. Migrating data is usually
the most painful, most manual part of replacing a legacy system. Here it became a conversation.

A concrete example. Like most HOA boards, ours lived in email. In my first two and a half weeks,
board business alone generated **68 messages**. I pointed Claude at my inbox and had it read,
classify, and triage everything down to **16 threads and 7 attachments** — then, through the MCP
server, file each item where it belonged: Action Items, Violations, and Property Improvement
Requests. (I know those exact numbers *because* Claude counted them on the way through.) An
afternoon of reading and copy-paste became a few minutes of supervised automation.

The financial history got the same treatment. I downloaded the old budgets, CD investment records,
and account statements as PDFs from the legacy portal, handed them to Claude, and watched it
reconstruct the **budget, CD investments, and the full transaction ledger all the way back to
2020** — every line properly dated and categorized, written straight in through the MCP server. No
re-keying, no spreadsheet archaeology; just Claude reading the documents and filing what it found.

It's not a free-for-all: access is gated by per-user API keys with read or read/write scopes,
there is a structural no-delete guardrail, and every single tool call is written to an audit log.
Powerful, but accountable.

### Jira for your HOA

Once that history lives in a real system instead of an inbox, you can *manage* it like one. Action
Items, Violations, and Requests each get a **kanban board with swim lanes** — drag a card from
"Open" to "In Progress" to "Done," exactly the way my teams and I run software projects every day.
To a developer that sounds almost mundane, and that's precisely the point: the project-management
tooling we take for granted at work never reached the volunteer running a neighborhood. It's
essentially Jira, built into the HOA. A board member now gets a clear, shared view of what's open,
who owns it, and what's stuck — instead of scrolling back through a month of email to reconstruct
it.

### A modern, boring-in-the-good-way stack

Under the hood it's **.NET 10** (FastEndpoints, EF Core, PostgreSQL) and **React 19** (TanStack
Query, Tailwind, Leaflet), with OIDC single sign-on. CQRS handlers, real database migrations, and
integration tests against a real Postgres. Nothing exotic — deliberately. The novelty is the map
and the MCP layer; everything else is the kind of solid, well-trodden foundation you want under
something a board will actually depend on.

## What "under three weeks" really says

I want to be precise about the claim, because it's easy to over-read. A single board volunteer
replaced a third-party management company's legacy product with a purpose-built, modern one in
under three weeks. That happened because AI collapsed the cost of **two** things at once: writing
the application, and migrating the data into it. Either one alone used to be a project of months.

That's the headline, but it's worth being honest about the rest. AI was extraordinary at volume
and velocity — scaffolding endpoints, wiring the map, transforming messy email history into clean
records. The judgment still had to come from me: what stewardship should mean in software, which
Texas statutes actually apply, where a "helpful" automation would quietly become the thing
neighbors resent. The leverage is real and it is large. It is not a substitute for knowing what
you're building or why.

## Reimagined, and live

The platform is in production today, running our community's day-to-day operations. The streets in
my neighborhood are all named after plants, which always felt like a small promise about the kind
of place it's meant to be — something tended, not policed. That's the same promise I tried to build
into the software: technology in service of being a better neighbor, not more red tape.

Two and a half weeks on the board, and the tooling already reflects the community I want to live
in. That's what AI changes — not that it writes the code, but that it puts building the *right*
thing within reach of the person who actually understands the problem.
