Эта команда удалит все изображения, которые не связаны с контейнером. Вы также можете использовать следующую команду для удаления всех неиспользуемых изображений, которые не используются ни одним контейнером:


        docker-compose down
        docker image prune -f
        docker image prune -a -f
        docker builder prune

        docker-compose down && docker image prune -f && docker image prune -a -f &&  docker builder prune

# Зайти в контейнер 

        docker exec -it bx-php-fpm sh -c "echo a && echo b"
       
        docker exec -it bx-php-fpm bash



connection to localhost in local machine



# Автоматическая очистка кеша Docker и освобождение места

1. **Remove all stopped containers**:
   ```sh
   docker container prune
   ```

2. **Remove all unused images**:
   ```sh
   docker image prune -a
   ```

3. **Remove all unused volumes**:
   ```sh
   docker volume prune
   ```

4. **Remove all unused networks**:
   ```sh
   docker network prune
   ```

5. **Remove all unused data (containers, images, volumes, and networks)**:
   ```sh
   docker system prune -a
   ```

These commands will help you clear the Docker cache and free up space.




start docker for running connect in local docker machine to localhost
 
        docker run --network host <image_name>



docker delete all containers

        docker rm $(docker ps -a -q)
docker delete all images

        docker rmi $(docker images -q)
docker delete all volumes 

        docker volume rm $(docker volume ls -q)
docker delete all networks
    
            docker network rm $(docker network ls -q)
docker delete all containers, images, volumes, networks
    
            docker rm $(docker ps -a -q) && docker rmi $(docker images -q) && docker volume rm $(docker volume ls -q) && docker network rm $(docker network ls -q)
docker delete all containers, images, volumes, networks, and cache

            docker rm $(docker ps -a -q) && docker rmi $(docker images -q) && docker volume rm $(docker volume ls -q) && docker network rm $(docker network ls -q) && docker system prune -a
docker delete all containers, images, volumes, networks, and cache, and stop all containers
    
               docker stop $(docker ps -q) && docker rm $(docker ps -a -q) && docker rmi $(docker images -q) && docker volume rm $(docker volume ls -q) && docker network rm $(docker network ls -q) && docker system prune -a
docker delete all containers, images, volumes, networks, and cache, and stop all containers, and remove all stopped containers
    
                docker rm $(docker ps -a -q) && docker rmi $(docker images -q) && docker volume rm $(docker volume ls -q) && docker network rm $(docker network ls -q) && docker system prune -a && docker stop $(docker ps -q) 
