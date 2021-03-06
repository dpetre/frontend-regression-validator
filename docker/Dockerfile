FROM pytorch/pytorch

# Installing build dependencies
RUN apt-get update && apt-get install -y git curl unzip

RUN apt-get install -y xorg

RUN curl -sS -o - https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add && \
    echo "deb [arch=amd64]  http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google-chrome.list && \
    apt-get -y update && \
    apt-get -y install google-chrome-stable

# Prepare environment UTF-8
RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get install -y locales
RUN sed -i -e 's/# en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/' /etc/locale.gen && \
    locale-gen
ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en
ENV LC_ALL en_US.UTF-8

# Prepare fred
RUN mkdir /work && \
    cd /work && \
    git clone https://github.com/adobe/frontend-regression-validator.git

# Preparing Python build environment
RUN /opt/conda/bin/activate base && python3 -m pip install -r /work/frontend-regression-validator/requirements.txt

# Extract the model files
RUN cd /work/frontend-regression-validator/fred/inference && cat model_files.bz2.parta* > model_files.bz2 && tar xjf model_files.bz2

# Start notebook
CMD cd /work/frontend-regression-validator/fred && git pull && python3 run.py
