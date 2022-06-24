![Logo](http://i.imgur.com/hCtfFAw.png)

Merlyn is a chatbot that uses artificial intelligence to identify the intent of it's user.

Special thanks to ![@alfredfrancis](https://github.com/alfredfrancis) for creating the ![ai-chatbot-framework](https://github.com/alfredfrancis/ai-chatbot-framework) used in this project.

### Installation

### Using docker-compose

```sh
docker-compose up -d
```

### Using Helm

```sh
helm dep update helm/ai-chatbot-framework

helm upgrade --install --create-namespace -n ai-chatbot-framework ai-chatbot-framework helm/ai-chatbot-framework

# port forward for local installation
kubectl port-forward --namespace=ai-chatbot-framework service/ingress-nginx-controller 8080:80
```

### Using Docker

```sh

# pull docker images
docker pull alfredfrancis/ai-chatbot-framework_backend:latest
docker pull alfredfrancis/ai-chatbot-framework_frontend:latest

# start a mongodb server
docker run --name mongodb -d mongo:3.6

# start iky backend
docker run -d --name=iky_backend --link mongodb:mongodb -e="APPLICATION_ENV=Production" alfredfrancis/ai-chatbot-framework_backend:latest

# setup default intents
docker exec -it iky_backend python manage.py migrate

# start iky gateway with frontend
docker run -d --name=iky_gateway --link iky_backend:iky_backend -p 8080:80 alfredfrancis/ai-chatbot-framework_frontend:latest

```

### without docker

- Setup Virtualenv and install python requirements

```sh
virtualenv -p python3 venv
source venv/bin/activate
pip install -r requirements.txt
python manage.py migrate
python run.py
```

- Production

```sh
APPLICATION_ENV="Production" gunicorn -k gevent --bind 0.0.0.0:8080 run:app
```

- Open http://localhost:8080/

#### Update Frontend Dist

- Run Development mode

```sh
cd frontend
npm install
ng serve
```

- Take Production build

```sh
cd frontend
ng build --prod --optimize
```

### Heroku

[![Deploy](https://www.herokucdn.com/deploy/button.png)](https://heroku.com/deploy)

- add your dev/production configurations in config.py

### DB

#### Restore

You can import some default intents using following steps

- goto http://localhost:8080/agent/default/settings
- click 'choose file'
- choose 'examples/default_intents.json file'
- click import

### Dependencies documentations

- [SKLearn documentation](http://scikit-learn.org/)
- [CRFsuite documentation](http://www.chokkan.org/software/crfsuite/)
- [python CRfSuite](https://python-crfsuite.readthedocs.io/en/latest/)

**Free Software, Hell Yeah!**
