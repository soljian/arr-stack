# Arr stack

![image](.readme/overseerr.jpg)

![image](.readme/plex.jpg)

# Add YGG indexer to prowlarr

1. Bash into the prowlarr container `docker compose exec -it prowlarr bash`
2. Go to the custom indexer configs `cd /config/Definitions/Custom`
3. Create a file and paste the content available in the [ygege.yml](custom_indexers/ygege.yml) file `vi ygege.yml`
