---
layout: post
title:  "Serverspec Behind a Jump Server"
date:   2014-12-03 21:00:00
categories: code
---

Rest easy your [Serverspec](http://serverspec.org) tests don't have to suffer any longer!

Straight up, I don't advocate jump servers, jump hosts, bastion hosts, gateway servers or any such things they are a pain and a risk.
What I do advocate however, is writing tests for infrastructure!

For this hack to work you just need the following three things:

 * Netcat installed on your jump server
 * SSH key authentication working for connections to your jump server, no passwords
 * Some modifications made to your `spec_helper.rb`.

Utilising the ProxyCommand SSH option your problems can be solved easily. Take a look at the following snippet:

~~~ruby
require 'serverspec'
require 'net/ssh'
require 'net/ssh/proxy/command'

proxy = Net::SSH::Proxy::Command.new('ssh jumpguy@jump.server.enterprise.com nc %h %p')

RSpec.configure do |config|
  set :host, 'server.behind.jump.host.enterprise.com'
  set :ssh_options => proxy

  set :backend, :ssh
  set :request_pty, true
end
~~~


The secret sauce here is `proxy = Net::SSH::Proxy::Command.new('ssh jumpguy@jump.server.enterprise.com nc %h %p')`, it will
make sure all new SSH connections are routed through a jump host of your choice.

I like this solution as minimal code is required and you can address the hosts you which you would like to test via the actual FQDN.

I've used similar work arounds using Net::SSH::Gateway; however, in this case it was not as successful. It meant I would always
have my target host or `set :host` pointing to a server with a different hostname, such as `localhost` or my bastion host address.

Happy testing!
