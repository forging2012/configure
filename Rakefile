IGNORES   = %w(.gitignore .git)
IRREGULAR = {
  "files/sublime/*" => "~/.config/sublime-text-2/Packages/User/",
  "files/ssh_config" => "~/.ssh/config",
  'scripts' => '~/.scripts'
}

COPYFILES = {
  "files/xmonad.desktop" => '/usr/share/xsessions/xmonad.desktop',
  "files/environment" => "/etc/environment",
  "files/20-thinkpad.conf" => "/etc/X11/xorg.conf.d/20-thinkpad.conf",
  "files/rc.conf" => "/etc/rc.conf",
}

FILES = Dir.entries('.').select do |x|
  x !~ /^\.*$/ && x =~ /^\./ && !(IGNORES||[]).include?(x)
end.inject({}) do |s,x|
  s.merge({x.to_s => File.join(ENV['HOME'],x.to_s)})
end.merge(IRREGULAR)

task :make_dwm do
  system('cd dwm && make && sudo make install')
end

task :make_xmonad do
  system('xmonad --recompile')
end

def exec(str)
  FILES.map do |k,v|
    command = str.gsub('KEY',File.join(Dir.pwd,k)).gsub('VALUE',v)
    puts("\e[32m#{command}\e[0m")
    system(command)
  end

  COPYFILES.map do |k,v|
    command = "sudo cp --remove-destination #{k} #{v}"
    puts command
    system command
  end
end

task :install do
  exec("mkdir ~/.vim-tmp -p") unless File.exist?("#{ENV['HOME']}/.vim-tmp")
  exec("ln -nfs KEY VALUE")
  puts "\e[33mInstall Complete\e[0m"
  system("git clone http://github.com/gmarik/vundle.git ~/.vim/bundle/vundle") unless File.exist?(File.expand_path("~/.vim/bundle/vundle"))
end

task :remove do
  exec("rm -f VALUE")
  puts "\e[33mAll Removed\e[0m"
end
