# Rediscord
keep record id sync with dynamic redis set or zset

## Install

add `gem 'rediscord'` to Gemfile, and run `bundle install`

## Example
* set
```ruby
class Post < ApplicationRecord
  enum level: [ :one, :two, :there ]

  include Rediscord
  redis_set key: ->(m){ "level_#{m.level}_post_set" }, redis: Redis.new
end
```
```ruby
Post.create(level: :one)   # id: 1
Post.create(level: :one)   # id: 2
Post.create(level: :two)   # id: 3
Post.create(level: :there) # id: 4
```
> what's in redis?

![](http://ww1.sinaimg.cn/large/006tKfTcjw1f6ixn9wi02j31kw10atd4.jpg)

* zset

```ruby
class Post < ApplicationRecord
  enum level: [ :one, :two, :there ]

  include Rediscord
  redis_zset key: ->(m){ "level_#{m.level}_post_zset" }, score: ->(m){ m.updated_at.to_i }, redis: Redis.new
end
```
```ruby
Post.create(level: :one)
Post.create(level: :one)
Post.create(level: :two)
Post.create(level: :there)
```
![](http://ww2.sinaimg.cn/large/006tKfTcjw1f6ixp5r14bj31kw10ajvt.jpg)
