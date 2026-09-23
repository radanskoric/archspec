---
title: Vanilla Rails
nav_order: 3
description: Use ArchSpec's vanilla Rails preset to keep behavior on models, protect conventional boundaries, and forbid extra abstraction directories.
seo:
  title: Vanilla Rails Architecture
---

# Vanilla Rails

Vanilla Rails is a Rails architecture for rich models and thin controllers,
without service objects, form objects, policy objects, presenters, decorators,
or view components.

```ruby
architecture :vanilla_rails
```

It uses the conventional Rails components and keeps controller-only calls out
of models and services. Models may share helpers and reference controllers
(for example, `ApplicationController.renderer`); concerns may reference their
includers. Use `share_helpers: false` to forbid helper dependencies, or
`concerns: 'app/**/concerns/**/*.rb'` to opt into concern independence.
The stricter dependency boundaries remain in [Rails]({% link _architectures/rails.md %}).

It requires these directories to stay empty:

- `app/services`
- `app/forms`
- `app/policies`
- `app/decorators`
- `app/presenters`
- `app/components`

It also defines `views` for `app/views/**/*.erb` and `records` using
`descendants_of: 'ApplicationRecord'`, alongside the directory-based `models`
component. Views cannot depend on records: `User.count`, `User.active.count`,
and even `User.new` produce `dependencies.forbid` violations when `User` is an
ApplicationRecord descendant. There are no method-level exceptions.

References to non-record classes such as `Current` remain allowed. Calls on
controller-assigned objects such as `@user` are not type-inferred, so this rule
does not comprehensively prevent database access from views.

The `components:` option replaces the default component map, so you can
override the view paths or the `records` selector. Omitting either `views` or
`records` omits the view-to-record rule.

See the [Vanilla Rails guide]({% link _guides/vanilla-rails.md %}) for the
reasoning and for project-specific rules you can add on top.
