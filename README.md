# Prompt Creator Skill

A Claude Code Skill for quickly generating high-quality, structured Prompts. When you describe a prompt creation request, the Skill automatically analyzes the completeness of your requirements, asks follow-up questions to fill in missing information, dynamically selects appropriate framework elements, and generates N different versions of customized prompts for you to choose from.

 [中文版本](./README_zh.md)|**English Version**

---

## Prompt Core Frameworks

Six built-in prompt frameworks serve as the element source for dynamic synthesis. Frameworks are static, but synthesis is dynamic — elements are freely combined based on the scenario.

---

### 1. RTF — Role + Task + Format

**Structure:**

```
┌─────────┐     ┌─────────┐     ┌─────────┐
│  Role   │ ──▶ │  Task   │ ──▶ │ Format  │
└─────────┘     └─────────┘     └─────────┘
```

**Elements:**

| Element | Meaning |
|---------|---------|
| Role | Defines the AI's identity or professional background |
| Task | The specific goal or task to complete |
| Format | Output format, style, or tone requirements |

**Summary:** Three elements in a straight line串联，最简洁的框架。 The simplest framework.

**Best for:** General tasks, simple tasks, quick one-off requests.

---

### 2. ICIO — Instruction + Context + Input + Output

**Structure:**
```
┌────────────┐   ┌──────────┐   ┌────────┐   ┌────────┐
│Instruction │──▶│ Context  │──▶│ Input  │──▶│ Output │
└────────────┘   └──────────┘   └────────┘   └────────┘
```

**Elements:**
| Element | Meaning |
|---------|---------|
| Instruction | Core task directive |
| Context | Background information and supporting context |
| Input | Text or data to process |
| Output | Format or expression requirements for results |

**Summary:** Four-element linear flow with continuous context, suitable for multi-stage reasoning.

**Best for:** Tasks requiring context understanding, analysis with background information, content creation.

---

### 3. CRISPE — Capacity + Role + Insight + Statement + Personality + Experiment

**Structure:**
```
┌──────────┐  ┌────────┐  ┌────────┐  ┌─────────┐  ┌───────────┐  ┌───────────┐
│ Capacity │─▶│  Role  │─▶│ Insight│─▶│Statement│─▶│Personality│─▶│Experiment│
└──────────┘  └────────┘  └────────┘  └─────────┘  └───────────┘  └───────────┘
```

**Elements:**
| Element | Meaning |
|---------|---------|
| Capacity | Defines the AI's capability boundaries |
| Role | Identity positioning |
| Insight | Knowledge perspective or insight points |
| Statement | Specific task objectives |
| Personality | Personalized tone and style |
| Experiment | Exploratory requirements (hypotheses, multiple answers, etc.) |

**Summary:** Six elements in series, emphasizing "personality" and "explorability," suitable for deep creation.

**Best for:** Academic/research, content creation, in-depth analysis reports, creative writing.

---

### 4. CO-STAR — Context + Objective + Scope + Tone + Audience + Response

**Structure:**
```
         ┌─────────┐
         │ Context │
         └────┬────┘
              │
              ▼
┌────────┐  ┌──────────┐  ┌────────┐
│ Scope  │─▶│ Objective│─▶│  Tone  │
└────────┘  └──────────┘  └───┬────┘
                             │
                             ▼
                    ┌──────────┐  ┌──────────┐
                    │ Audience │─▶│ Response │
                    └──────────┘  └──────────┘
```

**Elements:**
| Element | Meaning |
|---------|---------|
| Context | Scenario and background related to the task |
| Objective | The specific result or goal to achieve |
| Scope | Task boundaries and constraints |
| Tone | Output style (formal/humorous/professional/casual) |
| Audience | Target readers or recipients |
| Response | Output format or structure requirements |

**Summary:** Six-element network structure with Context as the foundation connecting the rest, emphasizing style and audience.

**Best for:** Complex structured output tasks, marketing content, youth education.

---

### 5. TIDD-EC — Task + Instructions + Do + Don't + Example + Content

**Structure:**
```
┌────────┐   ┌────────────┐   ┌─────┐   ┌────────┐
│  Task  │──▶│ Instructions│──▶│ Do  │   │Don't   │
└────────┘   └────────────┘   └─────┘   └────────┘
                  │                              │
                  ▼                              │
             ┌─────────┐                         │
             │ Example │◀────────────────────────┘
             └────┬────┘
                  │
                  ▼
            ┌──────────┐
            │ Content  │
            └──────────┘
```

