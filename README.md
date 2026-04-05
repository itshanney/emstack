# emstack
A simple stack of agent skills for software engineering managers

## Skills

- **advisor**: Advisor that channels a Product Manager and helps flush out the true essence of the request to produce a solid design doc for the architect.
- **architect**: Senior engineer that takes the design doc from the advisor and turns it into a technical plan to be implemented by a coder.

## Installation

To install these skills locally for use with Gemini and Claude, use the provided Ansible playbook:

```bash
ansible-playbook install_skills.yml
```

This will automatically copy the `SKILL.md` files to the correct `.gemini` and `.claude` folders in your home directory so they can be picked up as agents.
