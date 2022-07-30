FROM python:3.10.5-bullseye

RUN apt update && apt install -y \
    git \
    curl \
    vim \
    htop \
    jq \
    nkf \
    libgl1-mesa-dev

RUN pip install --upgrade pip
RUN pip install --upgrade setuptools
RUN pip install --upgrade ipykernel

WORKDIR /app
COPY . /app

RUN pip install -r requirements.txt

RUN useradd -m user

CMD [ "sleep", "infinity" ]
