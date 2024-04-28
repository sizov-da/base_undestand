

        docker exec -it bx-php-fpm sh -c "echo a && echo b"
       
        docker exec -it bx-php-fpm bash



connection to localhost in local machine

start docker for running connect in local docker machine to localhost
 
        docker run --network host <image_name>
