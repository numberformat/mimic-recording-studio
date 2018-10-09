# Mimic Recording Studio

![demo](demo.gif)

Mycroft's open source Mimic technologies are Text-to-Speech engines, which take a piece of written text and convert it into spoken audio. The latest generation of this technology, [Mimic 2](https://github.com/MycroftAI/mimic2), uses machine learning techniques to create a model, which can speak a specific language, sounding like the voice on which it was trained.

The Mimic Recording Studio simplifies the collection of training data from individuals, each of which can be used to produce a distinct voice for Mimic.

## Quick Start

### Dependencies

* [Docker](https://docs.docker.com/) (community edition is fine)
* [Docker Compose](https://docs.docker.com/compose/install/)

Why docker? To make this super easy to set up and run cross platforms.

### Build & Run

* `git clone https://github.com/MycroftAI/mimic-recording-studio.git`
* `cd mimic-recording-studio`
* `docker-compose up` to build and run
  * Alternatively, you can build and run separately. `docker-compose build`, `docker-compose up`
* In browser, go to `http://localhost:3000`

**Note:**
First `docker-compose up` will take a while as this command will also build the docker containers. Subsequent `docker-compose up` should be quicker to boot.

## Manual Build and Start

Windows machine may need to default to this process. There is a known issue with windows and docker toolbox when trying to run the frontend container.

### Frontend

#### Dependencies

* [node & npm](https://nodejs.org/en/)
* [create-react-app](https://github.com/facebook/create-react-app)
* [yarn](https://yarnpkg.com/en/) - optional for faster build, install, and start

#### Build & Run

* `cd frontend/`
* `npm install`, alternatively `yarn install`
* `npm start`, alternatively `yarn start`

### Backend

#### Dependencies

* python 3.5 +
* [ffmpeg](https://www.ffmpeg.org/)

#### Build & Run

* `cd backend/`
* `pip install -r requirements.txt`
* `python run.py`

## Data

### Audios

#### wav files

Audios can be found in the `backend/audio_file/{uuid}/` directory. The backend automatically trims the beginning and ending silence for all wav files using [ffmpeg](https://www.ffmpeg.org/).

#### {uuid}-metadata.txt

Can also be found in `backend/audio_file/{uuid}/`. This file maps the wav file name to the phrase spoken. This along with the wav files are what you needed to get started on training [Mimic 2](https://github.com/MycroftAI/mimic2).

### Corpus

For now, we have an english corpus, `english_corpus.csv` made available which can be found in `backend/prompt/`. To use your own corpus follow these steps.

1. create a csv file in the same format as `english_corpus.csv` using tabs (`\t`) as the delimiter.
2. add your corpus to the `backend/prompt` directory.
3. change the `CORPUS` environment variable in `docker-compose.yml` to your corpus name.

**IMPORTANT:**
For now, this requires you to reset the sqlite database to use the new corpus. If you've recorded on another corpus and would like to save that data, you can simply rename your sqlite db found in `backend/db/` to another name. The backend will detect that `mimicstudio.db` is not there and create a new one for you. You may continue recording data for your new corpus.

## Technologies

### Frontend

The web UI is built using javascript and [React](https://reactjs.org/) and [create-react-app](https://github.com/facebook/create-react-app) as a scaffolding tool. Refer to [CRA.md](/frontend/CRA.md) to find out more on how to use create-react-app.

#### Functions

* Record and play audio
* Generate audio visualization
* Calculate and display metrics

### Backend

The web service is built using python, [Flask](http://flask.pocoo.org/) as the backend framework, [gunicorn](https://gunicorn.org/) as a http webserver, and [sqlite](https://www.sqlite.org/index.html) as the database.

#### Functions

* Process audio
* Serves corpus and metrics data
* Record info in database
* Record data to the file system

### Docker

Docker is used to containerized both applications. By default, frontend uses network port `3000` while the backend uses networking port `5000`. You can configure these in the `docker-compose.yml` file.

## Contributions

PR's are accepted!
