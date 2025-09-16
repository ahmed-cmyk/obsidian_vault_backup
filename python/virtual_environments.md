# Python Virtual Environments

Virtual environments are isolated Python environments that allow you to manage dependencies for different projects separately. This prevents conflicts between packages required by different projects and ensures that each project has its own clean set of dependencies.

## Why Use Virtual Environments?

1.  **Dependency Isolation:** Different projects often require different versions of the same library. Without virtual environments, installing a new version for one project might break another.
2.  **Clean Project Dependencies:** Each project's `requirements.txt` file accurately reflects only the packages it needs, making it easier to share and deploy.
3.  **Avoid System-Wide Conflicts:** Prevents polluting your global Python installation with project-specific packages.
4.  **Reproducibility:** Ensures that your project can be run consistently across different machines or by other developers.

## Common Tools for Virtual Environments

Python offers several tools for creating and managing virtual environments:

-   `venv` (built-in, recommended for Python 3.3+)
-   `virtualenv` (third-party, more features, supports older Python versions)
-   `pipenv` (higher-level tool for dependency management and virtual environments)
-   `conda` (for data science, manages environments and packages beyond Python)

This note will focus on `venv`, as it's built into Python 3.3+ and is sufficient for most use cases.

## Using `venv`

`venv` is the standard way to create virtual environments in Python 3. It creates a lightweight virtual environment with its own site-packages directory, isolated from the system Python.

### 1. Creating a Virtual Environment

Navigate to your project directory in the terminal and run:

```bash
python3 -m venv myenv
# Or simply 'python -m venv myenv' if 'python' points to Python 3
```

-   `python3 -m venv`: Invokes the `venv` module.
-   `myenv`: The name of the directory where the virtual environment will be created. You can name it anything, but common names are `venv`, `.venv`, or `env`.

This command creates a directory named `myenv` (or whatever you chose) in your project folder. Inside, you'll find a `bin` (or `Scripts` on Windows) directory containing the Python executable and `pip` for that specific environment.

### 2. Activating the Virtual Environment

Before installing packages or running your project, you need to *activate* the virtual environment. Activation modifies your shell's `PATH` variable so that `python` and `pip` commands refer to the executables within your virtual environment, not the system-wide ones.

-   **On macOS/Linux:**
    ```bash
    source myenv/bin/activate
    ```

-   **On Windows (Command Prompt):**
    ```cmd
    myenv\Scripts\activate.bat
    ```

-   **On Windows (PowerShell):**
    ```powershell
    myenv\Scripts\Activate.ps1
    ```

After activation, your terminal prompt will usually change to indicate that you are in the virtual environment (e.g., `(myenv) your_username@your_machine:~/your_project$`)

### 3. Installing Packages

Once activated, use `pip` as usual. Packages will be installed into your virtual environment's `site-packages` directory.

```bash
(myenv) pip install requests beautifulsoup4
```

### 4. Deactivating the Virtual Environment

When you're done working on the project, you can deactivate the environment:

```bash
deactivate
```

This will revert your shell's `PATH` to its original state.

### 5. Freezing Dependencies (`requirements.txt`)

It's good practice to keep a record of your project's dependencies. You can generate a `requirements.txt` file that lists all installed packages and their versions:

```bash
(myenv) pip freeze > requirements.txt
```

To install dependencies from a `requirements.txt` file in a new environment (e.g., on another machine or after cloning a project):

```bash
# 1. Create and activate the new virtual environment
python3 -m venv new_env
source new_env/bin/activate

# 2. Install dependencies
(new_env) pip install -r requirements.txt
```

### 6. Deleting a Virtual Environment

To remove a virtual environment, simply delete its directory:

```bash
rm -rf myenv # On macOS/Linux
rd /s /q myenv # On Windows Command Prompt
Remove-Item -Recurse -Force myenv # On Windows PowerShell
```

## Best Practices

-   **Name Convention:** Use a consistent name like `venv` or `.venv` for your virtual environment directory. It's common to add this name to your project's `.gitignore` file so it's not committed to version control.
-   **Project Root:** Create the virtual environment in the root directory of your project.
-   **Activate Always:** Always activate your virtual environment before installing packages or running project code.
-   **`requirements.txt`:** Regularly update your `requirements.txt` file using `pip freeze > requirements.txt` to reflect changes in your project's dependencies.

## `virtualenv` (Alternative)

While `venv` is built-in, `virtualenv` is a third-party alternative that offers some additional features, such as supporting older Python versions and more customization options. If you need these features, you can install it via `pip`:

```bash
pip install virtualenv
```

Then, use it similarly:

```bash
virtualenv myenv
source myenv/bin/activate
```

## `pipenv` (Higher-level Tool)

`pipenv` aims to combine package management (`pip`) and virtual environment management into a single tool. It automatically creates and manages virtual environments and uses `Pipfile` and `Pipfile.lock` for dependency management, which are more robust than `requirements.txt`.

```bash
pip install pipenv

# In your project directory:
pipenv install # Creates a virtual env and Pipfile
pipenv install requests # Installs requests and adds to Pipfile
pipenv shell # Activates the virtual env
pipenv run python your_script.py # Runs a command within the env
```

## `conda` (for Data Science)

If you work extensively with data science, machine learning, or scientific computing, `conda` (from Anaconda or Miniconda) is a popular choice. It's a package and environment manager that works for Python, R, and other languages, and can manage non-Python dependencies.

```bash
conda create --name myenv python=3.9 numpy pandas
conda activate myenv
conda install matplotlib
conda deactivate
```
