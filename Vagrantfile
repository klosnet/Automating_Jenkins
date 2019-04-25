#!/bin/sh -eux
# Name: Automating and Configuring Jenkins
# Author: Alex Kloster
#
# Description: Used to build Jenkins using a generic centos7 OS using vagrant and bash.

# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

$script = <<ENDSCRIPT
# Install the the EPEL repository.
  sudo yum --assumeyes --enablerepo=extras install epel-release;

  sudo yum --assumeyes update
  sudo yum --assumeyes upgrade
  sudo yum --assumeyes makecache fast

# Packages needed beyond a minimal install to build and run Jenkins.
  sudo yum --assumeyes install java-1.8.0-openjdk wget;

# Install Jenkins repo
  sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo

# Import Jenkins Key
  sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key

# Install Jenkins using repo
  sudo yum --assumeyes install jenkins
# wget http://mirrors.jenkins.io/war-stable/latest/jenkins.war
# sudo java -jar jenkins.war --httpPort=8000 &

# Copy of default Jenkins configuration
 #sudo cp /etc/sysconfig/jenkins /etc/sysconfig/jenkins.bak

# Add arguments to Jenkins to run on port 8000
  sudo printf "JENKINS_ARGS=--httpPort=8000" >> /etc/sysconfig/jenkins

# Start the Jenkis service using systemd
  sudo systemctl start jenkins.service

# Enable Jenkins to start on boot
  sudo systemctl enable jenkins.service

# Add port 8000 to firewalld, reload firewalld
  firewall-cmd --zone=public --add-port=8000/tcp --permanent
  firewall-cmd --zone=public --add-service=http --permanent
  firewall-cmd --reload

ENDSCRIPT

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  #config.vm.box = "puppetlabs/centos-7.0-64-puppet"
  config.vm.box = "generic/centos7"
config.vm.network "forwarded_port", guest: 8000, host: 8001
config.vm.network :private_network, ip: "192.168.33.10"
  config.vm.provision "shell", inline: $script
end
