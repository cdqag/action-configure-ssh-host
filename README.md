# Configure SSH Host

A GitHub Action to create a new ssh/config entry for given host

## Usage

```yaml
- uses: cdqag/action-configure-ssh-host@v1
  with:
    name: my-server
    host: example.com
    port: 22
    user: deploy
    private-key: ${{ secrets.SSH_PRIVATE_KEY }}
```

## Inputs

* `name` **Required**

    A unique name for the server config entry.

* `host` **Required**

    The host address.

* `port` _Default: '22'_

    SSH port.

* `user` **Required**

    An user.

* `private-key` **Required**

    Private SSH key for the user.

* `fail-if-entry-already-exists` _Default: 'true'_

    If set to `true`, the action will fail if the SSH config entry already exists. If set to `false`, it will skip the rest of the steps.

## Outputs

* `server-name`

    The name for the server in the config (sanitized version of the input name).

## Examples

### Basic SSH configuration

This example will:

* create an SSH config entry named `production-server`
* configure connection to `prod.example.com` on port 22
* use the `deploy` user for authentication
* use the private key from secrets

```yaml
- uses: cdqag/action-configure-ssh-host@v1
  with:
    name: production-server
    host: prod.example.com
    user: deploy
    private-key: ${{ secrets.SSH_PRIVATE_KEY }}
```

### SSH configuration with custom port

This example will:

* create an SSH config entry named `staging-server`
* configure connection to `staging.example.com` on port 2222
* use the `ubuntu` user for authentication
* use the private key from secrets

```yaml
- uses: cdqag/action-configure-ssh-host@v1
  with:
    name: staging-server
    host: staging.example.com
    port: 2222
    user: ubuntu
    private-key: ${{ secrets.SSH_PRIVATE_KEY }}
```

## How it works

This action:

1. **Sanitizes the server name** - converts the provided name to a safe format for SSH config
2. **Creates SSH directory** - ensures `~/.ssh` exists with proper permissions (700)
3. **Checks for duplicates** - verifies the host entry doesn't already exist in SSH config
4. **Creates private key file** - saves the private key to `~/.ssh/{server-name}.key` with secure permissions (600)
5. **Scans host keys** - adds the server's public keys to `~/.ssh/known_hosts`
6. **Creates SSH config entry** - adds a new Host entry to `~/.ssh/config`

After running this action, you can connect to your server using:

```bash
ssh {server-name}
```

## Resources

*  This action is based on [invi5H/ssh-action](https://github.com/invi5H/ssh-action), but it seems abandoned and not maintained anymore.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.
