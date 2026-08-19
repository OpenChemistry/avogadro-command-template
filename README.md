# avogadro-plugin-template

A [copier](https://copier.readthedocs.io/) template for Avogadro 2 plugins.

It generates a **Pixi project** plugin (`pypixi`) providing one or more
**menu commands** — the most common shape by far. Energy models, electrostatic
models, file formats, and input generators are not covered; see
[avo-plugin-demo](https://github.com/OpenChemistry/avo-plugin-demo) for
examples of those.

## Usage

```bash
pixi global install copier     # or: uv tool install copier
copier copy /path/to/avogadro-plugin-template avogadro-myplugin
cd avogadro-myplugin
pixi install
pixi run test
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
├── src/avogadro_myplugin/
│   ├── __init__.py         argparse: <feature> [--lang] [--debug]
│   ├── run.py              match/case dispatch on the feature identifier
│   ├── commands.py         one worked command, plus a commented second
│   └── customization/
│       └── translate_options.json
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
  with `menu-priorities`. This is what Avogadro 1.103 reads. The nested
  `path.menu` / `path.item.label` form that appears in `avo-plugin-demo` and in
  parts of the documentation is not yet implemented.
- `user-options` is a filesystem path relative to the plugin directory, so the
  options JSON can live anywhere; this template keeps it inside the package so
  it also ships in a built wheel.

## Licenses

The `license` question offers BSD-3-Clause and MIT, and writes the full text.
For anything else, pick either one, then replace `LICENSE` and change the
`license` field in `pyproject.toml` to the right SPDX identifier.

## Reference

- [Writing plugins](https://two.avogadro.cc/develop/plugins/)
- [Input and output JSON](https://two.avogadro.cc/develop/plugins/io.html)
