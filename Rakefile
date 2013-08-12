require 'resque/tasks'

Rake::Task['resque:work'].enhance [:setting_redis_prereq]

task :setting_redis_prereq do
  require 'resque'
  Resque.redis = Redis.new(:host => ENV['OPENSHIFT_REDIS_HOST'], :port => ENV['OPENSHIFT_REDIS_PORT'], :password => ENV['REDIS_PASSWORD'], :thread_safe => true)
  Dir["#{ENV['OPENSHIFT_REPO_DIR']}/resque/*"].each {|file| require file}
end
