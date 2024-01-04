# A C# sample GitHub CLI Extension

A very simple sample GitHub CLI Extension implemented in C# .NET.

The code includes a GitHub Actions workflow that:
- Builds and packages the sample extension for multiple platforms.
- Publishes releases of the sample extension.

# Technologies
- C#
- .NET 8.0
- [GitHub CLI](https://cli.github.com/)
- GitHub Actions
  - [softprops/action-gh-release](https://github.com/softprops/action-gh-release/tree/v1/)
- [Dev Containers](https://containers.dev/)

## Installation of the sample GitHub CLI Extension

Prerequisite: install the [GitHub CLI](https://cli.github.com/).

Install:

```shell
gh extension install XpiritBV/gh-sample-ext-csharp
```

Upgrade:

```shell
gh extension upgrade sample-ext-csharp
```

Uninstall:

```shell
gh extension remove sample-ext-csharp
```

## Usage of the sample GitHub CLI Extension
The extension adds a `sample-ext-csharp` command to the GitHub CLI.

```bash
gh sample-ext-csharp
```
When run, the extension simply outputs a short greeting message.

## Developer Notes

### Dev Container
This repo includes a [Dev Container](https://containers.dev/)
that eleminates the need to install the [GitHub CLI](https://cli.github.com/), .NET 8.0 SDK, and other tools on your local machine.

If you're using VS Code, see [Developing inside a Container](https://code.visualstudio.com/docs/devcontainers/containers).

### GitHub CLI Docs
- [Using GitHub CLI extensions](https://docs.github.com/en/github-cli/github-cli/using-github-cli-extensions)
- GitHub CLI [`gh extension` command reference](https://cli.github.com/manual/gh_extension)
- [Creating GitHub CLI extensions](https://docs.github.com/en/github-cli/github-cli/creating-github-cli-extensions)

Catalog of [GitHub CLI extensions implemented in C#](https://github.com/topics/gh-extension?l=c%23)


## Building and Packaging the sample GitHub CLI Extension

A build of the sample extension is triggered on push to `main` or by pull request.

The `build` job defined in the `.github\workflows\ci.yml` file shows how the extension is built and then packaged for multiple target platforms.

## Creating Releases the sample GitHub CLI Extension

### Create a Draft Release of the Extension
A draft release is created in the repo when a tag with a 'v' prefix is pushed to 'main'. For example:
```shell
git tag v1.2.3
git push origin v1.2.3
```
A new build of the extension will be triggered, and a draft release with the given tag will be created.

The `publish` job defined in the `.github\workflows\ci.yml` file shows how the draft release is created for the built and packaged extension.

The [softprops/action-gh-release](https://github.com/softprops/action-gh-release/tree/v1/) GitHub Action is used to create the draft release.


### Publishing a Release of the Extension
A draft release in the repo can be published by editing the release and switching it from draft to published.

That can be done using the GitHub CLI. For example:

```shell
gh release edit v1.2.3 --draft=false
```

Once the release is published, users can install the extension as described above.
