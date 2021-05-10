# A simple and easy-to-use, yet powerful and language agnostic git hook for monorepos.

![demo](./demo.gif)

Despite being a very young project, it is ready to use, even if it remains a beta under active development.

You are welcome to use it and enjoy it's simplicity.
**If you encounter any bug or weird behavior, don't be afraid to open an issue :)**

## Get started

```bash
$ npm install @escape.tech/mookme
$ mookme init
```

Example of JSON `mookme` hook file installing `commitlint` :

*`{args}` are replaced with the hook arguments when the command is executed. See [the  git documentation on hooks](https://git-scm.com/book/en/v2/Customizing-Git-Git-Hooks)*

```JSON
# commit-msg.json
{
    "steps": [{
        "name": "commit lint",
        "command": "cat {args} | ./node_modules/@commitlint/cli/cli.js"
    }]
}
```

More examples in the "**Writing hooks**" section !

## Commands

### `init`

The main initialization command. It :

- prompts for one or multiple packages folder path
- asks you to select one or multiple package at each path
- creates the `.hooks` folder in each package where you can write **dedicated hooks !** that will be triggered only when changes in this package occur
- creates a `.hooks` folder at the root of your project where you can write **project-wide hooks** that will be triggered on every commit
- writes `.git/hooks` files

#### Options 

- `--only-hook` (optional)

Skip prompters and only write `.git/hooks` files. This is for installation in an already-configured project.

### `run`

Mainly used for debugging and dry run :

`mookme run --type pre-commit -a "test arguments"`

#### Options

- `-t --type` (required)

The type of hook to run, has to be one of `pre-commit`, `prepare-commit-msg`, `commit-msg`, `post-commit`.

- `-a --args` (optional)

The arguments that would be normally passed by git to the hook

### Writing hooks

Hooks are stored in JSON files called `{hook-type}.json` where the hook type is one of the available git hooks, eg :

- `pre-commit`
- `prepare-commit-msg`
- `commit-msg`
- `post-commit`

A hook is nothing more than a list of steps, which are commands to execute.

#### Available options

- `steps`

The list of steps being executed by this hook

- `type`

A flag used mainly to tell `mookme` this is a python hook, and might need a virtual environment to be activated.

- `venvActivate`

A path to a `<venv>/bin/activate` script to execute before the command if the hook type is `python`

