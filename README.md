# Better-OIG-Together

## Data Model

How should we think about the training data for OIG bots? A _training set_ is a _set_ of _slices_, 
where each _slice_ contains a set of (input, output) pairs. Each slice corresponds to one file 
in the `data` folder.

For example, if the data folder contains
```
data
|- pile.yaml
|- soda.yaml
```
during training, the training set will contain the union of both `pile` and `soda`.
Note that different slices can be weighted differently, which will be specified in 
the file `training.yaml` (see "Model Training" for details)

### Data Format

You can provide data in various formats. 

1. You can provide a collection of input/output pairs
```
IOPairs:
- input: INPUT TEXT STRING
  output: OUTPUT TEXT STRING
- input: INPUT TEXT STRING
  output: OUTPUT TEXT STRING
...
```

2. You can provide us the link to your dataset on HuggingFace
```
HuggingFace:
- link: LINK TO YOUR DATASET
```

3. You can prepare your dataset as in OpenAI jsonl format (https://platform.openai.com/docs/guides/fine-tuning) 
and put it in a link that we can `wget` or `curl`
```
OpenAIJsonl:
- link: LINK TO YOUR DATASET
```

## Model Training

Each merged pull request will trigger (currently manually) to the training of a model. 
Hyper-parameters, including the specific mixture of data, will be specified in `training.yaml`:
```
Training:
  - lr: 0.0001
  - momentum: 0.99
Mixture:
  - pile: 0.5
  - soda: 0.5
```
After training, a file `training_log` will be committed to the repository. And a file 
`model.yaml` will be made available in the repository specifying where to find this model
and (optionally) Together API end-point to query such a model.

## How to Contribute?

You can help us to make OIG better in three ways.

### Finding "Bugs"

If you realize that the bug is not performing well, please open an Issue, specifying 
your input and the bot's output.

### Fixing "Bugs"

If you have data that you believe could be useful to fix some of the issues, please
add your data into the `data` folder and make a pull request associated with the Issue
that you think this will fix.

We will review these pull requests, train a model, and merge them.

### Specialization

You don't have to always merge into the main branch. If you have specific things to 
try out (e.g., a `text2sql` bot), feel free to open a new branch work there!



