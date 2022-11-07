# Apache2 PHP 7.1.19

## Apache och PHP 7.1.19 i lokal docker container så att du kan testa kompabilitet med FileMaker Server installerad php

## Installera Docker desktop på Mac
https://hub.docker.com/editions/community/docker-ce-desktop-mac

### Installera open-jdk
Det kan hända att du behöver open-jdk men det är inte säkert.
https://jdk.java.net/15/
Ladda ner tar.gz för macOS/x64
Packa upp och placera jdk-15.0.1.jdk i ´/Library/Java/JavaVirtualMashines´

### Skapa ett nätverk
docker network create -d bridge docker-net

## Starta webservern
docker-compose up  -d --build
Webservern nås nu på localhost:8071

### Lägga till ny site
1. Klona valfri fil i config/apache2/sites-available, redigera och döp den till tex sitenamn.localhost.conf
2. Skapa en "symbolic link" till filen i "config/apache2/sites-enabled" med terminalen genom att gå dit och skriva ´ln -s ../sites-available/kundnamn.localhost.conf'
3. Starta om Apachetjänsten i containern med kommandot ´docker exec docker-apache2-php74 apachectl restart´ eller starta om den i Docker Desktop

### Anpassa webservern
#### docker-compose.yml
container_name: docker-apache2-php74 // Här kan du ändra namnet på containern.
ports:
      - "8071:80" // Här kan du ändra vilken port på din maskin som ska användas. 8071 betyder att du kan ange localhost:8071 för att komma till servern.

#### docker/apache2-php74/Dockerfile
Här kan du (om du luskar ut hur) lägga till extensions och annat som ska laddas med php

#### Bygg om containern om du ändrat i "Dockerfile" eller docker-compose.yml
docker-compose up -d --force-recreate --build

#### php.ini
hittar du i config/php/php.ini
Om du ändrar den så kör följande gitkommando för att undanta den från uppdatering om du synkar mot git-servern
git update-index --assume-unchanged config/php/php.ini

#### Insprirerat av
https://www.pixelite.co.nz/article/using-docker-for-local-php-development-2/

