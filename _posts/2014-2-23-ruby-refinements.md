---
layout: post
title: Ruby Refinements
---
Want to monkey patch a ruby class, but hesitant of the impact of global changes?  Ruby 2.1 introduces Refinements, which allow you to monkey patch a class, limited to a local scope.

For example, let's say you frequently have the need to calculate factorials. You think about monkey patching in a ! method onto fixnum, but know that it will lead to unintended consequences if others using your code are not aware ( e.g. someone tries to coerce a numeric to a boolean by using !!number ).

So instead, we define a refinement inside a Factorial module -
{% highlight ruby %}
  module Factorial
    refine Fixnum do
      def !
        (1..self).reduce(:*) || 1
      end
    end
  end
{% endhighlight %}
Now, we can use this anywhere we include our module, but won't impact code elsewhere.
{% highlight ruby %}
  puts !5 # prints false
using Factorial
puts !5 # prints 120
{% endhighlight %}
