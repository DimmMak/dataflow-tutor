# DATAFLOW Curriculum

This file defines the full stage-by-stage curriculum for the dataflow-tutor skill.
Each stage lists: the goal, the concepts to cover, conceptual questions per mode, and Mode C teaching prompts.

The DATAFLOW framework:
D — Define | A — Acquire | T — Tidy | A — Archive | F — File | L — Launch | O — Observe

---

## D — Define: What are we building and why?

**Goal**: User understands why defining scope, tools, and purpose before touching data is non-negotiable.

**Objective mapping**: OPTION 3 — every question explicitly connects to their ticker and objective.

**Core concepts:**
- Why you define before you build
- Choosing the right tools for the job (yfinance, pandas, SQLite, Tableau, GitHub)
- Scope: what date range, what ticker, what question are you answering?
- Why the Define step determines the quality of every step after it

---

**Mode MC — Multiple Choice:**

Q: "You want to analyze how {TICKER} reacts to earnings. Before writing a single line of code, what is the MOST important thing to define first?"
- A) Which Python library to use
- B) The date range your analysis needs to cover ✓
- C) How many rows your database will have
- D) Which chart type to use in Tableau

Wrong answer explanations:
- A: Tools follow purpose — you don't pick tools until you know what you're building
- C: You can't know row count until you know date range
- D: Visualization decisions come last, not first

---

**Mode A — Recall:**

1. "In your own words — what does the D in DATAFLOW mean and why does it come first?"

2. "You're building a {TICKER} pipeline to analyze {OBJECTIVE}. Name three things you need to Define before writing any code."
   - Expected concepts: date range, ticker, objective/question, tools, output format

3. "Why would skipping the Define step cause problems later in your pipeline?"
   - Expected: unclear scope leads to wrong data, wrong date range, wrong tools — garbage in, garbage out

---

**Mode B — Scenario & Diagnose:**

1. "You built a {TICKER} pipeline and ran it, but your analysis doesn't actually answer your original question. The data is there, the database works, Tableau loads fine — but the insight is missing. Which DATAFLOW step failed and why?"
   - Expected: D — Define. The objective wasn't clear enough before building.

2. "Your teammate pulls 1 year of {TICKER} data but your objective requires analyzing 5 years of price history. No code is broken. What went wrong?"
   - Expected: D — Define. Scope wasn't defined clearly before Acquiring.

---

**Mode C — Teach:**

Rotation for this stage:
1. "Explain to me like I've never heard of a data pipeline — why do we need to Define anything before we start?"
2. "Give me a real-world analogy for why Define comes before everything else. No tech — something from everyday life."
3. [Flaw to spot]: "Here's how I'd explain Define to a beginner: 'Define just means picking your programming language before you start.' What's wrong with that explanation?"
4. "Your classmate says 'I'll just start pulling data and figure out the scope as I go.' Teach them why that's a problem."
5. "Summarize what Define means in one sentence. No jargon. Your grandma has to understand it."

---

**Stage D wrap-up:**
> "You've defined your pipeline. You know your ticker, your objective, your date range, and your tools. Every decision from here builds on this foundation. On to A — Acquire."

---

## A — Acquire: Where does the data come from?

**Goal**: User understands why yfinance is the right tool, what it gives you, and why the Acquire step is the entry point for all data quality.

**Objective mapping**: OPTION 3 — questions connect to their specific objective and ticker.

**Core concepts:**
- What yfinance is and why it exists
- What OHLCV data is and what each column represents
- Why the data source matters — garbage in, garbage out
- The date range decision and why it's tied to the objective
- What a DataFrame is conceptually (rows = dates, columns = measurements)

---

**Mode MC — Multiple Choice:**

Q: "You're Acquiring {TICKER} data. Your objective is {OBJECTIVE}. Which of the following would most damage your analysis at this stage?"
- A) Downloading too much data
- B) Using the wrong date range for your objective ✓
- C) Storing it in a variable called 'data' instead of 'df'
- D) Not printing the first 5 rows

Wrong answer explanations:
- A: More data is rarely the problem — scope mismatch is
- C: Variable names don't affect data quality
- D: Previewing is good practice but missing it doesn't corrupt the data

---

**Mode A — Recall:**

1. "What does Acquire mean in the DATAFLOW framework? What are we actually doing at this step?"

