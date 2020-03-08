Vagrant.configure(2) do |config|
  config.vm.box = "ubuntu/xenial64"
  config.vm.network "forwarded_port", guest:80, host:8080, auto_correct: true
  config.vm.synced_folder ".", "/var/www/html"  
config.vm.provider "virtualbox" do |vb|
  vb.memory = "1024"  
end
config.vm.provision "shell", inline: <<-SHELL
  # Packages vom lokalen Server holen
  # sudo sed -i -e"1i deb {{config.server}}/apt-mirror/mirror/archive.ubuntu.com/ubuntu xenial main restricted" /etc/apt/sources.list 
  sudo apt-get update
  sudo apt-get -y install apache2 
SHELL
end
config.vm.provision "shell", inline: <<-SHELL 
    # Debug ON!!!
    set -o xtrace
	sudo apt-get update
	sudo apt-get -y install debconf-utils 
	sudo apt-get -y install apache2 
	sudo debconf-set-selections <<< 'mysql-server mysql-server/root_password password admin'
	sudo debconf-set-selections <<< 'mysql-server mysql-server/root_password_again password admin'
	sudo apt-get -y install php libapache2-mod-php php-curl php-cli php-mysql php-gd mysql-client mysql-server 
	# Admininer SQL UI 
	sudo mkdir /usr/share/adminer
	sudo wget "http://www.adminer.org/latest.php" -O /usr/share/adminer/latest.php
	sudo ln -s /usr/share/adminer/latest.php /usr/share/adminer/adminer.php
	echo "Alias /adminer.php /usr/share/adminer/adminer.php" | sudo tee /etc/apache2/conf-available/adminer.conf
	sudo a2enconf adminer.conf 
	sudo service apache2 restart 
SHELL
end
