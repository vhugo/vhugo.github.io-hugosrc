+++
date = "2016-01-23T04:49:23-02:00"
title = "Ruby on Rails development environment with Docker"
categories = ["development"]
tags=["hugo", "ruby", "rails", "docker", "gallery", "photos"]

+++

# Learning Ruby and Rails

This was how I created my first project to learn more about Ruby and Rails, I decided to create a web application for photo gallery. For starter, I wanted to keep my dev env isolated and more controlled, reason why I opt to use [Docker](https://www.docker.com/), I had it already installed, as well as [Ruby](https://www.ruby-lang.org/) and [Rails](http://rubyonrails.org/).

I created the new project with `rails new --skip-bundle gallery` on my workstation, used `--skip-bundle` because I'm going to install all Gems in the container, it added all the files and folder I needed to start my Rails project, then I added everything into a Git repository, just to keep track of my steps.

To start the Docker container setup, I've Added a new file called `Dockerfile`. Thanks [Marko Locher's](https://blog.codeship.com/running-rails-development-environment-docker/) article for the kickstart, here is its content:

```
FROM ruby:2.3

# Install apt based dependencies required to run Rails as
# well as RubyGems. As the Ruby image itself is based on a
# Debian image, we use apt-get to install those.
RUN apt-get update && apt-get install -y \
  build-essential \
  patch \
  ruby-dev \
  zlib1g-dev \
  liblzma-dev \
  nodejs

# Configure the main working directory. This is the base
# directory used in any further RUN, COPY, and ENTRYPOINT
# commands.
RUN mkdir -p /app
WORKDIR /app

# Copy the Gemfile as well as the Gemfile.lock and install
# the RubyGems. This is a separate step so the dependencies
# will be cached unless changes to one of those two files
# are made.
COPY Gemfile ./
RUN gem install bundler && bundle install --jobs 20 --retry 5

# Copy the main application.
COPY . ./

# Expose port 3000 to the Docker host, so we can access it
# from the outside.
EXPOSE 3000

# Configure an entry point, so we don't need to specify
# "bundle exec" for each of our commands.
ENTRYPOINT ["bundle", "exec"]

# The main command to run when the container starts. Also
# tell the Rails dev server to bind to all interfaces by
# default.
CMD ["bundle", "exec", "rails", "server", "-b", "0.0.0.0"]
```

Then built the image with `docker build -t gallery .` and then start the app container with `docker run -d --name gallery -v "$PWD":/app -p 8080:3000 gallery` if you check your browser on your container's address and port 8080, mine look like this `http://192.168.99.100:8080/`, then you should be able to see Rails welcome page. 

## What is the command Dockerrun I've been using

Now every time I want to run any rails command, I'm going to use Docker, it will look something like this `docker run --rm -v "$PWD":/app gallery rails g controller Albums index` to make things easier for me in the console, I created an alias in my `~/.bash_profile` with the following command `alias dockerrun='docker run --rm -v "$PWD":/app'` so now I can do this `dockerrun gallery rails g controller Albums index`
