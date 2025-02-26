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

## Using the Logging System

The package includes a logging system using `RichHandler` for formatted console output.

- To create a logger instance:

  ```python
  from packagename import get_logger
  logger = get_logger("my_logger")
  ```

- Set the log level dynamically:

  ```python
  logger.setLevel("DEBUG")
  ```

- Use it for logging messages:

  ```python
  logger.info("This is an info message")
  logger.warning("This is a warning message")
  ```

## Managing Dependencies with Poetry

This project uses [Poetry](https://python-poetry.org/) for dependency management and packaging.

- Install dependencies:

  ```sh
  poetry install
  ```

- Add a new package:

  ```sh
  poetry add package-name
  ```

- Add a development dependency:

  ```sh
  poetry add --group dev package-name
  ```

  Development dependencies are only needed for development and are not included when installing the package in production.

For more details, check the [Poetry documentation](https://python-poetry.org/docs/).
