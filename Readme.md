# Documenthor Demo

This repo aims to give a short overview of how to setup documenthor and gives an overview of its functionality.

## What does Documenthor do?

Documenthor is a prow plugin that aims to simplify documentation management. It is specifically useful if your documentation resides in a different repository than the source code.

Documenthor blocks any PRs from being merged until either a link to a documentation is provided or the documentation is deferred to a later point in time.

Usual Flow:

1. A contributor opens a PR
2. Until a specific field inside the PR description is filled, a `do-not-merge/docs-needed` is applied which blocks any merging. The contributor then has three options to make the PR mergeable:
    * Add the link to another PR (from a different repo), which contains the documentation
    * Add the keyword `NONE` into the field => PR requires no documentation and is ready to merge
    * Add the keyword `TBD` into the field => Documentation will be handed in later, but PR should be merged already. Documenthor will also mark these PRs with the `docs/tbd` GH label
3. PR can be merged
4. (if `TBD` option was chosen) Before any major releases, maintainers can filter in GH for any PRs that have the `docs/tbd` label and ping contributors to hand in missing documentation. Once documentation is written, the PRs description can be edited to include a link to the doc PR. Documenthor will then automatically remove the label.

## How to setup Documenthor

1. Create the following GH labels in your Repository

    * docs/provided
    * docs/tbd
    * docs/none
    * do-not-merge/docs-needed

2. TODO required secrets/repo permissions

3. Apply the `prow/documenthor.yaml` into your cluster where prow is running.

4. Copy the contents of the `prow/plugins.yaml` file from this repo into your plugins.yaml file under the `external_plugins` key.

5. Documenthor reacts to a specifically formatted code-block inside a pull request. For a reference see the `.github/PULL_REQUEST_TEMPLATE.md` file. In general we recommend to setup a pull-request template and copy , to automatically have the correctly formatted code-block on new pull-requests.

6. Setup the `tide` config in your `config.yaml` to only merge PRs if the `do-not-merge/docs-needed` label is not set
