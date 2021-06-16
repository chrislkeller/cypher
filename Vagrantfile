# NPR Apps Vagrant definitions

# provisioning (run as sudo)
$root_sh = <<-ROOT
sudo apt-get update && sudo apt-get upgrade

sudo apt install -y curl wget git-core gcc make zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev libssl-dev

sudo apt-get install -y python python-pip python3 python3-pip git poppler-utils imagemagick mongodb postgresql

ROOT

# add cartography libraries and tools
$gdal_sh = <<-GDAL
sudo apt-get install gdal-bin
GDAL

# non-sudo provisioning (mostly Node)
$user_sh = <<-USER
# # install NVM
# curl -s -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.34.0/install.sh | bash
# \. ~/.nvm/nvm.sh
# nvm install stable
# # npm i -g grunt-cli grunt-init mapshaper topojson
USER

$python_sh = <<-PYTHON
git clone https://github.com/pyenv/pyenv.git $HOME/.pyenv
echo '\n# python path' >> ~/.bashrc
echo 'export PYTHONPATH=$HOME/.pyenv/shims/python' >> ~/.bashrc
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bashrc
echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(pyenv init -)"' >> ~/.bashrc
echo '\n'
source ~/.bashrc
# pyenv install -v 3.7.5
# pyenv install -v 2.7.17
PYTHON

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/trusty64"
  config.vm.synced_folder Dir.home, "/home/vagrant/host"
  config.vm.network "private_network", ip: "192.168.187.187"
  # config.vm.network "forwarded_port", guest: 8000, host: 8000
  config.vm.network "forwarded_port", guest: 7777, host: 7777
  config.ssh.insert_key = false
  config.disksize.size = "30GB"

  # standard setup
  config.vm.provision "root", type: "shell", inline: $root_sh
  # config.vm.provision "user", type: "shell", inline: $user_sh, privileged: false
  # config.vm.provision "python", type: "shell", inline: $python_sh, privileged: false

  # extra provisioning
  # config.vm.provision "gdal", type: "shell", inline: $gdal_sh, run: "never"
end
