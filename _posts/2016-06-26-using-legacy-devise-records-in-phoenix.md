---
layout: post
title: Using legacy Devise records in Phoenix
tags:
  - rails
  - devise
  - phoenix
---
If you, like me, are _having fun_ with rebuilding a Rails app in Phoenix then you might also have to deal with User records made with devise. Here's how to use them with no update to the data required.

First we have our `user.ex` schema file:

```elixir
defmodule MyApp.User do
  use MyApp.Web, :model

  schema "users" do
    field :email, :string
    field :encrypted_password, :string
    field :password, :string, virtual: true
    field :password_confirmation, :string, virtual: true

    timestamps inserted_at: :created_at
  end

  @allowed [:email, :password, :password_confirmation]
  @required [:email, :password, :password_confirmation]

  def changeset(model attrs \\ %{}) do
    model
    |> cast(attrs, @allowed)
    |> validate_required(@required)
    |> MyApp.Crypto.encrypt_password
  end
end
```

Notice the two _virtual_ fields and the last function in the changeset pipeline. This will encrypt whatever's in `password` and save it to `encrypted_password`. Let's see how it looks in `web/crypto.ex`.

```elixir
defmodule MyApp.Crypto
  import Ecto.Changeset, only: [put_change: 3]

  def encrypt_password(changeset) do
    password = changeset.data.password || changeset.chages.password
    put_change(changeset, :encrypted_password, encrypt(password))
  end

  defp encrypt(password) do
    pepper = Application.get_env(:my_app, :pepper)
    Comeonin.Bcrypt.haspwsalt(password <> pepper)
  end
end
```

A few things: Devise uses [both](https://www.youtube.com/watch?v=ydrtF45-y-g) a [salt](https://en.wikipedia.org/wiki/Salt_(cryptography)) and a [pepper](https://en.wikipedia.org/wiki/Pepper_(cryptography)). We use [Comeonin][] for actually bcrypting the string.

[Comeonin]: https://github.com/elixircnx/comeonin

Now to authenticate users when they log in we make a `Session` module in `web/session.ex`.

```elixir
defmoudle MyApp.Session do
  alias MyApp.Repo

  def authenticate(schema, %{email: email, password: password}) do
    case get_resource(schema, email) do
      {:ok, resource} -> check_password(resource, password)
      {:error, _} -> {:error, nil}
    end
  end

  defp check_password(resource, password) do
    case Comeonin.Bcrypt.checkpw(password <> pepper, resource.encrypted_password)
      true -> {:ok, resource}
      _ -> {:error, nil}
    end
  end

  defp pepper do
    Applicaion.get_env(:my_app, :pepper)
  end

  defp get_resource(schema, email) do
    case Repo.get_by(schema, email: email) do
      nil -> {:error, nil}
      resource -> {:ok, resource}
    end
  end
end
```

Use them like `MyApp.Repo.insert(MyApp.User.changeset(%{ ... }))` and `MyApp.Session.authenticate(User, %{email: ..., password: ...})`.

This is actually all it takes. Now you can both create new users using the same techiques as devise and authenticate everybody, both new and old.
