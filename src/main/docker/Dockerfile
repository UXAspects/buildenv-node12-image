#
# Copyright 2020-2023 Open Text.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#      http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#

#
# Base on the standard Maven 3.5.3 image
#
FROM maven:3.5.3-jdk-8

#
# Install Node.js (which includes npm)
#
RUN curl -sL https://deb.nodesource.com/setup_12.x | bash - && \
    apt-get install -y nodejs && \
    echo "Node.js version: $(node -v)" && \
    npm install npm@latest -g && \
    echo "NPM version: $(npm -v)"

RUN curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | \
    apt-key add - && \
    echo "deb https://dl.yarnpkg.com/debian/ stable main" | \
    tee /etc/apt/sources.list.d/yarn.list && \
    apt update && apt install yarn

RUN curl -fsSL https://download.docker.com/linux/ubuntu/gpg | \
    apt-key add - && \
    echo "deb [arch=amd64] https://download.docker.com/linux/debian $(lsb_release -cs) stable" | \
    tee /etc/apt/sources.list.d/docker.list

RUN apt update

#
# Install Cypress Dependencies (https://docs.cypress.io/guides/guides/continuous-integration.html#Dependencies)
#
RUN apt install -y libgtk2.0-0 libgtk-3-0 libgbm-dev libnotify-dev libgconf-2-4 libnss3 libxss1 libasound2 libxtst6 xauth xvfb

#
# Install Docker for Loki visual regression testing and dependencies required for Nx and build tooling
#
RUN apt install -y docker-ce docker-ce-cli containerd.io build-essential

#
# Install Chrome (Required for automated tests)
#
# Note: Google has phased out fetching a specific version of Chrome, which is why we're fetching from a mirror:
# https://stackoverflow.com/questions/70752674/404-on-dl-google-com-for-stable-chrome
RUN wget http://mirror.cs.uchicago.edu/google-chrome/pool/main/g/google-chrome-stable/google-chrome-stable_83.0.4103.97-1_amd64.deb
RUN dpkg -i google-chrome-stable_83.0.4103.97-1_amd64.deb; apt-get -fy install

#
# Fetch chromedriver to save a download on every build
#
RUN curl https://chromedriver.storage.googleapis.com/83.0.4103.39/chromedriver_linux64.zip --create-dirs -o /tmp/chromedriver_linux64.zip
