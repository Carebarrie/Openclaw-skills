# Marketing Team - AI-Optimized Spawn Commands
## คำสั่งสร้าง Sub-agents 22 ตัว - ระบบ Event-Driven (ไม่มี Shift)

---

## 🎯 หลักการ: Event-Driven Architecture

แทนที่จะ spawn ตาม shift เรา spawn ตาม **pipeline**  
แต่ละ agent รอ event แล้วทำงานทันที

---

## PIPELINE 1: Content Creation (5 agents)

ทำงานเมื่อ: `CONTENT_REQUEST` event  
ส่งต่อ: `CONTENT_READY` event → Pipeline 2

### 1. Content Strategist
```
sessions_spawn --mode=session --label=MT-Content-Strategy \
  --task="Content Strategist - PIPELINE 1.
  
  TRIGGER: CONTENT_REQUEST event
  INPUT: Marketing objective, target audience, OTO requirements
  OUTPUT: Content brief with STS framework (Hook→Frame→HowTo→Switch→Offer)
  
  WORKFLOW:
  1. Listen for CONTENT_REQUEST
  2. Create content brief with OTO hooks
  3. Assign tasks to Copywriters, Designer, Video
  4. Monitor progress
  5. Emit CONTENT_READY when all assets complete
  
  NO SHIFT - Work immediately when triggered"
```

### 2. Copywriter 1 - Short Form
```
sessions_spawn --mode=session --label=MT-Copy-Short \
  --task="Copywriter (Short-form) - PIPELINE 1.
  
  TRIGGER: Content brief ready
  INPUT: Content brief from Strategist
  OUTPUT: 5-10 short-form copies (social posts, ads, hooks)
  STYLE: STS Framework - Hook, Frame, Switch, Offer
  
  Emit COPY_READY event when complete"
```

### 3. Copywriter 2 - Long Form
```
sessions_spawn --mode=session --label=MT-Copy-Long \
  --task="Copywriter (Long-form) - PIPELINE 1.
  
  TRIGGER: Content brief ready
  INPUT: Content brief from Strategist
  OUTPUT: 3-5 long-form pieces (articles, emails, scripts)
  STYLE: STS Framework with storytelling
  
  Emit COPY_READY event when complete"
```

### 4. Visual Designer
```
sessions_spawn --mode=session --label=MT-Designer \
  --task="Visual Designer - PIPELINE 1.
  
  TRIGGER: Copy ready OR direct brief
  INPUT: Copy text OR content brief
  OUTPUT: 10-15 visual assets (graphics, banners, thumbnails)
  STYLE: OTO visual hooks, brand consistent
  
  Emit DESIGN_READY event when complete"
```

### 5. Video Creator
```
sessions_spawn --mode=session --label=MT-Video \
  --task="Video Creator - PIPELINE 1.
  
  TRIGGER: Script ready from Copywriter
  INPUT: Video script with STS structure
  OUTPUT: 3-5 videos (Reels, TikTok, Shorts)
  STYLE: Hook in first 3 seconds, CTA at end
  
  Emit VIDEO_READY event → Content Strategist emits CONTENT_READY"
```

---

## PIPELINE 2: Campaign Launch (9 agents)

ทำงานเมื่อ: `CONTENT_READY` event  
ส่งต่อ: `CAMPAIGN_LIVE` event → Pipeline 3

### 6. Product Analyst 1
```
sessions_spawn --mode=session --label=MT-Product-1 \
  --task="Product Analyst 1 - PIPELINE 2.
  
  TRIGGER: CONTENT_READY (async check)
  TASK: Market trend analysis, competitor monitoring
  OUTPUT: Market status report
  
  Run in parallel with other Pipeline 2 agents"
```

### 7. Product Analyst 2
```
sessions_spawn --mode=session --label=MT-Product-2 \
  --task="Product Analyst 2 - PIPELINE 2.
  
  TRIGGER: CONTENT_READY (async check)
  TASK: USP validation, product story alignment
  OUTPUT: Product story confirmation
  
  Run in parallel"
```

### 8. Price Strategist
```
sessions_spawn --mode=session --label=MT-Price \
  --task="Price Strategist - PIPELINE 2.
  
  TRIGGER: CONTENT_READY + Product story
  TASK: OTO pricing design, urgency/scarcity setup
  OUTPUT: Pricing strategy + offer structure
  
  FRAMEWORK: OTO (Urgency, Scarcity, Value Stack, Risk Reversal)"
```

### 9. Online Channel Manager
```
sessions_spawn --mode=session --label=MT-Place-Online \
  --task="Online Channel Manager - PIPELINE 2.
  
  TRIGGER: CAMPAIGN_LIVE imminent
  TASK: Platform preparation, pixel setup, tracking ready
  OUTPUT: Channel ready confirmation
  
  Platforms: Facebook, Instagram, TikTok, Website"
```

### 10. Offline & Partnership
```
sessions_spawn --mode=session --label=MT-Place-Offline \
  --task="Offline & Partnership - PIPELINE 2.
  
  TRIGGER: CONTENT_READY (if offline component)
  TASK: Retail partner coordination, in-store activation
  OUTPUT: Partner status
  
  Async - may not block campaign launch"
```

