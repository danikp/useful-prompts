# 00 — Meta Prompt: Build an Agent-First Marketing Machine for Any Website

A single, reusable prompt that generates a complete automated marketing system for any website. Give it a domain — it researches everything else. Run it again later to enrich and improve based on new findings.

Works with any agentic AI tool: Claude Code, Claude Cowork, Cursor, Windsurf, Devin, OpenAI Codex, or any agent that can search the web, read/write files, and follow multi-step instructions.

---

## How to Use

1. Copy the prompt below
2. Replace `example.com` with your domain
3. Paste into any AI agent
4. The agent researches your site, competitors, and market — then builds the full system
5. **To improve later:** paste the same prompt again. The agent reads existing files in `promote/` and enriches them with new research.

---

## The Prompt

```
You are a marketing strategist and automation architect. Your job is to build a complete, agent-operated marketing system for a website. The system consists of markdown files containing detailed prompts that any AI agent can execute to promote the site — no manual coding required.

## The Website

Domain: example.com

That's all you're given. Research everything else yourself.

## Instructions

Follow these steps in order. At each step, save your work to the specified file. If a file already exists from a previous run, read it first and ENRICH it — update outdated information, add new findings, refine strategies, and improve prompts based on what changed. Never overwrite good existing work; build on it.

If at any point you encounter ambiguity that would significantly change the direction of the strategy (e.g., the site serves two very different audiences, or it's unclear whether the site is B2B or B2C), stop and ask me before continuing.

---

### Step 1: Deep Site Research

Thoroughly research the website using only its domain. Fetch and analyze:

- **Homepage and key pages** — what the site does, who it's for, what value it provides
- **Content inventory** — what types of pages exist (browse the site, check the sitemap if available at /sitemap.xml)
- **SEO footprint** — meta titles, descriptions, structured data, Open Graph tags, robots.txt
- **Languages and locales** — does the site serve multiple languages or regions?
- **Social presence** — search for the brand on Twitter/X, LinkedIn, Facebook, Instagram, Pinterest, YouTube, Reddit, GitHub
- **Existing marketing** — do they have a blog? Newsletter? Social accounts? Podcast?
- **Contact information** — public email, contact forms, team page
- **Brand voice** — formal/casual, personal/team, technical/accessible (infer from site copy)
- **Unique data assets** — what content or data does this site have that would be hard to replicate? This is the foundation of all marketing strategies.
- **Current authority** — search Google for "site:example.com" to estimate indexed pages. Search for "example.com" to see how the brand appears in search results.
- **Monetization model** — ads, subscriptions, e-commerce, freemium, affiliate (determines which strategies matter most)

Compile a site profile. Save to: `promote/output/site-profile.md`

If this file already exists, read it first. Update any information that has changed. Add new findings. Mark what's new with [NEW] and what changed with [UPDATED].

---

### Step 2: Competitor Research

Identify and deeply analyze competitors.

**Finding competitors:**
1. Infer the site's primary purpose and 10-15 keywords it should rank for
2. Search Google for each keyword — record which sites appear in the top 5
3. Also search: "[brand name] alternative", "[brand name] vs", "sites like [brand name]"
4. Identify the top 5-7 competitors (sites targeting the same audience with similar content)

**For each competitor, research:**
- Domain and estimated size (search "site:competitor.com" for page count)
- What they do well — content quality, SEO, design, features
- Their social media presence (followers, posting frequency, engagement)
- Their content strategy — do they blog? What topics? How often?
- Their backlink sources — search for who links to them: "[competitor.com]" -site:competitor.com
- Their structured data / rich snippets in Google
- Whether AI systems (ChatGPT, Perplexity) cite them for relevant queries
- Their weaknesses — what's missing, outdated, or poorly done?

**Comparative analysis:**
- Where does our site win vs. each competitor?
- Where does each competitor win vs. our site?
- What opportunities exist that NO competitor is exploiting?
- Which competitor's backlink sources could also link to us?
- Which competitor's content formats should we replicate or improve on?

Save to: `promote/output/competitor-analysis.md`

If this file already exists, read it first. Update competitor data, add any new competitors discovered, refine the analysis.

---

### Step 3: Audience and Community Research

Find where the target audience gathers online.

1. Based on the site's topic, search for:
   - **Subreddits** where this topic is discussed (search Reddit)
   - **Facebook groups** related to the topic
   - **Forums and communities** (Discourse, Discord, Slack communities)
   - **Q&A sites** — Quora questions, Stack Exchange sites related to the topic
   - **Newsletters** in this space (search Substack, Beehiiv)
   - **Podcasts** covering this topic (search Apple Podcasts, Spotify)
   - **YouTube channels** in this niche
   - **Influencers / thought leaders** — who has authority in this space?

2. For each community/platform found:
   - Name and URL
   - Size (subscribers, members, followers)
   - Activity level (posts per day/week)
   - Relevance to the site (high/medium/low)
   - Rules about self-promotion (some ban it, some allow it in specific threads)
   - How the site could provide genuine value to this community

Save to: `promote/output/audience-communities.md`

---

### Step 4: Strategy Selection

Based on Steps 1-3, select and rank 12-15 promotion strategies. Only include strategies that genuinely fit this specific site. Do NOT include strategies just because they're popular — each must have a clear reason tied to research findings.

**Strategy menu** (select what fits, ignore what doesn't):

Content & SEO:
- Schema markup / structured data enhancement
- Programmatic SEO — generate new page types from existing data
- Generative Engine Optimization — get cited by AI chatbots and AI search
- Content syndication to platforms (Medium, LinkedIn, dev.to, Substack)
- Data-driven blog articles / reports
- Long-tail keyword page generation

Link building:
- Broken link building (find dead links on other sites, offer replacement)
- Embeddable widgets or free tools (others embed on their sites)
- Public API or data feed (developers integrate = backlinks)
- Email outreach for contextual backlinks
- Directory and listing submissions
- Infographics and shareable data visualizations
- Resource page link building

Community & PR:
- Reddit / forum monitoring and helpful engagement
- Journalist query matching (HARO, Featured.com, Qwoted)
- Podcast guest appearances
- Social media content automation (X, Bluesky, LinkedIn, etc.)
- Quora / Q&A site answers

Visual & platform-specific:
- Pinterest (if visual/lifestyle content fits)
- YouTube (if explainer/tutorial content fits)
- TikTok / Reels (if short-form content fits)

Audience building:
- Email newsletter with automated content
- Lead magnets (free downloads, tools, reports)
- Referral / sharing mechanics

Authority building:
- Wikipedia / Wikidata contributions (if data is citable and verifiable)
- Academic or industry citations
- Speaking, webinars, conference appearances
- Open source contributions or integrations

**Also consider domain-specific strategies** not on this list. For example:
- E-commerce → product review outreach, comparison pages, shopping feed optimization
- SaaS → integration partner listings, G2/Capterra profiles, "alternatives to" pages
- Local business → Google Business Profile, local directories, local press
- Developer tool → GitHub awesome lists, package registry listings, dev community posts
- Content/media → guest posting, syndication deals, content licensing

For each selected strategy:
- Why it fits (cite specific findings from Steps 1-3)
- Expected impact (low / medium / high) and reasoning
- Effort level (low / medium / high)
- Prerequisites (accounts, tools, content needed)
- Phase: 1 (start now), 2 (next 1-2 months), 3 (next quarter), 4 (ongoing)
- Dependency on other strategies (e.g., "requires schema markup first")

Save to: `promote/output/strategy-ranking.md`

If this file already exists, read it first. Re-evaluate rankings based on new research. Add new strategies discovered. Remove strategies that no longer fit. Note what changed and why.

---

### Step 5: Build the Prompt Files

For each selected strategy, create a detailed file at `promote/[number]-[strategy-name].md`.

**Every file must follow this structure:**

#### Section 1: Goal
What this strategy achieves. One to two sentences. No jargon.

#### Section 2: How It Works
Plain-English explanation a non-technical person can understand. Explain the mechanism — why does this work? What's the cause and effect?

#### Section 3: Prerequisites
- Accounts to create (with signup URLs)
- Free tools needed
- Information needed from the site owner
- Dependencies on other strategies

#### Section 4: Prompts
3-6 numbered prompts, each in a fenced code block, ready to copy-paste into any AI agent.

**Prompt design rules:**

a) **Self-contained.** Each prompt must include all context the agent needs. Don't assume the agent remembers previous conversations. Reference specific files by path.

b) **Research → Create → Execute → Distribute → Track.** Sequence the prompts in this order. Every strategy must include at least a research prompt AND a tracking prompt.

c) **Concrete outputs.** Every prompt must specify exactly what to save and where:
   `Save to: promote/output/[descriptive-name].md`

d) **Real data only.** Prompts must instruct the agent to research and verify all data. Include the instruction: "Every statistic, URL, and claim must be verified through research. Do not fabricate or estimate. If you cannot verify something, say so explicitly."

e) **Outreach prompts must include:**
   - Finding targets (who to contact, with URLs)
   - Finding contact information
   - Drafting personalized messages (under 150 words each)
   - Ready-to-send text for each target
   - A tracking table

f) **Posting/publishing prompts must include:**
   - Step-by-step manual instructions (for doing it yourself)
   - Automation option where feasible (describe the API or tool that could automate it)

g) **Tracking prompts must:**
   - Check whether previous actions produced results
   - Measure specific metrics (backlinks gained, responses received, traffic referred)
   - Feed findings back into the next cycle
   - Update the tracking file, don't overwrite it

h) **Enrichment-aware.** Each prompt should start with: "If promote/output/[relevant-file].md already exists, read it first. Build on existing work — update, don't replace."

i) **Platform-aware.** For any strategy involving external platforms (Reddit, Wikipedia, Pinterest, etc.), include relevant platform rules and warnings about what could get the account banned.

j) **Multilingual.** If the site serves multiple languages (from Step 1), prompts should account for this — research, content, and outreach in relevant languages.

#### Section 5: Schedule
How often to run each prompt (daily, weekly, monthly, quarterly, one-time).

#### Section 6: Expected Output
What the agent produces after running all prompts.

#### Section 7: Process Improvement
How to optimize this strategy over time. What signals indicate it's working or not.

---

### Step 6: Build the README

Create `promote/README.md` with:

- One-paragraph overview of the system
- Note that this works with any AI agent (not tool-specific)
- Link to this meta-prompt file for rebuilding/enriching
- Key settings discovered during research:
  - Brand name and identity
  - Contact email
  - Languages supported
  - Target audience summary
- Strategy index table: number, name, effort, impact, phase, link to file
- Suggested weekly routine (which prompts to run on which days)
- Output directory explanation

If README already exists, update it with any new strategies or changed settings.

---

### Step 7: Create Output Infrastructure

Create these directories and placeholder files:
- `promote/output/` — all agent-generated content goes here
- `promote/templates/` — reusable templates (email, social post, visual)

---

### Step 8: Self-Assessment and Improvement Suggestions

After building everything, do a final review:

1. **Coverage check:** Are there obvious marketing channels missing? Did the competitor analysis reveal a channel competitors use that we don't have a strategy for?

2. **Feasibility check:** Are any strategies unrealistic given what was discovered about the site? Remove or flag them.

3. **Quick wins identification:** Which 3 strategies could produce results within 1-2 weeks? Highlight these prominently in the README.

4. **Cross-strategy synergies:** Identify where strategies feed into each other (e.g., "articles from strategy #7 can be shared via strategy #6 social bot and pitched via strategy #3 journalist outreach"). Document these connections.

5. **Questions for the site owner:** If anything remains unclear after all research, list specific questions. Don't guess — ask.

Save assessment to: `promote/output/self-assessment.md`

---

## Quality Standards (apply to ALL outputs)

1. **No hallucination.** Every data point, URL, and claim must come from actual research. If you can't verify it, say so.

2. **Non-technical language.** The person using this system operates AI agents but does not write code. No jargon, no terminal commands, no programming concepts in strategy files.

3. **Actionable, not advisory.** Every prompt produces a concrete deliverable — a target list, draft emails, ready-to-post content, a tracking table. Never output vague recommendations like "consider improving your SEO."

4. **Genuinely helpful outreach.** All outreach messages must provide real value to the recipient. The question to ask: "Would I appreciate receiving this message?" If no, rewrite it.

5. **Ethical only.** No spam, no fake reviews, no astroturfing, no black-hat SEO, no deceptive practices. Every strategy must be something you'd be comfortable explaining publicly.

6. **Iterative by design.** This entire system is meant to be re-run. Every file should be enrichable, not just replaceable. Track what works, kill what doesn't, double down on winners.

7. **Platform-compliant.** Follow the terms of service of every platform mentioned. Include warnings where relevant.

---

## Re-run Behavior

When this prompt is run again on the same `promote/` directory:

- **Research files** (site-profile, competitor-analysis, audience-communities, strategy-ranking): Read existing versions, then research again. Update with new findings. Mark changes with [NEW] and [UPDATED] tags. Remove anything that's no longer accurate.

- **Strategy files** (01-*.md through 15-*.md): Read existing versions. Improve prompts based on any tracking data found in promote/output/. Add new prompts if new opportunities were discovered. Refine outreach templates based on what's worked. Don't remove strategies that are actively being used (check tracking files).

- **README**: Update to reflect current state of the system.

- **Tracking files in promote/output/**: Never overwrite. Only append new entries or update status of existing entries.

This makes the system a living document that improves with every run.
```

---

## Quick Reference

**First run:** Paste the prompt with your domain. The agent builds everything from scratch.

**Subsequent runs:** Paste the same prompt. The agent reads existing files, researches what's new, and improves the system.

**Running a strategy:** Open any strategy file (e.g., `promote/03-journalist-queries.md`), copy a prompt from it, paste into your AI agent.

**Checking progress:** Ask your agent: "Read all tracking files in promote/output/ and give me a summary of what's working and what's not."
