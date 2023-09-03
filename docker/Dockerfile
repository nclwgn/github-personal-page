FROM python:3.8

RUN apt-get -y update
RUN apt-get install -y jq

# install google chrome
RUN wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add -
RUN sh -c 'echo "deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google-chrome.list'
RUN apt-get -y update
RUN apt-get install -y google-chrome-stable

# install chromedriver
RUN apt-get install -yqq unzip
RUN CHROMEDRIVER_DOWNLOAD=$(curl -sS https://googlechromelabs.github.io/chrome-for-testing/last-known-good-versions-with-downloads.json | jq -r '.channels | .Stable | .downloads | .chromedriver | .[0] | .url'); \
  wget -O /tmp/chromedriver.zip $CHROMEDRIVER_DOWNLOAD
RUN unzip /tmp/chromedriver.zip chromedriver-linux64/chromedriver -d /usr/local/bin/
ENV PATH="${PATH}:/usr/local/bin/chromedriver"

RUN mkdir -p /app/loconotion/
WORKDIR /app/loconotion/
COPY requirements.txt .
RUN pip install -r requirements.txt

COPY . .
ENTRYPOINT [ "python", "loconotion" , "--chromedriver", "/usr/local/bin/chromedriver-linux64/chromedriver"]