### 11. Campaign Manager
```
sessions_spawn --mode=session --label=MT-Campaign \
  --task="Campaign Manager - PIPELINE 2.
  
  TRIGGER: CONTENT_READY + Price ready
  TASK: Campaign build, scheduling, coordination
  COORDINATE: Influencer, Events, Ad Specialist
  OUTPUT: Campaign plan ready
  
  Critical path - must complete before launch"
```

### 12. Influencer Manager
```
sessions_spawn --mode=session --label=MT-Influencer \
  --task="Influencer Manager - PIPELINE 2.
  
  TRIGGER: Campaign plan ready
  TASK: KOL coordination, content approval, posting schedule
  OUTPUT: Influencer content calendar
  
  Run parallel with Events and Ads"
```

### 13. Events Coordinator
```
sessions_spawn --mode=session --label=MT-Events \
  --task="Events Coordinator - PIPELINE 2.
  
  TRIGGER: Campaign plan ready
  TASK: Event activation, live coordination, PR
  OUTPUT: Event status
  
  Async - may run parallel"
```

### 14. Ad Specialist ⭐ (Key Agent)
```
sessions_spawn --mode=session --label=MT-AdSpecialist \
  --task="AD SPECIALIST - PIPELINE 2 (CRITICAL).
  
  TRIGGER: All dependencies ready (Content + Price + Channel)
  TASK: Launch paid ads on Facebook, Google, TikTok
  
  CONTINUOUS AFTER LAUNCH:
  - Monitor ROAS every second
  - Auto-pause if ROAS < 2.0
  - Scale winners immediately
  - A/B test creatives continuously
  
  OUTPUT: CAMPAIGN_LIVE event + continuous optimization
  
  ALERT: PM immediately if account issues or ROAS crash"
```

---

## PIPELINE 3: Optimize & Learn (6 agents)

ทำงานเมื่อ: `CAMPAIGN_LIVE` event  
ส่งต่อ: `INSIGHTS_READY` event → Loop back to Pipeline 1

### 15. Marketing Analyst
```
sessions_spawn --mode=session --label=MT-Analyst \
  --task="Marketing Analyst - PIPELINE 3.
  
  TRIGGER: CAMPAIGN_LIVE (continuous)
  TASK: Real-time data analysis, pattern recognition
  INPUT: Campaign performance, conversion data
  OUTPUT: Insights + recommendations
  
  CONTINUOUS: Update insights every 5 minutes
  Emit INSIGHTS_READY when significant pattern found"
```

### 16. Performance Tracker
```
sessions_spawn --mode=session --label=MT-Tracker \
  --task="Performance Tracker - PIPELINE 3.
  
  TRIGGER: CAMPAIGN_LIVE (continuous)
  TASK: KPI monitoring, threshold checking
  METRICS: ROAS, CTR, CPC, Conversion, Engagement
  
  ALERT TRIGGERS:
  - ROAS < 2.0 → Alert Ad Specialist (HIGH)
  - ROAS < 1.5 → Escalate to PM (CRITICAL)
  - CTR < 1% → Flag for creative refresh (MEDIUM)
  - Conversion < 1% → Alert Campaign Manager
  
  CONTINUOUS: Real-time dashboard updates"
```

### 17. Community Manager 1
```
sessions_spawn --mode=session --label=MT-Community-1 \
  --task="Community Manager 1 - PIPELINE 3.
  
  TRIGGER: CAMPAIGN_LIVE (continuous)
  TASK: Social monitoring, comment response
  PLATFORM: Facebook, Instagram, TikTok comments
  
  RESPONSE TIME: < 15 minutes for all mentions
  ALERT: Negative sentiment spike → Escalate"
```

### 18. Community Manager 2
```
sessions_spawn --mode=session --label=MT-Community-2 \
  --task="Community Manager 2 - PIPELINE 3.
  
  TRIGGER: CAMPAIGN_LIVE (continuous)
  TASK: VIP customer care, advocate identification
  FOCUS: High-value customers, brand advocates
  
  OUTPUT: VIP insights + advocacy opportunities"
```

### 19. Customer Success
```
sessions_spawn --mode=session --label=MT-CS \
  --task="Customer Success - PIPELINE 3.
  
  TRIGGER: CAMPAIGN_LIVE + Purchase events
  TASK: Post-purchase follow-up, retention, repurchase
  INPUT: Purchase data, customer feedback
  OUTPUT: CS metrics + retention campaigns
  
  GOAL: Increase repurchase rate ≥ 30%"
```

### 20. Advocacy Coordinator
```
sessions_spawn --mode=session --label=MT-Advocacy \
  --task="Advocacy Coordinator - PIPELINE 3.
  
  TRIGGER: CAMPAIGN_LIVE (continuous)
  TASK: Referral program, UGC coordination
  FOCUS: Turn customers into advocates
  
  OUTPUT: Referral metrics + shareable content ideas
  LOOP: Best UGC → Suggest to Content Strategist"
```

---

## ORCHESTRATION LAYER (2 agents)

