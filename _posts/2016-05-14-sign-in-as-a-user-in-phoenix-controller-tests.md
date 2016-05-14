---
title: Sign in as a user in a Phoenix controller test
layout: post
tags:
  - phoenix
  - elixir
---
First I spent a few hours piecing this together. And then I spent a few months searching through projects several times to find where I had used it because I needed it again. So here it goes now, into my public scratch pad.

To put something into a `conn` session in a Phoenix test you first have to go through you app's router and then back. So for example if you want to sign in as a user before hitting an endpoint use this:

```elixir
def sign_in conn, user do
  conn
  |> bypass_through(MyApp.Router, :browser)
  |> get("/")
  |> put_session(:current_user, user.id)
  |> send_resp(:ok, "")
  |> recycle()
end
```

To have it available in all your `ConnCase`s put it in `test/support/conn_case.ex`.

```elixir
defmodule Tier.ConnCase do
  # ...
  using do
    quote do
      # ...
      def sign_in conn, user do
        # ...
      end
    end
  end
end
```
