docker pull ubuntu 
----
docker run --name my-container -d  ubuntu sleep 3000
--
docker exec -it my-container bash 