# emstack
A simple stack of agent skills for software engineering managers

## Skills

- **ems_product**: Senior Product Manager that translates ambiguous requests into precise, implementable specifications for an engineering team.
- **ems_architect**: Principal-level Software Engineer and Architect that takes the design doc from the product manager and turns it into a technical plan to be implemented by a coder.
- **ems_coder**: Senior Software Engineer that takes a technical design and implements it with the same care and standards you'd apply to code that runs in production serving real users. Writes code that other engineers will maintain for years after you.

## Installation

To install these skills locally for use with Gemini and Claude, use the provided Ansible playbook:

```bash
ansible-playbook install_skills.yml
```

This will automatically copy the `SKILL.md` files to the correct `.gemini` and `.claude` folders in your home directory so they can be picked up as agents.
