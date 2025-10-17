📋 System Requirements

RAM: 12 GB minimum (32 GB recommended)
Storage: 100 GB minimum
GPU: RTX 3060 or better (RTX 4090 recommended)
CUDA: 12.1+ (CUDA 12.8 fully supported)
OS: Ubuntu 22.04 LTS or compatible Linux distribution
Internet: Stable connection required

# 🔑 Prerequisites (Before Starting)

1. Create Hugging Face Token
Go to https://huggingface.co/settings/tokens
Click "New token"
Name it (e.g., "blockassist")
Set permission to "Write" (CRITICAL)
Copy and save your token (starts with hf_)

2. Gensyn Testnet Account
Will be created automatically on first login
No action needed now

🚀 Installation Guide

# Step 1: Install System Dependencies
Update system and install required packages:

```
sudo apt update && sudo apt install -y \
  make build-essential gcc \
  libssl-dev zlib1g-dev libbz2-dev libreadline-dev \
  libsqlite3-dev libncursesw5-dev xz-utils tk-dev \
  libxml2-dev libxmlsec1-dev libffi-dev liblzma-dev \
  curl git unzip zip \
  libxi6 libxrender1 libxtst6 libxrandr2 libglu1-mesa libopenal1
```

# Step 2: Clone BlockAssist Repository
```
git clone https://github.com/gensyn-ai/blockassist.git
cd blockassist
Verify you're in the blockassist directory:
```

# Step 3: Install Node.js 20.x
```
# Remove existing Node.js (if any)
sudo apt remove -y nodejs npm

# Add NodeSource repository for Node.js 20
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -

# Install Node.js 20
sudo apt install -y nodejs

# Verify installation
node --version  # Should show: v20.x.x
npm --version   # Should show: 10.x.x
```

# Step 4: Install Yarn Package Manager
```
curl -o- -L https://yarnpkg.com/install.sh | bash

# Add Yarn to PATH
export PATH="$HOME/.yarn/bin:$HOME/.config/yarn/global/node_modules/.bin:$PATH"

# Verify installation
yarn --version  # Should show: 1.22.x
```

# Step 5: Install Java 1.8.0_152
BlockAssist requires Java 8 for Minecraft/Malmo:
```
./setup.sh
exec $SHELL
```
Verify Java installation:
```
java -version  # Should show: 1.8.0_152
```

# Step 6: Install Pyenv (Python Version Manager)
```
curl https://pyenv.run | bash
```
Configure pyenv environment variables:

```
export PYENV_ROOT="$HOME/.pyenv"
export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init --path)"
eval "$(pyenv init -)"
```
Make pyenv permanent (add to shell config):
```
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bashrc
echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(pyenv init --path)"' >> ~/.bashrc
echo 'eval "$(pyenv init -)"' >> ~/.bashrc

# Reload shell
source ~/.bashrc
```

Verify pyenv:
```
pyenv --version  # Should show: pyenv 2.x.x
```

# Step 7: Install Python 3.10
```
# Install Python 3.10.13 (latest stable 3.10)
pyenv install 3.10.13

# Set Python 3.10.13 as global default
pyenv global 3.10.13

# Verify installation
python --version  # Should show: Python 3.10.13
which python      # Should show: /root/.pyenv/shims/python
```

# Step 8: Install Base Python Packages
Only install minimal packages - run.py will auto-install the rest:

```
# Upgrade pip
pip install --upgrade pip

# Install ONLY base requirements
pip install psutil readchar rich

# Verify installations
pip list | grep -E "psutil|readchar|rich"
```

⚠️ IMPORTANT: Do NOT manually install:

pip install -e . (run.py handles this)
pip install "mbag-gensyn[malmo]" (run.py handles this)
PyTorch packages (optional, see Step 9 if needed)

# Step 9: Crash Prevention & Optimization
9.1 - Create Evaluation Directory
Prevents ValueError crash on first run:

```
mkdir -p data/evaluation
touch data/evaluation/.keep

# Verify directory exists
ls -la data/evaluation/
```

9.2 - Setup HuggingFace Token
Create .env file with your token:
```
echo 'HF_TOKEN=hf_your_token_here' > .env
⚠️ Replace hf_your_token_here with your actual token from Prerequisites
```

Verify token is saved:
```
cat .env  # Should show: HF_TOKEN=hf_...
```

9.3 - Install PyTorch for RTX 4090 (OPTIONAL)
Note: run.py auto-installs PyTorch, but you can pre-install for RTX 4090 optimization:
```
pip install --no-cache-dir torch torchvision torchaudio \
  --index-url https://download.pytorch.org/whl/cu121
```

