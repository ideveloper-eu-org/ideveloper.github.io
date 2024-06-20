## Release-it
> this is release tools for front-end


For publish monorepos we need above plugins:

@release-it-plugins/workspaces
@release-it/conventional-changelog

The configuration is as follows：

```js
{
  "git": {
    "commitMessage": "chore: release ${version}" // This is to match commitlint
  },
  "npm": false, // Here is important
  "plugins": {
    "@release-it-plugins/workspaces": true,
    "@release-it/conventional-changelog": {
      "preset": {
        "name": "conventionalcommits",
        "type": [
          {
            "type": "feat",
            "section": "Features"
          },
          {
            "type": "fix",
            "section": "Bug Fixes"
          }
        ]
      },
      "infile": "CHANGELOG.md"
    }
  }
}

```
To publish we just run command `pnpm release` in root folder
