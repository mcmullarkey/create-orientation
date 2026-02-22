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
claude
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

## Sources

See `resources/bibliography.md`