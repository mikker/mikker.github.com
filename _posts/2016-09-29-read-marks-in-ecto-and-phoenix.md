---
layout: post
title: Read marks with Ecto and Phoenix
tags:
  - ecto
  - phoenix
---
As always, Rails has an easy, pluggable way to do this with [unread](https://github.com/ledermann/unread) but it isn't too hard to add the same to your Phoenix app. Let's try!

First we'll generate the table and schema that holds the read marks. This assumes you have two tables: `users` and `stories`.

```sh
mix phoenix.gen.model ReadMark read_marks \
  user_id:references:users \
  story_id:references:stories
```

Open up the newly generated `web/models/read_mark.ex` and set up the associations:

```elixir
defmodule MyApp.ReadMark do
  use MyApp.Web, :model

  schema "read_marks" do
    belongs_to :user, MyApp.User
    belongs_to :story, MyApp.Story

    timestamps()
  end
end
```

And in the existing models:

```elixir
defmodule MyApp.User do
  schema "users" do
    has_many :read_marks, MyApp.ReadMark, on_delete: :delete_all
  end
end

defmodule MyApp.Story do
  schema "stories" do
    has_many :read_marks, MyApp.ReadMark, on_delete: :delete_all
  end
end
```

This is all the setup we need.

We can now get unread stories for a user with a query like:

```elixir
defmodule MyApp.Story do
  #...

  def unread_by(query, user) do
    from s in query,
      left_join: rm in assoc(s, :read_marks),
      on: rm.user_id == ^user.id,
      where: is_nil(rm.id)
  end
  def unread_by(user), do: unread_by(DRBot.Story, user)
end
```

And use it like:

```elixir
user = Repo.get(MyApp.User, 1)
unread_stories = MyApp.Story.unread_by(user) |> Repo.all
```

We can even chain it with other queries:

```elixir
query = from s in MyApp.Story, where: s.published_on == ^today

unread_stories_published_today =
  MyApp.Story.unread_by(query, user)
  |> Repo.all

# or the other way around

query = MyApp.Story.unread_by(user)

unread_stories_published_today =
  (from s in query, where: s.published_on == ^today)
  |> Repo.all
```
