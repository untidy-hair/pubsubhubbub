# Pubsubhubbub

[![Gem Version](http://img.shields.io/gem/v/pubsubhubbub-rails.svg)][gem]
[![Dependency Status](http://img.shields.io/gemnasium/Gargron/pubsubhubbub.svg)][gemnasium]

[gem]: https://rubygems.org/gems/pubsubhubbub-rails
[gemnasium]: https://gemnasium.com/Gargron/pubsubhubbub

This is a mountable PubSubHubbub server conforming to the v0.4 of the spec.

## Usage

In `routes.rb`:

```ruby
mount Pubsubhubbub::Engine, at: 'pubsubhubbub', as: :pubsubhubbub
```

In feed:

```erb
<link rel="hub" href="<%= pubsubhubbub_url %>" />
```

If you want to override topic verification (which is done before subscribe/unsubscribe events to confirm that the topic is in fact using the hub), you can add a custom initializer, e.g. `config/initializers/pubsubhubbub.rb`:

```ruby
Pubsubhubbub.verify_topic = lambda { |topic_url| topic_url == 'http://mysite.com/my-feed' }
```

You can also override the fetching of the topic when it is updated, for example if it's generally not publicly accessible or if you generate the feed from the Rails app where you mounted the hub.

```ruby
Pubsubhubbub.render_topic = lambda { |topic_url| FeedsController.render(:show) }
```

If you want to notify the subscribers without hitting the `/pubsubhubbub` HTTP endpoint, there's a convenience method you can use right from your app:

```ruby
Pubsubhubbub.publish(pubsubhubbub_url, feed_url(user))
```

## Installation
Add this line to your application's Gemfile:

```ruby
gem 'pubsubhubbub-rails', require: 'pubsubhubbub'
```

And then execute:
```bash
$ bundle
```

Or install it yourself as:
```bash
$ gem install pubsubhubbub-rails
```

## License
The gem is available as open source under the terms of the [MIT License](http://opensource.org/licenses/MIT).
