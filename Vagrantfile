Vagrant.configure('2') do |config|
  config.vm.box = 'ubuntu/focal64'

  config.vm.hostname = 'myvagrant'

  config.vm.network :private_network, ip: '192.168.60.10'
  config.vm.network 'forwarded_port', guest: 3000, host: 3000 # frontComponent
  config.vm.network 'forwarded_port', guest: 3010, host: 3010 # 

  config.vm.provider :virtualbox do |vb|
    vb.gui = false
    vb.cpus = 2
    vb.memory = 4096
    vb.customize ['modifyvm', :id, '--natdnsproxy1', 'off']
    vb.customize ['modifyvm', :id, '--natdnshostresolver1', 'off']
  end

  config.vm.synced_folder ".", "/vagrant", disabled: true

  config.vm.provision "file", source: "~/.ssh/id_rsa", destination: "~/.ssh/id_rsa"
  config.vm.provision "file", source: "~/.ssh/id_rsa.pub", destination: "~/.ssh/id_rsa.pub"

  config.vm.provision 'shell', inline: <<-SHELL
    echo "fs.inotify.max_user_watches = 65536" >> /etc/sysctl.conf && sysctl -p

    # vscodeのremote developmentで開発するためvagrant内でもgitを使えるようにする
    apt-get install git

    # time zoneを東京にする
    timedatectl set-timezone Asia/Tokyo

    curl -fsSL https://get.docker.com -o get-docker.sh
    sh get-docker.sh
    usermod -aG docker vagrant

    # dockerを更新する場合にはverを調整すること
    curl -L "https://github.com/docker/compose/releases/download/1.27.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
    chmod +x /usr/local/bin/docker-compose
  SHELL
end
