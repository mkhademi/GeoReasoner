# GeoReasoner

## Grounded Multimodal Reasoning for Business Profile Freshness Verification

GeoReasoner is a multimodal reasoning system that combines retrieval, grounding, visual evidence generation, verification, and agentic reasoning to determine whether newly arriving visual evidence supports, contradicts, or is insufficient to validate existing business profile metadata.

The project is inspired by large-scale mapping systems where business profiles must remain accurate despite continuously changing real-world conditions. New imagery may indicate that a business remains unchanged, has been remodeled, changed category, or is no longer active. GeoReasoner explores how multimodal reasoning can help maintain business profile freshness using visual evidence.

---

## Problem Statement

Business profiles become stale over time.

Businesses may:

- Change categories
- Remodel their storefronts
- Update branding
- Change offerings
- Close permanently

Meanwhile, new visual evidence continuously arrives through:

- User-generated imagery
- Street-level imagery
- Historical business imagery

The challenge is determining whether newly arriving imagery remains consistent with existing business profile metadata.

GeoReasoner addresses this problem using a grounded multimodal reasoning pipeline.

---

## System Architecture

Current Business Profile Metadata
+ New Visual Evidence
→ Retrieval
→ Grounding
→ Evidence Generation
→ Structured Reasoning
→ Evidence Aggregation
→ Confidence Calibration
→ Verification
→ Final Assessment

Verification outputs:

- SUPPORTED
- CONTRADICTED
- INSUFFICIENT_EVIDENCE

---

## Pipeline Components

### Phase 0 — Foundation Models

Models used throughout the pipeline:

- CLIP for multimodal retrieval and grounding
- BLIP for visual evidence generation
- Qwen2.5-Instruct for verification experiments

---

### Phase 1 — Retrieval

Retrieves business-relevant imagery using a shared vision-language embedding space.

**Input**
- User query

**Output**
- Top-K relevant images

---

### Phase 2 — Metadata Grounding

Measures consistency between visual evidence and business metadata.

**Input**
- Image
- Business metadata

**Output**
- Consistency score

---

### Phase 3 — Visual Evidence Generation

Generates interpretable visual descriptions using image captioning.

**Input**
- Image

**Output**
- Natural language evidence

Example:

> a bicycle parked in front of a store

---

### Phase 4 — Structured Reasoning

Combines metadata, visual evidence, and grounding signals into human-readable explanations.

**Output**
- Assessment
- Explanation

---

### Phase 5 — Evidence Aggregation

Combines multiple evidence sources into a unified assessment.

Signals include:

- Metadata
- Caption evidence
- Grounding score

---

### Phase 6 — Confidence Calibration

Adjusts confidence based on evidence quality rather than similarity scores alone.

This prevents overconfident reasoning when visual evidence is weak.

---

### Phase 7 — Verification

GeoReasoner explores three verification strategies.

#### Rule-Based Verification

Uses explicit evidence keywords and grounding signals.

**Strengths**
- Interpretable
- Deterministic

**Weaknesses**
- Conservative
- Limited semantic understanding

#### LLM-Based Verification

Uses a language model to evaluate metadata claims using visual evidence.

**Strengths**
- Better semantic reasoning

**Weaknesses**
- Susceptible to hallucination

Observed failure modes:

- Claim-conditioned hallucination
- Evidence injection
- Semantic overreach

#### Evidence-First Verification

Separates evidence extraction from claim evaluation.

Architecture:

Caption
→ Evidence Extraction
→ Structured Evidence
→ Verification

This improves transparency and reduces direct exposure to metadata claims during evidence generation.

---

### Phase 8 — Evaluation

A qualitative evaluation was conducted using representative business-related imagery.

Evaluation dimensions:

- Metadata grounding
- Rule-based verification
- LLM-based verification
- Failure analysis

Key findings:

- Rule-based verification achieved high precision but low recall.
- LLM verification improved semantic reasoning but introduced hallucination failure modes.
- Evidence-first verification improved transparency but did not completely eliminate hallucination.

---

### Phase 9 — Agentic Freshness Verification

GeoReasoner extends verification with agentic evidence collection.

Rather than terminating when evidence is insufficient, the system actively gathers additional evidence and re-evaluates the claim.

Workflow:

Current Business Profile
+ New Image
→ Verification

SUPPORTED
→ Profile Valid
→ Stop

CONTRADICTED
→ Potential Profile Drift
→ Flag For Review
→ Stop

INSUFFICIENT_EVIDENCE
→ Retrieve Additional Evidence
→ Re-Verify

This enables iterative evidence gathering when individual images provide incomplete information.

---

## Example Agent Behavior

Business Profile:

> coffee shop

Evidence 1:

> a bicycle parked in front of a store

Verification:

> INSUFFICIENT_EVIDENCE

Agent Action:

> Retrieve additional evidence

Evidence 2:

> two people holding cups of coffee in their hands

Verification:

> SUPPORTED

Final Decision:

> Profile remains valid

---

## Key Findings

### Rule-Based Verification

**Pros**
- Interpretable
- Deterministic
- Resistant to hallucination

**Cons**
- Conservative
- High false-negative rate

### LLM-Based Verification

**Pros**
- Strong semantic understanding

**Cons**
- Claim-conditioned hallucination
- Unsupported evidence attribution
- Semantic overreach

### Agentic Verification

**Pros**
- Handles incomplete evidence
- Improves decision quality through iterative evidence collection

---

## Future Work

Potential extensions include:

- Reflective verification agents
- Multi-agent verification architectures
- Evidence citation and attribution
- Chain-of-verification prompting
- Automated retrieval of additional evidence
- Business profile drift detection
- Large-scale evaluation on real-world imagery datasets

---

## Repository Structure

```text
GeoReasoner/
│
├── README.md
├── GeoReasoner.ipynb
├── data/
│   └── sample_images/
├── images/
├── requirements.txt
└── LICENSE
```

---

## How To Run

1. Clone the repository.

2. Install dependencies.

```bash
pip install -r requirements.txt
```

3. Open the notebook.

```bash
jupyter notebook GeoReasoner.ipynb
```

4. Run cells sequentially from Phase 0 through Phase 9.

---

## Technologies

- Python
- PyTorch
- Transformers
- CLIP
- BLIP
- Qwen2.5
- Hugging Face
- Google Colab

---

## License

MIT License
