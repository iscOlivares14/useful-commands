## Using WSL

If you enabled Windows Subsystem Linux to work you could face some permissions issues between Windows and your Subsystem in my case Ubuntu Bionic.

Vagrant version installed on Windows and WSL must match, and you need to set an environment variable lo let WSL access Windows file system correctly.

```bash
export VAGRANT_WSL_ENABLE_WINDOWS_ACCESS="1"
```
More details: 
https://devblogs.microsoft.com/commandline/automatically-configuring-wsl/### Using WSL