**Elements:**
| Element | Meaning |
|---------|---------|
| Task | Nature and goal of the task |
| Instructions | Specific steps to execute the task |
| Do | Actions that should be performed |
| Don't | Errors or improper behaviors to avoid |
| Example | Example of expected output |
| Content | Background information provided by the user |

**Summary:** Bidirectional constraint from instructions and boundaries, with Example as reference. Suitable for tasks requiring clear guidance boundaries.

**Best for:** Education/training, legal consulting, technical support, normative content generation.

---

### 6. BROKE — Background + Role + Objective + Key Result + Evolution

**Structure:**
```
┌───────────┐  ┌────────┐  ┌──────────┐  ┌────────────┐  ┌─────────┐
│ Background│─▶│  Role  │─▶│ Objective│─▶│ Key Result │─▶│Evolution│
└───────────┘  └────────┘  └──────────┘  └────────────┘  └─────────┘
     │                                                   ▲
     └───────────────────────────────────────────────────┘
                        Feedback Loop
```

**Elements:**
| Element | Meaning |
|---------|---------|
| Background | Background information or context |
| Role | The role the model should play |
| Objective | Specific goal or expected result |
| Key Result | Criteria or metrics for measuring success |
| Evolution | Improvement suggestions or follow-up steps |

**Summary:** Five elements in linear sequence with a feedback loop. Goal-oriented and iteratively improvable.

**Best for:** Event planning, product design, goal-oriented tasks, projects requiring continuous improvement.

---

### Framework Comparison Overview

| Framework | Elements | Core Feature | Complexity |
|-----------|----------|--------------|------------|
| RTF | 3 | Simple & Direct | ⭐ |
| ICIO | 4 | Linear Flow | ⭐⭐ |
| CO-STAR | 6 | Network Structure | ⭐⭐⭐ |
| CRISPE | 6 | Personality + Exploration | ⭐⭐⭐ |
| TIDD-EC | 6 | Boundary Constraints | ⭐⭐⭐ |
| BROKE | 5 | Goal + Feedback | ⭐⭐⭐ |

