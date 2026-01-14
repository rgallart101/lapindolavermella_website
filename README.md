# lapindolavermella_website

## Create and activate a virtual environment

```shell
$ python -m venv .venv
$ source ./.venv/bin/activate
```

## Install requirements

Ensure your virtual environment is activated.

```shell
(venv)$ pip install -r requirements.txt  
```

## Install the Flex theme

Clone `pelican-themes` from Github:

```shell
$ git clone https://github.com/getpelican/pelican-themes
```

Ensure your virtual environment is activated.

```shell
(venv)$ pip install -r requirements.txt  
```

Install the `Flex` theme:

```shell
(venv)$ pelican-themes --install %path-to-pelican-themes%/Flex
```

## Generate the website and upload it:

To upload remotely create a `.env` file with the following environment variables:

```bash
ENV_SSH_HOST=<remote host>
ENV_SSH_PORT=<remote port>
ENV_SSH_USER=<remote user>
ENV_SSH_TARGET_DIR=<remote dir>
```

and then run `./upload_remote`
