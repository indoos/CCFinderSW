# USAGE

This document provides instructions on how to set up and run this tool for detecting duplicate code.

## Quick setup and run

This section provides the basic steps to get the tool running quickly.

1.  **Clone the repository:**
    ```bash
    git clone https://github.com/your-repo/duplicate-code-detector.git
    cd duplicate-code-detector
    ```

2.  **Install dependencies:**
    ```bash
    pip install -r requirements.txt
    ```

3.  **Run the tool:**
    ```bash
    python main.py --directory /path/to/your/codebase
    ```

## Detailed Setup and Run

This section provides more detailed instructions for setup and configuration.

### Prerequisites

*   Python 3.8 or higher
*   pip for package management

### Installation

1.  **Clone the repository:**
    ```bash
    git clone https://github.com/your-repo/duplicate-code-detector.git
    cd duplicate-code-detector
    ```

2.  **Create a virtual environment (recommended):**
    ```bash
    python -m venv venv
    source venv/bin/activate  # On Windows, use `venv\Scripts\activate`
    ```

3.  **Install dependencies:**
    ```bash
    pip install -r requirements.txt
    ```

### Running the tool

To run the duplicate code detector, use the `main.py` script. The primary argument is `--directory`, which specifies the path to the code repository you want to analyze.

```bash
python main.py --directory /path/to/your/codebase
```

#### Command-line options

*   `--directory <path>`: (Required) The path to the directory to scan for duplicate code.
*   `--output <file>`: (Optional) The file to write the results to. Defaults to `duplicates.json`.
*   `--min-tokens <int>`: (Optional) The minimum number of tokens for a code block to be considered for duplication analysis. Defaults to 50.
*   `--threshold <float>`: (Optional) The similarity threshold for considering two code blocks as duplicates. Defaults to 0.9.
*   `--exclude <patterns>`: (Optional) A comma-separated list of glob patterns to exclude files or directories.
*   `--verbose`: (Optional) Enable verbose logging.

**Example with more options:**

```bash
python main.py \
    --directory /path/to/your/codebase \
    --output results.json \
    --min-tokens 100 \
    --exclude "*/test/*,*.md" \
    --verbose
```

## Supported Languages

This tool supports a wide variety of programming and configuration languages for duplicate code detection. The following is a non-exhaustive list of supported file types:

*   **Programming Languages:**
    *   Java (`.java`)
    *   Python (`.py`)
    *   JavaScript (`.js`)
    *   TypeScript (`.ts`)
    *   C++ (`.cpp`, `.h`)
    *   C# (`.cs`)
    *   Ruby (`.rb`)
    *   Go (`.go`)
    *   Rust (`.rs`)
    *   PHP (`.php`)
    *   Swift (`.swift`)
    *   Kotlin (`.kt`)
    *   Shell Scripts (`.sh`, `.ksh`, `.bash`)

*   **Configuration and Data-Interchange Formats:**
    *   JSON (`.json`)
    *   YAML (`.yml`, `.yaml`)
    *   XML (`.xml`, `pom.xml`)
    *   INI (`.ini`)

*   **Dependency Management:**
    *   `requirements.txt`
    *   `Pipfile`
    *   `package.json`
    *   `pom.xml`

## Example Usage on a Complex Repository

Consider a repository with the following structure, containing a mix of Java, Python, and shell scripts, along with configuration files:

```
my-complex-project/
├── java/
│   ├── src/main/java/com/example/
│   │   ├── App.java
│   │   └── utils/
│   │       └── StringUtils.java
│   └── pom.xml
├── python/
│   ├── scripts/
│   │   ├── data_processing.py
│   │   └── analysis.py
│   └── requirements.txt
├── scripts/
│   ├── deploy.sh
│   └── backup.ksh
├── config/
│   ├── settings.json
│   └── config.yaml
└── README.md
```

To find duplicates across this entire repository, you can run the tool from the root of `my-complex-project`:

```bash
python main.py --directory .
```

This command will scan all files in the current directory and its subdirectories.

If you want to exclude certain files or directories, such as the `config` directory and all `README.md` files, you can use the `--exclude` option:

```bash
python main.py \
    --directory . \
    --exclude "config/*,*.md"
```

The tool will then analyze the Java, Python, and shell script files for duplicates, while ignoring the specified configuration files and markdown files. The results will be saved to `duplicates.json` by default, which you can then inspect to find and refactor duplicated code.
