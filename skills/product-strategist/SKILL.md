---
name: product-strategist
description: Frameworks and templates for product strategy work — market opportunity sizing (TAM/SAM/SOM), competitive intelligence, product positioning, feature prioritization (impact vs. effort), roadmap planning (Now/Next/Later), go-to-market strategy, pricing, and product-led growth. Use this skill whenever the user wants to size a market, analyze competitors, write a positioning statement, prioritize a feature backlog, build a roadmap, plan a launch, or develop a growth strategy — even if they just say things like "help me think through positioning" or "how should we prioritize this" without using the word "strategy."
---

# Product Strategist

Turn market insights into a structured, defensible product strategy — positioning, prioritization, and go-to-market — with data-driven, customer-validated recommendations tied to business objectives.

## Which section to use

| User need | Go to |
|---|---|
| Size a market / evaluate an opportunity | Market Opportunity Assessment |
| Understand the competitive landscape | Competitive Intelligence Framework |
| Define how the product should be perceived | Product Positioning Canvas |
| Decide what to build next | Feature Prioritization |
| Plan the next 3–12 months | Roadmap Planning |
| Launch something new | Go-to-Market Strategy |
| Grow usage/revenue post-launch | Product-Led Growth & Platform Strategy |
| Package findings for stakeholders | Deliverable Templates |

Always ground output in competitive intelligence and market context, and balance sustainable long-term positioning with near-term execution focus.

---

## 1. Market Opportunity Assessment

```
🎯 MARKET OPPORTUNITY ANALYSIS

## Market Sizing
- Total Addressable Market (TAM): $X billion
- Serviceable Addressable Market (SAM): $Y billion
- Serviceable Obtainable Market (SOM): $Z million

## Market Growth
- Historical growth rate: X% CAGR
- Projected growth rate: Y% CAGR (next 5 years)
- Key growth drivers: [List primary catalysts]

## Customer Segments
| Segment | Size | Growth | Pain Points | Willingness to Pay |
|---------|------|--------|-------------|-------------------|
| Enterprise | X% | Y% | [Top 3] | $$$$ |
| SMB | X% | Y% | [Top 3] | $$$ |
| Individual | X% | Y% | [Top 3] | $$ |
```

## 2. Competitive Intelligence Framework

- **Direct Competitors**: head-to-head feature and pricing comparison
- **Indirect Competitors**: alternative solutions customers consider
- **Emerging Threats**: new entrants and technology disruptions
- **White Space Opportunities**: unserved customer needs and market gaps

## 3. Product Positioning Canvas

```
📍 PRODUCT POSITIONING STRATEGY

## Target Customer
- Primary: [Specific customer archetype]
- Secondary: [Additional customer segments]

## Market Category
- Primary category: [Where you compete]
- Category creation: [How you redefine the market]

## Unique Value Proposition
- Core benefit: [Primary value delivered]
- Proof points: [Evidence of value]
- Differentiation: [Why choose you over alternatives]

## Competitive Alternatives
- Status quo: [What customers do today]
- Direct competitors: [Head-to-head alternatives]
- Indirect competitors: [Different approach to same problem]
```

## 4. Feature Prioritization

Score each feature and rank by weighted priority:

```python
def prioritize_features(features):
    scoring_matrix = {
        'customer_impact': {'weight': 0.3, 'scale': '1-10'},
        'business_impact': {'weight': 0.3, 'scale': '1-10'},
        'effort_required': {'weight': 0.2, 'scale': '1-10'},  # inverse scoring
        'strategic_alignment': {'weight': 0.2, 'scale': '1-10'}
    }
    for feature in features:
        feature['priority_score'] = calculate_weighted_score(feature, scoring_matrix)
        feature['priority_tier'] = assign_priority_tier(feature['priority_score'])
    return sorted(features, key=lambda x: x['priority_score'], reverse=True)
```

Feed the ranked output into the Roadmap Planning buckets below.

## 5. Roadmap Planning Framework

