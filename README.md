# Pandora Blank

This is a "[blank](https://en.wikipedia.org/wiki/Planchet)" repository which can be used to create tools within the Pandora ecosystem. This has all of the defaults that we require for Pandora software including requirements management with poetry and documentation.

If you are starting a new Pandora tool from scratch, consider forking this repository and updating it to begin your tool.

## Cloning the Repository

To get started, fork this repository on GitHub, then clone your fork:

```sh
  git clone https://github.com/your-username/pandora-blank.git
  cd pandora-blank
```

## Updating the Repository with a New Package Name

1. Rename the `packagename` directory inside `src/` to your new package name.
2. Update `pyproject.toml`:
   - Change `name = "packagename"` to `name = "your_new_package"`.
3. Update `mkdocs.yml`:
   - Change `site_name: packagename` to `site_name: your_new_package`.
4. Update all imports in the codebase from `packagename` to `your_new_package`.

## Using the Config File System

The package includes a configuration system that loads and saves user preferences.

The configuration file is stored in a directory that users should be able to access invariant of what sort of machine they are using.

### Setting default configuration

You can update the default configuration for yorur package by updating the `reset_config` function with your defaults. For example, here is the function from one of our existing tools:

    ```python
    def reset_config():
        config = configparser.ConfigParser()
        config["SETTINGS"] = {
            "storage_dir": user_data_dir("pandorapsf"),
            "log_level": "INFO",
            "vis_psf_download_location": "https://zenodo.org/records/11228523/files/pandora_vis_2024-05.fits?download=1",
            "nir_psf_download_location": "https://zenodo.org/records/11153153/files/pandora_nir_2024-05.fits?download=1",
            "vis_psf_creation_date": "2024-05-14T11:38:14.755119",
            "nir_psf_creation_date": "2024-05-08T15:02:58.461202",
        }

        with open(CONFIGPATH, "w") as configfile:
            config.write(configfile)
    ```

This sets the default settings, but users can always update the settings themselves if they choose to.
Settings can be reset to defaults by the user at any time using:

  ```python
  from packagename import reset_config
  reset_config()
  ```

If you update default settings, users will have to reset their config to get these changes.

### Updating configuratino as a user

Your users can load the configuration with:

  ```python
  from packagename import load_config
  config = load_config()
  ```

If they change the configuration they can save it using

  ```python
  from packagename import save_config
  config["SETTINGS"]["log_level"] = "INFO"
  save_config(config)
  ```

Users can find where the configuration file is stored using

  ```python
  from packagename import CONFIGDIR
  print(CONFIGDIR)
  ```

## Using the Logging System

The package includes a logging system using `RichHandler` for formatted console output. If you clone this repo, you will automatically have a logger. You can use it like this:

  ```python
  from packagename import logger
  logger.info("This is an info message")
  logger.warning("This is a warning message")
  ```

The default logger level is set in the config file, but you can update this in your session using the following, where I set the logger level to `"INFO"`.

  ```python
  from packagename import logger
  logger.setLevel("INFO")
  ```

## Managing Dependencies with Poetry

This project uses [Poetry](https://python-poetry.org/) for dependency management and packaging.

After you clone the repository and update to your new package name you will need to install the package using

  ```sh
  poetry install
  ```

(If you do not update the package name you will install a python package called `packagename`.)

To add new dependencies to your package you can use

  ```sh
  poetry add dependency
  ```

- Add a development dependency:

  ```sh
  poetry add --group dev dependency
  ```

Development dependencies are only needed for development and are not included when installing the package in production. Add development dependencies for things like testing and building docs.

If you are using the package and need these development dependencies you should use

  ```sh
  poetry install --with dev
  ```

If you think your versions of packages are out of date you can run

  ```sh
  poetry update
  ```

For more details, check the [Poetry documentation](https://python-poetry.org/docs/).

## Updating the package version and publishing to pypi

The package version is automaticaly found from the `pyproject.toml` file. This means when you are ready to release a new version you only need to follow these steps:

1. Update the version number in the `pyproject.toml` file
2. Build the distribution using

    ```sh
    poetry build
    ```

3. Publish the package to pypi

    ```sh
    poetry publish
    ```

You should consider using the industry standard for updating version numbers which is major.minor.patch

- Major version changes indicate breaking changes that may not be backward-compatible.
- Minor version changes introduce new features in a backward-compatible manner.
- Patch version changes include bug fixes or small improvements that do not affect compatibility.

For example, if a package is at version 1.2.3, then:

- Increasing the major version to 2.0.0 means changes were introduced that break the API
- Increasing the minor version to 1.3.0 means new features were added while maintaining compatibility.
- Increasing the patch version to 1.2.4 means minor bug fixes or tweaks were made without changing functionality.

It is ok to release new versions often.
