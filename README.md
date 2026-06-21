Demo project for Module 7 - Docker

Steps taken to complete demo:

1. Clone remote repo for the project and push to this repo (so we can have quick access to the code for reference)

2. Download Docker images for running mongoDB and mongo-express (UI)
docker pull mongo
docker pull mongo-express

3. Create new network for running mongoDB and mongo-express
create network -> docker network create mongo-network
check network was created -> docker network ls
<img width="396" alt="Capture d’écran 2022-10-15 à 18 42 30" src="https://user-images.githubusercontent.com/62488871/195998098-dc03d071-051f-4e9e-b801-c74b66509fc8.png">

4. Run mongoDB container
docker run -p 27017:27017 -d -e MONGO_INITDB_ROOT_USERNAME=admin -e MONGO_INITDB_ROOT_PASSWORD=password --name mongo --net mongo-network mongo

5. Run mongo-express container and connect it to mongoDB
docker run -d -p 8081:8081 -e ME_CONFIG_MONGODB_ADMINUSERNAME=admin -e ME_CONFIG_MONGODB_ADMINPASSWORD=password --net mongo-network --name mongo-express -e ME_CONFIG_MONGODB_SERVER=mongo mongo-express

6. Connect node server with mongoDB container
<img width="554" alt="Capture d’écran 2022-10-15 à 19 07 59" src="https://user-images.githubusercontent.com/62488871/195999139-741587f7-8934-4381-b1d6-04d21811b0cb.png">
<img width="1175" alt="Capture d’écran 2022-10-15 à 19 08 16" src="https://user-images.githubusercontent.com/62488871/195999153-691ce01c-7553-4589-9462-a35cd0054e49.png">

7. Run containers this time using docker compose file, test it works, then stop containers using docker compose cmd
docker-compose -f docker-compose.yaml up
docker-compose -f docker-compose.yaml down

8. Build app using Docker image file
create the file
docker build -t my-app:1.0 .
<img width="454" alt="Capture d’écran 2022-10-15 à 20 11 32" src="https://user-images.githubusercontent.com/62488871/196001865-eaf64bed-7d4a-4a67-8e44-57e34d118be0.png">

9. Run newly built image above
<img width="470" alt="Capture d’écran 2022-10-15 à 20 18 12" src="https://user-images.githubusercontent.com/62488871/196002134-1d0446db-c74b-49ea-95fd-52b7f9ead8c0.png">

10. Create repository for Docker image with AWS ECR
<img width="1073" alt="Capture d’écran 2022-10-15 à 21 29 45" src="https://user-images.githubusercontent.com/62488871/196004605-c907651d-7073-43ea-899b-6f105e63dc0c.png">

11. Tag and push image to the repository on AWS
docker tag my-app:latest 395901524773.dkr.ecr.eu-central-1.amazonaws.com/my-app:latest
docker push 395901524773.dkr.ecr.eu-central-1.amazonaws.com/my-app:1.0
<img width="1094" alt="Capture d’écran 2022-10-15 à 21 37 49" src="https://user-images.githubusercontent.com/62488871/196004847-a04258c4-04d2-41f5-9523-1ea596199c87.png">

12. Make change in code, rebuild image and push that img version to repo
<img width="1125" alt="Capture d’écran 2022-10-15 à 21 43 37" src="https://user-images.githubusercontent.com/62488871/196005024-222554aa-c2fd-4a9f-8b2e-93bfd064e0b3.png">

13. Simulate running of app on server by using docker compose file that pulls image from private docker repo and pulls mongo images from docker hub (public)
<img width="560" alt="Capture d’écran 2022-10-15 à 22 12 10" src="https://user-images.githubusercontent.com/62488871/196005877-63c68f0f-57e4-4de6-ba47-d7e000463e64.png">

14. Create volume and attach to mongoDB container to persist data
<img width="365" alt="Capture d’écran 2022-10-15 à 22 38 36" src="https://user-images.githubusercontent.com/62488871/196006646-f72d335b-36f2-4c7f-9057-fb2036c4ed43.png">

15. Create Docker Hosted repo on Nexus
<img width="812" alt="Capture d’écran 2022-10-15 à 23 08 18" src="https://user-images.githubusercontent.com/62488871/196007627-f15d3a61-2f69-409f-8462-eefc0214a0fb.png">
<img width="887" alt="Capture d’écran 2022-10-15 à 23 11 09" src="https://user-images.githubusercontent.com/62488871/196007706-cfed70d6-727b-416a-ae03-b139b8fbc02d.png">
<img width="648" alt="Capture d’écran 2022-10-15 à 23 14 41" src="https://user-images.githubusercontent.com/62488871/196007806-3f2b8617-e06c-4351-aa58-2b66f6b7dcdc.png">
<img width="1220" alt="Capture d’écran 2022-10-15 à 23 15 13" src="https://user-images.githubusercontent.com/62488871/196007820-acc963bd-0a69-4add-b198-049ce91c77e2.png">
<img width="807" alt="Capture d’écran 2022-10-15 à 23 17 01" src="https://user-images.githubusercontent.com/62488871/196007896-3d295644-4100-4423-9a8c-c46d5066d8e0.png">
<img width="861" alt="Capture d’écran 2022-10-15 à 23 25 00" src="https://user-images.githubusercontent.com/62488871/196008123-ccc14033-73ed-4de3-8576-ac9f904f8b20.png">
<img width="352" alt="Capture d’écran 2022-10-15 à 23 25 19" src="https://user-images.githubusercontent.com/62488871/196008135-d9d7abec-004e-4ea8-adbf-e29c8cfc7368.png">
<img width="523" alt="Capture d’écran 2022-10-15 à 23 30 26" src="https://user-images.githubusercontent.com/62488871/196008294-575f7800-69b0-47d2-ac01-9af16148e034.png">
<img width="454" alt="Capture d’écran 2022-10-15 à 23 31 15" src="https://user-images.githubusercontent.com/62488871/196008313-3ed656ce-17fd-4b69-b696-9f1499303327.png">

16. Deploy Nexus as Docker Container
- create new droplet
- ssh into remote server
- install docker
- create volume -> docker volume create --name nexus-data
- run docker nexus image -> docker run -d -p 8081:8081 --name nexus -v nexus-data:/nexus-data sonatype/nexus
<img width="661" alt="Capture d’écran 2022-10-15 à 23 51 55" src="https://user-images.githubusercontent.com/62488871/196008917-000e7d5b-92de-4000-a4af-2c8f7f4eba3e.png">
<img width="676" alt="Capture d’écran 2022-10-15 à 23 58 05" src="https://user-images.githubusercontent.com/62488871/196009102-834ae21e-b316-4c53-a569-4dc44bb76cc7.png">


