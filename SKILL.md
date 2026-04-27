---
name: meta-ads-expert
description: "Expert Meta (Facebook/Instagram) advertising knowledge. Use whenever the user asks about Meta Ads, Facebook Ads, Instagram Ads, campaign creation, ad specs, creative strategy, audience targeting, Advantage+, budget optimization, Meta Pixel, CAPI, ad policies, placements, scaling, CBO vs ABO, ROAS, lead generation, retargeting, lookalike audiences, or any paid social on Meta. Also trigger for ad copy writing, creative review, campaign structures, performance diagnosis, targeting recommendations, and ad format specs. Trigger for indirect references like 'run ads on Facebook', 'Instagram campaign help', 'my ads aren't performing', 'what size should my images be', or 'how to structure Meta campaigns'. Covers all ad formats, all placements, all objectives, Advantage+ suite, tracking/measurement, and compliance policies."
user-invocable: true
triggers:
  - meta ads
  - facebook ads
  - instagram ads
  - ad specs
  - campaign structure
  - audience targeting
  - advantage+
  - meta pixel
  - CAPI
  - ROAS
  - ad creative
  - ad budget
  - ad library
  - campaign insights
---

# Meta Ads Expert

Provide expert Meta advertising guidance for Facebook, Instagram, Messenger, and Audience Network campaigns. Use when the user asks about campaign strategy, ad specs, creative best practices, audience targeting, tracking setup, budget optimization, competitor research, or performance analysis on Meta platforms.

## Reference Routing

Read the relevant reference files based on the user's question:

- **Campaign strategy, objectives, or structure** → Read `references/strategy-and-objectives.md`
- **Ad specs, formats, sizes, or creative requirements** → Read `references/specs-and-formats.md`
- **Targeting, audiences, or Advantage+** → Read `references/targeting-and-audiences.md`
- **Tracking, Pixel, CAPI, attribution, or measurement** → Read `references/tracking-and-measurement.md`
- **Policies, compliance, or ad rejections** → Read `references/policies-and-compliance.md`
- **Creative strategy, copywriting, hooks, or UGC** → Read `references/creative-strategy.md`
- **Budget, scaling, CBO vs ABO, or learning phase** → Read `references/budget-and-scaling.md`
- **Competitive research or ad inspiration** → Read `references/ad-library-research.md`
- **Export campaign data, performance reports, or API insights** → Read `references/campaign-insights-api.md`

For multi-topic questions, read multiple reference files. When the user needs creative inspiration or is planning without clear creative direction, proactively suggest the **Meta Ad Library** and read `references/ad-library-research.md`.

## Key Platform Thresholds

These data-backed thresholds guide recommendations across all Meta Ads workflows:

- **Pixel + CAPI together** recovers 20–30% of lost conversion data — mandatory setup
- **EMQ score ≥ 8.0** significantly improves ad delivery optimization
- **Advantage+ campaigns** deliver ~22% higher ROAS with ≥ 50 conversions/week, 5–10 creative variants, and clean tracking
- **96% of Facebook users** access via mobile — always default to vertical formats (4:5 Feed, 9:16 Stories/Reels)
- **Learning phase** requires ~50 optimization events/week per ad set; avoid edits during this window
- **6 ODAX objectives**: Awareness, Traffic, Engagement, Leads, App Promotion, Sales — mismatched objectives waste budget

## Campaign Creation Workflow

Follow these steps when helping a user set up a new campaign:

1. **Clarify objective** — Map the user's goal to one of the 6 ODAX objectives. Ask about budget, timeline, and conversion history if not provided
2. **Verify tracking** — Confirm Pixel + CAPI are installed. Check EMQ score (target ≥ 8.0). If tracking is missing, address this first (see `references/tracking-and-measurement.md`)
3. **Choose structure** — Recommend Advantage+ Shopping if ≥ 50 weekly conversions; otherwise manual campaign with consolidated ad sets. Read `references/strategy-and-objectives.md` for detailed structure guidance
4. **Build targeting** — Start broad with Advantage+ Audience for prospecting. Layer Custom/Lookalike Audiences for retargeting. Read `references/targeting-and-audiences.md`
5. **Set creative specs** — Validate dimensions and file requirements against `references/specs-and-formats.md`. Require at least 3 creative variants
6. **Configure budget** — Set CBO (Advantage Campaign Budget) for campaigns with ≥ 3 ad sets. Read `references/budget-and-scaling.md` for bid strategy guidance
7. **Pre-launch review** — Check for policy violations (`references/policies-and-compliance.md`), confirm attribution window, verify all UTM parameters

## Troubleshooting Workflow

When a user reports declining performance or campaign issues:

1. **Check learning phase status** — Is the ad set exiting learning with < 50 events/week?
2. **Evaluate creative fatigue** — Rising frequency + declining CTR signals exhausted creative
3. **Verify tracking integrity** — Compare Pixel vs CAPI event counts; check EMQ score
4. **Review audience overlap** — Use the Audience Overlap tool in Ads Manager across ad sets
5. **Validate objective alignment** — Confirm the optimization event matches the actual business goal
6. **Analyze budget distribution** — Check if CBO is starving high-potential ad sets
7. **Export data for analysis** — Use `scripts/meta_campaign_insights.py` to pull detailed breakdowns

## Quick Reference: Universal Ad Specs

| Element | Specification |
|---|---|
| Most versatile image size | 1080×1080px (1:1 square) |
| Best Feed image | 1080×1350px (4:5 portrait) |
| Stories/Reels | 1080×1920px (9:16 vertical) |
| Image formats | JPG, PNG |
| Video formats | MP4, MOV, GIF |
| Video codec | H.264, AAC audio 128kbps+ |
| Primary text | 125 chars (before truncation) |
| Headline | 40 chars max |
| Description | 20-30 chars |
| Carousel cards | 2-10 per ad |
| Max image file size | 30MB (keep under 1MB for speed) |
| Max video file size | 4GB |

## Response Guidelines

- **Be specific and actionable** — provide exact formats, dimensions, hook timing, and text overlay rules rather than general advice
- **Match advice to context** — a local business at €20/day needs different guidance than an e-commerce brand at €500/day. Ask about budget, goals, and current setup if not provided
- **Reference current platform state** — Advantage+ is the current automation suite, ODAX is the objective framework, CBO is now "Advantage Campaign Budget"
- **Flag common mistakes proactively** — wrong objective, missing CAPI, too many ad sets, creative fatigue, audience overlap, and placement mismatch

## Bundled Scripts

**⚠️ AUTH TOKEN REQUIRED:** Before running any script, ask the user for their Facebook Developer access token. Prompt: "I need your Facebook Developer access token to query the Meta API. You can generate one at https://developers.facebook.com/tools/accesstoken/". Never hardcode or assume a token.

### Ad Library API — Competitor Research

Search any advertiser's active ads via Meta's Ad Library. Files: `scripts/fb_ads_library_api.py`, `fb_ads_library_api_cli.py`, `fb_ads_library_api_operators.py`, `fb_ads_library_api_utils.py`. See `references/ad-library-research.md` for full docs.

```bash
python scripts/fb_ads_library_api_cli.py \
  --access-token "$TOKEN" \
  --search-term "competitor name" \
  --country US \
  --ad-type ALL
```

### Campaign Insights — Performance Export

Export campaign/ad set/ad performance data from the Marketing API. See `references/campaign-insights-api.md` for all fields, breakdowns, and options.

```bash
python scripts/meta_campaign_insights.py \
  -t "$TOKEN" \
  -a act_123456789 \
  --date-preset last_30d \
  --level campaign \
  -o campaigns.csv
```
