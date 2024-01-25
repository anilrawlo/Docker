# Docker
Docker script


#!/bin/bash

# Get a list of Docker repositories
repositories=$(docker image ls --format '{{.Repository}}' | sort -u)

# Loop through each repository
for repo in $repositories; do
    # Get a list of image IDs for the repository, excluding the latest two
    images_to_delete=$(docker image ls "$repo" --format '{{.ID}}' | sort -r | awk 'NR>2')

    # Loop through and delete the older images
    for image_id in $images_to_delete; do
        docker rmi "$image_id"
        echo "Deleted: $repo:$image_id"
    done
done
