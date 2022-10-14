FROM amd64/ruby:2.7.2

RUN curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - \
  && echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list

RUN apt update -qq && \
    apt install -y build-essential \
    libpq-dev

RUN apt-get update -qq && apt-get install -y nodejs 

# Dockerfile内で使用する変数名を指定    
ARG WORKDIR
ENV APP_ROOT /$WORKDIR
RUN mkdir $APP_ROOT
WORKDIR $APP_ROOT
# このファイル（Dockerfile）と同じ階層にあるGemfile等をコンテナにコピー
COPY ./Gemfile $APP_ROOT/Gemfile
COPY ./Gemfile.lock $APP_ROOT/Gemfile.lock
# コンテナのGemfileを参考にGemをインストール
RUN bundle install
# このファイルが含まれているディレクトリをコンテナのルートディレクトリにコピー
COPY . $APP_ROOT
# RUN bundle config set --global force_ruby_platform true

COPY ./entrypoint.sh /usr/bin/
RUN chmod +x /usr/bin/entrypoint.sh
ENTRYPOINT ["entrypoint.sh"]


CMD ["rails", "server", "-b", "0.0.0.0"]