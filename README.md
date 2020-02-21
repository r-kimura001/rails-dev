$ docker-compose run app rails new . --force --no-deps --database=mysql --skip-bundle  
$ vi config/database.yml

```
default: &default
  adapter: mysql2
  encoding: utf8
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
  username: admin   # docker-compose.ymlの設定と合わせる
  password: admin   # docker-compose.ymlの設定と合わせる
  host: db   # docker-compose.ymlのサービス名と合わせる

development:
  <<: *default
  database: myapp   # docker-compose.ymlの設定と合わせる
  host: db   # docker-compose.ymlのサービス名と合わせる

（中略）

test:
  <<: *default
  database: myapp_test   # docker-compose.ymlの設定と合わせる
  host: db-test   # docker-compose.ymlのサービス名と合わせる

（中略）

production:
  <<: *default
  database: myapp_production
  username: admin   # docker-compose.ymlの設定と合わせる
  password: <%= ENV['TESTAPP_DATABASE_PASSWORD'] %>
```

$ docker-compose build  
$ docker-compose up -d  
$ docker-compose run app rails db:create  
$ docker-compose exec app ash  
$ rails -v  // バージョンが表示されればOK  
$ exit  

※localhost:13000にアクセスし、トップ画面が表示されればOK