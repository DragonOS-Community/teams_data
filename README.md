# DragonOS teams structure

This repository contains the structure of the DragonOS teams. 
## Documentation

* [TOML schema reference](docs/toml-schema.md)

## Using the CLI tool

It's possible to interact with this repository through its CLI tool.

### Verifying the integrity of the repository

This repository contains some sanity checks to avoid having stale or broken
data. You can run the checks locally with the `check` command:

```
cargo run check
```

Note that some of these checks will be skipped due to missing API tokens.

### Adding a person to the repository

It's possible to fetch the public information present in a GitHub profile and
store it in a person's TOML file:

```
cargo run add-person <github-username>
```

You can also add additional information, such as someone's Discord or Zulip ID by adding additional fields to their `.toml` file.

To determine someone's Zulip ID, find them in the list of people on the
right-hand side in Zulip, click the "three dots" menu, and copy the 'User ID'
into the toml file:

```
zulip-id = <user id>
```

### Querying information out of the repository

There are a few CLI commands that allow you to get some information generated
from the data in the repository.

You can get a list of all the people in a team:

```
cargo run dump-team all
```

You can get a list of all the email addresses subscribed to a list:

```
cargo run dump-list all@dragonos.org
```

You can get a list of all the users with a permission:

```
cargo run dump-permission perf
```


You can also print a list of users with individual access to repositories

```
# Group the accesses by repository
cargo run dump-individual-access --group-mode repo

# Group the accesses by contributor
cargo run dump-individual-access --group-mode person
```


### Building the static API

You can build locally the content of `https://team-api.infra.dragonos.org.cn/v1/`
by running the command:

```
cargo run static-api output-dir/
```

The content will be placed in `output-dir/`.

### Encrypting email addresses

If an email address in a list needs to be confidential it's possible to encrypt
it. Encrypted email addresses look like this:

```
encrypted+3eeedb8887004d9a8266e9df1b82a2d52dcce82c4fa1d277c5f14e261e8155acc8a66344edc972fa58b678dc2bcad2e8f7c201a1eede9c16639fe07df8bac5aa1097b2ad9699a700edb32ef192eaa74bf7af0a@rust-lang.invalid
```

The production key is accessible to select Infrastructure Team members, so if
you need to add an encrypted email address you'll need to reach out to that
team. The key is stored in the following parameter on AWS SSM Parameter Store:

```
/prod/sync-team/email-encryption-key
```

The `cargo run encrypt-email` and `cargo run decrypt-email` interactive CLI
commands are available for infra team members to interact with encrypted
emails. The `dragonos_team_data` (with the `email-encryption` feature enabled) also
provides a module to programmatically encrypt and decrypt.


## Thanks

This repo is forked from `rust-lang/team`,  and we are grateful for the original authors!
