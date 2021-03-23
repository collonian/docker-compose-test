# Real World docker-compose

## 준비
Submodule을 포함하여 git repo.를 clone 한다.
```
git clone --recurse-submodules git@github.com:collonian/realworld-compose.git
```

## docker-compose 구성
docker-compose.yaml 파일에는 2개의 서비스(`frontend`, `backend`)가 설정되어 있으며, 
각각 `frontend`와 `backend` 네트워크에 연력되어 있다. `frontend`는 3000번 포트로 서비스를 제공하며, 
`backend`는 8080 포트로 서비스를 제공한다.

`backend`의 경우 database 접속을 위하여 `mysql-network`에도 접속하고 있으며,
`mysql-network`는 이미 생성되어 있다고 가정한다.

`dev` 환경과 `prod` 환경을 분리하기 위해, 각 환경별로 `docker-compose.dev.yaml`, `docker-compose.prod.yaml`파일이 있다.
`dev` 환경에서는 docker 이미지를 빌드하도록 되어 있으며, `prod`환경에서는 이미지빌드 없이 등록되어 있는 이미지를 사용한다.

## docker-compose 사용

1. Database network
   Database에 대한 네트워크를 분리하기 위하여 docker network를 생성한다.
   ```
   sudo docker network create mysql-network
   ```
1. Database   
   `mysql` 폴더로 이동한 후, `docker-compose up` 명령으로 database를 기동한다.
   ```shell
   cd ./mysql
   sudo docker-compose up -d
   ```
1. 빌드   
   `docker-compose build` 명령을 이용하여 Application에 필요한 이미지를 빌드한다. 
   ```shell
   sudo docker-compose -f docker-compose.yaml -f docker-compose.dev.yaml build
   ```

1. Application 기동   
   `docker-compose up` 명령을 이용하여 Application을 기동한다.   
   backend는 http://localhost:8080 으로 접근할 수 있고, frontend는 http://localhost:3000 으로 접속할 수 있다.

   dev 환경으로 기동하려면 `docker-compose.yaml`, `docker-compose.dev.yaml`을 이용한다.   
   dev 환경으로 기동하면, localhost:5005를 이용하여 Backend에 대한 remote debugger 접속이 가능하다. 
   ```shell
   sudo docker-compose -f docker-compose.yaml -f docker-compose.dev.yaml up -d
   ```
   
   frontend/backend에 대한 로그는 `docker-compose log` 명령을 이용하여 확인할 수 있다.
   ```shell
   sudo docker-compose logs -f frontend
   sudo docker-compose logs -f backend
   ```
   
   prod 환경으로 기동하려면 `docker-compose.yaml`, `docker-compose.prod.yaml`을 이용한다.   
   prod 환경 기동 시, application에 대한 Docker 빌드를 수행하지 않는다.
   ```shell
   sudo docker-compose -f docker-compose.yaml -f docker-compose.prod.yaml up -d
   ```
   
## etc.
* Docker Image 빌드에서 사용되는 TAG는 `.env`에 기록되어 있다.
* Database에 대한 설정은 `./mysql/.env`에 기록되어 있다.
* Database에 대한 접근 설정은 `.env`에 기록되어 있다.