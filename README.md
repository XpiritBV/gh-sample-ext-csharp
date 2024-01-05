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
that eliminates the need to install the [GitHub CLI](https://cli.github.com/), .NET 8.0 SDK, and other tools on your local machine.

If you're using VS Code, see [Developing inside a Container](https://code.visualstudio.com/docs/devcontainers/containers).

### GitHub CLI Docs
- [Using GitHub CLI extensions](https://docs.github.com/en/github-cli/github-cli/using-github-cli-extensions)
- GitHub CLI [`gh extension` command reference](https://cli.github.com/manual/gh_extension)
- [Creating GitHub CLI extensions](https://docs.github.com/en/github-cli/github-cli/creating-github-cli-extensions)

Catalog of [GitHub CLI extensions implemented in C#](https://github.com/topics/gh-extension?l=c%23)

## Developing a GitHub CLI Extension in C#

When installed, a GitHub CLI Extension is loaded by the CLI from a release in a GitHub repo.

### Creating a Repo for a GitHub CLI Extension

The name of the GitHub repo that the extension is loaded from must be in the format:
```
gh-EXTENSION-NAME
```

#### Tell the world about your Extension
If your GitHub repo and you want others to be able to find your extension, you can add the  `gh-extension` repository topic to your repo. So your extension will appear on the [gh-extension topic page](https://github.com/topics/gh-extension).

See [Classifying your repository with topics](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/classifying-your-repository-with-topics)
 in GitHub Docs for more info.


### Releasing a GH CLI Extensions

#### 1. Build executables for each supported platform

A separate platform-specific executable must be created for each platform that the extension should run on. Where each executable created is self-contained, and bundles the application and its dependencies, along with the .NET runtime and libraries, in a single file for the specific runtime.

The name of the of each executable must be in the format:
```
gh-<extension-Name><OS>-<architecture>[.file-extension]
```
Note: Windows executables must have a `.exe` file extension.

Examples:
|.NET runtime | OS | Architecture	| Executable Name |
|-------------|----|--------------|-----------------|
| `osx-x64` | Darwin (Mac OS) | AMD 64-bit | `gh-yourext-amd64` |
| `osx-arm64` | Darwin (Mac OS) |	ARM 64-bit	| `gh-yourext-darwin-arm64` |
| `linux-x64` | Linux	| AMD 64-bit	| `gh-yourext-linux-amd64` |
| `win-x64` | Windows	| AMD 64-bit	| `gh-yourext-windows-amd64.exe`|

See the `build` job the `.github\workflows\ci.yml` file for details on how the platform-specific executable files are are created.

See [here](https://github.com/cli/cli/blob/14f704fd0da58cc01413ee4ba16f13f27e33d15e/pkg/cmd/extension/manager.go#L696) for a list of OS and Architecture combinations recognized by the GH CLI.

#### 2. Create a release and attached the executables
Create a release in the repo and attach each of the extension's platform-specific executables to the release.

The `publish` job in the `.github\workflows\ci.yml` file
uses the [softprops/action-gh-release](https://github.com/softprops/action-gh-release/tree/v1/) GitHub Action to create the release and attach the platform-specific executables as release artifacts.

## Building and Packaging the sample GitHub CLI Extension

A build of the sample extension is triggered on push to `main` or on a pull request.

The `build` job defined in the `.github\workflows\ci.yml` file shows how the extension is built and then packaged for multiple target platforms.

## Releasing the sample GitHub CLI Extension

Publishing a release of the sample extension is done in two steps
1. Creating a draft release on push or PR.
1. Manually publishing the draft release.

Details for these steps are described in the following subsections.
Along with notes for publishing the release in a single step.

### Create a Draft Release of the Extension
A draft release of the extension is created in the repo when a tag with a 'v' prefix is pushed to 'main'.
For example:
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

### Automatically Create and Publish a Release of the Extension
To automatically publish a release of the extension whenever `v` prefixed tag is pushed, edit the `publish` job the `.github\workflows\ci.yml` file and set the `draft` input to `false`.

```yaml
      - name: 🚀 create and publish release
        uses: softprops/action-gh-release@v1
        with:
          draft: false
          . . .
```
See [softprops/action-gh-release](https://github.com/softprops/action-gh-release/tree/v1/)