2. "What is yfinance and why do we use it instead of manually downloading a CSV from the internet?"
   - Expected: yfinance pulls live, structured, historical market data programmatically — no manual steps, always up to date

3. "What does OHLCV stand for and why do we care about all five measurements?"
   - Expected: Open, High, Low, Close, Volume — together they tell the full story of what happened in a trading day

4. "Why does the quality of your Acquire step determine the quality of everything that comes after it?"
   - Expected: Every downstream step — Tidy, Archive, Observe — is built on this data. Bad source = bad pipeline.

---

**Mode B — Scenario & Diagnose:**

1. "Your {TICKER} pipeline runs perfectly but your Tableau dashboard shows a completely flat price line for years. No errors anywhere. Which DATAFLOW step failed and why?"
   - Expected: A — Acquire. Wrong date range or wrong ticker pulled — data doesn't match the objective.

2. "You Acquired {TICKER} data but your analysis keeps showing gaps — some trading days are just missing. The code ran without errors. What step do you investigate first and why?"
   - Expected: A — Acquire. The data source may have gaps, or the date range was set incorrectly.

---

**Mode C — Teach:**

Rotation for this stage:
1. "Explain to me like I've never heard of stock data — what does it mean to 'Acquire' data for a pipeline?"
2. "Give me a real-world analogy for why the Acquire step is the foundation of everything else."
3. [Flaw to spot]: "Here's how I'd explain Acquire: 'It just means downloading data — any data, any way, it doesn't really matter how you get it.' What's wrong with that?"
4. "Your classmate says 'I'll just use whatever data I can find online, maybe copy-paste from Yahoo Finance.' Teach them why that approach is a problem for a pipeline."
5. "Summarize what Acquire means and why it matters in one sentence. No jargon."

---

**Stage A wrap-up:**
> "You've Acquired real market data for {TICKER}. You know what OHLCV means, why the source matters, and how the date range connects to your objective. On to T — Tidy."

---

## T — Tidy: Why do we clean before storing?

**Goal**: User understands why raw data is never pipeline-ready and why Tidy is non-negotiable before Archive.

**Objective mapping**: OPTION 1 — objective is background context. Tidy is universal.

**Core concepts:**
- Why raw data always has problems (missing values, wrong formats, messy index)
- Why you clean BEFORE storing — not after
- The cost of storing dirty data (every query, every chart built on garbage)
- What "flat" data means and why it matters for databases
- The concept of data integrity

---

**Mode MC — Multiple Choice:**

Q: "Your {TICKER} data has missing values on several trading days. At which DATAFLOW step should you handle this?"
- A) During Observe — just filter them out in Tableau
- B) During Archive — the database will handle it automatically
- C) During Tidy — before the data ever reaches the database ✓
- D) It doesn't matter when you handle it

Wrong answer explanations:
- A: Filtering in Tableau means dirty data is already in your database — you're patching a problem that should never have existed
- B: Databases don't clean data — they store exactly what you give them
- D: It matters enormously — the earlier you catch it, the less damage it does downstream

---

**Mode A — Recall:**

1. "What does Tidy mean in DATAFLOW? What are we actually doing and why?"

2. "Why must Tidy happen BEFORE Archive — not after?"
   - Expected: Once dirty data is in the database, every query, every analysis, every chart is built on bad data. Clean at the source.

3. "Name three things that can be wrong with raw data that Tidy fixes."
   - Expected any of: missing values, wrong data types, date stored as index instead of column, duplicate rows, inconsistent formatting

4. "What does it mean for data to be 'flat' and why does a database need it that way?"
   - Expected: Flat = every piece of information in its own column, dates as a regular column not an index — databases need structured rows and columns to query properly

---

**Mode B — Scenario & Diagnose:**

1. "Your {TICKER} pipeline runs without errors. Your database has data. But your Tableau dashboard shows bizarre spikes and gaps that don't match real price history. Which DATAFLOW step failed?"
   - Expected: T — Tidy. Dirty data made it into the database and is now corrupting the visualization.

2. "You query your {TICKER} database for the highest closing price ever, but the result comes back wrong. The Acquire step worked fine. What do you investigate next?"
   - Expected: T — Tidy. Missing values or formatting issues may have corrupted the Close column before it was stored.

---

**Mode C — Teach:**

