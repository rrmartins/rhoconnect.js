require 'find'
require 'erb'
require 'rake/rdoctask'
require 'digest/sha2'
require 'rexml/document'

#Look, another big fat hack. Make it so we can remove tasks from rake -T by setting comment to nil
module Rake
  class Task
    attr_accessor :comment
  end
end

$app_basedir = pwd
chdir File.dirname(__FILE__)

ver = File.read("version.txt").chomp.gsub(/\./, "_").gsub(/,/, "_")

src_dir = "js"
build_dir = "build"
dist_dir = "distrib"

max_name = "rhosync-"+ver+".js"
min_name = "rhosync-"+ver+".min.js"
zip_name = "rhosync-"+ver+".zip"

desc "Build rhosync.js client package"
task :clean do
  rm_rf dist_dir
end


namespace "build" do

  desc "Build rhosync.js client package"
  task :rhosync_js do

    mkdir_p dist_dir

    #cp_r src_dir, tmp_dir, :preserve => true

    modnames = [
        "rhosync.js",
        "rhosync.common.js",
        "rhosync.protocol.js",
        "rhosync.domain.js",
        "rhosync.storage.js",
        "rhosync.engine.js",
        "rhosync.notify.js"
    ]

    File.open(dist_dir+"/"+max_name, "w") do |of|
      modnames.each do |modname|
        File.open(src_dir +"/" +modname, "r") do |mf|
          mf.readlines.each do |str|
            of.puts(str)
          end
        end
      end
    end

    max_pathname = dist_dir+"/"+max_name
    min_pathname = dist_dir+"/"+min_name
    zip_pathname = dist_dir+"/"+zip_name

    puts `java -jar #{build_dir}/google-compiler.jar --js #{max_pathname} --warning_level QUIET --js_output_file #{min_pathname}`
    puts `jar cvMf #{zip_pathname} -c #{dist_dir} #{max_name} #{min_name}`
  end
end
