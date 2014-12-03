---
layout: post
title:  "Serverspec behind a jump server"
date:   2014-12-03 21:00:00
categories: code
---

Rest easy your [Serverspec](http://serverspec.org) tests don't have to suffer any longer!

Straight up, I don't advocate jump servers, jump hosts, bastion hosts, gateway servers or any such things they are a pain and a risk.
What I do advocate however, is writing tests for infrastructure!

All you will need for this hack is netcat or similar installed on your jump server and some modifications made to your `spec_helper.rb`.

Utilising the ProxyCommand SSH option your problems can be solved easily. Take a look at the following snippet:

~~~ruby
require 'serverspec'
require 'net/ssh'

Net::SSH::Config.for('*')[:proxy] = "ssh #{'jump.server.enterprise.com'} nc %h %p"

RSpec.configure do |config|
  set :host, 'server.behind.jump.host.enterprise.com'

  set :backend, :ssh
  set :request_pty, true
end
~~~


The secret sauce here is `Net::SSH::Config.for('*')[:proxy] = "ssh #{'jump.server.enterprise.com'} nc %h %p"`, it will
make sure all new SSH connections are routed through a jump host of your choice.

I like this solution as minimal code is required and you can address the hosts you which you would like to test via the actual FQDN.

I've a similar work around using Net::SSH::Gateway; however, this was not very successful. It meant I would always
have my target host or `set :host` pointing to a server with a different hostname, such as `localhost` or my bastion host address.

Happy testing!
