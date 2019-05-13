# NAB-detectors

This is a companion repository for the [Numenta Anomaly Benchmark
(NAB)](https://github.com/numenta/NAB). If you are unfamiliar with NAB please
refer to the main repository first.

In 2019 the main body of the benchmark's code was ported to Python 3. The
Numenta detector code was outside the scope of that project and was moved here.

This repository can be used to replicate results listed on the scoreboard of
the main repository for the following detectors:

    numenta
    numentaTM
    htmjava  # See README.md in nab/detectors/htmjava for more additional setup

## Installation

### Initial requirements

You need to manually install the following:

- [Python 2.7](https://www.python.org/download/)
- [pip](https://pip.pypa.io/en/latest/installing.html)
- [NumPy](http://www.numpy.org/)
- [NuPIC](http://www.github.com/numenta/nupic)

### Clone this repository

`git clone git@github.com:numenta/NAB-detectors.git`

### Install the Python requirements

    cd NAB-detectors
    (sudo) pip install -r requirements.txt

This will install the required modules.

## Usage

**Note**: The following assumes you have a working Python 2.7 environment.

### NAB Benchmark Data 

The canonical benchmark data is stored in the main NAB repository. You will need to
clone the main NAB repository into a sibling directory.

    cd ..
    git clone git@github.com:numenta/NAB.git

### Running a Detector

One or more detectors can be run against the benchmark data by specifying
which detector or detectors to run and the path to the main NAB data
directory.

For example, to run the `numenta` detector move into the `NAB-detectors`
repository and run:

    cd NAB-detectors
    python run.py -d numenta --detect --rootDir path/to/NAB/

Note: By default `run.py` tries to use all the cores on your machine. The above
command should take about 20-30 minutes on a current powerful laptop with 4-8
cores.

For debugging you can run subsets of the data files by modifying and
specifying specific label files (see section below). Please type:

    python run.py --help

to see all the options.

Note that to replicate results exactly as in the NAB paper you may need to checkout
the specific version of NuPIC (and associated nupic.core) that is noted in the
[Scoreboard](https://github.com/numenta/NAB#additional-scores):

    cd /path/to/nupic/
    git checkout -b nab {TAG NAME}
    cd /path/to/nupic.core/
    git checkout -b nab {TAG NAME}

##### Run a subset of NAB data files

For debugging it is sometimes useful to be able to run your algorithm on a
subset of the NAB data files or on your own set of data files. You can do that
by creating a custom `combined_windows.json` file that only contains labels for
the files you want to run. This new file should be in exactly the same format as
`combined_windows.json` except it would only contain windows for the files you
are interested in.

**Example**: an example file containing two files is in
`labels/combined_windows_tiny.json`. (Part of the main NAB repository) The following 
command shows you how to run NAB on a subset of labels:

    cd /path/to/NAB-detectors
    python run.py -d numenta --detect --rootDir path/to/NAB --windowsFile labels/combined_windows_tiny.json

This will run the `detect` phase of NAB on the data files specified in the above
JSON file. Note that scoring and normalization are not supported with this
option. Note also that you may see warning messages regarding the lack of labels
for other files. You can ignore these warnings.

### Detector Output

After running a detector against the data the `path/to/NAB/results` directory
will be updated with the results for the detector run.
