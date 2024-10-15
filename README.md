# stroomvalidator

# Step-by-Step Guide with Code Examples

Install Dependencies: Make sure you have Docker and Docker Compose installed.

```
sudo apt-get update
sudo apt-get install docker.io docker-compose
```
Generate a Schnorr Key Pair: Use the following command to generate a key pair.

```
mkdir ~/stroom-validator
cd ~/stroom-validator
schnorr-keygen --output private.key
```
Create a Postgres Database: Use the following SQL commands to set up your database.

```
CREATE DATABASE stroom_db;
CREATE USER stroom_user WITH PASSWORD 'your_password';
GRANT ALL PRIVILEGES ON DATABASE stroom_db TO stroom_user;
```
Write Your Docker Compose File:

```
version: '3.8'
services:
  validator:
    image: stroom-validator:latest
    environment:
      - DATABASE_URL=postgres://stroom_user:your_password@postgres:5432/stroom_db
      - VALIDATOR_KEY=your_validator_key
    ports:
      - "8080:8080"
    depends_on:
      - postgres
  postgres:
    image: postgres:latest
    environment:
      - POSTGRES_USER=stroom_user
      - POSTGRES_PASSWORD=your_password
      - POSTGRES_DB=stroom_db
    volumes:
      - pg_data:/var/lib/postgresql/data

volumes:
  pg_data:
```
Start the Validator: Use Docker Compose to start your validator and database services.

```
docker-compose up -d
```
Connect to Bitcoin and Ethereum Networks: Update your validator configuration to connect to the necessary networks.
