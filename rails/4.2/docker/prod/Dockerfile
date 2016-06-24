FROM ___REPLACEME!!!__:base

ADD Gemfile* $APP_HOME/
RUN bundle install

ADD . $APP_HOME
ADD config/database.docker.yml $APP_HOME/config/database.yml
RUN bundle exec rake assets:precompile

