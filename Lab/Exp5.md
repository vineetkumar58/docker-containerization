<img width="1470" height="243" alt="image" src="https://github.com/user-attachments/assets/5a30d02e-c4ee-41a5-a3a7-b529c0f4c9d9" /># Experiment-5: Docker - Volumes, Environment Variables, Monitoring & Networks

## PART 1 – DOCKER VOLUMES
### LAB 1 – Data is Ephemeral
- Created container.
- Inside it created a text file.
- After restarting container test file remains.
- But after removing the container text file also gone.
  
 ![ ](Screenshots/Exp5/1.png)

### LAB 2 – Volume Types

#### 1.Anonymous Volume
- You’ll see random hash name,Why?
- Because:
  - No volume name provided
  - Docker auto-created volume
    
![ ](Screenshots/Exp5/2.png)
![ ](Screenshots/Exp5/3.png)

#### 2.Named Volume
- What Happens?
- Docker maps: ``` mydata → /var/lib/docker/volumes/mydata ```

![ ](Screenshots/Exp5/4.png)

#### 3.Bind Mount
- bind mount links: Host folder ↔ Container folder

![ ](Screenshots/Exp5/5.png)

### LAB 3 – Database with Volume
- Ran mysql
- Stopped and Removed it
- Ran again
- Why Data Still Exists?
- Because: Database files are stored in volume, not in container.

![ ](Screenshots/Exp5/6.png)

-EXAMPLE 2 : WITH CONFIGURATION FILES:
![ ](Screenshots/Exp5/18.png)


## PART 2 – ENVIRONMENT VARIABLES

### LAB - 1 :Setting env variables
### Method 1 –> -e flag
![ ](Screenshots/Exp5/7.png)
![ ](Screenshots/Exp5/77.png)

### Method 2 –> --env-file
-Create env file:
![ ](Screenshots/Exp5/8.png)

-use env file:
![ ](Screenshots/Exp5/88.png)

-use multiple env file:
![ ](Screenshots/Exp5/888.png)

- Environment variables allow:
  - Configuration without changing image
  - 12-factor app principle
    
### LAB - 2 : Environment Variables in Applications
![ ](Screenshots/Exp5/app.png)
![ ](Screenshots/Exp5/dock.png)

### LAB - 3 : TEST Environment Variables
![ ](Screenshots/Exp5/test.png)


## PART 3 – MONITORING

#### 1. 
``` Docker stats ```
- Shows:
  - CPU %
  - Memory usage
  - Network IO
  - Block IO

![ ](Screenshots/Exp5/9.png)

``` docker stats container1 container2```
![ ](Screenshots/Exp5/app1app2.png)

``` docker stats --no-stream```
![ ](Screenshots/Exp5/nostr.png)

``` docker stats --all```
![ ](Screenshots/Exp5/statsall.png)

#### 2. 
``` docker top container-name ```
- Shows running processes inside container.

![ ](Screenshots/Exp5/10.png)

#### 3.
```docker logs container-name```
```docker logs -f container-name```
    
![ ](Screenshots/Exp5/11.png)

#### 4.
```docker inspect container-name```

![ ](Screenshots/Exp5/12.png)
![ ](Screenshots/Exp5/13.png)

## PART 4 – NETWORKS
#### 1.
```docker network ls```
![ ](Screenshots/Exp5/14.png)

#### Custom Bridge Network :
![ ](Screenshots/Exp5/15.png)

#### Host Network:
![ ](Screenshots/Exp5/16.png)

#### None Network:
![ ](Screenshots/Exp5/17.png)

#### overlay Network:
![ ](Screenshots/Exp5/overlay.png)

#### Multi container application example
![ ](Screenshots/Exp5/multi.png)
![ ](Screenshots/Exp5/multi1.png)

#### Newtork inspection and debugging
![ ](Screenshots/Exp5/inspect.png)
![ ](Screenshots/Exp5/inspect1.png)
![ ](Screenshots/Exp5/inspect2.png)

#### Port Mapping
![ ](Screenshots/Exp5/port.png)
