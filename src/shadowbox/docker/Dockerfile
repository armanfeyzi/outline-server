# Copyright 2018 The Outline Authors
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

# Replace the build arg with a direct FROM statement
FROM node:16.18.0-alpine

ARG VERSION=latest
# Save metadata on the software versions we are using.
LABEL shadowbox.node_version=16.18.0
LABEL shadowbox.github.release=${VERSION}

# The user management service doesn't quit with SIGTERM.
STOPSIGNAL SIGKILL

# We use curl to detect the server's public IP. We need to use the --date option in `date` to
# safely grab the ip-to-country database.
RUN apk add --no-cache --upgrade coreutils curl

RUN mkdir -p /etc/periodic/weekly/
RUN echo '#!/bin/sh\necho "MMDB update skipped during build"' > /etc/periodic/weekly/update_mmdb.sh
RUN chmod +x /etc/periodic/weekly/update_mmdb.sh

COPY . /

RUN /etc/periodic/weekly/update_mmdb.sh

# Install shadowbox.
WORKDIR /opt/outline-server

CMD ["/cmd.sh"]