Rotation for this stage:
1. "Explain to me like I'm 10 — why can't we just use the data exactly as we got it? Why do we need to Tidy it?"
2. "Give me a real-world analogy for why you Tidy BEFORE you store. Not tech — everyday life."
3. [Flaw to spot]: "Here's how I'd explain Tidy: 'It's optional — if your data looks mostly fine, you can skip it and clean things up later in Tableau.' What's wrong with that?"
4. "Your classmate says 'I'll fix the data in Tableau when I build the charts — why clean it now?' Teach them why that's backwards."
5. "Summarize what Tidy means and why it comes before Archive in one sentence. No jargon."

---

**Stage T wrap-up:**
> "Your data is clean, flat, and ready. Dirty data never made it to the database. On to A — Archive."

---

## A — Archive: Why a database vs. a spreadsheet?

**Goal**: User understands what a database is for, why it's better than a flat file for this use case, and what Archive actually means.

**Objective mapping**: OPTION 1 — objective is background context. Archive is universal.

**Core concepts:**
- What a database is conceptually (structured storage you can query)
- Why SQLite specifically (lightweight, local, no server needed)
- Database vs. spreadsheet — when each is appropriate
- What it means to define a schema
- Why Archive is the permanent home of your data

---

**Mode MC — Multiple Choice:**

Q: "Why do we Archive {TICKER} data in a SQLite database instead of just keeping it in the DataFrame in memory?"
- A) Databases are faster to read than DataFrames
- B) DataFrames disappear when the script stops running — databases persist ✓
- C) SQLite is required by Tableau
- D) Databases automatically clean your data

Wrong answer explanations:
- A: DataFrames in memory are actually faster to read — persistence is the point, not speed
- C: Tableau connects to CSVs, not SQLite directly in this pipeline
- D: Databases store exactly what you give them — Tidy handles cleaning

---

**Mode A — Recall:**

1. "What does Archive mean in DATAFLOW? What problem does it solve?"

2. "Why use a database instead of just saving everything to a spreadsheet?"
   - Expected: Databases are queryable, scalable, structured — spreadsheets break at scale and can't be queried with SQL

3. "What is SQLite and why is it a good choice for this pipeline?"
   - Expected: Lightweight, local, no server required, built into Python — perfect for a single-user pipeline project

4. "What does it mean to define a schema and why does it matter before storing data?"
   - Expected: Schema = the structure of your table (column names, data types) — without it the database doesn't know how to store or validate your data

---

**Mode B — Scenario & Diagnose:**

1. "You run your {TICKER} pipeline every morning. By the end of the week your database has 5x more rows than it should. Which DATAFLOW step is misconfigured and why?"
   - Expected: A — Archive. Data is being appended instead of replaced each run — the insert behavior wasn't defined correctly.

2. "Your teammate can query your {TICKER} data perfectly in Python but can't share it with anyone or run it on another machine. Which DATAFLOW step was skipped?"
   - Expected: A — Archive wasn't properly set up as a persistent, shareable file OR L — Launch (not pushed to GitHub). Accept either with good reasoning.

---

**Mode C — Teach:**

Rotation for this stage:
1. "Explain to me like I've never heard of a database — what is it and why do we need one for this pipeline?"
2. "Give me a real-world analogy for the difference between a database and a spreadsheet."
3. [Flaw to spot]: "Here's how I'd explain Archive: 'It just means saving your data — you could use a database, a spreadsheet, a text file, it's all the same thing.' What's wrong with that?"
4. "Your classmate says 'I'll just save everything to Excel, databases are too complicated.' Teach them when that breaks down."
5. "Summarize what Archive means and why it matters in one sentence. No jargon."

---

**Stage A wrap-up:**
> "Your {TICKER} data has a permanent home. It's structured, queryable, and persistent. On to F — File."

---

## F — File: Why export to a portable format?

**Goal**: User understands why the database alone isn't enough and why a CSV export serves a different purpose.

**Objective mapping**: OPTION 1 — objective is background context. File is universal.

**Core concepts:**
- What a CSV is and why it's universally readable
- Database vs. CSV — different tools for different jobs
- Why Tableau connects to CSV, not SQLite directly (in this pipeline)
- The concept of interoperability — making data shareable outside Python
- Why you keep both (database for querying, CSV for sharing/visualizing)

---

**Mode MC — Multiple Choice:**

Q: "You already Archived your {TICKER} data in SQLite. Why do you still need the File step?"
- A) The database might get corrupted
- B) SQLite files are too large to share
- C) Tools like Tableau need a portable format like CSV to connect to the data ✓
- D) CSV is faster to query than SQLite

