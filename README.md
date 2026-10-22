# agents-specifications
Guides for agents

## How to use

```sh
git submodule add git@github.com:At16th/agents-specifications.git .agents
mkdir -p .claude/skills
ls -1 .agents/skills | while read -r skill; do ln -sfn ../../.agents/skills/$skill .claude/skills/$skill; done
cat >> AGENTS.md <<'EOF'
Follow the shared standards in `.agents/core.md` before doing anything else.
EOF
cat >> CLAUDE.md <<'EOF'
Read vendor agnostic `AGENTS.md` file.
EOF
```

Cloning a repo that already uses this:

```sh
git clone --recurse-submodules <repo>
# or, in an existing clone:
git submodule update --init
```

Pulling newer standards (do it deliberately, then commit the bump):

```sh
git submodule update --remote .agents
git add .agents && git commit -m "Bump agent standards"
```
