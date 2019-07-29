# -*- coding: utf-8 -*-

desc 'windows ping'
task :ping do
  sh "ansible -k -i inventory/hosts windows -m win_ping"
end

desc 'windows facts'
task :facts do
  sh "ansible -k -i inventory/hosts windows -m setup"
end

# Local Variables:
# mode: Ruby
# indent-tabs-mode: nil
# End:
