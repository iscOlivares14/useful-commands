## Using WSL

If you enabled Windows Subsystem Linux to work you could face some permissions issues between Windows and your Subsystem in my case Ubuntu Bionic.

Sometimes git can detect the remote configuration or even the .git directory, and you need to set an environment variable lo let WSL access Windows file system correctly.

```bash
export GIT_DISCOVERY_ACROSS_FILESYSTEM=1
```
More details: 
https://newbedev.com/git-discovery-across-filesystem-not-set

## Save local git repo in github

