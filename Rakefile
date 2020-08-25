# -*- coding: utf-8 -*-

require 'rake/clean'

task :inventory do
  win_ipaddr = nil
  File.open('/etc/resolv.conf') {|f|
    for line in f
      case (line)
      when /^nameserver /
        win_ipaddr = line.split[1]
      end
    end
  }
  IO.write('inventory/win_hosts', <<-EOF)
[windows]
win_local ansible_host=#{win_ipaddr}

[windows:vars]
ansible_user=toki
ansible_port=5986
ansible_connection=winrm
ansible_winrm_server_cert_validation=ignore
  EOF
end

desc 'windows ping'
task :ping => [ :inventory ] do
  sh "ansible -ki inventory/win_hosts windows -m win_ping"
end

desc 'windows facts'
task :facts => [ :inventory ] do
  sh "ansible -ki inventory/win_hosts windows -m setup"
end

desc 'run playbook'
task :run => [ :inventory ] do
  sh "ansible-playbook -ki inventory/win_hosts site.yml"
end

desc 'run final playbook'
task :fin => [ :inventory ] do
  sh "ansible-playbook -ki inventory/win_hosts final.yml"
end

desc 'converto README markdown to html'
task :readme => [ 'README.html' ]

file 'README.html' => [ 'README.md' ] do
  sh "pandoc --from=markdown --to=html5 --standalone --self-contained --css=$HOME/.pandoc/github.css --output=README.html README.md"
end
CLOBBER.include 'README.html'

# Local Variables:
# mode: Ruby
# indent-tabs-mode: nil
# End:
