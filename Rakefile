require 'resque/tasks'

# first run setting_prereq then resque:work
Rake::Task['resque:work'].enhance [:setting_prereq]

task :setting_prereq do
  require 'resque'
  require 'logger'

  Resque.redis = Redis.new(:host => ENV['OPENSHIFT_REDIS_HOST'], :port => ENV['OPENSHIFT_REDIS_PORT'], :password => ENV['REDIS_PASSWORD'], :thread_safe => true)

  unless ENV['LOG'].nil?
    Resque.logger = Logger.new(ENV['LOG'])
    Resque.logger.level = Logger::DEBUG
  end

  Resque.list_range()

  # load the jobs
  Dir["#{ENV['OPENSHIFT_REPO_DIR']}/resque/*"].each {|file| require file}
end
