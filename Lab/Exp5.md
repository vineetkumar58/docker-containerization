# Experiment-5: Docker - Volumes, Environment Variables, Monitoring & Networks

## PART 1 – DOCKER VOLUMES
### LAB 1 – Data is Ephemeral
- Created container.
- Inside it created a text file.
- After restarting container test file remains.
- But after removing the container text file also gone.

### LAB 2 – Volume Types

#### 1.Anonymous Volume
- You’ll see random hash name,Why?
- Because:
  - No volume name provided
  - Docker auto-created volume
