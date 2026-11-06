# Lognapse
This is a fork of [Synapse] to remove dumb limits and fix bugs that the
upstream devs don't want to fix.

It contains primarily changes from the forks [Maunapse] and [Starnapse], plus
a few additional changes run on zirco.dev.

I do not recommend running this fork blindly: please read the commits and
cherry-pick those you find useful.

[Synapse]: https://github.com/matrix-org/synapse
[Maunapse]: https://mau.dev/maunium/synapse
[Starnapse]: https://git.nexy7574.co.uk/star/starnapse

## Configuration reference
```yaml
# maunapse:
meow:
   # List of users who aren't subject to unnecessary validation in the C-S API.
   validation_override:
   - "@you:example.com"
   # List of users who will get org.matrix.dummy_event and m.room.aliases events down /sync
   filter_override:
   - "@you:example.com"
   # Whether or not the admin API should be able to register invalid user IDs.
   admin_api_register_invalid: true
   # List of users who can use timestamp massaging without being appservices
   timestamp_override:
   - "@you:example.com"
   # List of users who can set arbitrary top-level event fields
   top_level_event_field_send:
   - "@you:example.com"

# starnapse:
federation:
  # Servers to "spoil" outgoing events for (server soft-blocking).
  # Servers in this list will receive tampered copies of events, forcing them to redact them locally before continuing.
  # This allows you to prevent specific remote servers seeing the contents of your events.
  # Exact server names are required - glob and regex is not supported.
  # This is a nuclear option!
  spoil_servers:
    - server.example
    - evil.server.example
  # When an event is being sent to a spoiled remote (above), if the event's sender is not in this list, the event
  # will not be spoiled. This allows you to limit whose events are spoilt.
  # If the list is empty/not provided, all users are spoilt.
  spoil_users:
    - "@user1:starstruck.example"
  # Servers to outright deny federation with (server hard-blocking).
  # Servers in this list will not be permitted to make federation requests to your server at all, and all outgoing
  # federation requests to blocked remotes (including end-to-end encryption and media) will also be prevented.
  # Exact server names are required - glob and regex is not supported.
  # This is a double nuclear option!
  federation_domain_denylist:
    - denied.domain.example
```
