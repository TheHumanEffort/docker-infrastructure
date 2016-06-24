FROM ruby:2.3.1

RUN apt-get update -qq && apt-get install -y build-essential default-jre

# for postgres
RUN apt-get install -y libpq-dev

# for nokogiri
RUN apt-get install -y libxml2-dev libxslt1-dev

# for capybara-webkit
RUN apt-get install -y libqt4-webkit libqt4-dev xvfb

# for a JS runtime
RUN apt-get install -y nodejs

RUN apt-get install -y wkhtmltopdf

ENV APP_HOME /application
RUN mkdir $APP_HOME
WORKDIR $APP_HOME

ADD Gemfile* $APP_HOME/
RUN bundle install

# For production use, which we're not actually using atm:
# ADD . $APP_HOME
# ADD config/database.docker.yml $APP_HOME/config/database.yml
# RUN bundle exec rake assets:precompile

