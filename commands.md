## Start Scan
python runserver.py -cf config/config.ini -scf config/shared-config.ini -ns -v

## Start Map
python runserver.py -cf config/config.ini -scf config/shared-config.ini -os

## Start LevelUP
python tools/levelup.py -w 30 -ams 1000 -cf config/config.ini -scf config/shared-config.ini

# docker:
docker build -t rocketmap .

docker build -t rocketmap-lvlup . -f DockerfileLvlUp 

Start Scan

## FULL Log
docker run --name scanner rocketmap -cf config/config.ini -scf config/shared-config.ini -ns -v

## SCAN GYMS
docker run --name scanner rocketmap -cf config/config.ini -scf config/shared-config.ini -ns -gi -speed -st 15 -w 32 -bh

## SCAN GYMS WITH STATUS ACCOUNTS
docker run --name scanner rocketmap -cf config/config.ini -scf config/shared-config.ini -ns -gi -ps -enc -speed -st 75 -w 832 --no-api-store

##Start Map Web
docker run --name map -p 5000:5000 rocketmap -cf config/config.ini -scf config/shared-config.ini -os -gi -enc

docker run --name map -p 5000:5000 rocketmap -cf config/config.ini -scf config/shared-config.ini -os -gi --heroku -enc


## LevelUP
docker run --name levelup rocketmap-up -w 30 -ams 1000 -cf config/config.ini -scf config/shared-config.ini


# To Deploy on Heroku
Copy content from heroku/Dockerfile to root Dockerfile
heroku container:push web

VOILÁ!

## Deploy Velha Central e Passo Manso
docker run --name scanner-velha-passomanso rocketmap -cf config/configVelhaPassoManso.ini -scf config/shared-config.ini -ns -gi -speed -st 15 -w 32 -enc

## Deploy Centro
docker run --name scanner-centro rocketmap -cf config/configCentro.ini -scf config/shared-config.ini -ns -gi -speed -st 15 -w 32 -enc -DC
docker run --name lvlup-centro rocketmap-lvlup -w 100 -ams 1000 -cf config/configCentro.ini -scf config/shared-config.ini

## Deploy Velha VictorKonder
docker run --name scanner-velha-victorkonder rocketmap -cf config/configVelhaVictorKonder.ini -scf config/shared-config.ini -ns -gi -speed -st 15 -w 32
