---
layout: page
title: Defining responses
---

# Coming soon. 

{% highlight yaml %}
    definitions:
      HelloWorldResponse:
        required:
          + message
        properties:
          message:
            type: string
          age:
            type: number
      ErrorResponse:
        required:
          + message
        properties:
          message:
            type: string
{% endhighlight %}