# Install

Ref : https://blog.leapgio.cloud/api-gateway-kong/

![title](/images/kong/image-12.png)

API Gateway คือ Reverse Proxy อยู่ระหว่าง User Clients กับ Services ทำหน้าที่นำทาง (Routing) และกระจาย (Load Balancing) Request ที่เข้ามาสู่ Services ย่อยๆของเราซึ่งอาจจะเป็น Web App, Mobile App หรือ IoT อีกทั้งยังรวมส่วนงานบางส่วนของ Services ต่างๆของเราเข้ามารวมอยู่ในตัวของมันเอง อาทิเช่น SSL termination, Authentication, Transformations, IP whitelisting, Rate limiting, Logging and monitoring และอื่นๆอีกมากมาย ซึ่งทำให้ผู้ดูแลสามารถบริหารจัดการได้จากส่วนกลางอย่างมีประสิทธิภาพยิ่งขึ้น Services จะมี Performance ที่ดีขึ้น การพัฒนาระบบใหม่ๆได้เร็วและสะดวกสบายยิ่งขึ้นจากการตัดงานที่เหมือนกันบางส่วนออกไป

![title](/images/kong/image-10.png)

## docker network create kong-net

## docker compose up -d
```yaml
version: "3.7"
volumes:
  kong_data: {}

networks:
  kong-net:
services:
  #######################################
  # Postgres: The database used by Kong
  #######################################
  kong-database:
    image: postgres:9.6
    container_name: kong-postgres
    restart: on-failure
    networks:
      - kong-net
    volumes:
      - kong_data:/var/lib/postgresql/data
    environment:
      POSTGRES_USER: kong
      POSTGRES_PASSWORD: ${KONG_PG_PASSWORD:-kong}
      POSTGRES_DB: kong
    ports:
      - "5432:5432"
    healthcheck:
      test: ["CMD", "pg_isready", "-U", "kong"]
      interval: 30s
      timeout: 30s
      retries: 3
  #######################################
  # Kong database migration
  #######################################
  kong-migration:
    image: ${KONG_DOCKER_TAG:-kong:latest}
    command: kong migrations bootstrap
    networks:
      - kong-net
    restart: on-failure
    environment:
      KONG_DATABASE: postgres
      KONG_PG_HOST: kong-database
      KONG_PG_DATABASE: kong
      KONG_PG_USER: kong
      KONG_PG_PASSWORD: ${KONG_PG_PASSWORD:-kong}
    depends_on:
      - kong-database
  #######################################
  # Kong: The API Gateway
  #######################################
  kong:
    image: ${KONG_DOCKER_TAG:-kong:latest}
    restart: on-failure
    networks:
      - kong-net
    environment:
      KONG_DATABASE: postgres
      KONG_PG_HOST: kong-database
      KONG_PG_DATABASE: kong
      KONG_PG_USER: kong
      KONG_PG_PASSWORD: ${KONG_PG_PASSWORD:-kong}
      KONG_PROXY_LISTEN: 0.0.0.0:8000
      KONG_PROXY_LISTEN_SSL: 0.0.0.0:8443
      KONG_ADMIN_LISTEN: 0.0.0.0:8001
    depends_on:
      - kong-database
    healthcheck:
      test: ["CMD", "kong", "health"]
      interval: 10s
      timeout: 10s
      retries: 10
    ports:
      - "8000:8000"
      - "8001:8001"
      - "8443:8443"
      - "8444:8444"
  #######################################
  # Konga database prepare
  #######################################
  konga-prepare:
    image: pantsel/konga:latest
    command: "-c prepare -a postgres -u postgresql://kong:${KONG_PG_PASSWORD:-kong}@kong-database:5432/konga"
    networks:
      - kong-net
    restart: on-failure
    depends_on:
      - kong-database
  #######################################
  # Konga: Kong GUI
  #######################################
  konga:
    image: pantsel/konga:latest
    restart: always
    networks:
      - kong-net
    environment:
      DB_ADAPTER: postgres
      DB_URI: postgresql://kong:${KONG_PG_PASSWORD:-kong}@kong-database:5432/konga
      NODE_ENV: production
    depends_on:
      - kong-database
    ports:
      - "1337:1337"
```
[download docker-compose.yml](/Kong/docker-compose.yml)


หลังจากติดตั้งเสร็จเราก็จะได้ Running Container
![title](/images/kong/image-1536.png)

จากนั้นเราเปิดหน้า Konga Management ขึ้นมา http://<YOUR PUBLIC IP>:1337 ทำการ Register Admin

![title](/images/kong/image-13.png)

สร้าง Connection เพื่อไปต่อกับ Kong โดยใช้ port 8001

![title](/images/kong/image-2.png)