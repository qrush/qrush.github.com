desc "deploy site to litanyagainstfear.com"
task :deploy do
  require 'rubygems'
  require 'highline/import'
  require 'net/ssh'

  branch = "master"

  username = ask("Username:  ") { |q| q.echo = true }
  password = ask("Password:  ") { |q| q.echo = "*" }

  Net::SSH.start('litanyagainstfear.com', username, :port => 1337, :password => password) do |ssh|
    commands = "cd ~/litanyagainstfear/cached-copy; "
    branches.each do |branch|

      commands << <<EOF
git checkout #{branch} 
git pull origin #{branch}
git checkout -f
rm -rf _site
jekyll 
mv _site ../_#{branch}
mv ../#{branch} _old
mv ../_#{branch} ../#{branch}
rm -rf _old
EOF
    end
      commands = commands.gsub(/\n/, "; ")
      #puts commands
      ssh.exec commands
  end
end
