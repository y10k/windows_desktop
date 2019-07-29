# -*- coding: utf-8 -*-

desc 'windows ping'
task :ping do
  sh "ansible -k -i inventory/hosts windows -m win_ping"
end

desc 'windows facts'
task :facts do
  sh "ansible -k -i inventory/hosts windows -m setup"
end

desc 'run playbook'
task :run do
  sh "ansible-playbook -k -i inventory/hosts site.yml"
end

# Local Variables:
# mode: Ruby
# indent-tabs-mode: nil
# End:
