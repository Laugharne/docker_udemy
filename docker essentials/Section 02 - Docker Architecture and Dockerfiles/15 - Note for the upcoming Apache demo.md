[**Course: Docker Essentials | Udemy**](https://www.udemy.com/course/docker-essentials/learn/lecture/17426276#overview)

## Note for the upcoming Apache demo

Hello everyone!

So, in the previous lecture, we containerized Nginx web-server and ubuntu as the runtime environment for it.  We exposed the Nginx container's port 80 and mapped it with Docker host's port 8080.

Our container is still serving on the host port 8080.

Now, in order to run our Apache container successfully, we need to free up port 8080 for course continuity purposes.

Use the following command to do so:

`docker container stop cont_expose`

  

Note: This updated text has been added as a result of multiple student queries.

  

Happy Learning:)