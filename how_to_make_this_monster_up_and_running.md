---
layout: post
title: 'HOW TO RUN THIS MONSTER'
permalink: /how_to_make_this_monster_up_and_running/
foo: Bar
author: Alex Charnou
---

![hoho](https://thumbs.gfycat.com/DiligentWellmadeAustraliansilkyterrier-size_restricted.gif)

first of all please make sure that:
1. your elasticsearch instance is up and running on :9200 port. Please edit `Procfile.local` in /app and comment out everything else except line with `elastic:`
2. your `/etc/hosts` file contains changes from REAMDE.md in /app
3. `direnv` is working on your machine (check out README.md in /app again)
4. your database accepts connections with provided in the `database.yml` credentials

also these steps require that all system requirements for /app, /devices, /cirro are met. don't forget about `budnle install` and `yarn install`

a first step is to populate Devices database. In order to do that you have to run
`bundle exec rake db:setup` in /devices repo. If everything went smoothly - you'll be able to run the rails console in /devices and observe entities created for you - like `OperatingSystem.all`

a second step is to run rails server in /devices `bundle exec rails c -p 4003` AND `foreman s` in separate terminal tabs

a third step is to run `bundle exec rake db:setup` in /app. If everything went smoothly - you'll be able to run the rails console in /app and observe entities created for you - like `Tester.all`

a fourth step is to edit `Procfile.local` back - now we need at least `sidekiq` up and running. In two separate tabs you should run `bundle exec rails s -p 4001` and `foreman s -f Procfile.local`

a fifth step is to get /cirro up and running with `bin/setup` and `foreman start` - everything in the README.md

a sixth step is to migrate our users from /app to /cirro to be able to authenticate - https://github.com/test-IO/app#migrate-existing-users-to-cirro

AND NOW WE ARE TALKING!

you should be able to visit `tester.app.localhost:4001` -> enter credentials(in /app README.md) -> vualá
