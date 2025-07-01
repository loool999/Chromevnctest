# VNC Server Setup Guide

## Overview
A complete VNC server setup with XFCE desktop environment, audio support, and Google Chrome browser.

## Server Details
- **VNC Server**: TigerVNC
- **Desktop Environment**: XFCE4 with full desktop experience
- **Display**: :3 (Port 5903)
- **Resolution**: 1920x1080 (24-bit color depth)
- **Audio**: PulseAudio enabled
- **Password**: `vnc123`

## Installed Applications
- **Google Chrome** - Modern web browser
- **Firefox ESR** - Alternative web browser
- **XFCE File Manager (Thunar)** - File management
- **Mousepad** - Text editor
- **XFCE Terminal** - Command line interface
- **Task Manager** - System monitoring
- **Archive Manager** - File compression/extraction
- **Image Viewer (Ristretto)** - Photo viewing
- **Various XFCE plugins** - Enhanced desktop functionality

## Connection Information

### VNC Client Connection
- **Host**: `localhost` or your server's IP address
- **Port**: `5903`
- **Display**: `:3`
- **Password**: `vnc123`

### Example Connection Commands
```bash
# Using vncviewer
vncviewer localhost:5903

# Using tigervnc viewer with authentication
vncviewer -passwd ~/.vnc/passwd localhost:5903

# Using remmina (GUI client)
# Host: localhost:5903
# Password: vnc123
```

## Management Scripts

### Start VNC Server
```bash
./start-vnc
```

### Stop VNC Server
```bash
./stop-vnc
```

### Manual Commands
```bash
# Start VNC server manually
vncserver :3 -depth 24 -geometry 1920x1080

# Stop VNC server manually
vncserver -kill :3

# List running VNC sessions
vncserver -list
```

## Features

### Desktop Environment
- **Full XFCE Desktop** with taskbar, desktop icons, and system tray
- **Custom wallpaper** - Ubuntu-themed background
- **Audio support** via PulseAudio
- **File manager** integration
- **Multiple workspace** support

### Applications Available
- **Google Chrome** - Desktop shortcut available
- **Firefox ESR** - Alternative browser
- **Text editor** - Mousepad for file editing
- **Terminal** - XFCE Terminal for command line
- **File manager** - Thunar for file operations
- **System monitor** - Task manager for system info

### Audio Configuration
- PulseAudio server automatically starts with VNC session
- Audio mixer controls available through desktop
- Compatible with VNC clients that support audio forwarding

## Configuration Files

### VNC Configuration
- **Config**: `~/.vnc/config`
- **Password**: `~/.vnc/passwd`
- **Startup Script**: `~/.vnc/xstartup`

### Desktop Configuration
- **XFCE Settings**: `~/.config/xfce4/`
- **Wallpaper**: `/home/scrapybara/ubuntu-wallpaper.jpg`

## Security Notes

⚠️ **Important Security Information**:
- Default password is `vnc123` - change this for production use
- VNC traffic is not encrypted by default
- Server listens on all interfaces (0.0.0.0)
- Consider using SSH tunneling for secure remote access

### Changing VNC Password
```bash
vncpasswd
# Enter new password when prompted
# Restart VNC server after changing
```

### SSH Tunnel Example
```bash
# On client machine - create SSH tunnel
ssh -L 5903:localhost:5903 username@your-server

# Then connect VNC client to localhost:5903
vncviewer localhost:5903
```

## Troubleshooting

### Common Issues

**VNC Server Won't Start**
```bash
# Check if port is already in use
netstat -tuln | grep 5903

# Kill existing session
vncserver -kill :3

# Check VNC log files
ls -la ~/.vnc/
cat ~/.vnc/cloud:3.log
```

**Desktop Doesn't Load**
```bash
# Verify xstartup script is executable
chmod +x ~/.vnc/xstartup

# Check XFCE is properly installed
which startxfce4

# Restart VNC server
./stop-vnc && ./start-vnc
```

**Audio Not Working**
```bash
# Check PulseAudio status
pulseaudio --check -v

# Restart PulseAudio
pulseaudio --kill
pulseaudio --start

# Check audio devices
pactl list short sources
pactl list short sinks
```

**Chrome Won't Start**
```bash
# Run Chrome with debugging
google-chrome-stable --no-sandbox --disable-dev-shm-usage

# Check for missing dependencies
ldd /usr/bin/google-chrome-stable
```

### Log Files
- VNC Server Log: `~/.vnc/cloud:3.log`
- XFCE Log: `~/.xsession-errors`
- System Log: `/var/log/syslog`

## Performance Optimization

### For Better Performance
1. **Reduce color depth**: Use `-depth 16` instead of 24
2. **Lower resolution**: Use 1280x720 for slower connections
3. **Compression**: Use VNC client compression settings
4. **Network**: Use wired connection when possible

### Example Optimized Start Command
```bash
vncserver :3 -depth 16 -geometry 1280x720
```

## Advanced Configuration

### Custom Resolution
```bash
# Add custom resolution
vncserver :3 -geometry 1600x900 -depth 24
```

### Multiple Sessions
```bash
# Start additional VNC sessions
vncserver :4 -geometry 1920x1080 -depth 24
vncserver :5 -geometry 1280x720 -depth 16
```

### Autostart Applications
Edit `~/.vnc/xstartup` to add applications that start automatically:
```bash
# Add before 'startxfce4 &'
google-chrome-stable &
mousepad &
```

## Support

For technical support or issues:
1. Check the troubleshooting section above
2. Review log files for error messages
3. Verify all components are properly installed
4. Test with different VNC clients

---

**Setup Date**: $(date)
**Version**: TigerVNC Server with XFCE4
**Status**: Ready for use