version: "3.8"
services:
  db_container:
    image: haneeshdevops/db_persisted:latest
    container_name: db_container
    restart: always
    ports:
      - "5432:5432"
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: Quantum9963
      POSTGRES_DB: QUANTUM
    volumes:
      - postgres-data:/var/lib/postgresql/data

  myapplication:
    image: haneeshdevops/ecom_persisted:latest
    container_name: myapplication
    restart: always
    ports:
      - "8084:8084"
    environment:
      SPRING_DATASOURCE_URL: jdbc:postgresql://db_container:5432/QUANTUM
      SPRING_DATASOURCE_USERNAME: postgres
      SPRING_DATASOURCE_PASSWORD: Quantum9963
    depends_on:
      - db_container

volumes:
  postgres-data:
