

```ruby
SlackErrorNotifier.configure do |config|
  config.webhook_url = '...'
  config.storage = :redis # default ':redis'
  config.storage.redis.namespace = 'slack_notifications.' # default 'slack_error_notifier.'
  config.catch_all = false # default false
end

SlackErrorNotifier.new('MyCustomNS', throttle: 1.hours, count_duration: :day) do |error, count|
  if count
    "Custom error triggered ! (#{count} today) : #{error.message}"
  else
    "Custom error triggered ! (first today) : #{error.message}"
  end
end

error = Exception.new('my error message!')
SlackErrorNotifier::MyCustomNS.call(error) # will send slack notification "Custom error triggered ! (first today) : my error message!"
SlackErrorNotifier::MyCustomNS.call(error) # will not send slack notification but incr error counter

```

## TODO

- Integrations :
  - Bugsnag integration
  - Sentry integration
- Storages :
  - Redis
  - SQL ?
  - Custom ?
