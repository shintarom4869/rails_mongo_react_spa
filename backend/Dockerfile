FROM ruby:2.7.4-slim

WORKDIR /app

RUN apt-get update -y -qq && \
    apt-get install -y -qq \
      curl \
      git-core \
      build-essential \
      libmagickwand-dev \
      nodejs \
      && \
    rm -rf /var/lib/apt/lists/*

RUN curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - && \
  echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list && \
  apt-get update && apt-get install yarn && \
  rm -rf /var/lib/apt/lists/*

COPY Gemfile Gemfile.lock /app/
ENV BUNDLE_APP_CONFIG=.bundle BUNDLE_PATH=/bundle
RUN bundler version && bundle install --path=/bundle --jobs=4

COPY . /app/
# コンテナ起動時に毎回実行されるスクリプト
COPY entrypoint.sh /usr/bin/
RUN chmod +x /usr/bin/entrypoint.sh
ENTRYPOINT ["entrypoint.sh"]
EXPOSE 3000

CMD ["rails", "server", "-b", "0.0.0.0"]
