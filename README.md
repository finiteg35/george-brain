# George Brain

Bishop's second brain — a private repo for personalizing capabilities and maintaining deep context on George.

Think of this as the human side of the agent-brain pairing:
- **agent-brain** → how Bishop operates (skills, tools, workflows)
- **george-brain** → who George is (context, preferences, projects, goals)

## Structure

```
george-brain/
├── profile/
│   ├── about.md          # Who George is — background, role, personality
│   ├── preferences.md    # Communication style, work habits, pet peeves
│   └── goals.md          # Short and long-term goals
├── projects/
│   ├── README.md         # Active project index
│   ├── everblack.md      # Everblack platform + produce app context
│   ├── phone-agent.md    # Vapi phone agent build
│   └── minecraft.md      # Minecraft server + Bedrock setup
├── context/
│   ├── people.md         # Key people in George's world
│   ├── businesses.md     # GMF, vendors, customers
│   └── tech-stack.md     # Tools, servers, infrastructure
└── notes/
    └── README.md         # Freeform notes and session captures
```

## Usage

Bishop reads and updates this repo to maintain rich, durable context across sessions — beyond what fits in memory snippets.
