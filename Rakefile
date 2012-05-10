require 'rubygems'
require 'rake'
require 'rake/testtask'
require 'rake/packagetask'
require 'rake/gempackagetask'

spec = eval(File.read('spree_core.gemspec'))
Rake::GemPackageTask.new(spec) do |p|
  p.gem_spec = spec
end

desc "Release to gemcutter"
task :release => :package do
  require 'rake/gemcutter'
  Rake::Gemcutter::Tasks.new(spec).define
  Rake::Task['gem:push'].invoke
end

desc "Default Task"
task :default => [:spec, :cucumber ]

desc "Regenerates a rails 3 app for testing"
task :test_app do
  require '../lib/generators/spree/test_app_generator'
  class CoreTestAppGenerator < Spree::Generators::TestAppGenerator

    def install_spree_core
      inside "test_app" do
        run 'bundle exec rake spree_core:install'
      end
    end

    def migrate_db
      run_migrations
    end

    protected
    def full_path_for_local_gems
      <<-gems
gem 'spree_core', :path => \'#{File.dirname(__FILE__)}\'
      gems
    end
  end
  CoreTestAppGenerator.start
end

ENV['SPREE_GEM_PATH'] = __FILE__
require File.expand_path("../../core/lib/tasks/common", __FILE__)
