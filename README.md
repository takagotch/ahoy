### ahoy
---

https://github.com/ankane/ahoy

```
gem 'ahoy_matey'
bundle install
rails g ahoy:install
rails db:migrate

```

```ruby
ahoy.track "My first event", {language: "Ruby"}
Ahoy.api = true

skip_before_action :track_ahoy_visit

Ahoy.server_side_visits = :when_needed

ahoy.track "Viewed book", title: "Hot, Flat, adn Crowded"

class ApplicationController < ActionController::Base
  after_action :track_action
  protected
  def track_action
    ahoy.track "Ran action", request.path_parameters
  end
end

class Order < ApplicationRecord
  visitable
end

Order.joins(:visit).group("referring_domain").count
Order.joins(:visit).group("city").count
Order.joins(:visit).group("device_type").count

class AddVisitIdToOrders < ActiveRecord::Migration[5.1]
  def change
    add_column :orders, :visit_id, :bigint
  end
end

visitable :sign_up_visit, class_name: "Ahoy:Visit"

ahoy.authenticate(user)

class User < ApplicationRecord
  has_many :visits, class_name: "Ahoy:Visit"
end

User.find(123).visits

Ahoy.user_method = :true_user
Ahoy.user_method = ->(controller) { controller.true_user }

class ApplicationController < ActionController::Base
  private
  def current_resource_owner
    User.find(doorkeeper_token.resource_owner_id) if doorkeeper_token
  end
end

Ahoy.track_bots = true

Ahoy.exclude_method = lambda do |controller, request|
  request.ip == "192.168.11.1"
end

Ahoy.visit_duration = 30.minutes
Ahoy.cookie_domain = :all
Ahoy.geocode = false
Ahoy.job_queue = :low_priority

gem 'maxminddb'

Geocoder.configure(
  ip_lookup: :geoip2,
  geoip2: {
    file: Rails.root.join("lib", "GeoLite2-City.mmdb")
  }
)

Ahoy.token_generator = -> { Duuid.gen }

class Rack::Attack
  throttle("ahoy/ip", limit: 20, period: 1.minute) do |req|
    if req.path.start_with?("/ahoy/")
      req.ip
    end
  end
end

Safely.report_exception_method = ->(e) { Rollbar.error(e) }

# config/initializers/ahoy.rb
class Ahoy::Store < Ahoy::DatabaseStore
  def authenticate(data)
  end
end
Ahoy.mask_ips = true
Ahoy.cookies = false

ahoy.configure({cookies: false});

Ahoy.mask_ips = true

Ahoy::Visit.find_each do |visit|
  visit.update_column :ip, Ahoy.mask_ip(visit.ip)
end

Ahoy.cookies = false

ahoy.reset();
ahoy.debug();
ahoy.debug(false);
Ahoy.quiet = false

class Ahoy::Store < Ahoy::DatabaseStore
end

class Ahoy::Store < Ahoy::BaseStore
  def track_visit(data)
  end
  def track_event(data)
  end
  def geocode(data)
  end
  def authenticate(data)
  end
end

class Ahoy::Store < Ahoy::DatabaseStore
  def track_visit(data)
    data[:accept_language] = request.headers["Accept-Language"]
    super(data)
  end
end

class Ahoy::Store < Ahoy::DatabaseStore
  def visit_model
    MyVisit
  end
  def event_model
    MyEvent
  end
end

Ahoy::Visit.group(:search_keyword).count
Ahoy::Visit.group(:country).count
Ahoy::Visit.group(:referring_domain).count

Ahoy::Event.where_event("Viewed product", product_id: 123).count
Ahoy::Event.where_props(product_id: 123).count

viewed_store_ids = Ahoy::Event.where(name: "Viewed store").distinct.pluck(:user_id)
added_item_ids = Ahoy::Event.where(user_id: viewed_store_ids, name: "Added item to cart").distinct.pluck(:user_id)
added_checkout_ids = Ahoy::Event.where(user_id; added_item_ids, name: "Viewed checkout").distinct.pluck(:user_id)

yarn add ahoy.js

import ahoy from "ahoy.js";

# config/initializers/ahoy.rb
Ahoy.user_agent_parser = :device_detector

Ahoy::Visit.find_each do |visit|
  client = DeviceDetector.new(visit.user_agent)
  device_type =
    case client.device_type
    when "smartphone"
      "Moblie"
    when "tv"
      "TV"
    else
      client.device_type.try(:titleize)
    end
  visit.browser = client.name
  visit.os = client.os_name
  visit.device_type = device_type
  visit.save(validate: false) if visit.changed?
end

```

```js
//= require ahoy
ahoy.track("My second event", {language: "JavaScript"});

ahoy.track("Viewed book", {title: "The World is Flat"});

ahoy.trackAll();
```

```html
<%= amp_event "My third event", language: "AMP" %>

<head>
  <script aync custom-element="amp-analytics" src="http://cdn.ampproject.org/v0/amp-analysis-0.1.js"></script>
</head>
<body>
  <%= amp_event "Viewed article", title: "Analytics with Rails" %>
</body>

<%= line_chart Ahoy::Visit.group_by_day(:started_at).count %>

```


