# Setting Up Course Project

1. Clone course repo: `git clone git@github.com:rekibnikufesin/nodejs-api-swagger.git`
2. Download and install Docker: `https://hub.docker.com/editions/community/docker-ce-desktop-mac/`
3. Login or create a new Docker account. 
4. **Increase Memory** via *Slider* and *Apply & Restart* on Docker. [Reference to issue](https://github.com/rekibnikufesin/nodejs-api-swagger/issues/2#issuecomment-291282276).
5. Install the correct version of elasticsearch: `npm install -g elasticsearch@12.1.3`
6. Install the correct image of elasticsearch: `docker pull elasticsearch:7.6.2`
7. Install the correct image of Kibana: `docker pull kibana:7.6.2`
8. Replace the code in `docker-compose.yml` with this:

```
version: '2.1'
services:
  elasticsearch:
    image: elasticsearch:7.6.2
    volumes:
      - ./es_data:/usr/share/elasticsearch/data
    ports:
      - 9200:9200
      - 9300:9300
    networks:
      - es

  kibana:
    image: kibana:7.6.2
    ports:
      - 5601:5601
    environment:
      ELASTICSEARCH_URL: 'http://elasticsearch:9200'
    networks:
      - es
    depends_on:
      - elasticsearch

networks:
  es:
    driver: bridge
```
9. Run Docker compose in the root directory: 

```
docker run -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" docker.elastic.co/elasticsearch/elasticsearch:7.6.2
```

The containers will run in the foreground. See data imported:

```
http://localhost:9200/todo/_search?
```

10. In a seperate terminal, change directory into the `todo-api`.
12. Ensure you have swagger installed globally: `npm install swagger -g`
13. Run `npm install` to install the necessary dependencies: `npm install` **First make sure you have node version 10.^**
14. Run the node command to import the sample data created by Docker images: `node utils/import.js`
14. Start Swagger: `swagger project start`
15. In a different terminal start Swagger editor: `swagger project edit`
16. Open http://127.0.0.1:54982 to **see Swagger editor**
17. Open http://localhost:9200/todo/_search? to **see data imported**
18. Open http://localhost:10010/todo/2 to see **individual todos**
19. Done! ✨ 

**Note:** You should have Docker, the `swagger project start` command, and `swagger project edit` command running in seperate windows, all at the same time. 
