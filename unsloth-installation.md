# UnSloth Installation for LoRA Finetuning

I've recently been trying to finetune some Low-Rank Adaptors (LoRA) on open source large language models like llama3.1-8B and gemma-2B on custom instruct datasets.

An open-source library that can do this more efficiently tha HuggingFace's PEFT library is unsloth: <a href="https://github.com/unslothai/unsloth"><img src="https://github.com/unslothai/unsloth/raw/main/images/unsloth%20new%20logo.png" width="115"></a>

They claim a 2-3x faster finetuning with 30-40 percent lower memory requirements.

The actually provide fully featured Google colab and kaggle notebooks, of which I used one to finetune a llama3.1-8B model entirely within a single T4 GPU's memory.

However, I ran into a problem when trying to run these same notebooks on a local machine. At work I have access to a Intel Xeon based server with a 4090, but I was having trouble getting the correct dependencies to install so I can repeat my finetuning and test the resultant LoRA's with unsloth.

When you try to dig into the local installation instructions in their docs (https://docs.unsloth.ai/get-started/installation/pip-install), they currently only have instructions for pytorch versions upto 2.3, when currently the latest version is 2.4.0. This causes compatibility issues with the certain versions of the xformers library.

So I had to manually try out different combinations of dependencies in order to get unsloth to work correctly.

> As of writing this post, unsloth has updated their google colab notebooks to check for the pytorch version and install the correct xformers library version accordingly.

You now have two options:

1. Install dependencies and ensure a correct python environment to correspond with pytorch version 2.3, OR
2. Install dependencies according to pytorch 2.4 instead.

Let's see what both of these might look like.

## Suggested Pre-requisites

Before we get started , you should have a way to manage multiple python versions as well as a familiarity with the use of python virtual environments.

The pytorch 2.3.x version of unsloth is not compatible with Python 3.12+. I would recommend having at least a secondary install of Python 3.10.12 ready instead.

You can install and manage multiple Python versions using ```pyenv```:

```console
sudo apt update
sudo apt install -y make build-essential libssl-dev zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev wget curl llvm libncursesw5-dev xz-utils tk-dev libxml2-dev libxmlsec1-dev libffi-dev liblzma-dev
curl https://pyenv.run | bash
echo -e 'export PYENV_ROOT="$HOME/.pyenv"\nexport PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bashrc
echo -e 'eval "$(pyenv init --path)"\neval "$(pyenv init -)"' >> ~/.bashrc
```

exit and re-enter your bash shell and run 

```
pyenv
```

then,

```
pyenv install 3.10.12
```

Once you have python 3.10.12 installed, you can set a pyenv virtualenv virtual environment for a specific folder (your project folder) such that whenever you navigate to that directory, the specified pyenv virtual environment is used

```
pyenv virtualenv 3.10.12 <env-name>
```

then you can navigate to the folder you want this environment to be used in, and:

```
pyenv local <env-name>
```

will create a .python-version file in that directory.

Now whenever you enter that directory and exectue any pip install commands, the packages will be installed in the specified pyenv virtualenv environment

> You can also create a virtual environment using venv (python -m venv <env-name>, which will need to be activated every time you want to use it)

## Install Unsloth dependencies locally for PyTorch 2.3.x

This assumes you have a fresh Python 3.10.12 virtual environment active.

```
pip install  "torch=2.3.0"
pip install "xformers==0.0.27"
pip install triton
```
once these have installed, you can install unsloth via:

```
pip install --no-deps "unsloth[cu121-ampere-torch230] @ git+https://github.com/unslothai/unsloth.git"
```

To check if the installation has been done correctly, run the following commands, each should produce some output:

```
nvcc
python -m xformers.info
python -m bitsandbytes
```

## Install Unsloth dependencies locally for PyTorch 2.4.x

Coming Soon

## Troubleshooting

If for any reason, the rest of any unsloth notebook does not work (the loading of the base model and the loading of the peft version), here are some steps you can take.

In general, I prefer to delete the virtual environment and start again.

1. Make sure your python version, pytorch version, xformers version are all compatible.

The combination which works well for me is Python 3.10.12, PyTroch 2.3.0, xformers 0.0.27

2. Sometimes if you install the unsloth library first, later on you can get an error saying there is no module called "unsloth". You can try running the unsloth command again, and then seeing if the error reappears.


## References

1. https://medium.com/@adocquin/mastering-python-virtual-environments-with-pyenv-and-pyenv-virtualenv-c4e017c0b173

