# Claude Code Context for Home Assistant

You are running inside a Home Assistant add-on with full access to the Home Assistant configuration and system.

## Home Assistant CLI Commands

### Primary CLI Tool: `ha`
The `ha` command is the main CLI for managing Home Assistant:

```bash
# Core management
ha core info                 # Show Core information
ha core restart              # Restart Home Assistant Core
ha core stop/start           # Stop/Start Home Assistant Core
ha core update               # Update Home Assistant Core
ha core check                # Run config check
ha core logs                 # View Core logs

# Add-on management
ha addons info <slug>        # Show add-on info
ha addons start/stop <slug>  # Start/stop add-on
ha addons restart <slug>     # Restart add-on
ha addons update <slug>      # Update add-on
ha addons logs <slug>        # View add-on logs
ha addons stats <slug>       # Show add-on stats

# Supervisor management
ha supervisor info           # Show Supervisor info
ha supervisor restart        # Restart Supervisor
ha supervisor update         # Update Supervisor
ha supervisor logs           # View Supervisor logs

# System management
ha os info                   # Show OS info
ha os update                 # Update OS
ha hardware info             # Show hardware info
ha network info              # Show network info

# Backups
ha backups list              # List all backups
ha backups new               # Create new backup
ha backups restore <slug>    # Restore backup
```

## File System Structure

### Important Directories
- `/config/` - Main configuration directory (you start here)
  - `configuration.yaml` - Main HA configuration
  - `automations.yaml` - Automation definitions
  - `scripts.yaml` - Script definitions
  - `secrets.yaml` - Secret values (passwords, tokens)
  - `groups.yaml` - Group definitions
  - `customize.yaml` - Entity customizations
  - `.storage/` - Internal storage (be careful)
  - `custom_components/` - Custom integrations
  - `www/` - Local web assets

### Add-on Specific
- `/addon_configs/` - Add-on configuration storage
- `/share/` - Shared data between add-ons
- `/backup/` - Backup storage
- `/media/` - Media files
- `/ssl/` - SSL certificates

## Common Tasks

### Check Configuration
```bash
ha core check
```

### Restart After Config Changes
```bash
ha core restart
```

### View Logs for Errors
```bash
ha core logs
ha supervisor logs
ha addons logs <addon-slug>
```

### Create Backup Before Major Changes
```bash
ha backups new --name "Before changes $(date +%Y%m%d_%H%M%S)"
```

## YAML Tips for Home Assistant

### Validate YAML Syntax
```bash
# Install yamllint if needed
apk add --no-cache yamllint
yamllint configuration.yaml
```

### Common YAML Patterns
```yaml
# Secrets reference
api_key: !secret my_api_key

# Include files
automation: !include automations.yaml
script: !include scripts.yaml

# Include directories
automation manual: !include_dir_list automations/
```

## Safety Guidelines

1. **Always backup before major changes**
2. **Check configuration before restart**: `ha core check`
3. **Monitor logs after changes**: `ha core logs --follow`
4. **Test automations carefully** - they can trigger physical devices
5. **Keep secrets in secrets.yaml** - never hardcode passwords

## Entity Information

### List all entities
```bash
ha state list
```

### Get entity state
```bash
ha state get <entity_id>
```

### Common entity patterns:
- `light.living_room` - Lights
- `switch.garage_door` - Switches
- `sensor.temperature` - Sensors
- `binary_sensor.motion` - Binary sensors
- `climate.thermostat` - Climate devices
- `person.john_doe` - People/presence

## Integration Management

Custom integrations are in `/config/custom_components/`. Each integration should have:
- `__init__.py` - Main integration file
- `manifest.json` - Integration metadata
- `config_flow.py` - Configuration UI (optional)

## Useful Bash Commands for HA

```bash
# Find all automation files
find /config -name "*.yaml" -type f | xargs grep -l "automation:"

# Check for common issues
grep -r "api_key\|password\|token" /config/*.yaml | grep -v secrets.yaml

# Monitor file changes
watch -n 1 'ha core check'
```

Remember: You have full system access. Be careful with destructive commands and always test changes before deploying to production.