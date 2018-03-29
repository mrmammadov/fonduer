# Fonduer

[![GitHub license](https://img.shields.io/github/license/HazyResearch/fonduer.svg)](https://github.com/HazyResearch/fonduer/blob/master/LICENSE)
[![GitHub stars](https://img.shields.io/github/stars/HazyResearch/fonduer.svg)](https://github.com/HazyResearch/fonduer/stargazers)
[![PyPI](https://img.shields.io/pypi/v/fonduer.svg)](https://pypi.org/project/fonduer/)
[![PyPI - Python Version](https://img.shields.io/pypi/pyversions/fonduer.svg)](https://pypi.org/project/fonduer/)
[![GitHub issues](https://img.shields.io/github/issues/HazyResearch/fonduer.svg)](https://github.com/HazyResearch/fonduer/issues)
[![Travis](https://img.shields.io/travis/HazyResearch/fonduer.svg)](https://travis-ci.org/HazyResearch/fonduer)
[![Coveralls github](https://img.shields.io/coveralls/github/HazyResearch/fonduer.svg)](https://coveralls.io/github/HazyResearch/fonduer)

`Fonduer` is a framework for building knowledge base construction (KBC)
applications from _richy formatted data_ and is implemented as a library on
top of a modified version of [Snorkel](https://hazyresearch.github.io/snorkel/).

_Note that Fonduer is still actively under development, so feedback and
contributions are welcome. Let us know in the
[Issues](https://github.com/HazyResearch/fonduer/issues) section or feel free
to submit your contributions as a pull request._

## Reference

[Fonduer: Knowledge Base Construction from Richly Formatted Data](https://arxiv.org/abs/1703.05028)

```
@article{wu2017fonduer,
  title={Fonduer: Knowledge Base Construction from Richly Formatted Data},
  author={Wu, Sen and Hsiao, Luke and Cheng, Xiao and Hancock, Braden and Rekatsinas, Theodoros and Levis, Philip and R{\'e}, Christopher},
  journal={arXiv preprint arXiv:1703.05028},
  year={2017}
}
```

## Installation

### Dependencies

We use a few applications that you'll need to install and be sure are on your
PATH.

For OS X using [homebrew](https://brew.sh):

```bash
brew install poppler
brew install postgresql
```

On Debian-based distros:

```bash
sudo apt-get install poppler-utils
sudo apt-get install postgresql
```

For the Python dependencies, we recommend using a
[virtualenv](https://virtualenv.pypa.io/en/stable/). Once you have cloned the
repository, change directories to the root of the repository and run

```bash
virtualenv -p python3 .venv
```

Once the virtual environment is created, activate it by running

```bash
source .venv/bin/activate
```

Any Python libraries installed will now be contained within this virtual
environment. To deactivate the environment, simply run `deactivate`.

Then, install `Fonduer` by running

```bash
pip install fonduer
```

## Learning how to use `Fonduer`

The [`Fonduer`
tutorials](https://github.com/hazyresearch/fonduer-tutorials) cover the
`Fonduer` workflow, showing how to extract relations from hardware datasheets
and scientific literature.

## FAQs

<details><summary>How do I connect to PostgreSQL? I'm getting "fe_sendauth no
password supplied".</summary><br>

There are [four main
ways](https://dba.stackexchange.com/questions/14740/how-to-use-psql-with-no-password-prompt)
to deal with entering passwords when you connect to your PostgreSQL database:

1. Set the `PGPASSWORD` environment variable
   ```
   PGPASSWORD=<pass> psql -h <host> -U <user>
   ```
2. Using a [.pgpass file to store the
   password](http://www.postgresql.org/docs/current/static/libpq-pgpass.html).
3. Setting the users to [trust
   authentication](https://www.postgresql.org/docs/current/static/auth-methods.html#AUTH-TRUST)
   in the pg_hba.conf file. This makes local development easy, but probably
   isn't suitable for multiuser environments. You can find your hba file
   location by running `psql`, then querying
   ```
   SHOW hba_file;
   ```
4. Put the username and password in the connection URI:
   ```
   postgres://user:pw@localhost:5432/...
   ```

</details>

<details><summary>I'm getting a CalledProcessError for command 'pdftotext -f 1
-l 1 -bbox-layout'?</summary><br>

Are you using Ubuntu 14.04 (or older)? Fonduer requires `poppler-utils` to be
[version `0.36.0` or greater](https://poppler.freedesktop.org/releases.html).
Otherwise, the `-bbox-layout` option is not available for `pdftotext`.

If you must use Ubuntu 14.04, you can [install
manually](https://poppler.freedesktop.org). As an example, to install `0.53.0`:

```bash
sudo apt-get install build-essential checkinstall
wget poppler.freedesktop.org/poppler-0.53.0.tar.xz
tar -xf ./poppler-0.53.0.tar.xz
cd poppler-0.53.0
./configure
make
sudo checkinstall
```

We highly recommend using at least Ubuntu 16.04 though, as we haven't done
testing on 14.04 or older.

</details>

## For Developers

We are following [Semantic Versioning 2.0.0](https://semver.org/) conventions.
The maintainers will create a git tag for each release and increment the
version number found in
[fonduer/\_version.py](https://github.com/HazyResearch/fonduer/blob/master/fonduer/_version.py)
accordingly. We deploy tags to PyPI automatically using Travis-CI.

To install locally, you'll need to install `pandoc`:

```
sudo apt-get install pandoc
```

which is used to create the reStructuredText file that the setuptools expects.

### Tests

To test changes in the package, you install it in [editable
mode](https://packaging.python.org/tutorials/distributing-packages/#working-in-development-mode)
locally in your virtualenv by running:

```
make dev
```

Then you can run our tests

```
make test
```
