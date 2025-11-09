# sshmx - SSH Session Manager for tmux

`sshmx` is a lightweight Bash utility that helps you manage and launch SSH sessions inside [`tmux`](https://github.com/tmux/tmux).  
It integrates with your existing `~/.ssh/config` or lets you add/remove sessions interactively, and provides handy **tmux key bindings** to quickly open SSH connections in popup windows.

---

## ✨ Features
- **Interactive SSH session selector** using [fzf](https://github.com/junegunn/fzf) with multi-select and preview
- **Automatic JSON session store** (`~/.sshmx/sessions.json`)
- **Import from `~/.ssh/config`** or create sessions manually
- **tmux integration**:
  - New SSH sessions open in dedicated `tmux` windows
  - Permanent popup window shortcuts (customizable via `~/.tmux.conf`)
  - Automatic fingerprint acceptance (no host key verification prompts)
- **Interactive SSH key picker** with preview, fingerprint display, and file browser
- **Jump host (ProxyJump) support**
- **Password and private key support** (passwords stored encrypted with GPG; `sshpass` used for authentication)
- **Group support**: Organize and connect to all sessions in a group at once
- **Tag support**: Connect to sessions by comma-separated tags
- **Custom color system**: User-configurable color mappings (e.g., map "blue" → "colour17")
- **Export/Import sessions** for backups and sharing
- **Optional colored output** with [chromaterm](https://github.com/hSaria/Chromaterm)
- **Host IP resolution** using `getent` (if available)
- **Self-installing** script – just run once and it sets itself up
- **Sync with `~/.ssh/config`** preserving encrypted passwords and groups
- **Logging** for debugging parsing issues
- **Session notes**: Add, view, and delete per-session notes
- **Favorites**: Mark and quickly access favorite sessions
- **Templates**: Create reusable session templates for quick setup
- **Health checks**: Verify host connectivity with `nc` or TCP
- **Statistics**: View session counts, usage, and most-connected hosts
- **Bulk edit**: Update groups, jumps, tags, and colors across multiple sessions
- **Search**: Fuzzy search by name, host, group, tags, or description
- **Quick connect**: Partial name matching for fast access
- **Exports**: To SSH config, Ansible inventory, or CSV
- **Automatic daily backups** with retention
- **Full bash completion** for all commands and options

---

## 📦 Requirements
- [tmux](https://github.com/tmux/tmux)
- [fzf](https://github.com/junegunn/fzf)
- [jq](https://github.com/stedolan/jq)
- [gpg](https://gnupg.org/) (for password encryption)
- [sshpass](https://linux.die.net/man/1/sshpass) *(optional, for password auth)*
- [chromaterm](https://github.com/hSaria/Chromaterm) *(optional, for colored output)*
- `getent` *(optional, for hostname to IP resolution; usually available on Linux)*
- `nc` *(optional, for health checks)*

---

## 🚀 Installation
Clone the repository and run the script with the `--install` flag:

```bash
git clone https://github.com/mrbooshehri/sshmx.git
cd sshmx
./sshmx --install
```

This will:
* Create a symlink to `~/.local/bin/sshmx`
* Add keybindings to your `~/.tmux.conf` (or create one if missing)
* Add bash completion for all commands
* Add `~/.local/bin` to your PATH (via `~/.bashrc` or `~/.zshrc`)
* Initialize color mapping configuration

Reload tmux:

```bash
tmux source-file ~/.tmux.conf
```

And source your shell RC file:

```bash
source ~/.bashrc  # or ~/.zshrc
```

---

## 🔑 Usage

### Basic Commands

| Command                        | Description                                                       |
| ------------------------------ | ----------------------------------------------------------------- |
| `sshmx`                        | Launch interactive session selector with `fzf` (multi-select, preview) |
| `sshmx --add` / `sshmx -a`     | Add a new SSH session interactively                               |
| `sshmx --edit` / `sshmx -e`    | Edit an existing session interactively                            |
| `sshmx --remove` / `sshmx -r`  | Remove one or more sessions interactively (with backup)           |
| `sshmx --clone`                | Clone an existing session                                         |
| `sshmx --bulk-edit`            | Bulk-edit multiple sessions (groups, jumps, tags, colors)         |

### Installation & Configuration

| Command                        | Description                                                       |
| ------------------------------ | ----------------------------------------------------------------- |
| `sshmx --install` / `sshmx -i` | Install script, create symlink, add tmux bindings, bash completion |
| `sshmx --sync` / `sshmx -s`    | Sync `~/.sshmx/sessions.json` with `~/.ssh/config` (preserves passwords/groups) |
| `sshmx --configure-colors`     | Configure color name to tmux color mappings (with 256-color palette preview) |
| `sshmx --validate-config`      | Validate SSH config syntax                                        |

### Search & Navigation

| Command                        | Description                                                       |
| ------------------------------ | ----------------------------------------------------------------- |
| `sshmx --search`               | Search sessions by name/host/group/tags/description               |
| `sshmx --quick` / `sshmx -q`   | Quick connect by partial name match                               |
| `sshmx --history` / `sshmx -h` | Show connection history (most recent first)                       |

### Groups & Tags

| Command                        | Description                                                       |
| ------------------------------ | ----------------------------------------------------------------- |
| `sshmx --groups` / `sshmx -g`  | Select and connect to all sessions in chosen group(s)             |
| `sshmx --tags`                 | Connect to all sessions with selected tag(s)                      |
| `sshmx --list-groups`          | List all sessions organized by group                              |

### Favorites & Templates

| Command                        | Description                                                       |
| ------------------------------ | ----------------------------------------------------------------- |
| `sshmx --favorites`            | Show and connect to favorite sessions                             |
| `sshmx --add-favorite`         | Add sessions to favorites                                         |
| `sshmx --templates`            | List available templates                                          |
| `sshmx --add-template`         | Create new session template                                       |
| `sshmx --use-template`         | Create session from template                                      |

### Notes & Documentation

| Command                        | Description                                                       |
| ------------------------------ | ----------------------------------------------------------------- |
| `sshmx --add-note`             | Add note to a session                                             |
| `sshmx --view-notes`           | View notes for a session                                          |
| `sshmx --delete-note`          | Delete a note from a session                                      |

### Health & Statistics

| Command                        | Description                                                       |
| ------------------------------ | ----------------------------------------------------------------- |
| `sshmx --check`                | Check connectivity of selected sessions                           |
| `sshmx --check-all`            | Check connectivity of all sessions                                |
| `sshmx --stats`                | Show session statistics and usage                                 |

### Multiplex Operations

| Command                        | Description                                                       |
| ------------------------------ | ----------------------------------------------------------------- |
| `sshmx --multiplex` / `sshmx -m` | Combine windows into synchronized pane for multi-host commands   |
| `sshmx --demultiplex` / `sshmx -d` | Restore multiplexed windows to separate panes                    |

### Import/Export

| Command                        | Description                                                       |
| ------------------------------ | ----------------------------------------------------------------- |
| `sshmx --export [file]`        | Export `sessions.json` to a backup file                           |
| `sshmx --import [file]`        | Import sessions from a JSON file (backs up existing)              |
| `sshmx --export-config [file]` | Export to SSH config format                                       |
| `sshmx --export-ansible [file]` | Export to Ansible inventory format                               |
| `sshmx --export-csv [file]`    | Export to CSV format                                              |

### Other

| Command                        | Description                                                       |
| ------------------------------ | ----------------------------------------------------------------- |
| `sshmx --completion`           | Generate bash completion script                                   |
| `sshmx --debug`                | Debug sessions.json structure                                     |
| `sshmx --help`                 | Show help message                                                 |

---

## 🎨 Color Configuration

The script supports a user-configurable color mapping system that lets you use friendly color names (like "blue", "white") that map to actual tmux color values (like "colour17", "colour7").

### Configure Colors

```bash
sshmx --configure-colors
```

This opens an interactive menu where you can:
1. **Edit a color mapping** - Map friendly names to tmux colors
2. **Reset to defaults** - Restore default color mappings
3. **Edit color map file directly** - Advanced editing
4. **Show tmux color reference** - View 256-color palette with visual demonstration
5. **Back** - Return to main menu

### Color Reference

The color reference displays all 256 colors with their numbers, making it easy to pick the exact color you want:

```
256 Color Palette Demonstration:
=================================

  0   1   2   3   4   5   6   7   8   9  10  11  12  13  14  15  16  17
 18  19  20  21  22  23  24  25  26  27  28  29  30  31  32  33  34  35
...

Note: The number shown is the 'colourN' value for tmux
Example: Number 17 = colour17, Number 7 = colour7
```

### Example Configuration

1. Run `sshmx --configure-colors`
2. Select "Edit a color mapping"
3. Choose "blue" from the list
4. Enter "colour17" as the tmux value
5. Repeat for "white" → "colour7"

Now when you add or edit sessions and select "blue" as background and "white" as foreground, it will automatically use colour17 and colour7.

### Supported Color Formats

- **Named colors**: `black`, `red`, `green`, `yellow`, `blue`, `magenta`, `cyan`, `white`
- **Bright colors**: `brightblack`, `brightred`, `brightgreen`, etc.
- **256 colors**: `colour0` to `colour255`
- **Hex colors**: `#RRGGBB` (e.g., `#1a1a1a`)
- **Default**: `default` (terminal default color)

---

## 🔑 SSH Key Picker

When adding or editing sessions, sshmx provides an interactive SSH key picker:

```bash
sshmx --add
# When prompted "Use SSH key? (y/n):"
```

The picker shows:
- All SSH keys in `~/.ssh/` (id_*, *_rsa, *_ed25519, *.pem)
- File info (size, permissions)
- Key fingerprint (if valid)
- Options for:
  - `[No key]` - Don't use a key
  - `[Enter path manually]` - Type a custom path
  - Browse and select from found keys

---

## 🔐 Password Encryption

Passwords are automatically encrypted with GPG when you add them:

```bash
sshmx --add
# Enter password when prompted
# Password is encrypted and stored as password_encrypted
```

**Important**: Configure GPG agent for better experience:

```bash
echo "default-cache-ttl 3600" >> ~/.gnupg/gpg-agent.conf
gpgconf --kill gpg-agent
```

This caches your GPG passphrase for 1 hour, so you don't have to re-enter it for every connection.

---

## 🚫 SSH Fingerprint Handling

sshmx automatically disables SSH host key checking to avoid fingerprint prompts:

```bash
# These options are automatically added:
-o StrictHostKeyChecking=no
-o UserKnownHostsFile=/dev/null
```

This means you won't be prompted to accept host fingerprints on first connection. If you need strict host key checking for security, you can modify the `SSH_OPTIONS` constant in the script.

---

## 🖥️ Tmux Keybindings

After installation, the following keybindings are available in tmux:

| Shortcut                | Action                                      |
|-------------------------|---------------------------------------------|
| `prefix + C-s`          | Open session selector in popup              |
| `prefix + C-g`          | Open group selector in popup                |
| `prefix + C-m`          | Open multiplex selector in popup            |
| `prefix + C-q`          | Demultiplex panes back to separate windows  |
| `prefix + C-y`          | Connection history in popup                 |
| `prefix + C-k`          | Check all hosts                             |

**Note**: `prefix` is typically `Ctrl+b` in tmux.

---

## 📂 Directory Structure

```
~/.sshmx/
├── sessions.json          # Your SSH sessions
├── templates.json         # Session templates
├── history.json           # Connection history and favorites
├── color-map.json         # Custom color mappings
├── sshmx.log             # Activity log
└── backups/              # Automatic daily backups
    └── auto-backup-YYYYMMDD.json.gz
```

---

## 📝 Session JSON Format

```json
{
  "session_name": {
    "host": "example.com",
    "user": "username",
    "port": 22,
    "key": "~/.ssh/id_rsa",
    "password_encrypted": "base64_encrypted_data...",
    "jump": "bastion",
    "group": "production",
    "port_forward": "8080:localhost:80",
    "description": "Production web server",
    "tags": "web,production,critical",
    "bg_color": "blue",
    "fg_color": "white",
    "notes": ["Note 1", "Note 2"],
    "created": 1699564800,
    "modified": 1699564800
  }
}
```

---

## 🛠️ Example Workflows

### Quick Start

```bash
# 1. Sync from existing SSH config
sshmx --sync

# 2. Launch interactive selector
sshmx

# 3. Or use tmux shortcut
# Press: Ctrl+b then C-s
```

### Adding a Session with Custom Colors

```bash
# 1. Add a new session
sshmx --add

# 2. Follow prompts:
Session name: webserver
Host/IP: 192.168.1.100
User: admin
Port: 22

# 3. Use SSH key picker
Use SSH key? (y/n): y
# Select from ~/.ssh/ or enter path

# 4. Configure colors
Select colors (or press ESC for default):
# Use fzf to select "blue" for background
# Use fzf to select "white" for foreground

# Session created with your configured color mapping!
```

### Connect to Group

```bash
# Connect to all servers in "production" group
sshmx --groups

# Select "production" from list
# All sessions open in separate tmux windows
```

### Multiplex for Commands

```bash
# 1. Connect to multiple servers
sshmx --groups  # or select multiple sessions

# 2. Multiplex them
sshmx --multiplex

# 3. Commands are now sent to all servers simultaneously
# Type once, execute everywhere!

# 4. When done, demultiplex
sshmx --demultiplex

# Each server returns to its own window
```

### Search and Quick Connect

```bash
# Search by any field
sshmx --search
Search query: production web

# Or quick partial match
sshmx --quick
Quick connect: prod
# Instantly connects if unique, or shows matches
```

---

## 🔒 Security Best Practices

1. **Use SSH keys** instead of passwords whenever possible
2. **Configure GPG agent** to cache passphrases securely
3. **Review backups** before importing sessions
4. **Keep sessions.json private** (`chmod 600 ~/.sshmx/sessions.json`)
5. **Regularly rotate** SSH keys and passwords
6. **Enable strict host checking** in production if required (modify `SSH_OPTIONS`)
7. **Use jump servers** (bastions) for accessing internal networks
8. **Encrypt the entire ~/.sshmx directory** for additional security (optional)

---

## 🐛 Troubleshooting

### GPG Decryption Fails

```bash
# Set GPG_TTY
export GPG_TTY=$(tty)

# Configure agent
echo "default-cache-ttl 3600" >> ~/.gnupg/gpg-agent.conf
gpgconf --kill gpg-agent
```

### Colors Don't Appear

```bash
# Check color mapping
cat ~/.sshmx/color-map.json

# Reconfigure colors
sshmx --configure-colors

# Test in tmux
tmux setw window-style "fg=colour7,bg=colour17"
```

### Keybindings Don't Work

```bash
# Reload tmux config
tmux source-file ~/.tmux.conf

# Check bindings
tmux list-keys | grep sshmx

# Reinstall
sshmx --install
```

### Debug Sessions

```bash
# Validate JSON
sshmx --debug

# Check logs
tail -50 ~/.sshmx/sshmx.log
```

---

## 🤝 Contributing

Pull requests are welcome! If you find a bug or want a feature, open an [issue](../../issues).

### Development

```bash
# Enable debug output
bash -x sshmx [command]

# Check logs
tail -f ~/.sshmx/sshmx.log
```

---

## 📜 License

MIT License © 2025 mrbooshehri

---

## 🙏 Acknowledgments

- [tmux](https://github.com/tmux/tmux) - Terminal multiplexer
- [fzf](https://github.com/junegunn/fzf) - Fuzzy finder
- [jq](https://github.com/stedolan/jq) - JSON processor
- [chromaterm](https://github.com/hSaria/Chromaterm) - Color output
- All contributors and users!

---

## 📊 Changelog

### v2.1
- ✨ Added user-configurable color mapping system
- 🎨 Interactive 256-color palette viewer
- 🔑 Interactive SSH key picker with preview
- 🚫 Automatic SSH fingerprint acceptance
- 🐛 Fixed keybinding installation issues
- 📝 Improved documentation

### v2.0
- 🚀 Initial release with full feature set
- 📦 Session management with groups and tags
- 🔐 GPG password encryption
- 📊 Statistics and health checks
- 🎯 Templates and favorites
- 📤 Multiple export formats