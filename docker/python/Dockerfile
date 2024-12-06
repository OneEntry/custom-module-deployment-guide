ARG  TAG=latest

FROM python:${TAG} as base

WORKDIR /usr/src/app
COPY requirements.txt ./
RUN pip install \
    --user \
    --disable-pip-version-check \
    --no-cache-dir \
    --requirement requirements.txt


FROM python:${TAG}

WORKDIR /opt/cms-user-app
COPY --from=base /root/.local /root/.local
COPY . .
ENV PATH=/root/.local/bin:$PATH


ENTRYPOINT ${CHANGE_ME}
