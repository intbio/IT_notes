# Quick instructions on using Lomonosov1/2 supercomputers
## Connecting
- Generate public/private key pair and add public key in Octoshell (users.parallel.ru)
`ssh-keygen -t rsa -b 2048 -C 'mgu-user1' -f ~/.ssh/mgu`
- Make an ~/.ssh/config file
```
Host lomo
HostName lomonosov.parallel.ru
User user
IdentityFile PATH_TO_PRIVATE_KEY

Host lomo2
HostName lomonosov2.parallel.ru
User user
IdentityFile PATH_TO_PRIVATE_KEY
```
- connection then should be as easy as ```ssh lomo``` or ```ssh lomo2```


## Understanding queues (Lomonosov-2)
```
module add slurm
sinfo
```
UNDER DEVELOPMENT
