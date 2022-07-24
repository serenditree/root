# Serenditree

## About
This is the parent project and starting point for Serenditree.

## Development

### Environment
In the following steps `$_ST_HOME` refers to the root folder containing all submodules. If you clone into a folder
 called `Serenditree` you will get the following structure:

```
Serenditree/
├── .git
├── .gitmodules
├── README.md
├── branch
├── leaf
└── stem
```

The source is structured as follows:
- **Stem**: Command-line interface for development and operations. Building images and running containers for
   the database, messaging and map services is done here.
- **Branch**: The Java EE backend.
- **Leaf**: The Angular frontend.

Serenditree includes a command-line interface (CLI) at `$_ST_HOME/stem/cli.sh`. For convenience, you should add an 
alias to your `~/.bashrc`. Otherwise, you have to replace `sc` with `$_ST_HOME/stem/cli.sh` whenever you see it!

```sh
# Alias creation
echo "alias sc='bash $_ST_HOME/stem/cli.sh'" >> ~/.bashrc
source ~/.bashrc
```

### Get the source
```sh
git clone \
    --recurse-submodules \
    --remote-submodules \
    git@github.com:serenditree/root.git \
    $_ST_HOME
sc git 'checkout dev'
sc git 'pull --ff-only'
```

### Install tools
For initial image building and running the local development stack you will need the following:
```sh
sudo dnf install -y podman buildah git jq
```
Until base images are not in a public repository, you also need the yarn repo for building the node builder image.
```sh
sudo curl -L https://dl.yarnpkg.com/rpm/yarn.repo -o /etc/yum.repos.d/yarn.repo
```
For cluster installation, images and interaction you will need:
```sh
sudo dnf install -y tkn skopeo jq pass
# install crc
sc cluster install
```
Your pass password store needs the following items:
```
serenditree
├── crc.testing
├── data.url
├── github.com
├── json.web.key
├── quay.io
├── registry.redhat.io
├── root.seed
├── root.seed.root
├── root.user
└── root.user.root
```

### Before building
- Make sure a local maven repository exists! (``mkdir -p ~/.m2/repository``)
- If you want to use Red Hat base images you need a developer subscription and an access token available at ``pass serenditree/registry.redhat.io``. Otherwise you have to set the environment variable ``_ST_FROM=centos``.

