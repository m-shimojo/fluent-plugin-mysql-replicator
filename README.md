# fluent-plugin-mysql-replicator [![Build Status](https://travis-ci.org/y-ken/fluent-plugin-mysql-repicator.png?branch=master)](https://travis-ci.org/y-ken/fluent-plugin-mysql-repicator)

## Overview

Fluentd input plugin to track insert/update/delete event from MySQL server.

## Installation

`````
### native gem
gem install fluent-plugin-mysql-repicator

### td-agent gem
/usr/lib64/fluent/ruby/bin/fluent-gem install fluent-plugin-mysql-repicator
`````

## Tutorial

#### configuration

`````
<source>
  type mysql_replicator
  host localhost
  username your_mysql_user
  password your_mysql_password
  database myweb
  interval 5s
  tag replicator
  query SELECT id, text from search_test
</source>

<match replicator.*>
  type stdout
</match>
`````

#### sample query

`````
$ mysql -uguest replicator -e "create table search_test(id int auto_increment, text text, PRIMARY KEY (id))"
$ sleep 10
$ mysql -uguest replicator -e "insert into search_test(text) values('aaa')"
$ sleep 10
$ mysql -uguest replicator -e "update search_test set text='bbb' where text = 'aaa'"
$ sleep 10
$ mysql -uguest replicator -e "delete from search_test where text='bbb'"
`````

#### result

`````
$ tail -f /var/log/td-agent/td-agent.log
2013-11-25 18:22:25 +0900 replicator.insert: {"id":"1","text":"aaa"}
2013-11-25 18:22:35 +0900 replicator.update: {"id":"1","text":"bbb"}
2013-11-25 18:22:45 +0900 replicator.delete: {"id":"1"}
`````

## TODO

Pull requests are very welcome!!

## Copyright

Copyright © 2013- Kentaro Yoshida ([@yoshi_ken](https://twitter.com/yoshi_ken))

## License

Apache License, Version 2.0