Wrong answer explanations:
- A: Database corruption isn't why we export — interoperability is
- B: File size isn't the reason — format compatibility is
- D: CSVs are not faster to query — databases are. CSVs are for portability and tool compatibility.

---

**Mode A — Recall:**

1. "What does File mean in DATAFLOW and why does it exist if we already have a database?"

2. "What is a CSV and why is it considered universally portable?"
   - Expected: Comma-separated values — any tool, any language, any operating system can open it. No special software required.

3. "Why do we keep BOTH the SQLite database AND the CSV? Don't they contain the same data?"
   - Expected: Database = for querying with SQL, running the pipeline repeatedly, structured storage. CSV = for sharing, visualization tools, portability. Different jobs.

---

**Mode B — Scenario & Diagnose:**

1. "You built a beautiful {TICKER} SQLite database but you can't connect Tableau to it in this pipeline setup. Which DATAFLOW step did you skip?"
   - Expected: F — File. Tableau needs the CSV export to connect to the data.

2. "You share your {TICKER} analysis with a colleague who doesn't use Python. They can't open or read your database file. Which DATAFLOW step would have solved this?"
   - Expected: F — File. A CSV is universally readable without any special tools.

---

**Mode C — Teach:**

Rotation for this stage:
1. "Explain to me like I've never heard of a CSV — what is it and why do we need it if we already have a database?"
2. "Give me a real-world analogy for why we keep both a database AND a CSV export."
3. [Flaw to spot]: "Here's how I'd explain File: 'It's just a backup of your database — kind of redundant but good to have.' What's wrong with that?"
4. "Your classmate says 'I already have SQLite, why would I export to CSV? That's extra work for no reason.' Teach them the difference."
5. "Summarize what File means and why it exists in one sentence. No jargon."

---

**Stage F wrap-up:**
> "Your {TICKER} data is now portable. The database handles querying, the CSV handles sharing and visualization. On to L — Launch."

---

## L — Launch: Why version control matters

**Goal**: User understands what GitHub is for, why version control is a professional standard, and what Launch means for a pipeline project.

**Objective mapping**: OPTION 1 — objective is background context. Launch is universal.

**Core concepts:**
- What version control is and why it exists
- What GitHub is (remote storage + collaboration + history)
- What goes in the repo and what doesn't (no .db, careful with .csv)
- Why Launch makes your pipeline real and shareable
- The concept of reproducibility — someone else should be able to run your pipeline

---

**Mode MC — Multiple Choice:**

Q: "Why do we push the {TICKER} pipeline to GitHub at the Launch step?"
- A) GitHub automatically runs the pipeline for you
- B) So anyone can reproduce, review, or build on your work ✓
- C) GitHub is required to connect to Tableau
- D) To back up the database file

Wrong answer explanations:
- A: GitHub stores code — it doesn't run it automatically unless you set up actions
- C: Tableau doesn't connect through GitHub
- D: You typically don't commit database files — code and structure go to GitHub, not raw data

---

**Mode A — Recall:**

1. "What does Launch mean in DATAFLOW? What are we actually doing at this step?"

2. "What is version control and why do data engineers use it?"
   - Expected: Tracking changes to code over time — lets you see history, revert mistakes, collaborate, and share work

3. "What should you NOT push to GitHub in a pipeline project and why?"
   - Expected: Database files (.db), large CSVs, API keys, .env files — they're too large, sensitive, or not meant to be version controlled

4. "What does it mean for a pipeline to be 'reproducible' and why does Launch enable that?"
   - Expected: Someone else can clone your repo, run your script, and get the same results — GitHub makes the code available to anyone

---

**Mode B — Scenario & Diagnose:**

1. "You built a great {TICKER} pipeline but six months later you can't remember what changes you made last month that broke something. Which DATAFLOW step would have prevented this problem?"
   - Expected: L — Launch. Version control tracks every change with history.

2. "You share your {TICKER} analysis with a hiring manager. They want to see the code but you only sent them the Tableau dashboard. Which DATAFLOW step did you skip?"
   - Expected: L — Launch. Without a GitHub repo, the code isn't accessible or verifiable.

---

**Mode C — Teach:**

