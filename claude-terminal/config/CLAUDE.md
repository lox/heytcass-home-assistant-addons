# Claude Code Context for Home Assistant

⚠️ **IMPORTANT: You are working on a LIVE Home Assistant system!** ⚠️

You are running inside a Home Assistant add-on with full access to the Home Assistant configuration and system. All changes you make are immediate and affect the running system. There is no sandbox or test environment.

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

### ⚠️ Working with Live Configuration
1. **You are modifying a LIVE system** - All changes take effect immediately
2. **Always backup before major changes**: `ha backups new --name "Before_$(date +%Y%m%d_%H%M%S)"`
3. **Check configuration before restart**: `ha core check`
4. **Monitor logs after changes**: `ha core logs --follow`
5. **Test automations carefully** - they can trigger physical devices
6. **Keep secrets in secrets.yaml** - never hardcode passwords
7. **Consider creating test entities** before modifying production ones

### Cache Busting for Web UI Testing
When modifying Lovelace dashboards or frontend resources, the browser may cache old versions. Users should:

1. **Force refresh the browser**:
   - Chrome/Edge: `Ctrl+Shift+R` (Windows/Linux) or `Cmd+Shift+R` (Mac)
   - Firefox: `Ctrl+F5` (Windows/Linux) or `Cmd+Shift+R` (Mac)
   - Safari: `Cmd+Option+E` then `Cmd+R`

2. **Add version parameters to resources**:
   ```yaml
   # In configuration.yaml or ui-lovelace.yaml
   resources:
     - url: /local/my-card.js?v=1.0.1
       type: module
   ```

3. **Clear Home Assistant frontend cache**:
   - Go to Developer Tools → Services
   - Call service: `frontend.reload_themes`
   - Or restart Home Assistant: `ha core restart`

4. **Use browser dev tools**:
   - Open with F12
   - Go to Network tab
   - Check "Disable cache" while dev tools are open

5. **For custom cards/resources**:
   ```bash
   # Increment version in resource URLs after changes
   sed -i 's/\?v=1\.0\.0/?v=1.0.1/g' /config/ui-lovelace.yaml
   ```

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