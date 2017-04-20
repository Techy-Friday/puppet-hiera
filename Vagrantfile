# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.

Vagrant.configure(2) do |config|

NAME = 'VUL HIER JE NAAM IN'

  $install_puppet = <<-'EOS'
    $url = "http://pm.puppetlabs.com/puppet-agent/2016.4.3/1.7.2/repos/windows/puppet-agent-1.7.2-x64.msi"
    $msi = "C:\Windows\Temp\puppet.msi"
    if ( ![System.IO.File]::Exists( $msi )) {
      (New-Object System.Net.WebClient).DownloadFile($url, $msi)
      msiexec /qn /norestart /i $msi
    }
  EOS

  $install_git = <<-'EOS'
    $url = "https://github.com/git-for-windows/git/releases/download/v2.12.0.windows.1/Git-2.12.0-64-bit.exe"
    $exe = "C:\Windows\Temp\git.exe"
    if ( ![System.IO.File]::Exists( $exe )) {
      (New-Object System.Net.WebClient).DownloadFile($url, $exe)
      & $exe /SILENT /COMPONENTS="icons,ext\reg\shellhere,assoc,assoc_sh"
    }
  EOS

  $install_sublime = <<-'EOS'
    $url = "https://download.sublimetext.com/Sublime%20Text%20Build%203126%20x64%20Setup.exe"
    $exe = "C:\Windows\Temp\Sublime%20Text%20Build%203126%20x64%20Setup.exe"
    if ( ![System.IO.File]::Exists( $exe )) {
      (New-Object System.Net.WebClient).DownloadFile($url, $exe)
      & $exe /SILENT /COMPONENTS="icons,ext\reg\shellhere,assoc,assoc_sh"
    }
  EOS

  $config_puppet = <<-EOS
    echo "[main]" > 'C:\\Tmp\\puppet.conf'
    echo "certname = #{NAME}-win2k12r2" >> 'C:\\Tmp\\puppet.conf'
    echo "server = puppet" >> 'C:\\Tmp\\puppet.conf'
    echo "environment = production" >> 'C:\\Tmp\\puppet.conf'
    echo "runinterval = 1h" >> 'C:\\Tmp\\puppet.conf'
    cat 'C:\\Tmp\\puppet.conf' | Out-File -Encoding ascii 'C:\\ProgramData\\PuppetLabs\\puppet\\etc\\puppet.conf'

    puppet apply -e "host { 'puppet': ensure => 'present', ip => '192.168.1.3', }"
  EOS

  $install_centos = <<-EOS
    sudo yum update
    sudo yum -y install wget vim git
    echo "[main]" > /etc/puppetlabs/puppet/puppet.conf
    echo "certname = #{NAME}-centos7" >> /etc/puppetlabs/puppet/puppet.conf
    echo "server = puppet" >> /etc/puppetlabs/puppet/puppet.conf
    echo "environment = production" >> /etc/puppetlabs/puppet/puppet.conf
    echo "runinterval = 1h" >> /etc/puppetlabs/puppet/puppet.conf
    sudo /opt/puppetlabs/bin/puppet apply -e "host { 'puppet': ensure => 'present', ip => '192.168.1.3', }"
  EOS

  $install_ubuntu = <<-EOS
    sudo apt-get -y update
    sudo apt-get -y install git vim wget
    echo "[main]" > /etc/puppetlabs/puppet/puppet.conf
    echo "certname = #{NAME}-ubuntu" >> /etc/puppetlabs/puppet/puppet.conf
    echo "server = puppet" >> /etc/puppetlabs/puppet/puppet.conf
    echo "environment = production" >> /etc/puppetlabs/puppet/puppet.conf
    echo "runinterval = 1h" >> /etc/puppetlabs/puppet/puppet.conf
    sudo /opt/puppetlabs/bin/puppet apply -e "host { 'puppet': ensure => 'present', ip => '192.168.1.3', }"
  EOS

  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://atlas.hashicorp.com/search.

  config.vm.define "ubuntu" do |ubuntu|
    ubuntu.vm.box = "puppetlabs/ubuntu-16.04-64-puppet"
    ubuntu.vm.hostname = "#{NAME}-ubuntu"
    ubuntu.vm.box_check_update = false
    ubuntu.vm.provision "shell", inline: $install_ubuntu
    ubuntu.vm.network "public_network"
    ubuntu.vm.network "forwarded_port", guest: 22, host: 2222, id: "ssh"
    ubuntu.vm.network "forwarded_port", guest: 80, host: 8080, id: "http"
  end
  config.vm.define "centos7" do |centos7|
    centos7.vm.box = "puppetlabs/centos-7.2-64-puppet"
    centos7.vm.hostname = "#{NAME}-centos7"
    centos7.vm.box_check_update = false
    centos7.vm.provision "shell", inline: $install_centos
    centos7.vm.network "public_network"
    centos7.vm.network "forwarded_port", guest: 22, host: 2232, id: "ssh"
    centos7.vm.network "forwarded_port", guest: 80, host: 8085, id: "http"
  end
  config.vm.define "win" do |win2k12r2|
    win2k12r2.vm.box = "opentable/win-2012r2-standard-amd64-nocm"
    win2k12r2.vm.hostname = "#{NAME}-W2k12"
    win2k12r2.vm.box_check_update = false
#    win2k12r2.vm.network "public_network"
    win2k12r2.vm.network "forwarded_port", guest: 80, host: 8090, id: "http"
    win2k12r2.vm.network "forwarded_port", guest: 3389, host: 33389, id: "rdp"
    win2k12r2.vm.provision "puppet", type: 'shell', privileged: false, inline: $install_puppet
    win2k12r2.vm.provision "git", type: 'shell', privileged: false, inline: $install_git
    win2k12r2.vm.provision "sublime", type: 'shell', privileged: false, inline: $install_sublime
    win2k12r2.vm.provision "config", type: 'shell', privileged: false, inline: $config_puppet
  end
end
