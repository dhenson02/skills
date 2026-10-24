# Skills

These are a combination of custom skills I've made and a list of the skills I use.

## Public

Use this list to download the latest version of the public skill

- [gh-axi](https://github.com/kunchenguid/gh-axi)
- [lavish](https://github.com/kunchenguid/lavish-axi)
- [no-mistakes](https://github.com/kunchenguid/no-mistakes)
- [postgresql-optimization](https://skills.sh/github/awesome-copilot/postgresql-optimization)
- [postgresql-table-design](https://skills.sh/wshobson/agents/postgresql-table-design)
- `show-me`: `npx skills add humanlayer/skills --skill show-me`
- vercel skills: `npx skills add vercel-labs/agent-skills`
  - `vercel-react-best-practices`
  - `web-design-guidelines`

## Custom

These are ones I created

- `refactor-docs-to-skill`
  - Ingest documentation that is ready to become a skill (is broken up into matching sections, etc)
  - Output is what goes into `SKILL.md` for a new skill
  - Helps when trying to use new libraries or libraries that have conflicting names with popular ones LLMs already use (and might halucinate otherwise)
- `apache-echarts` 
  - Made using [Chrome extension](./skill-maker) to gather docs manually from library/package/etc web site
  - Then condensed/skillified with [refactor-docs-to-skill](./refactor-docs-to-skill) and saved as a new skill
- `ui-design`
  - My personal preferences for avoiding bad/AI-like UI design
  - Only certain things avoided here, some AI-like elements I let through
