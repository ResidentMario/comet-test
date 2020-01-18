# comet-test

This repo tests out some CometML features.

## Experiments

* The unit of value in CometML is an **experiment**. An experiment is a single algorithm run.
* Strictly speaking there are four experiment classes in the Python SDK: `Experiment`, `ExistingExperiment` (for connecting to, you know), `OfflineExperiment` (which logs to a file instead of streaming to the Internet), and `APIExperiment` (used for querying an existing experiment instead of streaming to/from it).
* Experiments may be symlinked to multiple projects via `create_symlink`.
* You can IFrame an experiment in a notebook using `display`.
* Metrics are logged via monkey patching the requisite libraries. A `disable_mp` method is provided for disabling this behavior, which is recommended for non-trivial use, as the monkey patching is a go-to-market strategy.
* It's good practice to `end` an experiment at completion.
* Experiment logs are available via various `get_*` and `log_*` methods. There are a lot of different loggable things, so much so in fact that the UI is very confusing.
* `send_notification` is interesting, it's a hook to an email notification system built into Comet.
* The UI has a reproduce button, which points to some logabble values. In the case of a run this is Environment Information, Git Information, and Run Command. In the case of a notebook the Git Information and Run Command options seem to be replaced with Notebook Information, which is an IPYNB download. Overall this feature is not that useful in my opinion, a README does much better for this function.
* Offline experiments are uploaded to the cloud using a Python lib utility script or a CLI wrapper thereof.
* The notebook write-out is kind of janky, albeit for the right reason. The cells are automatically flattened out into a script. Any cells that get run more than once will be included more than once. This output format is reproducible, however, which is why they do it this way.

## Experiment UI

* The experiment web UI is only controllable in the browser, e.g. it's not possible to adjust it using libraries.
* Experiments are bucketed into projects. Experiments can appear in more than one project via symlinks (see previous section).
* Experiments in a project are subselected using views. Views are combinations of query builder that lets you subselect experiments using tag boolean logic, and a panel of model metric visualizations and assets that you headline the page with.
* The UI is the works, there's a ton you can do with it. The charting can be custom-emplaced, experiments can be archived, the table components are resizable...

## Hyperparameter search

* The optimizer is configured using a JSON fragment with a well-defined schema. The fragment basically states what the goals are, e.g. minimize this loss using these parameters.
* The resulting `Optimizer` object acts as a source for model parameters and (passthrough to the web UI) sink for model losses.
* If used via the CLI, the optimizer supports threads via the `-j` parameter.
* Search is supported via grid, random (preferable to grid, see my notes), and Bayesian search (via a specific algorithm).
