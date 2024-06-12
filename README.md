# groovy-lint-playground

This repository demonstrates how to implement and use `npm-groovy-lint` for linting Groovy scripts. The setup includes a pre-push Git hook to run linting before pushing changes to the repository and a GitHub Actions workflow to run linting to further ensure code quality after a pull request is created.

## Linter

`npm-groovy-lint` is used to lint Groovy code. Please check [the official npm-groovy-lint document](https://nvuillam.github.io/npm-groovy-lint/) for further details bout `npm-groovy-lint`. 

### Linting Command

The linting command for pre-push is defined in the `package.json` file:

```json
{
  "scripts": {
    "lint": "npm-groovy-lint scripts/* -o log.txt && cat log.txt && rm log.txt"
  }
}
```

If you want to change the cope of linting applied, swap `scripts/*` with your desired directory or file. 

The linting command for Github Actions is defined in the `.reviewdog.yml` file: 

```yaml
  npm-groovy-lint:
    cmd: |
      npm-groovy-lint scripts/* -o log.txt || true
      cat log.txt
```

## Setup pre-push hook

To set up the linting and the pre-push hook, follow these steps:

1. Install Dependencies
    Ensure you have Node.js and npm installed. Then, install the required npm package:

    ```bash
    npm install
    ```

2. Configure the Pre-Push Hook
    Run the setup script to install the pre-push hook:

    ```bash
    ./setup.sh
    ```

3. Push Commit
    The linter is executed once you push your commit to the remote branch: 

    ```bash
    git push origin <branch>
    ```

## CI with GitHub Actions
This repository is configured to run linting twice: locally using the pre-push hook and after a pull request is created using GitHub Actions. The GitHub Actions workflow ensures that code pushed to the repository meets the linting standards, providing an additional layer of code quality assurance.

### Reviewdog
`reviewdog` is used to post comments on pull requests for CI. It integrates with GitHub Actions to provide inline linting results as comments on the PR. Please check [the reviewdog document](https://medium.com/@haya14busa/reviewdog-a-code-review-dog-who-keeps-your-codebase-healthy-d957c471938b#.8xctbaw5u) for further details bout `reviewdog`. 