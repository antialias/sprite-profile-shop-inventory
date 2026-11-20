# sprite-profile-shop-inventory

A [sprite-profiles](https://github.com/antialias/sprite-profiles) profile published by the
service it connects to: a woodworking shop's tool inventory, served as an MCP server at
`woodworking.haunt.house`. Add it to a [Fly.io Sprite](https://sprites.dev) and Claude Code
can look up the shop's tools, their specs and what is missing, while the Sprite never holds
a key.

It is a small example of the idea behind sprite-profiles: a service describes how it plugs
into a Sprite, in its own repository, and anyone composes it with other profiles. The
repository's name, `sprite-profile-<name>`, is what makes `@antialias/shop-inventory` find it.

## Use it

```sh
# What it can touch: the connector, the network, the MCP server, the skill
sprite-profiles show @antialias/shop-inventory

# Once per Fly org: store the shop's token in a connector (asked for with echo off)
sprite-profiles connect @antialias/shop-inventory

# Launch a Sprite with Claude Code and the shop
sprite-profiles launch claude-code @antialias/shop-inventory --name my-sprite
```

`connect` creates a custom API connector named `antialias.shop-inventory`. It admits only
Sprites labelled `profile:antialias.shop-inventory`, which the launch sets, and only the
path `/mcp`. Inside the Sprite, Claude Code's `shop` server points at the Sprites gateway,
which adds the token on the way out.

## What a token can see

Tokens are issued by the shop's owner; there is no sign-up. A token with the `read` scope
can search the inventory and read each tool, its specs and what is known about it. The
server leaves out purchase records, serial numbers, locations and photos for that scope,
and refuses every change.

## What is in it

- `profile.toml`: the connector declaration (base URL, Bearer header, the one endpoint), the
  MCP server, and the network rule for the gateway.
- `claude/skills/shop-inventory/SKILL.md`: how Claude Code should use the server, and that
  it reads only.

Versions are tags: `v1.0.0` is what `@antialias/shop-inventory@1` picks. The older address
`antialias/shop-inventory-profile/shop-inventory@v1` still resolves, through GitHub's
redirect, to the first commit.

## License

Apache-2.0. See [LICENSE](LICENSE).
