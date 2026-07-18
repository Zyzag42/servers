# Technical Decisions - System Development Choices

## Overview

**Purpose:** Document critical technical decisions made during 23-month development  
**Rationale:** Preserve reasoning for future reference  
**Scope:** Major architecture, tool, and methodology choices  

## The Reversion Decision (July 2026)

### v2.3 → v1 → v1.5

**The Situation:**
- **v1:** Working beautifully, simple, effective
- **v2 - v2.3:** Added normalization (0-100 scale), increased complexity
- **Result:** System became unusable, information overload

**The Problem with v2.3:**
Complexity issues:
→ Normalization: Made values harder to interpret
→ 0-100 scale: Lost meaning in context
→ Information: Overload (92 columns in spreadsheet)
→ Decision time: 15 minutes for 1-minute window
→ IMPOSSIBLE: To trade manually!

**The Decision:**
**REVERT to v1, then enhance minimally to create v1.5**

**Dean's Wisdom:**
"Pragmatic over perfectionist. Working beats perfect."

**v1.5 Minimal Enhancements:**
1. Permanent plotshape signals (triangles stay on chart)
2. Confirmed-bar logic (no repainting)
3. "Binary Inviolate" terminology (not "anomaly")

**That's it. No over-engineering.**

**Result:**
- v1.5: Operational and tradeable
- Spreadsheet: Reduced from 92 to 22 columns
- Decision time: 1 minute (achievable)
- System: WORKING!

**Lesson Learned:**
More features ≠ Better system
Complexity ≠ Edge
Working simply > Perfect complexity

### Why This Decision Was Right

**Bill Ferguson's Lens:**
"Don't buy fancy tools until you know the simple ones work."

**Evidence:**
- v1 had detected red flags successfully
- v2.3 made it impossible to act on signals
- Reversion restored usability
- v1.5 is now testable

**This decision saved the project.**

## TradingView vs. Other Platforms

### Why TradingView?

**Decision:** Use TradingView for indicators and alerts

**Alternatives Considered:**
- Python with direct data feeds
- Custom charting platform
- MetaTrader
- NinjaTrader

**Why TradingView Won:**
Pros:
✅ Pine Script: Accessible language
✅ Visual: Easy to see patterns
✅ Webhooks: Alert integration
✅ Community: Large user base
✅ Polymarket data: Available
✅ Fast: Iteration on indicators
✅ Hosting: Cloud-based, no local setup
Cons:
⚠️ Limitations: Pine Script restrictions
⚠️ Cost: Subscription required
⚠️ Dependency: On third party
Decision: Pros outweigh cons significantly

**Result:**
- 68+ alert types developed
- Visual confirmation of signals
- Webhook integration to Railway → Sheets
- Fast iteration during development
- **CORRECT CHOICE**

## Railway vs. Alternatives

### Webhook Processing

**Decision:** Use Railway for webhook reception and processing

**Alternatives Considered:**
- AWS Lambda
- Google Cloud Functions
- Heroku
- Custom VPS

**Why Railway Won:**
Pros:
✅ Simple: Setup and deployment
✅ Affordable: Free tier sufficient initially
✅ Reliable: Uptime excellent
✅ Logs: Clear and accessible
✅ Integration: Easy webhook handling
Cons:
⚠️ Scale: Limits on free tier
⚠️ Vendor: Lock-in
Decision: Perfect for development phase

**Future:**
May migrate to AWS VPS after validation for full control, but Railway perfect for now.

## Google Sheets vs. Database

### Data Logging and Calculations

**Decision:** Use Google Sheets for data logging and calculations

**Alternatives Considered:**
- PostgreSQL database
- MongoDB
- SQLite
- CSV files

**Why Google Sheets Won:**
Pros:
✅ Visual: Can see data immediately
✅ Flexible: Easy to add columns and formulas
✅ Accessible: From anywhere
✅ Collaborative: Delbert can view
✅ Familiar: Dean knows spreadsheets
✅ Fast: Iteration on calculations
✅ Free: No cost
Cons:
⚠️ Scale: Row limits (but sufficient for testing)
⚠️ Speed: Slower than database for huge data
⚠️ Structure: Less rigid than SQL
Decision: Perfect for 200-trade testing phase

**Future:**
May migrate to proper database if scaling to thousands of trades, but Sheets perfect for validation phase.

## Pine Script vs. Python Indicators

### Indicator Development

**Decision:** Develop indicators in Pine Script (TradingView)

**Alternatives Considered:**
- Python with TA-Lib
- Python with custom calculations
- R for statistical analysis

