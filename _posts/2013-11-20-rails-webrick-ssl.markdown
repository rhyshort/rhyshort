---
layout: post
title: Turning on SSL for a Rails application using WEBrick
date: 2013-11-20 17:01:02.000000000 +00:00
---

Working on one of my assignments for university I was creating a Ruby on Rails application, one of the would be nice features to have for this was using SSL to secure the connection to the application. I struggled with getting this working at first but some googling I found a [helpful blog post](https://www.altamiracorp.com/blog/employee-posts/configuring-webrick-to-use-ssl), which I used to get SSL working. Long story short you edit /bin/rails file to include some configuration,  I thought I would post the file here for future reference (Excuse the indentation the code plug-in I’m using appears to have a width limit).  
```ruby
#!/usr/bin/env ruby
require 'rubygems'
require 'rails/commands/server'
require 'rack'
require 'webrick'
require 'webrick/https'
```
  
```ruby
module Rails
  class Server < ::Rack::Server
    def default_options super.merge({ Port: 3000,
      environment : (ENV['RAILS_ENV'] || "development").dup,
      daemonize: false,
      debugger:  false,
      pid: File.expand_path("tmp/pids/server.pid"),
      config: File.expand_path("config.ru"),
      SSLEnable: true,
      SSLVerifyClient: OpenSSL::SSL::VERIFY_NONE,
      SSLPrivateKey: OpenSSL::PKey::RSA.new(File.open("/path/to/your/private/key").read),
      SSLCertificate: OpenSSL::X509::Certificate.new(File.open("/path/to/your/certificate").read),
      SSLCertName: [["CN", WEBrick::Utils::getservername]]
    })
    end
  end
end
```
```ruby
APP_PATH = File.expand_path('../../config/application', __FILE__)<br></br>
require_relative '../config/boot'<br></br>
require 'rails/commands'
```


