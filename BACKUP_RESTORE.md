## Backup & Restore remote /home folder
The follwoing are the commands for backuping and restoring remote /home folder.
Please make sure the local side have suffiecient space for backuping !
```bash
RMIP=<192.168...>

# Creact tarball from the remote and Output to here
ssh mis@"$RMIP" "sudo -S tar -cpzvf - -C / ./home" | cat > ./backup.tar.gz

# Remove /home if needed
ssh mis@"$RMIP" "sudo -S  rm -rf /home"

# Send tarball to stdin and Extract content to the remote 
cat ./backup.tar.gz | ssh mis@"$RMIP" "cd / && sudo -S tar -xpzvf -"
rm backup.tar.gz
```
Note:
Standalone '-': means "write the tarball to stdout" instead of a local file.
Command 'sudo -S': to read the password from stdin.
