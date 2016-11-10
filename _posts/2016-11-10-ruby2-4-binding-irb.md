---
layout: post
title: Ruby 2.4 Binding.irb
tags: [ruby]
---

Hello! [Ruby2.4.0.preview3](https://www.ruby-lang.org/en/news/2016/11/09/ruby-2-4-0-preview3-released/) was released yesterday. There are some really cool features in it. I will mention one of these. It is about debugging or open a REPL session. As you know before that release, Ruby developers use ```p``` or other lovely gem [pry](https://github.com/pry/pry) to debug. For now [```binding.irb```](https://github.com/ruby/ruby/commit/493e48897421d176a8faf0f0820323d79ecdf94a) provides to debug in your application.

First, we need to install lastes your version. With RVM we can do it!

```bash
$ rvm install 2.4.0-preview3
```

Don't forget to select right version.

```bash
$ rvm use 2.4.0-preview3
```

Now we can test that feature with a small Ruby program.

```ruby
require 'irb'

class Test
  def self.hello(name)
    binding.irb
    puts "Hello #{name}"
  end
end

puts Test.hello('ender')
```

```ruby
> ruby ruby24p3.rb
2.4.0-preview3 :001 > name
#=> "ender"
```

That's it. After Ruby 2.4 we won't need to write ```p``` we can open a REPL in anytime.

Cheers.