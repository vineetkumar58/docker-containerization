# Experiment-5: Docker - Volumes, Environment Variables, Monitoring & Networks

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
![ ](Screenshots/Exp4/2.png)
![ ](Screenshots/Exp4/3.png)

#### 2.Named Volume
- What Happens?
- Docker maps:```bash mydata → /var/lib/docker/volumes/mydata ```
