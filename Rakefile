# -*- coding: utf-8 -*-

require 'rake/clean'

desc 'windows ping'
task :ping do
  sh "ansible -ki inventory/hosts windows -m win_ping"
end

desc 'windows facts'
task :facts do
  sh "ansible -ki inventory/hosts windows -m setup"
end

desc 'run playbook'
task :run do
  sh "ansible-playbook -ki inventory/hosts site.yml"
end

desc 'run final playbook'
task :fin do
  sh "ansible-playbook -ki inventory/hosts final.yml"
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
