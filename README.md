# create-orientation: A Claude Code skill for determining a repo's core purpose

## Usage

This skill is designed to create an `orientation.md` file for a code repo by running

```
/create-orientation
```

in Claude Code or your agentic coding harness of choice.

This skill can stand alone, but is primarily designed to be used in conjunction with the [`agentic-learning-opportunities` skill](https://github.com/DrCatHicks/learning-opportunities) 

Once you've created the `orientation.md` file via `/create-orientation` you can then run

```
/learning-opportunities orientation
```

in Claude Code or your agentic coding harness of choice.

This command will provide self-quizzing lessons based on core elements of the repo.

## Background

The skill works to identify what language the repo primarily uses + the repo's purpose as determined from the README, language-dependent entry points (e.g, main.py for Python), test files, and recent git history.

You can find the orientation.md file in the `.claude/agentic-learning-opportunities/resources/` directory within your repo. You can add this file to version control so others can benefit from its guidance or put it in `.gitignore`.

Feel free to edit the `orientation.md` file by hand if it gets something wrong about the repo!