`Github #974`

# Default Linter Setup in New Environments

The vscode-python extension provides simple, correct, set up of linting, in new workspaces.

## Prior Process

Previously when a user would open a new workspace in VSCode with the vscode-python extension, the extension would check to see if that environment had linting set up yet. If no linting was set up, the extension would display a popup recommending the user install and enable pylint specifically, and if the user agreed they could press a button and have the extension take care of the install into their workspace's environment.

## New Process

The vscode-python extension will enable linting simply and in an expected manner when workspaces with no existing linter set up, are opened. The goal of the extension is to enable linting in every workspace with zero user interaction and no workspace modification at all, unless explicitly set up to never set up linting.

The **extension default linter** cannot be altered by users and is the last linter attempted to be set up if no other linter is configured at the user or workspace scope in settings. This linter is determined by the vscode-python core dev team and ships as part of the vscode-extension.

> Note: The new process will be placed behind a feature flag until it becomes stable. If the feature flag is disabled the prior behaviour is not changed.

## New Process Overview

The user settings for VSCode will include a similar setting to what exists today: `python.linting.enabled`. This can be set at the user or the workspace scope.

Whenever any workspace is opened, the extension will:

1. Check `python.linting.enabled` in the workspace and use scope, if it is `false` no linting setup process ever takes place. Otherwise continue...

2. Discover the currently configured default linter in this order: workspace-scope, user-scope, then the 'extension default linter'.

3. Check that the available linter dependencies exist in the workspace.

4. If the configured linter is not available in the workspace's environment, alert the user and if possible, provide an automatic setup option to them.

## Allowable Linters

We currently support a number of linters in the extension:
- Flake8
- MyPy
- Pep8
- Prospector
- PyDocStyle
- PyLama
- PyLint

Each of these are easily added to a workspace environment and should be automated by the extension.

We may add new linters to this list over time.

## To Specify the Default Linter for All New Workspaces

The user can always specify the default linter they would prefer. This is done by changing the setting at the user scope to enable each preferred linter. That is, set one or more of the following at the user scope:

```js
{
    python.linting.flake8Enabled: true,
    python.linting.mypyEnabled: true,
    python.linting.pep8Enabled: true,
    python.linting.prospectorEnabled: true,
    python.linting.pydocstyleEnabled: true,
    python.linting.pylamaEnabled: true,
    python.linting.pylintEnabled: true
}
```

If all of these are set to `false` at the user scope, the extension will check if the workspace has a settings.json file configured with any of the above settings.

If no user- or workspace- scope settings are configured to tell the vscode-python extension what linter the user prefers, it will fall back the the 'extension default linter'.


## Testing Plan

### Unit Tests

### System Tests
