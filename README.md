# epfl-shs-class
Set of instructions for using data in the frame of EPFL SHS class

## Part 1 - How to get the data

1. **Read and sign the NDA**, and give it back to the teachers.

2. **Generate a ssh-key**, following these [instructions](https://www.ssh.com/ssh/keygen/), and send the content of the `.ssh/id_rsa.pub` file to the teachers.
Important: put a password on your key

3. Once you receive the green light, **download the data**
     - open a  terminal and go to the repository where you want to download the data
     - `sftp impresso@dhlabsrv4.epfl.ch` => connect to the server via sftp (more instructions [here](https://www.tecmint.com/sftp-command-examples/)) 
     - `cd sharespace` => go in the folder where the data is
     - `ll`  => to see what is there
     - `mget *.bz2` to get all file ending with `.bz2`
     - `exit` to exit the server

## Part 2 - How to transform it

Data is in the form of `bz2` archives. These archives are on a journal-year basis, and contains newspaper articles, which have been 'rebuilt' from the OCR output. The format is `json-lines`: each line is a json object, i.e. an article.

Each article contains more information that what you need so it is a good idea to filter out things and get a version of what interests you only. In the folder where you have the archives, execute the following command:

`for f in *.bz2; do bzcat $f | jq -c '{id: .id, type: .tp, date: .d, title: .t, fulltext: .ft}' > "${f%.jsonl.bz2}-reduced.jsonl.bz2"`

what does the command do:
- iterate over the files having the suffix `bz2` (each file lies in the variable `$f`)
- open the archive (`bzcat`) and produce a stream of json
- send (pipe `|`) this stream into `jq` 
- apply some filtering on the json content
- send the output to a file which name is composed of the input file, completed with `-reduced`

You will now on work with the archives `-reduced.jsonl.bz2`. You can delete the others.

## Part 3 - Setting up your working environement 

### Python environment 

1. Download [Anaconda](https://www.anaconda.com/distribution/) in order to get the Conda environement manager.
2. Familiarize yourself with [Conda](https://conda.io/docs/user-guide/getting-started.html)
3. Open a terminal, go to your working repository and create an environement:
`conda create -n NAME python=3.6` where NAME is the name you want to give to the environement (e.g. digital-history)
4. Activate it:
`source activate NAME`

Useful commands (and more info [here](https://conda.io/docs/user-guide/tasks/manage-environments.html)):

```
conda info --envs => list your environments
source deactivate => deactivate an env
conda remove --name NAME --all => remove environment 'NAME'
```

### Working with Jupyter notebook

What it is: see this [tutorial](https://www.datacamp.com/community/tutorials/tutorial-jupyter-notebook) 

Conda already installs by default Jupyter when you create an environment. 

To launch a notebook, just execute this in your activated env:
`jupyter notebook`















