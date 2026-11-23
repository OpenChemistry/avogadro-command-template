# avogadro-command-template

A [copier](https://copier.readthedocs.io/) template for Avogadro 2 plugins.

It generates a **Pixi project** plugin providing one or more
**menu commands** — the most common need. Energy models are covered
by a separate template. If you're interested in electrostatic
models, file formats, and [input generators](https://github.com/openchemistry/avogenerators) please ask on the [Avogadro forum](https://discuss.avogadro.cc/) for examples.

## Usage

```bash
pixi global install copier     # or: uv tool install copier
copier copy gh:openchemistry/avogadro-command-template avogadro-myplugin
# you'll be asked some questions like the name of your plugin and the menu item
cd avogadro-myplugin
# edit your code as needed, e.g. src/{{module name}}/*.py
pixi install
pixi run test
# now you can use Install from Directory... in the Manage Plugins dialog
# to test out 
```

To pull in later improvements to this template:

```bash
copier update
```

(`copier update` requires the generated project to be a git repository with the
`.copier-answers.yml` file committed.)

## What you get

```
avogadro-myplugin/
├── pyproject.toml          [project] + [tool.pixi.*] + [tool.avogadro]
├── README.md
├── LICENSE
├── .gitignore
├── .gitattributes          pixi.lock as a binary-merge, generated file
├── .github/               daily check for new PyPI / conda-forge releases
├── src/avogadro_myplugin/
│   ├── __init__.py         argparse: <feature> [--lang] [--debug]
│   ├── run.py              match/case dispatch on the feature identifier
│   ├── commands.py         one worked command, plus a commented second
│   └── customization/
│       └── translate_options.json     JSON for the dialog if needed
└── tests/
    ├── conftest.py
    ├── data/water.json     a real Avogadro input JSON
    └── test_commands.py
```

The example command translates the molecule (or just the selected atoms) by a
user-supplied vector. It exercises the whole round trip — reading CJSON,
reading dialog options, honouring `selectedAtoms`, and returning an edited
molecule — with no third-party dependencies, so it runs immediately.

## Notes on the generated metadata

- The `[project.scripts]` entry point **must** begin with `avogadro-`;
  Avogadro's package manager scans for exactly that prefix.
- Menu placement uses the flat `menu` / `submenu` / `subsubmenu` / `item` keys
  with `menu-priorities`. This is what Avogadro reads. If you just want one
  menu item under a menu, you can skip `submenu` and `subsubmenu` as needed.
- `user-options` is a filesystem path relative to the plugin directory, so the
  options JSON can live anywhere; this template keeps it inside the package so
  it's easier to find and tweak.

## More questions?

If you have more questions or want more help getting your plugin started,
please create a post on the [Avogadro Forum](https://discuss.avogadro.cc/c/development/discussion-and-development-of-plugin-scripts/14).

## Dependency updates

The `track_dependencies` question lists packages to watch, and becomes
`[tool.update-dependencies]` in `pyproject.toml`. Once the plugin is on GitHub,
`.github/workflows/update-dependencies.yml` checks daily for new releases:
PyPI for packages in `[project].dependencies` or
`[tool.pixi.pypi-dependencies]`, conda-forge for `[tool.pixi.dependencies]`. A
conda-forge release only counts once it is built for every platform the plugin
lists, because pixi solves all of them. When a tracked package has a new
release, the workflow opens a pull request that raises that package's `>=`
floor and bumps the patch version in `[project].version`. Merge it, and
the Avogadro plugin index picks up the new commit; installed copies then show
"Update available", and updating reinstalls the plugin with the new release.

For the workflow to open pull requests, enable *Settings → Actions → General →
Allow GitHub Actions to create and approve pull requests* in the repository.
If `pixi.lock` is committed, the workflow also runs `pixi lock` and includes
the refreshed lockfile in the pull request.

## Licenses

The `license` question offers BSD-3-Clause and MIT, and writes the full text.
For anything else, pick either one, then replace `LICENSE` and change the
`license` field in `pyproject.toml` to the right SPDX identifier.

## Reference

- [Writing plugins](https://avogadro.cc/develop/plugins/)
- [Input and output JSON](https://avogadro.cc/develop/plugins/io.html)
