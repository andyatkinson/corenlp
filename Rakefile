require "bundler/gem_tasks"

require 'rake/testtask'
Rake::TestTask.new do |t|
  t.libs << "test"
  t.test_files = FileList['test/test_helper.rb', 'test/*_test.rb']
  t.verbose = true
end

task :default => :test

import "./lib/tasks/downloader.rake"
