# Smart Cooking Assistant: Senior Design Project Pitch

**Team:** Luke (Electrical Engineering), Zayd (Bioengineering), Ethan (Computer Science), Phillip (Computer Science), Shishir (Artificial Intelligence)  
**Working concept:** A retrofit sensing module mounted near an existing range hood that watches the cooktop and offers timely, conversational cooking guidance.

## The pitch

Cooking instructions often depend on events rather than a clock: *wait until the water boils*, *stir before the sauce sticks*, *add the next ingredient when the onions soften*. A conventional recipe app cannot see those events. We propose a small overhead module combining a visible-light camera, an optional thermal sensor, and an assistant that remembers what has happened during the cooking session. The cook could say, “I’m making tomato sauce,” then receive a prompt when a simmer begins, have a timer start when an ingredient is added, or ask, “Does this look reduced enough?”

The central question is: **Can a non-contact system maintain enough situational awareness to give useful guidance during an ordinary cook without requiring a specially authored, sensor-aware recipe?**

Our senior design contribution is an integrated prototype and a measured answer to that question: robust mounting and sensing, temporal cooking-state estimation, grounded AI reasoning, an interface, and evaluation during real cooking sessions.

## Background and prior work

Cooksy is an especially relevant predecessor. Launched commercially in 2022, it placed a visible-light and thermal camera above the cooktop, recorded time and temperature, and provided app-based cooking guidance; its announced prices were \$299 and \$699 for the Pro model. [Cooksy launch announcement](https://www.prnewswire.com/news-releases/introducing-cooksy-the-ai-enabled-smart-kitchen-virtual-sous-chef-that-takes-the-guesswork-out-of-cooking-301493497.html)

Its published patent describes both **scripted sessions**, in which a cook follows digital recipe steps, and **freestyle sessions**, in which events and measurements can be logged to create a recipe. It also describes detecting actions such as stirring and adding food. We should credit those capabilities rather than suggest Cooksy was merely a timer or could only operate with a selected recipe. [Cooksy patent publication](https://patents.google.com/patent/US20210251263A1/en)

Cooksy's current website displayed an “Opening soon” page when checked for this proposal; that alone does not establish when or why its device ceased being marketed, nor does it prove the company failed. Our hypothesis is narrower: Cooksy was designed before consumer multimodal assistants became broadly available, and modern models may make **open-ended, context-aware dialogue about an ongoing cook** more practical. ChatGPT's image-understanding rollout, for example, was announced in September 2023, after Cooksy's launch. This is a technical opportunity to test, not an established explanation of Cooksy's commercial outcome. [Cooksy website](https://cooksy.com/) · [OpenAI image announcement](https://openai.com/index/chatgpt-can-now-see-hear-and-speak/)

The idea also has a difficult sensing constraint. An infrared camera measures emitted and reflected radiation; readings on reflective metal can be misleading. We will treat thermal data as a potentially useful trend, validate it on different cookware, and avoid treating a pan's apparent infrared temperature as food temperature. [FLIR thermal measurement guidance](https://docs.flir.com/T810603/en-US/latest/s07.html)

## Proposed experience and scope

The user names a dish or goal in plain language. The module tracks one or two visible vessels, detects a small set of cooking events, maintains a timestamped history, and offers a suggestion only when there is a reason to act. A companion web or mobile interface displays the camera view, current state, timers, and an event timeline. Spoken or typed questions let the cook correct the system: “I already added the pasta.”

**Core demonstration:** In a sauce or pasta workflow, detect a boil or simmer, recognize a coarse ingredient-addition event, start or suggest a timer, respond to a visual question using recent history, and issue an appropriately timed reminder. Demonstrate at least one unstructured session where the cook has not loaded a machine-readable recipe.

**Bounded first version:** One cooktop, fixed overhead camera, one active vessel, and two repeatable dishes (for example, pasta and tomato sauce). A second vessel, pan-frying, automatic ventilation recommendations, and richer ingredient recognition are extensions after the first workflow is reliable. The prototype attaches near a hood; it does **not** replace ventilation, control gas or heat, or alter mains wiring. It is an assistance prototype, not a certified fire or food-safety monitor.

## Technical approach

1. **Capture and calibration.** A heat-conscious, removable mount holds an RGB camera above the cooktop. If feasible, add a thermal array/camera with an aligned field of view. A local computer timestamps frames and sensor data. A one-time calibration maps image regions to burners and vessels.  
2. **Fast perception.** Lightweight local processing tracks vessels and extracts observable signals: bubbling/boiling, visible steam, color or texture changes, coarse ingredient additions, and stirring/motion. It publishes events with confidence and the image evidence used. Start with a few reliable detectors; use a multimodal model for ambiguous frames rather than promising universal recognition.  
3. **Session state.** An event log stores user intent, vessels, observations, inferred state, timers, and corrections. A state estimator uses multiple observations over time so that one occluded or blurry frame does not flip a state. Each claim carries a timestamp, source, and confidence; unknown remains a valid state.  
4. **Reasoning and intervention.** A multimodal model sees the user's goal, a compact state/history summary, and selected frames when queried or when a meaningful event occurs. It proposes guidance. A separate policy checks grounding, confidence, urgency, repetition, and whether the user has already acted before speaking. High-consequence claims require conservative wording or a request for confirmation.  
5. **Interface and feedback.** The app shows what the system believes and why, allows manual timer and state corrections, and records whether an alert was useful. Voice can be added once the visual and text loop works reliably; a local mute/privacy control should always be accessible.

RGB camera \+ optional thermal sensor

              ↓

Local capture and perception → timestamped events

              ↓

Cooking state and session history

              ↓

Selected frames \+ state \+ user goal → multimodal reasoning

              ↓

Intervention policy → app and optional voice

              ↑

         User corrections

**Data and privacy:** Process continuous video locally where practical. Send only selected images and compact state to any cloud model, disclose this clearly in the interface, and make session recording opt-in. Keep an offline path for sensor logging and basic timers if connectivity drops.

## Division of work

These are proposed ownership areas, with integration and testing shared across the team.

| Person | Primary ownership | Concrete deliverables |
| :---- | :---- | :---- |
| **Luke · EE** | Sensing hardware and electrical integration | Camera/thermal selection and bring-up; power and thermal considerations; removable mount with the team; timestamped sensor stream; hardware test plan. |
| **Zayd · Bioengineering** | Human-centered requirements and experimental validation | Cook interviews and workflow observations; cooking-state labeling protocol; cookware/food trials; usability and alert-burden study; analysis of whether prompts help cooks. |
| **Ethan · CS** | Session-state backend and system integration | Event schema and API; per-vessel timeline/state estimator; model orchestration; logging and replay; end-to-end integration. |
| **Phillip · CS** | User-facing software and deployment | App dashboard, timers, session controls, question/answer interface, correction flow, alert delivery, and demo-ready packaging. |
| **Shishir · AI** | Perception and grounded assistant behavior | Baseline detectors; RGB/thermal fusion experiments; multimodal prompts/evaluation; confidence calibration and intervention policy with Ethan. |

**Shared responsibilities:** Luke and Zayd validate placement, cleaning, and user safety; Zayd and Shishir define ground-truth labels; Ethan and Phillip integrate live and replay modes; everyone collects test sessions and reviews false alerts. Owners should agree early on one event contract so modules can be developed independently.

## Development steps and decision gates

| Stage | Build and test | Exit criterion |
| :---- | :---- | :---- |
| **1\. Requirements and feasibility** | Interview several home cooks; select two dishes and 4–6 observable events; inspect hood mounting, heat exposure, power, camera view, and privacy needs; set a parts budget. | Written scenarios, event definitions, mounting constraints, and a measurable MVP. |
| **2\. Capture and replay rig** | Mount RGB camera; record synchronized, consented cooking sessions and manual event annotations. Test candidate thermal hardware on different cookware before committing to it. | Reproducible data pipeline and at least one labeled full session per target workflow. |
| **3\. Reliable event baselines** | Implement vessel tracking, boiling/simmering and addition/motion baselines; compare RGB-only with RGB+thermal where available. | Per-event precision/recall and latency measured on held-out sessions; document failure modes. |
| **4\. Stateful assistant** | Implement event log, timers, user corrections, selective model calls, grounding checks, and a simple web interface. Begin with replayed sessions before live prompts. | A complete replay yields sensible state transitions and no unsupported automatic claims in reviewed examples. |
| **5\. Live integration** | Run the prototype during real cooking; measure end-to-end delay, missed events, false interruptions, and how often users correct it. | One reliable unstructured one-vessel demo with a meaningful question and timely prompt. |
| **6\. Improve and present** | Fix the dominant failure mode; add a second vessel or voice only if the core demo is stable. Package mounting, app, evaluation, and a recorded backup demo. | Repeatable live demonstration and a results table that makes limitations explicit. |

**Proposed evaluation metrics:** Event precision and recall; detection delay measured from human-labeled event onset; timer-start accuracy; factual grounding of responses against video/history; false prompts per cooking hour; response latency; and a short user rating of prompt usefulness. Set numerical targets after a pilot establishes realistic baselines. Compare against a simple timer/recipe-step baseline and, for perception, RGB-only versus sensor fusion. A useful outcome can include finding that thermal sensing adds little for particular cookware or events.

## Main risks and design responses

- **Occlusion, steam, glare, and variable lighting:** Use temporal evidence and confidence thresholds; ask the cook when the camera cannot tell.  
- **Thermal ambiguity on reflective cookware:** Test cookware types explicitly and use relative trends only when supported by validation.  
- **Hallucinated cooking guidance:** Ground model outputs in recent observations, distinguish observation from suggestion, and avoid confident claims about doneness or safe internal temperature without a probe.  
- **Latency and cost:** Run frequent simple detection locally; call the larger model on events and questions, with sparse image sampling.  
- **Alert fatigue:** Rate-limit repeated prompts and evaluate usefulness per interruption, not simply the number of messages generated.  
- **Scope:** Complete one excellent one-vessel workflow before adding burners, recipes, or ventilation control.

## Why this fits this team

The device gives each discipline a substantive contribution: Luke solves sensing and integration; Zayd defines what helpful cooking support means and tests it with people; Shishir builds and measures perception and AI behavior; Ethan and Phillip deliver the stateful software and usable interface. The result is a tangible end-to-end system with a clear technical question and measurable results, even if the final finding is that some cooking states remain too ambiguous for automatic guidance.  
