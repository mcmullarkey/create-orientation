# orient: A Claude Code plugin for orienting users to a repo

## Installation + usage

Add this repo as a Claude Code plugin marketplace + install the plugin

```
/plugin marketplace add https://github.com/mcmullarkey/orient.git
```
```
/plugin install orient@orient
```

Then reload Claude code + run the skill inside any repo to generate an `orientation.md` file for that repo

```
# Ctrl + C twice in quick succession to exit current Claude Code session
# Start new Claude Code session
claude
# Within Claude Code run the orient skill inside the repo of your choice
/orient
```

This plugin can stand alone, but is primarily designed to be used in conjunction with the [`learning-opportunities` plugin](https://github.com/DrCatHicks/learning-opportunities)

Once you've created the `orientation.md` file via `/orient` you can then run

```
/learning-opportunities orient
```

This command will provide self-quizzing lessons based on core elements of the repo.

## Background

The skill works to identify what language the repo primarily uses + the repo's purpose as determined from the README, language-dependent entry points (e.g, main.py for Python), test files, and recent git history.

You can find the orientation.md file in the `.claude/agentic-learning-opportunities/resources/` directory within your repo. You can add this file to version control so others can benefit from its guidance or put it in `.gitignore`.

Feel free to edit the `orientation.md` file by hand if it gets something wrong about the repo!

## Author

**Dr. Michael Mullarkey**

I'm a machine learning engineer who used to be a therapist + social science researcher. I'm thinking a lot about how to leverage agentic AI to help people learn skills, see [blendtutor](https://github.com/mcmullarkey/blendtutor) for another example.

- [Website](https://mcmullarkey.github.io/)

## Bibliography

### Program comprehension and codebase orientation

**Spinellis, D.** (2003). *Code Reading: The Open Source Perspective*. Addison-Wesley.
https://www.spinellis.gr/codereading/
- Basis for: start with the build system and README; read the directory tree as an architectural table of contents; identify architectural seams between components.

**Hermans, F.** (2021). *The Programmer's Brain*. Manning Publications.
https://www.manning.com/books/the-programmers-brain
- Basis for: follow the entry point and call graph one level at a time; use "beacons" (recognizable patterns) to orient quickly; experts sample strategically rather than reading exhaustively.

**Storey, M.-A., et al.** (2006). How Software Developers Use Tools, Cognitive Strategies, and Representations to Navigate Code. *IEEE Transactions on Software Engineering*.
https://ieeexplore.ieee.org/document/1579228
- Basis for: use the test suite as an executable specification; alternate between systematic and opportunistic traversal; follow data flow, not just control flow.

**Storey, M.-A., Zimmermann, T., Bird, C., Czerwonka, J., Murphy, B., & Kalliamvakou, E.** (2021). Towards a Theory of Software Developer Job Satisfaction and Perceived Productivity. *IEEE Transactions on Software Engineering*, 47(10), 2125–2142.
https://ieeexplore.ieee.org/document/8851296
- Basis for: developer productivity is not separable from social and psychological context; orientation practices that ignore belonging and autonomy will underperform.

**Spolsky, J.** Practitioner writing on reading unfamiliar codebases.
https://www.joelonsoftware.com
- Basis for: read git history to understand *why* code is the way it is; high-churn files are usually the core of the system.

**Dagenais, B., et al.** (2010). Moving into a New Software Project: Strangers in a Strange Land. *ICSE 2010*.
https://dl.acm.org/doi/10.1145/1806799.1806842
- Basis for: every codebase has a "gateway artifact" that once understood makes everything else legible; prioritize runnable examples over documentation.

### Developer psychology and learning culture

**Hicks, C. M.** (2021). *"It's Like Coding in the Dark": The Need for Learning Cultures within Coding Teams.* Catharsis Consulting white paper.
https://neverworkintheory.org/static/2022-04/hicks-catherine.pdf
- Basis for: environments that punish not-knowing create "learning debt" that compounds over time; psychological safety to ask questions is a precondition for effective orientation, not a nice-to-have.

**Hicks, C. M., Lee, C. S., & Ramsey, M.** (2024). Developer Thriving: Four Sociocognitive Factors That Create Resilient Productivity on Software Teams. *IEEE Software*, 41(4), 68–77.
https://dsl.pubpub.org/pub/dev-thriving/release/2
- Basis for: learning culture, agency, belonging, and self-efficacy predict developer productivity; an orientation that addresses all four will outperform one that focuses only on technical ramp-up.

**Lee, C. S. & Hicks, C. M.** (2024). Understanding and Effectively Mitigating Code Review Anxiety. *Empirical Software Engineering* (Springer).
https://link.springer.com/article/10.1007/s10664-024-10550-9
- Basis for: anxiety about submitting work for review is common regardless of seniority and measurably reduces engagement; explicitly normalizing early feedback as low-stakes during orientation reduces avoidance.

**França, C., Sharp, H., & da Silva, F. Q. B.** (2014). Motivated Software Engineers Are Engaged and Focused, While Satisfied Ones Are Happy. *ESEM 2014*.
https://dl.acm.org/doi/10.1145/2652524.2652545
- Basis for: motivation (engagement with the work itself) and satisfaction (happiness with conditions) are distinct and don't substitute for each other; orientation should cultivate genuine interest in the codebase, not just comfort.