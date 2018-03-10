## Start Scan
python runserver.py -cf config/config.ini -scf config/shared-config.ini -ns -v

## Start Map
python runserver.py -cf config/config.ini -scf config/shared-config.ini -os

## Start LevelUP
python tools/levelup.py -w 30 -ams 1000 -cf config/config.ini -scf config/shared-config.ini

# docker:
docker build -t rocketmap .

Start Scan

## FULL Log
docker run --name scanner rocketmap -cf config/config.ini -scf config/shared-config.ini -ns -v

## SCAN GYMS
docker run --name scanner rocketmap -cf config/config.ini -scf config/shared-config.ini -ns -gi

-enc --no-api-store
## SCAN GYMS WITH STATUS ACCOUNTS
docker run --name scanner rocketmap -cf config/config.ini -scf config/shared-config.ini -ns -gi -ps -enc -speed -st 75 -w 832

##Start Map Web
docker run --name map -p 5000:5000 rocketmap -cf config/config.ini -scf config/shared-config.ini -os -gi -enc

docker run --name map -p 5000:5000 rocketmap -cf config/config.ini -scf config/shared-config.ini -os -gi --heroku -enc

## LevelUP
docker run --name levelup rocketmap-up -w 30 -ams 1000 -cf config/config.ini -scf config/shared-config.ini


# To Deploy on Heroku
Open runserver.py and change 'args.port' to 'int(os.environ.get("PORT", -1))'
Copy content from heroku/Dockerfile to root Dockerfile
heroku container:push web

VOILÁ!

