OpenShift Resque Cartridge
=========================

Runs [Resque](https://github.com/resque/resque) on [OpenShift](https://openshift.redhat.com/app/login) using downloadable cartridge support.

[Redis cartridge](https://github.com/smarterclayton/openshift-redis-cart) needs to be installed, then we can proceed to installation of Resque cartridge, run this command in terminal

    rhc add-cartridge http://cartreflect-claytondev.rhcloud.com/reflect?github=pbrazdil/openshift-resque-cartridge --app YOUR_APPLICATION_NAME

Any log output will be generated to `$OPENSHIFT_RESQUE_DIR/logs/resque.log`

To check if it is all as we expected, lets see if Resque worker is running.

    rhc cartridge status resque -a YOUR_APPLICATION_NAME


How it works
------------
Every Resque job class must be listed in your code in `resque/` directory.

For example `resque/example_job.rb` may look like this

    class ExampleJob
        def self.queue
            :main
        end

        def self.perform(useless_param)
            sleep 10   #really hard task to process
        end
    end

Then if you want to add a new job, just call `Resque.enqueue`

    Resque.enqueue(ExampleJob, "some-cool-parameter")

In order to run it properly, `Resque.redis` must be pointed out to Redis running in your application. So, for example if you are running Rails, add a new file `config/initializers/resque_init.rb` with content below.

    Resque.redis = Redis.new(:host => ENV['OPENSHIFT_REDIS_HOST'], :port => ENV['OPENSHIFT_REDIS_PORT'], :password => ENV['REDIS_PASSWORD'], :thread_safe => true)


Example Rails app
-----------------

I made an [simple Rails application](https://github.com/pbrazdil/openshift-resque-rails-example) which demonstrate how Resque cartridge can be used.





