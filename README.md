# @davidlj95/website

This project was generated with [Angular CLI](https://github.com/angular/angular-cli) version 16.2.0.

## To start

First, run the `prebuild` command, to generate files that are required for the app to work

```shell
yarn run prebuild
```

## Development server

Run `ng serve` for a dev server. Navigate to `http://localhost:4200/`. The application will automatically reload if you
change any of the source files.

## Code scaffolding

Run `ng generate component component-name` to generate a new component. You can also
use `ng generate directive|pipe|service|class|guard|interface|enum|module`.

## Build

Run `ng build` to build the project. The build artifacts will be stored in the `dist/` directory.

## Running unit tests

Run `ng test` to execute the unit tests via [Karma](https://karma-runner.github.io).

## Running end-to-end tests

Run `ng e2e` to execute the end-to-end tests via a platform of your choice. To use this command, you need to first add a
package that implements end-to-end testing capabilities.

## Commit message guidelines

Commit messages follow the [conventional commits][conventional-commits] guidelines. This allows for automating the
semantic versioning release process using [semantic release][semantic-release]

[conventional-commits]: https://www.conventionalcommits.org/en/v1.0.0/

[semantic-release]: https://semantic-release.gitbook.io/semantic-release/

### Commit message lint

To enforce that, [`commitlint`][commitlint] is used. It reads the commit messages on a PR and ensures they follow
the [conventional commits][conventional-commits] convention.

You can use the following run script to ensure the last commit follows the guidelines:

```shell
yarn run commitlint-last
```

[commitlint]: https://github.com/conventional-changelog/commitlint

## Release

[Semantic Release][semantic-release] is used to automate the release process. Based on commit messages, a new major,
minor or patch release is generated for every push to main branch. For every release, a Git tag will be created, a
GitHub release will be created and a new version of the app will be published to `npm` public registry. All of this
is managed automatically by [Semantic Release][semantic-release] and CI/CD pipelines.

### Getting release info

In order to embed release information in the app, a script was created to export that kind of information using
[Semantic Release's JavaScript API](https://semantic-release.gitbook.io/semantic-release/developer-guide/js-api).

To generate the release info, run

```shell
yarn run generate-release-file
```

It will generate a `release.json` file in the root of the repository containing all info about releases that
[Semantic Release][semantic-release] provides.

> ⚠️ The API only outputs information when a new release has to be generated. To workaround this, a fake
> patch release is generated in case no release was needed so we get access to all that info. To distinguish a real
> run from a faked run, a `fake` property exists in that JSON.
> The only faked properties are about next release (`nextRelease` and `releases` at the moment of writing this). Info
> about last release
> is correct even when faking the release to generate the output (`lastRelease`, `commits` at the moment of writing
> this)

## Git hooks

Not a fan of, but it's useful to enforce conventional commits. Indeed, it's only one, a pre-commit hook. And checks your
commit message complies with the conventional commits format and configuration.

To use it, run

```shell
yarn run git-hooks
```

It will install Git hooks via [`husky`](https://typicode.github.io/husky/). Now everytime you commit, you'll be sure
commit message guidelines are enforced.

> This is very useful if you can push to `main` directly. Because there's no way to reject commits that don't follow the
> commit message guidelines. And once you push to `main`, you cannot amend that commit as per branch protection rules.

## Autogenerated files from templates

Some metadata about the app (like title, author, theme color...) was repeated across files. So used
[LiquidJS](https://liquidjs.com) to create template files (`.liquid` extension). Then, created a small script to render
templates into generated files using metadata defined in `metadata.ts` file (and some hardcoded constants in the
generation script)

If you update some metadata in that file, run the script to update files generated from templates. Otherwise, they
won't be in sync.

> Automatically updating generated files is not straightforward. Doing something before/after an Angular app build
> runs is [not an option right now][angular-build-hooks-issue]. Adding a bunch of
> [pre `npm` run scripts][npm-pre-post-scripts] left the `package.json` file quite cluttered. Also, won't work if
> issuing `ng` commands. So decided to leave the task to update generated files to the developer. At some point a CI
> check could be introduced to ensure generated files are up-to-date. Generated files are left in the repo because of
> that reason (and also so you can clone & run without issues). That metadata won't change often anyway.

[angular-build-hooks-issue]: https://github.com/angular/angular-cli/issues/11787

[npm-pre-post-scripts]: https://docs.npmjs.com/cli/v9/using-npm/scripts#pre--post-scripts

## Quirks

### Symlinked static assets don't work when serving locally

When using Angular development server (either regular or SSR one), symbolic links seem not to work for static assets.
So for instance `/profile.jpg` which is symlinked to `/assets/img/og.jpg` won't be served. And the server will redirect
you towards main page (`/`). It works when building though, so it will appear in the built version.

## Rendering font subsets

Some fonts included are a subset of a big font file. Before doing anything, please run

```shell
yarn run font-subsets
```

To generate them. Otherwise, those fonts won't be found and you may get some errors. Also, remember to run it if
changing the glyphs included in each font. Or to add the new glyphs if you want to use more glyphs of a font.

## CI/CD

GitHub Actions are used

### Commands ran by CI/CD

In order to ease running CI/CD commands locally, the `.ci` directory contains a `Makefile` intended to contain all
commands that will be run in CI/CD pipelines. So you can test those locally easily

Just:

```shell
cd .ci && make test # for instance
```

And see how a command run in the CI/CD behaves locally. Notice your machine's state may differ from the CI/CD machine
one.

### Running GitHub Actions workflows locally

You can use [`act`](https://github.com/nektos/act) to run GitHub Action workflows in your development machine. If using
macOS, install it using `brew`:

```shell
brew install act
```

Then, use one of the many `run-` targets in the CI/CD `Makefile` to simulate a CI/CD workflow run. For instance:

```shell
cd .ci && make run-main
```

Simulates the workflow triggered by a push to `main` branch.

> If you get a message related to Apple M-series chip:
> ```shell
> WARN  ⚠ You are using Apple M-series chip and you have not specified container architecture, you might encounter issues while running act. If so, try running it with '--container-architecture linux/amd64'. ⚠                      7s
> ```
> You can add ` --container-architecture linux/amd64` (notice a space when line begins) to your `~/.actrc` file. Create
> it if it doesn't exist.

> ℹ️ Images are set to [`js-*` ones](https://github.com/catthehacker/docker_images) via the `.actrc` file as medium
> default ones don't include `yarn` `:sadparrot:`

## Release

## Further help

To get more help on the Angular CLI use `ng help` or go check out
the [Angular CLI Overview and Command Reference](https://angular.io/cli) page.
