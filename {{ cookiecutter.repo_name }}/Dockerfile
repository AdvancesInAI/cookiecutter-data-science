# syntax=docker/dockerfile:1.0.0-experimental
## For this to work you must run `export DOCKER_BUILDKIT=1`
## then build using the command
##  docker build --ssh github_ssh_key=/Users/<your_username>/.ssh/id_rsa .

FROM python:3.7-slim-buster

COPY requirements.txt /tmp/
RUN python -m pip install --upgrade pip  &&\
    pip install --no-cache-dir -r /tmp/requirements.txt
#     mkdir config  &&\
#     mkdir secrets &&\
#     cp ../aiacore/secrets/.env secrets

#RUN addgroup --gid 999 aia
#RUN useradd --create-home --no-user-group --gid aia nns
#WORKDIR /home/nns
#USER nns

COPY {{ cookiecutter.repo_name }}/ /{{ cookiecutter.repo_name }}/

ENV IN_DOCKER_CONTAINER 1
EXPOSE 80
ENV PYTHONPATH "${PYTHONPATH}:/"
WORKDIR "/{{ cookiecutter.repo_name }}"
RUN cd /{{ cookiecutter.repo_name }} && apt-get purge -y ssh git

# RUN adduser -D nns_user
RUN useradd nnsuser
USER nnsuser

#ENTRYPOINT ["python", "-m", "sunbird.main"]
# CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000"]
CMD ["gunicorn", "-w", "4", "-k", "uvicorn.workers.UvicornWorker", "app.main:app"]