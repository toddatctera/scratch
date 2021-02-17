# bash tips and tricks and things

### Load passphrase protected ssh key to the ssh-agent

    eval $(ssh-agent)
    Agent pid 2157

Once ssh-agent is running, you will need to add your private key to its cache

    ssh-add ~/.ssh/id_ed25519
    
 [ssh-agent](https://wiki.archlinux.org/index.php/SSH_keys#SSH_agents)
