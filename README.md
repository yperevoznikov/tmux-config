# tmux-config

A customized tmux configuration with vim-tmux integration, smart pane switching, and the Dracula theme.

## Features

- **Vim Integration**: Seamless navigation between vim and tmux panes using Ctrl+hjkl (via [vim-tmux-navigator](https://github.com/christoomey/vim-tmux-navigator))
- **Smart Pane Switching**: Intelligent pane navigation that respects vim splits
- **Dracula Theme**: Beautiful color scheme with powerline support
- **Mouse Support**: Full mouse support for pane selection and window management
- **Custom Bindings**: 
  - `Ctrl+Space` as prefix (instead of default `Ctrl+b`)
  - `Ctrl+Shift+Left/Right` to move and switch windows
  - `Alt+o` to switch to last window
  - New panes and windows inherit current directory
- **Plugin Manager**: Uses [tpm](https://github.com/tmux-plugins/tpm) for plugin management

## Installation

### Prerequisites

- tmux (version 2.9+)
- curl
- Git (for tpm plugin manager)

### Quick Install

Run the installation script to:
1. Backup your current tmux configuration (if it exists)
2. Download the latest configuration from this repository

```bash
#!/bin/bash

# Backup existing config if it exists
if [ -f ~/.tmux.conf ]; then
    BACKUP_FILE="$HOME/.tmux.conf.back.$(date +%Y%m%d_%H%M%S)"
    cp ~/.tmux.conf "$BACKUP_FILE"
    echo "✓ Backed up existing config to: $BACKUP_FILE"
fi

# Download new configuration
curl -fsSL https://raw.githubusercontent.com/yperevoznikov/tmux-config/refs/heads/main/.tmux.conf -o ~/.tmux.conf
echo "✓ Downloaded tmux configuration to ~/.tmux.conf"

# Create plugins directory if it doesn't exist
mkdir -p ~/.tmux/plugins

# Clone tpm if not already installed
if [ ! -d ~/.tmux/plugins/tpm ]; then
    git clone https://github.com/tmux-plugins/tpm ~/.tmux/plugins/tpm
    echo "✓ Installed tmux plugin manager"
fi

echo ""
echo "Installation complete! Start a new tmux session and press Ctrl+Space + I to install plugins."
```

### Manual Installation

If you prefer to do it step by step:

```bash
# 1. Backup your current config
if [ -f ~/.tmux.conf ]; then
    cp ~/.tmux.conf ~/.tmux.conf.back.$(date +%Y%m%d_%H%M%S)
fi

# 2. Download the configuration
curl -fsSL https://raw.githubusercontent.com/yperevoznikov/tmux-config/refs/heads/main/.tmux.conf -o ~/.tmux.conf

# 3. Reload tmux
tmux source-file ~/.tmux.conf
```

## Post-Installation

After installation, the first time you start tmux, install the plugins:

```bash
tmux new-session -d
# Press: Ctrl+Space + I (capital i) to install plugins
```

Or run directly:

```bash
~/.tmux/plugins/tpm/bin/install_plugins
```

## Configuration

### Key Bindings

| Binding | Action |
|---------|--------|
| `Ctrl+Space` | Prefix key (instead of `Ctrl+b`) |
| `Ctrl+h/j/k/l` | Navigate panes (vim-aware) |
| `Ctrl+\` | Navigate to last active pane |
| `Ctrl+Shift+Left` | Move current window left |
| `Ctrl+Shift+Right` | Move current window right |
| `Ctrl+Space + c` | New window in current directory |
| `Ctrl+Space + "` | Split horizontally in current directory |
| `Ctrl+Space + %` | Split vertically in current directory |
| `Ctrl+Space + o` | Switch to last active window |
| `Ctrl+Space + r` | Reload configuration |

### Customization

Edit `~/.tmux.conf` to customize:

- **Location**: Change `Tbilisi` in the line:
  ```bash
  set -g @dracula-fixed-location "Tbilisi"
  ```

- **Plugins**: Modify the plugins list:
  ```bash
  set -g @plugin 'tmux-plugins/tpm'
  set -g @plugin 'christoomey/vim-tmux-navigator'
  set -g @plugin 'dracula/tmux'
  ```

- **Theme Settings**: Adjust Dracula theme options (all prefixed with `@dracula-`)

After making changes, reload with: `Ctrl+Space + r`

## Troubleshooting

### Plugins not loading
Make sure tpm is installed:
```bash
git clone https://github.com/tmux-plugins/tpm ~/.tmux/plugins/tpm
```

Then press `Ctrl+Space + I` in a tmux session to install plugins.

### Vim keybindings not working
Ensure vim-tmux-navigator is installed and your vim has the corresponding plugin configured. See [vim-tmux-navigator](https://github.com/christoomey/vim-tmux-navigator) for setup.

### Color scheme issues
If colors don't look right, ensure your terminal supports 256 colors:
```bash
echo $TERM
# Should output: xterm-256color or similar
```

## Reverting Changes

To restore your previous configuration:

```bash
rm ~/.tmux.conf
mv ~/.tmux.conf.back.YYYYMMDD_HHMMSS ~/.tmux.conf
tmux source-file ~/.tmux.conf
```

## Inspiration

This configuration is inspired by:
- [faroit's tmux configuration](https://gist.github.com/faroit/ee545a2cec29f5fcc26edb6fe415cfe0)
- [Practical tmux](https://mutelight.org/practical-tmux)

## License

This configuration is provided as-is. Feel free to fork and customize for your needs.
