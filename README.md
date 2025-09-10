# Block_Assist_v1
Updated for crash prevention



# Step 1: Installization 
```
sudo apt update
sudo apt install -y \
  make build-essential gcc \
  libssl-dev zlib1g-dev libbz2-dev libreadline-dev \
  libsqlite3-dev libncursesw5-dev xz-utils tk-dev \
  libxml2-dev libxmlsec1-dev libffi-dev liblzma-dev \
  curl git unzip \
  libxi6 libxrender1 libxtst6 libxrandr2 libglu1-mesa libopenal1
```

# Step 2: Clone repo
```
git clone https://github.com/gensyn-ai/blockassist.git
cd blockassist
```

# Step 3: Install Node.js 20
```
# Remove existing Node.js
sudo apt remove -y nodejs npm
# Install NodeSource repository for Node.js 20
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
# Install Node.js 20
sudo apt install -y nodejs
# Verify installation
node --version  # Should show v20.x.x
```


# Step 4:  Install Yarn
```
curl -o- -L https://yarnpkg.com/install.sh | bash
export PATH="$HOME/.yarn/bin:$HOME/.config/yarn/global/node_modules/.bin:$PATH"
```


# Step 5: Install Java
```
./setup.sh
exec $SHELL
```


# Step 6: Install Pyenv
```
curl https://pyenv.run | bash

export PYENV_ROOT="$HOME/.pyenv"
export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init --path)"
eval "$(pyenv init -)"
```


# Step 7: Install Python 3.10
```
pyenv install 3.10
pyenv global 3.10
```


# Step 8: Install Python 3.10
```
# Upgrade pip
pip install --upgrade pip

# Install BlockAssist
pip install -e . --no-cache-dir
pip install "mbag-gensyn[malmo]" --no-cache-dir
pip install psutil readchar
```


# Step 9: Crash Prevention Setup

# 9.1 - Create permanent evaluation directory (prevents ValueError crash)
```
mkdir -p data/evaluation
touch data/evaluation/.keep
```
# 9.2 - Add HF token (prevents 401 upload errors) " add your hf token "
```
echo 'HF_TOKEN=hf_your_write_token_here' > .env
```
# 9.3 - Install PyTorch for RTX 3090/4090 + CUDA 12.8/12.9
```
pip install --upgrade pip
pip uninstall -y torch torchvision torchaudio
pip install --no-cache-dir torch torchvision torchaudio --extra-index-url https://download.pytorch.org/whl/cu121
```
# 9.4 - Install additional ML dependencies
```
pip install --no-cache-dir datasets evaluate accelerate
```
# 9.5 -(Mannual post-update BlockAssist)
OPEN NEW TAB
```cd modal-login```
```yarn install --frozen-lockfile```
```yarn dev``` 
SWITCH TO MAIN TAB AND USE NEXT COMMANDS FROM BLOCKASSIST DIRECTORY

# 9.6 - Clear any existing GPU zombies
```
sudo pkill -f "minecraft|java|malmo" 2>/dev/null || true
```


# Step 10: Verify Setup
```
# Verify Python environment
export PYENV_ROOT="$HOME/.pyenv"
export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init --path)"
eval "$(pyenv init -)"

# Check GPU recognition
python -c "import torch; print(f'CUDA available: {torch.cuda.is_available()}'); print(f'GPU: {torch.cuda.get_device_name(0) if torch.cuda.is_available() else None}')"

# Verify HF token
grep HF_TOKEN .env || echo "⚠️ ERROR: HF_TOKEN missing!"

# Check evaluation directory
ls data/evaluation/.keep || echo "⚠️ ERROR: Evaluation directory not set up!"

```


# Step 11: Launch BlockAssist
```
export PYENV_ROOT="$HOME/.pyenv"
export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init --path)"
eval "$(pyenv init -)"

python run.p
```
