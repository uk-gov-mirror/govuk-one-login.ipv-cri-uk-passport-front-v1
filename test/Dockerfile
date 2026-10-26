FROM node:22.23.4-bookworm-slim@sha256:de170b39439eb0e5a3444ad7d60b015614a381e10fecae2e30ec9e735122f38d

# Install system dependencies for Playwright and AWS CLI
RUN apt-get update && apt-get install -y \
    curl \
    wget \
    unzip \
    libglib2.0-0 \
    libnss3 \
    libglu1 \
    libxcb1 \
    fonts-liberation \
    dbus-x11 \
    libxss1 \
    lsb-release \
    xdg-utils \
    gnupg \
    jq \
    && rm -rf /var/lib/apt/lists/*

COPY test/public-keys/aws-cli.gpg aws-cli.gpg

RUN curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip" \
    && curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip.sig" -o "awscliv2.zip.sig" \
    && gpg --import aws-cli.gpg \
    && gpg --verify awscliv2.zip.sig awscliv2.zip \
    && unzip awscliv2.zip \
    && ./aws/install \
    && rm -rf awscliv2.zip awscliv2.zip.sig aws-cli.gpg aws/

# Set working directory
WORKDIR /home/node

COPY . .

RUN npm ci && \
    npx playwright install --with-deps chromium

COPY test/run-tests.sh /run-tests.sh
RUN chmod +x /run-tests.sh

ENTRYPOINT ["/run-tests.sh"]