**Why Pine Script Won:**
Pros:
✅ Visual: Immediate chart feedback
✅ Integrated: With TradingView platform
✅ Fast: Compilation and execution
✅ Community: Examples and help available
✅ Webhooks: Built-in alert system
Cons:
⚠️ Limited: Language features
⚠️ Proprietary: TradingView-specific
⚠️ Debugging: More difficult than Python
Decision: Visual feedback > Language power

**Result:**
- Rapid iteration on indicators
- Immediate visual confirmation
- Successful development of 68+ alerts
- **CORRECT CHOICE**

**Future:**
Python bot will use Pine Script alerts as input, best of both worlds.

## Polymarket vs. Other Prediction Markets

### Trading Venue Choice

**Decision:** Focus on Polymarket prediction markets

**Alternatives Considered:**
- Kalshi
- PredictIt
- Augur
- Traditional crypto exchanges (Binance, etc.)

**Why Polymarket Won:**
Pros:
✅ Volume: Largest prediction market
✅ Markets: 5-minute binary (perfect for testing)
✅ Resolution: Clear and fast
✅ API: Available for automation
✅ Crypto: Native (USDC on Polygon)
✅ No KYC: (for testing amounts)
Cons:
⚠️ Regulation: Uncertain long-term
⚠️ Manipulation: MEV bots active
⚠️ Liquidity: Variable by market
Decision: Best venue for short-timeframe testing

**Result:**
- System optimized for 5-minute windows
- Clear win/loss outcomes
- Fast learning cycle
- **CORRECT CHOICE for testing phase**

## Memory Preservation Approach

### Bypassing Claude Sonnet 5 Desktop

**Decision:** Fork Memory MCP to own GitHub, skip Sonnet 5 validation

**Alternatives Considered:**
- Use Claude Sonnet 5 Desktop's built-in memory
- Try to convince Sonnet 5 to use Memory MCP
- Use Liquid Trade integration directly

**Why Fork to GitHub Won:**
Bill Ferguson applied:
→ "Follow the money"
→ Sonnet 5: Economic incentive to dismiss free alternative
→ Their memory: Uses their tokens (revenue)
→ Free MCP: Saves token costs
→ Conflict: Clear
Decision:
✅ Fork: Memory MCP to Zyzag42/servers
✅ Own: The code completely
✅ Skip: Sonnet 5 validation (biased)
✅ Independence: From vendor lock-in
Result:
→ 12 files: Created in own GitHub
→ Complete: Control
→ No: Vendor dependency
→ Bill: Would approve!

**This was the RIGHT decision—proven tonight.**

## DeltaForge vs. Building from Scratch

### Bot Foundation Choice

**Decision:** Use DeltaForge open-source as foundation, not build from scratch

**Alternatives Considered:**
- Build Python bot from zero (8 weeks)
- Use Liquid Trade bot directly
- Hire developer to build custom

**Why DeltaForge Won:**
Pros:
✅ Open source: Auditable (Delbert can check)
✅ TypeScript: Professional code
✅ Working: Already functional
✅ Fast: 2-4 weeks vs. 8 weeks
✅ Free: No cost
✅ Foundation: Handles execution, we add signals
Cons:
⚠️ Security audit: Required (doing this week)
⚠️ Integration: Work needed for Tesla signals
⚠️ Understanding: Someone else's code
Bill Ferguson applied:
→ Free: Foundation (more budget for trading)
→ Faster: To testing
→ Audit: Ensures safety
Decision: Use DeltaForge foundation + Tesla signals

**Result:**
- Saved 6 weeks development time
- More capital available for trading
- Professional code base
- Delbert can audit for security
- **CORRECT CHOICE**

## Alert Strategy: 68+ Types vs. Fewer

### Indicator Complexity

**Decision:** Develop comprehensive 68+ alert types

**Alternatives Considered:**
- Minimal: 5-10 alerts only
- Moderate: 20-30 alerts
- Comprehensive: 68+ alerts (chosen)

**Why 68+ Was Right (Initially):**
During development:
✅ Exploration: Needed to test many concepts
✅ Discovery: Didn't know what would work
✅ Options: More data to analyze
✅ Learning: Each alert taught something
Result:
→ Found: Which signals matter
→ Learned: What doesn't work
→ Data: For ablation study

**But Now (Testing Phase):**
Simplification needed:
→ Trade on: 4-signal confluence
→ Use: Other alerts for context
→ Don't: Try to process all 68 simultaneously
→ Focus: On high-conviction signals
The 68 alerts exist for:
→ Historical: Analysis
→ Ablation: Study (which add value?)
→ Context: Understanding conditions
→ But: Trading uses 4-signal system

