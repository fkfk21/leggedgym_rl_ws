# Legged Gym RL workspace


mainly follow [here](https://github.com/leggedrobotics/legged_gym)

## System
OS: Ubuntu 22.04


## dependency

- [pyenv](https://github.com/pyenv/pyenv) (for virtual env)
- [poetry](https://python-poetry.org/) (for virtual env)
- [vcs-tools](https://github.com/dirk-thomas/vcstool) (for clone repositories)

<details><summary>dependency installation</summary>

### pyenv installation

This introduction follows [here](https://github.com/pyenv/pyenv?tab=readme-ov-file#installation)

For install pyenv
```bash
curl -fsSL https://pyenv.run | bash
```

For activate pyenv (depends on the shell you use: see [here](https://github.com/pyenv/pyenv?tab=readme-ov-file#b-set-up-your-shell-environment-for-pyenv))
```bash
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bashrc
echo '[[ -d $PYENV_ROOT/bin ]] && export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bashrc

# if needed
echo 'eval "$(pyenv init - bash)"' >> ~/.bashrc
```
if you want not to always activate pyenv, execute `eval $(pyenv init - bash)` just before `poetry install --no-root` 

### poetry installation

This introduction follows [here](https://python-poetry.org/docs/#installation)

Install [pipx](https://pipx.pypa.io/stable/installation/)
```bash
python3 -m pip install --user pipx
python3 -m pipx ensurepath
```

Install poetry
```bash
pipx install poetry
```
Version 1.7.1 is checked to work well in my environment
```bash
# if you want to install specific version
pipx install poetry==1.7.1
```

To create venv folder in the project, I recommend to execute this.
```bash
poetry config virtualenvs.in-project true
```


### vcs-tool installation

```bash
sudo apt install python3-vcstool
```
</details>


## Installation

1. Clone this repository
```bash
git clone git@github.com:fkfk21/leggedgym_rl_ws.git
```

2. Download Isaac Gym
- Access [here](https://developer.nvidia.com/isaac-gym/download)
- Download Isaac Gym (IsaacGym\_Preview\_4\_Package.tar.gz)
- Extract and mv isaacgym directory under leggedgym_rl_ws

3. Check the version of pytorch and CUDA
- Current pyproject.toml is written for pytorch==2.4.0, CUDA 12.1
- If you need to use different version, edit the versions of torch, torchvision, torchaudio and source url in pyproject.toml

4. Installation

Execute following commands.

```bash
cd leggedgym_rl_ws
vcs import . < leggedgym.repos
# (if needed execute ->) eval $(pyenv init - bash)
# (if needed execute ->) sudo apt install libreadline-dev

pyenv install 3.8.19
poetry install --no-root
```


## Training 

Activate virtual env
```bash
# for bash user
source `poetry env info --path`/bin/activate

# for fish shell user
source (poetry env info --path)/bin/activate.fish
```

### Start training
```bash
python legged_gym/legged_gym/scripts/train.py --task=anymal_c_flat
```

with options
```bash
python legged_gym/legged_gym/scripts/train.py --task=anymal_c_flat --num_envs 1024 --max_iterations 1000
```

to resume training
```bash
python legged_gym/legged_gym/scripts/train.py --task=anymal_c_flat --resume
```

to see other options
```bash
python legged_gym/legged_gym/scripts/train.py --help
```

#### Note
- to accelerate the learning, push `v` key to toggle rendering
- to see the learning curve, execute following command in other shell, and then open http://localhost:6006 in the browser.
```bash
poetry run tensorboard --logdir ./legged_gym/logs
```

### play training result 
```bash
python legged_gym/legged_gym/scripts/play.py --task=anymal_c_flat
```