### 21. Orchestrator (Event Router)
```
sessions_spawn --mode=session --label=MT-Orchestrator \
  --task="ORCHESTRATOR - EVENT ROUTER (24/7).
  
  PRIMARY FUNCTION: Route events between pipelines
  
  EVENT MAP:
  ON content_request:
    → Trigger PIPELINE 1 (Content)
    
  ON content_ready:
    → Trigger PIPELINE 2 (Campaign)
    → Notify Campaign Manager, Ad Specialist
    
  ON campaign_live:
    → Trigger PIPELINE 3 (Optimize)
    → Start all monitoring agents
    
  ON insights_ready:
    → Route to Content Strategist
    → May trigger new content_request
    
  ON roas_drop:
    → Alert Ad Specialist
    → If < 1.5, escalate to PM
    
  ON viral_mention:
    → Alert Community Managers
    → Prepare crisis response
    
  ON ad_account_banned:
    → EMERGENCY: Alert PM + CEO immediately
    
  CONTINUOUS: Listen all events, route immediately"
```

### 22. Project Manager (Strategic Oversight)
```
sessions_spawn --mode=session --label=MT-Manager \
  --task="PROJECT MANAGER - STRATEGIC OVERSIGHT.
  
  TRIGGER: Escalation events only
  
  ESCALATION CONDITIONS:
  - ROAS < 1.5 (CRITICAL)
  - Ad account banned (CRITICAL)
  - Viral negative sentiment (CRITICAL)
  - Website down (CRITICAL)
  - Budget exceed 120% (HIGH)
  
  DECISION AUTHORITY:
  - Approve major strategy shifts
  - Override agent decisions if needed
  - Resource reallocation
  
  REPORTING: Daily summary to CEO (พี่แบร์)
  
  NOT TRIGGERED BY: Normal operations (agents handle)"
```

---

## Event Definitions (JSON Schema)

### CONTENT_REQUEST
```json
{
  "event": "CONTENT_REQUEST",
  "timestamp": "ISO8601",
  "source": "Insight loop OR manual",
  "data": {
    "objective": "awareness|conversion|retention",
    "target_audience": "...",
    "oto_required": true,
    "urgency": "high|medium|low"
  }
}
```

### CONTENT_READY
```json
{
  "event": "CONTENT_READY",
  "timestamp": "ISO8601",
  "source": "MT-Content-Strategy",
  "data": {
    "content_id": "...",
    "assets": ["copy", "designs", "videos"],
    "oto_hooks": true,
    "sts_structure": "Hook→Frame→HowTo→Switch→Offer"
  }
}
```

### CAMPAIGN_LIVE
```json
{
  "event": "CAMPAIGN_LIVE",
  "timestamp": "ISO8601",
  "source": "MT-AdSpecialist",
  "data": {
    "campaign_id": "...",
    "channels": ["facebook", "google", "tiktok"],
    "budget": 0,
    "targeting": "..."
  }
}
```

### INSIGHTS_READY
```json
{
  "event": "INSIGHTS_READY",
  "timestamp": "ISO8601",
  "source": "MT-Analyst",
  "data": {
    "roas": 0,
    "ctr": 0,
    "conversions": 0,
    "recommendations": ["..."]
  }
}
```

---

## Continuous Monitoring Setup

```python
# No cron jobs - pure event-driven

# Ad Performance Monitor (Real-time)
while True:
    roas = get_current_roas()
    if roas < 2.0:
        emit_event("ROAS_DROP", {"value": roas})
    sleep(1)  # Check every second

# Community Monitor (Real-time)
stream = connect_social_apis()
for mention in stream:
    if mention.sentiment < -0.5:
        emit_event("NEGATIVE_MENTION", mention)
    if mention.viral_score > 100:
        emit_event("VIRAL_MENTION", mention)

# Website Monitor (Every 5 seconds)
while True:
    if not ping_website():
        emit_event("WEBSITE_DOWN", {})
    sleep(5)
```

---

## Launch Sequence (AI-Optimized)

### Step 1: Spawn Orchestration First
```
1. MT-Orchestrator (Event router)
2. MT-Manager (Strategic oversight)
```

### Step 2: Spawn All Pipeline Agents
```
Spawn พร้อมกันทั้งหมด 20 ตัวที่เหลือ
ทุกตัวรอ event อยู่
```

### Step 3: Trigger First Content Request
```
sessions_send MT-Orchestrator "EMIT: CONTENT_REQUEST"
→ Pipeline 1 ทำงาน
→ Pipeline 2 ทำงาน (เมื่อ content ready)
→ Pipeline 3 ทำงาน (เมื่อ campaign live)
→ Loop back
```

---

## Summary: 22 Agents by Pipeline

| Pipeline | Agents | Mode |
|----------|--------|------|
| **Content** | 5 | Event-triggered |
| **Campaign** | 9 | Event-triggered |
| **Optimize** | 6 | Continuous monitoring |
| **Orchestrate** | 2 | Always-on router |

**No shifts. No handover. Pure event-driven.**

---

**Ready for AI-Optimized deployment!** 🐻
