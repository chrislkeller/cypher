# Vagrant definitions based on NPR Apps configuration

# provisioning (run as sudo)
$root_sh = <<-ROOT
sudo apt update -y && sudo apt upgrade -y
sudo apt install -y curl wget git gcc make zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev libssl-dev
sudo apt install -y python3 python3-pip python-is-python3
sudo apt upgrade -y
ROOT

# add packages
$packages_sh = <<-PACKAGES
sudo apt install -y poppler-utils imagemagick
PACKAGES

# add python
$python_sh = <<-PYTHON
sudo apt update && sudo apt upgrade -y
git clone https://github.com/pyenv/pyenv.git $HOME/.pyenv
echo '\n# python path' >> ~/.profile
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.profile
echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.profile
echo 'eval "$(pyenv init --path)"' >> ~/.profile
echo '\n'
source ~/.profile
pyenv install -v 3.9.0
pyenv install -v 3.7.5
pyenv install -v 2.7.17
pyenv global 3.9.0
pip install pipenv
PYTHON

# add databases
$dbs_sh = <<-DBS
sudo apt install -y mongodb postgresql mysql-server
DBS

# add cartography libraries and tools
$gdal_sh = <<-GDAL
sudo apt update
sudo apt install -y gdal-bin
GDAL

# add node.js
$node_sh = <<-NODE
# install NVM
curl -s -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.34.0/install.sh | bash
\. ~/.nvm/nvm.sh
nvm install stable
npm i -g mapshaper topojson
NODE

# $user_sh = <<-USER

# USER

Vagrant.configure("2") do |config|
  # Ubuntu 20.04.2.0 LTS (Focal Fossa)
  config.vm.box = "ubuntu/focal64"
  config.vm.synced_folder Dir.home, "/home/vagrant/host"
  config.vm.network "private_network", ip: "192.168.187.187"
  config.vm.network "forwarded_port", guest: 7777, host: 7777
  config.ssh.insert_key = false
  config.disksize.size = "30GB"

  # standard setup
  config.vm.provision "root", type: "shell", inline: $root_sh
  config.vm.provision "user", type: "shell", inline: $packages_sh, privileged: false

  # extra provisioning
  config.vm.provision "python", type: "shell", inline: $python_sh, privileged: false
#   config.vm.provision "databases", type: "shell", inline: $dbs_sh, privileged: false

  config.vm.provision "gdal", type: "shell", inline: $gdal_sh, privileged: false
  config.vm.provision "node", type: "shell", inline: $node_sh, privileged: false
end
