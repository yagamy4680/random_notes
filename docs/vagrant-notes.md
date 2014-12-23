### VirtualBox customization

customize the virtualbox at different timing:

```ruby
vb.customize('post-boot', ['guestcontrol', :id, 'exec', '--username', 'vagrant', 'pwd'])
vb.customize('post-comm', ['guestcontrol', :id, 'exec', '--username', 'vagrant', 'pwd'])
```

https://github.com/mitchellh/vagrant/blob/6a5fee01917d502e359f505eea91a716dfa5c916/plugins/providers/virtualbox/action.rb

```ruby
b.use Customize, "pre-boot"
b.use Boot
b.use Customize, "post-boot"
b.use WaitForCommunicator, [:starting, :running]
b.use Customize, "post-comm"
b.use CheckGuestAdditions
```

Guest-control via VBoxManage:
http://www.virtualbox.org/manual/ch08.html#vboxmanage-guestcontrol

```bash
# For Linux as guest
VBoxManage --nologo guestcontrol "My VM" execute --image "/bin/ls"
          --username foo --passwordfile bar.txt --wait-exit --wait-stdout -- -l /usr

# For Windows as guest
VBoxManage --nologo guestcontrol "My VM" execute --image "c:\\windows\\system32\\ipconfig.exe"
          --username foo --passwordfile bar.txt --wait-exit --wait-stdout
```


### Clean-up && Minimize box

https://github.com/ffuenf/vagrant-boxes/blob/master/packer/ubuntu-14.04-server-amd64/scripts/cleanup.sh
Minimize virtualbox image:

```bash
# Clean up
apt-get -y --purge remove linux-headers-$(uname -r) build-essential
apt-get -y --purge autoremove
apt-get -y purge $(dpkg --list |grep '^rc' |awk '{print $2}')
apt-get -y purge $(dpkg --list |egrep 'linux-image-[0-9]' |awk '{print $3,$2}' |sort -nr |tail -n +2 |grep -v $(uname -r) |awk '{ print $2}')
apt-get -y clean

# Cleanup Chef
rm -f /tmp/chef*deb
```