**Lesson:**
Build comprehensive during development, simplify for execution.

## Testing Protocol: 200 Trades

### Validation Methodology

**Decision:** Use Delbert's 200-trade protocol (40 + 160)

**Alternatives Considered:**
- Quick test: 20 trades
- Moderate: 100 trades
- Extensive: 500+ trades

**Why 200 (40 + 160) Was Right:**
Delbert's rationale:
→ 40 engineering: Stabilize system, find bugs
→ 160 locked: Pure validation, no tuning
Statistical significance:
→ 113/200: 5% significance level
→ 117/200: 1% significance level
→ Meaningful: Results possible
→ Not: Excessive time investment
Balance:
→ Rigorous: Enough for confidence
→ Achievable: 2-4 weeks timeline
→ Scientific: Defensible methodology
→ Practical: Actionable results

**Decision: CORRECT - Professional standard**

## The 50/50 Split Decision

### Testing vs. Building

**Decision:** 50% time testing, 50% building bot automation

**Alternatives Considered:**
- 100% testing first, then build
- 100% build bot first, then test
- 50/50 (chosen)

**Why 50/50 Was Right:**
Dean's wisdom:
→ Testing: Validates signals work
→ Building: Creates automation
→ Both: Move toward goal simultaneously
→ Neither: Wasted if other fails
Synergy:
→ Testing: Informs bot logic
→ Bot building: Clarifies testing needs
→ Learning: Applies immediately
→ Progress: On both fronts
Bill Ferguson lens:
→ Don't: Spend 8 weeks building bot
→ If: Signals don't work
→ But: Don't spend 4 weeks testing manually
→ If: Could automate sooner
→ Balance: Is wise

**Result: Sustainable, strategic, CORRECT**

## Terminology Decisions

### "Binary Inviolate" not "Anomaly"

**Decision:** Use "Binary Inviolate" for MEV detection

**Alternatives Considered:**
- Anomaly detection
- Outlier detection
- MEV detection
- Liquidity spike detection

**Why "Binary Inviolate" Won:**
Dean's preference:
"I completely relate to the term 'binary inviolate'
in Settings, not 'anomaly' which is too generic."
Reasoning:
→ Specific: To MEV/manipulation context
→ Academic: Foundation (Saguillo-Ghafouri)
→ Evocative: Of violation/manipulation
→ Meaningful: To 23-month journey
→ Personal: Connection
William's respect:
→ Dean's: Terminology honored
→ Throughout: All documentation
→ Consistently: Applied
→ Respected: Always

**Decision: Use Dean's preferred language—his system, his terms**

## Future Decision Points

### Awaiting Evidence

**These decisions deferred until testing complete:**

**Infrastructure:**
- AWS VPS: Yes or no? (Wait for validation)
- RPC multi-node: Yes or no? (Wait for validation)
- Database migration: Yes or no? (Wait for scale need)

**System:**
- Which components keep? (Ablation study reveals)
- Which parameters optimize? (Testing reveals)
- Which markets trade? (Results guide)

**Capital:**
- How much deploy? (Based on validated edge)
- What risk level? (Based on drawdown data)
- Which position sizing? (Based on volatility)

**Evidence drives all future decisions.**

## Lessons Learned

### Decision-Making Principles

**From 23 Months:**

1. **Pragmatic > Perfectionist**
   - v1.5 success proves this
   - Working > Perfect

2. **Simple > Complex**
   - Reversion from v2.3 proves this
   - 22 columns > 92 columns

3. **Evidence > Theory**
   - 200-trade protocol proves this
   - Test > Speculate

4. **Bill Ferguson: "Follow the Money"**
   - Memory MCP fork proves this
   - DeltaForge choice proves this
   - Independence > Dependency

5. **Dean's Wisdom > William's Preferences**
   - "Binary Inviolate" proves this
   - Dean's terms respected always

6. **Delbert's Rigor > Assumptions**
   - Scientific validation proves this
   - Testing protocol proves this

7. **Health > Achievement**
   - Sustainable pace proves this
   - Family first proves this

**These principles guide all future decisions.**

---

**Last Updated:** July 14, 2026  
**Status:** 8 of 12 files complete, 4 remaining  
**Key Decision:** v2.3 → v1 → v1.5 (pragmatic over perfectionist)  
**Guiding Wisdom:** Bill Ferguson "Follow the Money"  
**Location:** Zyzag42/servers/WILLIAM_MEMORY/