Why cu121 for CUDA 12.8?
PyTorch cu121 (CUDA 12.1) is backward/forward compatible with CUDA 12.8
PyTorch doesn't officially support CUDA 12.8 yet (only up to 12.6)
cu121 is the recommended version for CUDA 12.x systems

Verify PyTorch installation:
```
python -c "import torch; print(f'CUDA available: {torch.cuda.is_available()}'); print(f'GPU: {torch.cuda.get_device_name(0) if torch.cuda.is_available() else None}')"
```
Expected output:

`
CUDA available: True
GPU: NVIDIA GeForce RTX 4090
`

9.4 - Install Additional ML Dependencies (OPTIONAL)
```
pip install --no-cache-dir datasets evaluate accelerate
```

9.5 - Clear GPU Zombies (For Vast AI VMs)
```
sudo pkill -f "minecraft|java|malmo" 2>/dev/null || true
```

# Step 10: Verify Complete Setup
Run comprehensive verification:

```
echo "=== System Verification ==="
echo ""

# Python environment
echo "1. Python Version:"
python --version
echo ""

# Pyenv
echo "2. Pyenv Status:"
pyenv versions
echo ""

# Java
echo "3. Java Version:"
java -version
echo ""

# Node.js & Yarn
echo "4. Node.js & Yarn:"
node --version
yarn --version
echo ""

# GPU Check
echo "5. GPU Detection:"
python -c "import torch; print(f'CUDA available: {torch.cuda.is_available()}'); print(f'GPU: {torch.cuda.get_device_name(0) if torch.cuda.is_available() else None}')" 2>/dev/null || echo "PyTorch not installed (will auto-install on first run)"
echo ""

# HF Token Check
echo "6. HuggingFace Token:"
if grep -q HF_TOKEN .env 2>/dev/null; then
    echo "✅ HF_TOKEN configured in .env"
else
    echo "⚠️ WARNING: HF_TOKEN not found in .env"
fi
echo ""

# Evaluation Directory
echo "7. Evaluation Directory:"
if [ -d "data/evaluation" ]; then
    echo "✅ data/evaluation directory exists"
else
    echo "⚠️ WARNING: data/evaluation directory missing"
fi
echo ""

echo "=== Verification Complete ==="
```

# Step 11: Launch BlockAssist
11.1 - Activate Python Environment
```
# Ensure you're in blockassist directory
cd ~/blockassist  # or wherever you cloned it

# Load pyenv environment
export PYENV_ROOT="$HOME/.pyenv"
export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init --path)"
eval "$(pyenv init -)"
```

11.2 - Run BlockAssist
```
python run.py
```

# 11.3 - First Run: What to Expect
Stage 1: Dependency Installation (2-5 minutes)

run.py will auto-install remaining dependencies

You'll see packages being downloaded

DO NOT INTERRUPT THIS PROCESS

Stage 2: Modal-Login (Browser Authentication)

A browser window will open automatically at http://localhost:3000

If auto-open fails, manually open browser and visit http://localhost:3000

Log in with your Gensyn Testnet credentials

If first time, create account

# ⚠️ TROUBLESHOOTING Modal-Login:
If modal-login fails to start automatically:

Open a NEW TERMINAL TAB (keep first tab running)

Run:

```
cd ~/blockassist/modal-login
yarn install --frozen-lockfile
yarn dev
```

Keep this tab running
Return to main terminal tab
The browser login should now work

Stage 3: HuggingFace Token

If .env is configured (Step 9.2), token is used automatically
If not, you'll be prompted to paste your token manually
Stage 4: Minecraft Loading (3-5 minutes)

Two Minecraft windows will launch
First window = Your gameplay
Second window = AI observer (minimized)

Wait until fully loaded

Stage 5: Gameplay

Terminal will prompt: "Press ENTER to start episode"
Press ENTER
Click on first Minecraft window
Press ENTER again in game to enable controls
Complete the building task:
Left-click with pickaxe: Remove blocks
Left-click with dirt: Place on red tiles
Right-click with stone/glass/planks: Place on placeholders
Progress bar shows completion percentage

Stage 6: End Episode

Return to terminal
Press ENTER to end session
Training will begin automatically (5-10 minutes)

Stage 7: Model Upload

Model uploads to HuggingFace
Submission to Gensyn Testnet
Check logs: tail -f logs/blockassist-train.log