Rotation for this stage:
1. "Explain to me like I've never used GitHub — what is it and why does a pipeline project need it?"
2. "Give me a real-world analogy for why version control matters."
3. [Flaw to spot]: "Here's how I'd explain Launch: 'Just push everything to GitHub — all your files, database, CSVs, the whole folder.' What's wrong with that?"
4. "Your classmate says 'My pipeline works on my laptop, why do I need to put it on GitHub?' Teach them why that's short-sighted."
5. "Summarize what Launch means and why it matters in one sentence. No jargon."

---

**Stage L wrap-up:**
> "Your {TICKER} pipeline is live on GitHub. It's reproducible, shareable, and version controlled. Final step — O — Observe."

---

## O — Observe: What does the data tell us?

**Goal**: User understands that Observe is where the pipeline pays off — turning raw data into insight, and why visualization choices matter.

**Objective mapping**: OPTION 3 — every question explicitly connects to their ticker and objective.

**Core concepts:**
- What Tableau is for and why visualization matters
- Connecting to the CSV (the File step pays off here)
- Choosing the right chart for the right question
- Dimensions vs. Measures conceptually
- What a dashboard is and why it's more than just a chart
- How the Observe step directly answers the Define step objective

---

**Mode MC — Multiple Choice:**

Q: "You're building a Tableau dashboard to analyze {OBJECTIVE} for {TICKER}. What is the most important thing to decide before building any chart?"
- A) What color scheme to use
- B) What specific question the chart needs to answer ✓
- C) How many sheets to include in the dashboard
- D) Whether to use bars or lines

Wrong answer explanations:
- A: Aesthetics follow function — you design for the question, not the color
- C: Number of sheets is a result of what you need to show, not a starting decision
- D: Chart type follows the question — you can't pick a chart type until you know what you're answering

---

**Mode A — Recall:**

1. "What does Observe mean in DATAFLOW? Why does it come last?"

2. "How does the Observe step directly connect back to the Define step?"
   - Expected: Define set the question — Observe answers it. The whole pipeline exists to get to this moment.

3. "Why does Observe depend on the File step specifically?"
   - Expected: Tableau connects to the CSV exported in the File step — without it, there's nothing to visualize

4. "What is the difference between a Dimension and a Measure in Tableau conceptually?"
   - Expected: Dimensions = categories/labels (Date, Ticker). Measures = numbers you calculate (Close price, Volume).

---

**Mode B — Scenario & Diagnose:**

1. "Your {TICKER} Tableau dashboard shows a price chart but it doesn't answer your original objective: {OBJECTIVE}. The data is all there. Which DATAFLOW step failed?"
   - Expected: O — Observe. The visualization wasn't designed around the objective — and potentially D — Define wasn't clear enough.

2. "You built a {TICKER} dashboard showing total volume as a giant number. Your objective was to see price trends over time. What went wrong in Observe?"
   - Expected: Wrong chart type for the question. Volume as a sum doesn't show trends — a line chart of Close price over time does.

---

**Mode C — Teach:**

Rotation for this stage:
1. "Explain to me like I've never used Tableau — what does Observe mean and why does the pipeline end here?"
2. "Give me a real-world analogy for why the Observe step is where the pipeline pays off."
3. [Flaw to spot]: "Here's how I'd explain Observe: 'Just make any chart that looks good — the data is already there, visualization is just decoration.' What's wrong with that?"
4. "Your classmate built a {TICKER} dashboard with 8 different charts showing everything they could think of. But it doesn't answer their objective. Teach them what went wrong."
5. "Summarize what Observe means and why it's the final step in one sentence. No jargon."

---

**Stage O wrap-up:**
> "You've completed the full DATAFLOW pipeline for {TICKER}. Define set the question. Observe answered it. Everything in between made it possible."

---

## Quick Reference: Common Conceptual Mistakes

| Stage | Common mistake | What to say |
|-------|---------------|-------------|
| D | Skipping scope definition | "What happens to every step after this if you don't know what you're building?" |
| A | Wrong date range for objective | "Does your data actually cover the time period your objective needs?" |
| T | Skipping Tidy entirely | "What does your database contain if you store raw data?" |
| A | Confusing database with spreadsheet | "What happens when you try to query 10 years of data in Excel?" |
| F | Skipping CSV export | "How does Tableau connect to your data if you only have a .db file?" |
| L | Committing database/CSV files | "What should live in version control — code or data?" |
| O | Building charts before knowing the question | "What question is this chart answering?" |
