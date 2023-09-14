# GitHub Action to Operate on ASDF Systems

This GitHub Action performs ASDF operations on Common Lisp
systems.

[![Continuous Integration](https://github.com/melusina-org/asdf-operate/actions/workflows/continuous-integration.yaml/badge.svg)](https://github.com/melusina-org/asdf-operate/actions/workflows/continuous-integration.yaml)

This action is complemented by other actions related to the Common
Lisp eco system:

- [setup-common-lisp](https://github.com/melusina-org/setup-common-lisp)
- [setup-quicklisp](https://github.com/melusina-org/setup-quicklisp)
- [run-common-lisp-program](https://github.com/melusina-org/run-common-lisp-program)
- [make-lisp-system-documentation-texinfo](https://github.com/melusina-org/make-lisp-system-documentation-texinfo)


## Usage

Create a workflow file in the`.github/workflows` directory of your
working copy.  This workflow file should use a MacOS runner or a
Ubuntu Runner and use the branch `v1` of this action.


An [example workflow](#example-workflow) is available below. See the GitHub Help Documentation for
[Creating a workflow file](https://help.github.com/en/articles/configuring-a-workflow#creating-a-workflow-file)
to get started with GitHub workflows.


## Outcomes

Once the action has been executed succesfully, the required ASDF
operation has been performed on the selected Common Lisp system, using
the selected Common Lisp implementation.


## Inputs

* `implementation` — The Common Lisp implementation to setup QuickLisp
  for. This can be one of the values `abcl ecl sbcl` and
  in the future we would like to support all of `abcl clasp clisp ecl gcl sbcl`
  and maybe other implementations. Please open an issue to express
  interest for other implementations.

* `system`: The Common Lisp system to operate on.
* `operation`: The ASDF operation to perform.

## Outputs

This action has no outputs.

## Example Workflow

```yaml
name: 'Continuous Integration'
on:
  - workflow_dispatch
  - push

jobs:
  install-quicklisp-on-tier-1-platforms:
    strategy:
      matrix:
        implementation: ['abcl', 'ecl', 'sbcl']
        os: ['ubuntu-latest', 'macos-11', 'macos-12', 'macos-13']
    runs-on: '${{ matrix.os }}'
    name: 'Install Common Lisp'
    steps:
      - uses: actions/checkout@v3
      - name: 'Install MacPorts'
        if: runner.os == 'macOS'
        uses: melusina-org/setup-macports@v1
      - uses: melusina-org/setup-common-lisp@v1
        with:
          implementation: '${{ matrix.implementation }}'
      - uses: melusina-org/setup-quicklisp@v1
        with:
          implementation: '${{ matrix.implementation }}'
      - uses: ./
        with:
          implementation: '${{ matrix.implementation }}'
          system: 'alexandria'
          operation: 'compile-op'
```

## License
The scripts and documentation in this project are released under the [MIT License](LICENSE)
