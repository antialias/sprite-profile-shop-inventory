# shop-inventory-profile

A [sprite-profiles](https://github.com/antialias/sprite-profiles) profile published by the
service it connects to: a woodworking shop's tool inventory, served as an MCP server at
`woodworking.haunt.house`. Pick it by address, and Claude Code on a
[Fly.io Sprite](https://sprites.dev) can look up the shop's tools, their specs and what is
missing, while the Sprite never holds a key.

It is a small example of the idea behind sprite-profiles: a service describes how it
plugs into a Sprite, in its own repository, and anyone composes it with other profiles.

## Use it

From a checkout of sprite-profiles:

```sh
# See what it would do: domains, connector, MCP server, skill
mix profiles.plan -p claude-code -p antialias/shop-inventory-profile/shop-inventory@v1

# Once per Fly org: store the shop's token in a connector (asked for with echo off)
mix profiles.connect -p antialias/shop-inventory-profile/shop-inventory@v1

# Launch
mix profiles.launch -p claude-code -p antialias/shop-inventory-profile/shop-inventory@v1 --name my-sprite
```

`mix profiles.connect` creates a custom API connector named `antialias.shop-inventory`.
It admits only Sprites labelled `profile:antialias.shop-inventory`, which the launch sets,
and only the path `/mcp`. Inside the Sprite, Claude Code's `shop` server points at the
Sprites gateway, which adds the token on the way out.

## What a token can see

Tokens are issued by the shop's owner; there is no sign-up. A token with the `read` scope
can search the inventory and read each tool, its specs and what is known about it. The
server leaves out purchase records, serial numbers, locations and photos for that scope,
and refuses every change.

## What is in it

- `shop-inventory/profile.toml`: the connector declaration (base URL, Bearer header, the
  one endpoint), the MCP server, and the network rule for the gateway.
- `shop-inventory/claude/skills/shop-inventory/SKILL.md`: how Claude Code should use the
  server, and that it reads only.

## License

Apache-2.0. See [LICENSE](LICENSE).
