# Bat-peasant workflow

I have $20 ChatGPT plus subscription. That makes me able to use Codex as my coding agent tool.

So I have this idea that try to make codex work like me.

Current problem is that codex is not writing code like me. I work with typescript, python, java and go with lot of different project structure with different standards. And when i ask codex to write a code it writes a proper functional code. However its not suitable for that exact codebases style.

So i decided to create `/.agent` folder with following information

```text
.agents/
├── /context/*    # Agent memory, the codebases business logic library
│   ├── agent_memory.json          # Persistent agent memory and knowledge
│   ├── session_context.md         # Current conversation/session context
│   └── runtime_state.json         # Live runtime state
├── /docs/*       # Documentations, guides, and reference materials that related to the codebase.
│   ├── dynamo-command-builder.md            # System architecture overview
│   └── custom-axios.md                   # How to use and customize agents
├── /examples/*   # Example code snippets, configurations, templates and demo.
│   ├── basic_agent.md             # Simple agent example
│   ├── advanced_agent.json        # Complex agent configuration
│   └── demo_prompts/              # Example prompt collections
├── /metadata/*   # Codebase metadata, project structure, tech stack informations
│   ├── agent_profile.yaml         # Agent identity and capabilities
│   ├── manifest.json              # Agent manifest file
│   └── version.json               # Versioning information
├── /prompts/*    # Pre-defined prompts for the task.
│   ├── system_prompt.txt          # Main system prompt
│   ├── task_prompts/              # Task-specific prompts
│   │   ├── research_prompt.txt
│   │   └── analysis_prompt.txt
│   └── templates/                 # Reusable prompt templates
├── /rules/*      # Rules, constraints, standards for the codebase
│   ├── safety_rules.md            # Safety and ethical guidelines
│   ├── behavior_rules.yaml        # Core behavior constraints
│   └── response_guidelines.md     # Response style and tone rules
└── /skills/*     # Skills, tools, capabilities, and plugin modules
AGENTS.md         # Main index & documentation
```

## Example use case of this project

Lets assume i asked codex to do TASK_A. Codex then determines if it needs to load

## List of most frequent use cases

- Implementing new feature
- Update the existing feature
- Fix and debug existing new feature
- Review pull request for the codebase
- Refactor the existing codebase section (Local refactor, migration refactor, small function refactor, whole project refactor)
- Refactor the external repository to the current codebase
- Analyzing the current flow
- Write a test for certain piece of codebase.
- Write a small utility function.
