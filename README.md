dotdeploy
=========

Simple local deploy script for ruby app with thin server. Use, when you have access to the server an capistrano is too complex..

If it does not match to you directly, customize script youself or make pull request which will make it more generic.

Initially published at https://gist.github.com/aufi/488b0f7e4d56e624618f

Requirements
------------
  * bash, git, rails, thin, production environment assumed
  * thin server config located in #{Rails.root}/config/thin.yml

Installation
------------
```bash
#download script
wget https://raw.githubusercontent.com/aufi/dotdeploy/master/dotdeploy

#[if needed]
#add execute priviledge
chmod +x dotdeploy

#[if needed]
#move to some directory included in $PATH
sudo mv dotdeploy /usr/local/bin
```

Source
------
```bash
#!/bin/bash
#
# Script for semi-automated Rails app deploys locally on server
#
# See more at https://github.com/aufi/dotdeploy
#
set -e

export RAILS_ENV='production'

echo "Starting local deploy for $RAILS_ENV..."

echo "[1] updating sources..."
git pull

echo "[2] running bundle to install deps..."
bundle install

echo "[3] migrating schema..."
bundle exec rake db:migrate

echo "[4] compiling assets..."
bundle exec rake assets:precompile

echo "[5] restarting application server"
bundle exec thin -d -C ./config/thin.yml restart

echo "..deploy completed."

exit 0
```

TODO
----
make move universal server config files or allow other app servers restart simply
customize environment
