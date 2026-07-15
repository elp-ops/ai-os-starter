# Multi-Agent Build Stack (Reference Pattern)

When a build is large enough to justify it, use this orchestration model:

- **Primary assistant:** Orchestrator. Plans, coordinates, reviews, integrates output.
- **Specialist agents:** other models/tools brought in for a specific strength gap, e.g. a multimodal model for design-to-code work, or a cheaper model for high-volume simple tasks.

**When to use this pattern:**
- A build is large enough to parallelise independent pieces of work
- A specific model strength gap exists that your primary assistant doesn't cover well
- Cost optimisation matters at volume

**Default behaviour: don't apply this pattern unless the task clearly justifies the coordination overhead.** For most builds, one assistant handling everything directly is simpler and just as effective. Reach for this only when the complexity or cost genuinely earns it.
