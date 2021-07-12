---
layout: post
title: 'How to make this monster up and running'
permalink: /how_to_make_this_monster_up_and_running/
foo: Bar
author: Alex Charnou
---

![hoho](https://thumbs.gfycat.com/DiligentWellmadeAustraliansilkyterrier-size_restricted.gif)

Please skip all `db:setup`, `bin/setup`, `db:seed` from /app. /devices, /cirro README.md for now. If you already pupulated the databases - drop them with `bundle exec rake db:drop`. Also please make sure that:
1. your elasticsearch instance is up and running on :9200 port: you can edit `Procfile.local` in /app - comment out everything except line with `elastic:` and run
 ```markdown
foreman s -f Procfile.local
```
OR run
```markdown
docker run -e ES_JAVA_OPTS='-Xms500m -Xmx500m' -p 9200:9200 -p 9300:9300 --volume elasticsearch_data:/usr/share/elasticsearch/data elasticsearch:5.6
```

2. your `/etc/hosts` file contains changes from REAMDE.md in /app
3. `direnv` is working on your machine (check out README.md in /app again and check [this](https://direnv.net/docs/hook.html)): your database should accept connections with the provided in the `database.yml` credentials: `ENV['DB_USERNAME']` and `ENV['DB_PASSWORD']` which should be set in `.envrc` file (`cp .envrc.example .envrc`) and without `direnv` tool it wont workout ;)

also these steps require that all system requirements for /app, /devices, /cirro are met (yes, README.md again ;)). don't forget about `bundle install` and `yarn install`

a first step is to populate /devices database. In order to do that you have to run
`bundle exec rake db:setup` in /devices repo. If everything went smoothly - you'll be able to run the rails console in /devices and observe entities created for you - like `OperatingSystem.all`

a second step is to run the Rails server in /devices `bundle exec rails s -p 4003` AND `foreman s` in separate terminal tabs

a third step is to run `bundle exec rake db:setup` in /app in order to populate database there. If everything went smoothly - you'll be able to run the rails console in /app and observe entities created for you - like `Tester.all`

a fourth step is to edit `Procfile.local` back (if you did changes there) - now we need at least `sidekiq`(in addition to `elasticsearch`) up and running. In two separate tabs you should run `bundle exec rails s -p 4001` and `foreman s -f Procfile.local`

a fifth step is to get /cirro up and running with `bin/setup` and `foreman start` - everything in the README.md

a sixth step is to migrate our users from /app to /cirro in order to be able to authenticate - https://github.com/test-IO/app#migrate-existing-users-to-cirro

AND NOW WE ARE TALKING!

you should be able to visit `tester.app.localhost:4001` now -> just use [credentials](https://github.com/test-IO/app#credentials) from /app README.md -> vualá
