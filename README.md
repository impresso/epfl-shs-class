# epfl-shs-class

Set of instructions for using data in the frame of EPFL SHS class.

## Part 1 - How to get the data

1. **Read and sign the NDA**, and give it back to the teachers.

2. **To get your credentials on SwitchEngine**:

  - communicate your email address to your professors
  - you will then receive by email a voucher (invitation) to join the `impresso-teaching` project, which contains the S3 bucket with the impresso data.
  - following the link in the invitation you will be able to create an account, and this will give you EC2 credentials (access key and secret key)
  - further links to Switch documentation:
    - https://help.switch.ch/eduid/switchdrive-with-edu-id/
    - https://help.switch.ch/engines/documentation/s3-like-object-storage/


3. **Install `s3cmd`**
  - with `brew`on Mac OS
  - with `sudo apt-get install s3cmd` on e.g. Ubuntu
  - configure it
    - copy the file `.s3cfg` in this repo to your home (e.g. `~/`)
    - add access_key and secret_key to `.s3cfg`
    - type `s3cmd ls`: you should get a list of all buckets in the project

4. **Download the data**
  - ```s3cmd get s3://impresso-data/* ~/home/impresso-data/```

## Part 2 - How to transform it

**NB**: before reading further, install [`jq`](https://github.com/stedolan/jq/wiki/Installation), in case it's not yet installed on your system.

Data is in the form of `bz2` archives. These archives are on a journal-year basis, and contains newspaper articles, which have been 'rebuilt' from the OCR output. The format is `json-lines`: each line is a json object, i.e. an article.

Each article contains more information that what you need so it is a good idea to filter out things and get a version of what interests you only. In the folder where you have the archives, execute the following command:

`for f in *[0-9].jsonl.bz2; do bzcat $f | jq -c '{id: .id, type: .tp, date: .d, title: .t, fulltext: .ft}' | bzip2 > "${f%.jsonl.bz2}-reduced.jsonl.bz2" ; done`

what does the command do:
- iterate over the files having the suffix `.jsonl.bz2` preceded by a number (each file lies in the variable `$f`)
- open the archive (`bzcat`) and produce a stream of json
- send (pipe `|`) this stream into `jq`
- apply some filtering on the json content
- send the output to a file which name is composed of the input file, completed with `-reduced`

You will now on work with the archives `-reduced.jsonl.bz2`. You can delete the others.

## Part 3 - Setting up your working environment

### Python environment

1. Download [Anaconda](https://www.anaconda.com/distribution/) in order to get the Conda environement manager.
2. Familiarize yourself with [Conda](https://conda.io/docs/user-guide/getting-started.html)
3. Open a terminal, go to your working repository and create an environement:
`conda create -n NAME python=3.6` where NAME is the name you want to give to the environement (e.g. digital-history)
4. Activate it:
`source activate NAME`
5. install dependencies with `pip install -r requirements.txt`

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

### Starting working with the data

We've put a jupyter notebook in this repo ([Example.ipynb](https://github.com/impresso/epfl-shs-class/blob/master/Example.ipynb)) where you can get an idea where to start.

If you want to use Iramuteq, you will have to isolate the textual parts and print them as specified [here](http://www.iramuteq.org/documentation/formatage-des-corpus-texte).
