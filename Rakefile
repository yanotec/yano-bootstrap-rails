require "bundler/gem_tasks"
task :default => :spec

# path to your application root.
GEM_ROOT = Pathname.new File.expand_path('../',  __FILE__)
asset_path = Pathname.new File.expand_path('vendor/assets/',  GEM_ROOT)

desc "Update all assets"
task :update => %w(update:bootstrap update:font_awesome)

namespace :update do
  desc "Update twitter bootstrap assets"
  task :bootstrap do
    version = Yano::Bootstrap::Rails::BOOTSTRAP_VERSION

    Dir.chdir asset_path do
      puts `pwd`
      puts "Cleaning temp folder"
      ["rm -rf ", "mkdir -p "].each do |cmd|
        puts "#{cmd} ./tmp ./stylesheets/bootstrap ./javascripts/bootstrap"
        puts `#{cmd} ./tmp ./stylesheets/bootstrap ./javascripts/bootstrap`
      end
      puts "Downloading bootstrap.zip"
      puts "wget -O ./tmp/bootstrap.zip https://github.com/twbs/bootstrap/archive/v#{version}.zip"
      puts `wget -O ./tmp/bootstrap.zip https://github.com/twbs/bootstrap/archive/v#{version}.zip`
      puts "Unziping bootstrap.zip"
      puts `unzip -d ./tmp ./tmp/bootstrap.zip`
      puts "Copy js assets"
      puts "cp ./tmp/bootstrap-#{version}/dist/js/bootstrap*.js javascripts/bootstrap/"
      puts `cp ./tmp/bootstrap-#{version}/dist/js/bootstrap*.js javascripts/bootstrap/`

      puts "Copy font assets"
      puts "rm -rf fonts/glyphicons-halflings-regular.*"
      puts `rm -rf fonts/glyphicons-halflings-regular.*`

      fontnames = Dir["./tmp/bootstrap-#{version}/dist/fonts/*"].map do |path|
        filename = File.basename path

        puts "rm -f fonts/#{filename}"
        puts `rm -f fonts/#{filename}`
        puts "cp #{path} fonts/#{filename}"
        puts `cp #{path} fonts/#{filename}`

        filename
      end

      puts "Copy css assets"
      Dir["./tmp/bootstrap-#{version}/dist/css/*.css"].each do |file_path|
        filename = File.basename file_path

        File.open(file_path, 'r') do |file|
          File.open("./stylesheets/bootstrap/#{filename}.erb", 'w') do |new_file|
            while (line = file.gets)
              if line =~ /url/i
                fontnames.each do |font|
                  line = line.gsub("url(\"../fonts/#{font}", "url(\"<%= asset_path '#{font}' %>")
                  line = line.gsub("url('../fonts/#{font}", "url('<%= asset_path '#{font}' %>")
                end

                line = line.gsub("#{filename}.map", "<%= asset_path '#{filename}.map' %>")
              end

              new_file.puts line
            end # while gets
          end # write file css.erb
        end # open file css

        puts "cp #{file_path}.map source_maps/#{filename}.map"
        puts `cp #{file_path}.map source_maps/#{filename}.map`
      end

      puts `rm -rf ./tmp`
    end

    puts "\e[32mDone!\e[0m"
  end

  desc "Update jQuery UI assets"
  task :font_awesome do
    version = Yano::Bootstrap::Rails::FONT_AWESOME_VERSION

    Dir.chdir asset_path do
      puts `pwd`
      puts "Cleaning temp folder"
      ["rm -rf ", "mkdir -p "].each do |cmd|
        puts "#{cmd} ./tmp ./stylesheets/font_awesome"
        puts `#{cmd} ./tmp ./stylesheets/font_awesome`
      end

      puts "Downloading font_awesome.zip"
      puts "wget -O ./tmp/font_awesome.zip https://github.com/FortAwesome/Font-Awesome/archive/v#{version}.zip"
      puts `wget -O ./tmp/font_awesome.zip https://github.com/FortAwesome/Font-Awesome/archive/v#{version}.zip`

      puts "Unziping font_awesome.zip"
      puts `unzip -d ./tmp ./tmp/font_awesome.zip`

      puts "Copy font assets"
      puts "rm -rf fonts/*ont*wesome*"
      puts `rm -rf fonts/*ont*wesome*`

      fontnames = Dir["./tmp/Font-Awesome-#{version}/fonts/*"].map do |path|
        filename = File.basename path

        puts "rm -f fonts/#{filename}"
        puts `rm -f fonts/#{filename}`
        puts "cp #{path} fonts/#{filename}"
        puts `cp #{path} fonts/#{filename}`

        filename
      end

      puts "Copy css assets"
      Dir["./tmp/Font-Awesome-#{version}/css/*.css"].each do |file_path|
        filename = File.basename(file_path)

        File.open(file_path, 'r') do |file|
          File.open("./stylesheets/font_awesome/#{filename}.erb", 'w') do |new_file|
            while (line = file.gets)
              if line =~ /url/i
                fontnames.each do |font|
                  line = line.gsub("url(\"../fonts/#{font}", "url(\"<%= asset_path '#{font}' %>")
                  line = line.gsub("url('../fonts/#{font}", "url('<%= asset_path '#{font}' %>")
                end

                line = line.gsub("#{filename}.map", "<%= asset_path '#{filename}.map' %>")
              end

              new_file.puts line
            end # while gets
          end # write file css.erb
        end # open file css

        puts "cp #{file_path}.map source_maps/#{filename}.map"
        puts `cp #{file_path}.map source_maps/#{filename}.map`
      end

      puts `rm -rf ./tmp`
    end

    puts "\e[32mDone!\e[0m"
  end
end
