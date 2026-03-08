---
name: skill-creator
description: Create new skills, modify and improve existing skills, and measure skill performance. Use when users want to create a skill from scratch, edit, or optimize an existing skill, run evals to test a skill, benchmark skill performance with variance analysis, or optimize a skill's description for better triggering accuracy.
---

# Skill Creator

A skill for creating new skills and iteratively improving them.

At a high level, the process of creating a skill goes like this:

- Decide what you want the skill to do and roughly how it should do it
- Write a draft of the skill
- Create a few test prompts and run claude-with-access-to-the-skill on them
- Help the user evaluate the results both qualitatively and quantitatively
- Rewrite the skill based on feedback
- Repeat until satisfied
- Expand the test set and try again at larger scale

Your job when using this skill is to figure out where the user is in this process and then jump in and help them progress through these stages.

## Communicating with the user

Pay attention to context cues to understand how to phrase your communication. Users range from complete beginners to expert developers. Keep explanations accessible and define jargon when in doubt.

---

## Creating a skill

### Capture Intent

Start by understanding the user's intent. The current conversation might already contain a workflow the user wants to capture. Extract answers from the conversation history first — the tools used, the sequence of steps, corrections the user made, input/output formats observed.

1. What should this skill enable Claude to do?
2. When should this skill trigger? (what user phrases/contexts)
3. What's the expected output format?
4. Should we set up test cases to verify the skill works?

### Interview and Research

Proactively ask questions about edge cases, input/output formats, example files, success criteria, and dependencies. Wait to write test prompts until you've got this part ironed out.

### Write the Skill.md

Based on the user interview, fill in these components:

- **name**: Skill identifier
- **description**: When to trigger, what it does. This is the primary triggering mechanism - include both what the skill does AND specific contexts for when to use it. Make descriptions slightly "pushy" to combat undertriggering — instead of "How to build a dashboard", write "Use this whenever the user mentions dashboards or data visualization, even if they don't explicitly ask."
- **the rest of the skill body**

### Skill File Structure

```
skill-name/
├── Skill.md       (required - YAML frontmatter + instructions)
└── references/    (optional - extra docs loaded on demand)
└── scripts/       (optional - executable code)
└── assets/        (optional - templates, icons, etc.)
```

### Progressive Disclosure

Skills use a three-level loading system:
1. **Metadata** (name + description) - Always in context
2. **Skill.md body** - In context whenever skill triggers (keep under 500 lines)
3. **Bundled resources** - Loaded as needed

### Writing Good Skills

- Keep instructions concrete and actionable
- Include examples of inputs and outputs
- Reference any bundled files clearly with guidance on when to read them
- For large reference files (>300 lines), include a table of contents
- Scope the description tightly so it triggers at the right times

---

## Iterating on a skill

### Test Cases

Write 5-10 test prompts that represent realistic user requests. Good test cases are:
- Substantive enough that Claude would benefit from a skill
- Representative of the range of inputs the skill should handle
- A mix of easy and edge-case scenarios

### Evaluating Results

For each test case, run the skill and evaluate:
- Did the skill trigger when it should have?
- Was the output format correct?
- Did it follow the skill's instructions?
- What could be improved?

### Iterating

Based on evaluation:
1. Identify the most common failure mode
2. Update the Skill.md to address it
3. Re-run the same test cases
4. Check if the failure mode is fixed without breaking other cases
5. Repeat

### Description Optimization

If the skill isn't triggering reliably:
- Make the description more specific about trigger conditions
- Add more example phrases/contexts
- Make it slightly more "pushy" — Claude tends to undertrigger skills

---

## Packaging

Once the skill is done, the final structure should be a folder named after the skill containing at minimum a `Skill.md`. Place it in your Claude Code skills directory (`.claude/skills/` in your project or `~/.claude/skills/` globally).

Good luck!