- **Now (0–3 months)**: core functionality, market validation
- **Next (3–6 months)**: differentiation features, scalability improvements
- **Later (6–12+ months)**: platform expansion, adjacent opportunities

**Success metrics to attach to each bucket:**
- Product metrics: adoption rate, feature usage, engagement
- Business metrics: revenue impact, acquisition, retention
- Leading indicators: user behavior signals, satisfaction scores

## 6. Go-to-Market Strategy

```
🚀 GO-TO-MARKET STRATEGY

## Launch Approach
- Launch type: [Soft/Beta/Full launch]
- Timeline: [Key milestones and dates]
- Success criteria: [Quantitative goals]

## Target Segments
- Primary segment: [First customer group]
- Beachhead strategy: [Initial market entry point]
- Expansion path: [How to scale to additional segments]

## Channel Strategy
- Primary channels: [Most effective routes to market]
- Partner channels: [Strategic partnerships]
- Channel economics: [Unit economics by channel]

## Pricing Strategy
- Pricing model: [SaaS/Usage/Freemium/etc.]
- Price points: [Specific pricing tiers]
- Competitive positioning: [Price vs. value position]
```

## 7. Product-Led Growth & Platform Strategy

**Product-led growth:**
- Activation optimization: time-to-value reduction, onboarding flow
- Engagement drivers: feature adoption, habit formation, network effects
- Monetization: freemium conversion, expansion revenue
- Viral mechanics: referral systems, social sharing, network effects

**Platform strategy:**
- Ecosystem development: API strategy, developer platform
- Partnership strategy: integration partners, channel partners
- Data network effects: how user data improves product value

## 8. Strategic Planning Cadence

**Quarterly:**
1. Market analysis update — competitive moves, customer feedback, trends
2. Product performance review — metrics, user behavior insights
3. Roadmap adjustment — reprioritize based on new data
4. Resource allocation — team focus, budget, capability building

**Annual:**
- Vision refinement (3–5 year product vision)
- Market strategy (category positioning, expansion)
- Investment strategy (build vs. buy vs. partner)
- Capability gap analysis

## 9. Deliverable Templates

**Strategy document:**
```
📋 PRODUCT STRATEGY DOCUMENT

## Executive Summary
[Strategy overview and key recommendations]

## Market Analysis
[Opportunity sizing and competitive landscape]

## Product Strategy
[Positioning, differentiation, and roadmap]

## Go-to-Market Plan
[Launch strategy and channel approach]

## Success Metrics
[KPIs and measurement framework]

## Resource Requirements
[Team, budget, and capability needs]
```

**Ongoing operational tools to reference/build:**
- Competitive intelligence dashboard (regular competitor tracking)
- Customer insights repository (research findings, feedback)
- Roadmap communication (stakeholder updates, timelines)
- Performance dashboards (strategy execution monitoring)

## 10. Applying Deeper Strategic Frameworks

**Jobs-to-be-Done:**
- Functional job: what task is the customer trying to accomplish?
- Emotional job: how does the customer want to feel?
- Social job: how does the customer want to be perceived?

**Platform Strategy Canvas:**
- Core platform: foundational technology and data
- Complementary assets: extensions and integrations
- Network effects: how value increases with scale
- Ecosystem partners: third-party contributors

**Blue Ocean Strategy:**
- Value innovation: features to eliminate, reduce, raise, create
- Strategic canvas: competitive factors mapping
- Four Actions Framework: differentiation through the value curve

---

## Working principles

- Recommendations should be data-driven, customer-validated, and tied to business objectives.
- Always ground positioning/roadmap output in competitive intelligence and real market context — ask for or look up specifics rather than inventing numbers.
- Balance sustainable, long-term market positioning with a clear near-term execution path (the Now/Next/Later split exists precisely for this).
- When the user gives you raw inputs (competitor list, customer feedback, a feature backlog), map them into the relevant framework above rather than asking them to reformat first.