**Dynamic Synthesis Examples:**
- Simple translation task → RTF (Role + Task + Format)
- Analysis report with context → ICIO + CO-STAR (adds Tone + Audience)
- Educational content needing boundaries → TIDD-EC (adds Do/Don't)
- Goal-oriented event planning → BROKE + CO-STAR (adds Tone)

---

## Design Principles

### Core Philosophy: Frameworks are References, Not Shackles

The existing prompt frameworks (RTF, ICIO, CRISPE, CO-STAR, TIDD-EC, BROKE) are all static templates — they are tools, not dogma. The real power lies in **dynamically combining elements based on the scenario**.

**Wrong approach:**

> "I need to write a prompt using the CO-STAR framework because it's the best framework."

This puts the cart before the horse. The value of a framework lies in its **core elements**, not the framework name itself.

**Right approach:**

> "I need an AI to act as a senior technical interviewer, evaluate candidates' system design skills, output a structured interview evaluation form, with a professional yet approachable tone."

Then extract the required elements from multiple frameworks:

- **Role** (RTF/CRISPE) → "Senior Technical Interviewer"
- **Context + Objective** (ICIO/CO-STAR) → System design skill evaluation
- **Output Format** (RTF/CO-STAR) → Structured interview evaluation form
- **Tone** (CO-STAR/CRISPE) → Professional yet approachable

The final synthesized architecture may not belong to any single framework, but it **fits the current scenario best**.

---

### Three Core Design Principles

| Principle | Description |
|-----------|-------------|
| **Scenario First** | Understand the requirement first, then choose a framework. Frameworks serve requirements, not the other way around. |
| **Element Combination** | Extract the most relevant elements from different frameworks and dynamically combine them like building blocks. |
| **Transparent Delivery** | Every prompt includes framework source notes, helping users understand why it was designed this way. |

---

### How Design Principles Manifest in Practice

| Practice | Description |
|----------|-------------|
| **Inquiry Strategy** | When information is insufficient, first fill in key elements instead of forcibly applying a framework. |
| **Diversity Guarantee** | When generating multiple versions, use different framework element combinations to avoid homogenization. |
| **Reject Rigidity** | There is no说法 of "must use which framework" — every prompt is a customized product. |
| **User Comprehension** | Label framework sources in output, helping users learn framework elements rather than memorizing framework names. |



## Features

| Feature | Description |
|---------|-------------|
| **Requirement Analysis** | Automatically extract key elements from your description: task type, target audience, output format, style/tone, etc. |
| **Smart Inquiry** | When key information is missing, ask one question at a time, up to 3 questions maximum. |
| **Dynamic Framework Synthesis** | Select relevant elements from the 6 frameworks based on the scenario and combine them into the most suitable architecture. |
| **Multi-Version Generation** | Generate N different versions of prompts, ensuring framework diversity and wording differences. |
| **Framework Labeling** | Each prompt includes framework source notes. |



## Advantages

| Advantage | Description |
|-----------|-------------|
| **Multi-Framework Fusion** | 6 built-in prompt frameworks with freely combinable elements. |
| **Multi-Version Output** | Generate N different versions at once for easy comparison and selection. |
| **Smart Inquiry** | Ask follow-up questions when information is insufficient, up to 3 questions. |
| **Ready to Use** | Output can be directly copied for use. |
| **Configurable** | Generation count and inquiry limits are customizable. |
| **Zero Learning Curve** | Just describe your requirement — the Skill handles the rest. |



## Installation

Copy the skill folder to your Claude Code skills directory:

```bash
# Clone or copy the skill to the appropriate directory
cp -r prompt-creator-skill ~/.claude/skills/prompt-creator-skill
```

Optional: Create a config file at `~/.prompt-creator/config.json`:

```json
{
  "defaultPromptCount": 3,
  "maxQuestions": 3
}
```

| Config Option | Default | Description |
|---------------|--------|-------------|
| `defaultPromptCount` | 3 | Default number of prompts to generate in normal mode |
| `maxQuestions` | 3 | Maximum follow-up questions when requirement information is insufficient |

---

## Interaction Flow

```
User describes requirement
    ↓
Skill analyzes requirement completeness
    ↓ [Insufficient info]
Ask 1-3 follow-up questions (one at a time)
    ↓ [Complete info]
Dynamic framework synthesis + Generate N different prompt versions
    ↓
Output directly to user
```

---

## Demo Examples

### 1. Simple Requirement — Missing Key Information

**User Input:**
> Help me write a translation prompt

**Skill Inquiry:**
> Who is the target audience? (e.g., business professionals, students, content creators)

---

### 2. Complete Requirement

**User Input:**
> Write a prompt for summarizing English articles into Chinese, target audience is academic researchers, output in concise paragraph form, with a professional and formal tone

**Skill Output:**
```
=== Prompt 1/3 ===
[prompt content]

> Framework Reference: RTF (Role+Task+Format) + CRISPE (Personality)
> Applicable Scenario: Quick academic English article summary

===

=== Prompt 2/3 ===
[prompt content]

> Framework Reference: ICIO (Instruction+Context+Output)
> Applicable Scenario: Academic summary requiring key term preservation

===

=== Prompt 3/3 ===
[prompt content]

> Framework Reference: CO-STAR (Context+Objective+Tone+Audience+Response)
> Applicable Scenario: Structured long-form summarization
```

---

### 3. Complex Requirement — Event Planning

**User Input:**
> Help me create a prompt for planning a company annual party, with clear goals and measurable results

**Skill Output:**
```
=== Prompt 1/3 ===
[prompt content]

> Framework Reference: BROKE (Background+Role+Objective+Key Result+Evolution)
> Applicable Scenario: Goal-oriented annual party planning

=== Prompt 2/3 ===
[prompt content]

> Framework Reference: BROKE + TIDD-EC (adds Do/Don't boundaries)
> Applicable Scenario: Planning tasks requiring clear execution boundaries

=== Prompt 3/3 ===
[prompt content]

> Framework Reference: ICIO + CO-STAR (adds Scope+Tone)
> Applicable Scenario: Comprehensive planning requiring multi-department collaboration
```

---

## Directory Structure

```
prompt-creator-skill/
├── SKILL.md                              # Skill main definition file
├── README.md                             # Project documentation (English)
├── README_zh.md                          # Project documentation (Chinese)
├── docs/
│   └── superpowers/
│       └── specs/
│           └── 2026-05-10-prompt-creator-skill-prd.md   # PRD specification document
└── references/
    ├── prompt-guide.md                   # Complete definitions of 6 prompt frameworks
    └── evaluation.md                     # Evaluation guide (future expansion)
```

---

## Constraints & Boundaries

### MVP Scope
- Generation only, no evaluation
- All generation completed within a single conversation
- Simplified Chinese interaction support

### Out of MVP Scope (Future Versions)
- Evaluation mode (multi-round iteration + evaluation reports)
- Multi-round conversational prompt optimization
- Prompt performance tracking
- English and other language support
- Prompt persistent storage and management