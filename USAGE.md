# USAGE

This document provides instructions on how to set up and run this tool for detecting duplicate code.

## Quick setup and run

This section provides the basic steps to get the tool running quickly.

1.  **Clone the repository:**
    ```bash
    git clone https://github.com/your-repo/duplicate-code-detector.git
    cd duplicate-code-detector
    ```

2.  **Build the project using Gradle:**
    ```bash
    ./gradlew build
    ```

3.  **Run the tool:**
    The main command to run the tool is `CCFinderSW`. You need to specify the mode (`D` for detection), the directory, the language, and the output file.
    ```bash
    # For a Java project in the 'my-project' directory
    ./build/distributions/CCFinderSW-1.0/bin/CCFinderSW D -d path/to/my-project -l java -o my-project-duplicates
    ```
    This will create an output file named `my-project-duplicates.txt`.

## Detailed Setup and Run

This section provides more detailed instructions for setup and configuration.

### Prerequisites

*   Java 8 or higher
*   Gradle (the repository includes a Gradle wrapper, so you don't need to install it separately)

### Installation and Building

1.  **Clone the repository:**
    ```bash
    git clone https://github.com/your-repo/duplicate-code-detector.git
    cd duplicate-code-detector
    ```

2.  **Build the project:**
    The project uses the Gradle wrapper (`gradlew`) to build. This script will automatically download the correct Gradle version.
    ```bash
    ./gradlew build
    ```
    After a successful build, the runnable application will be located in the `build/distributions/` directory.

### Running the tool

To run the duplicate code detector, use the `CCFinderSW` script located in the `build/distributions/CCFinderSW-1.0/bin/` directory.

The basic syntax is:
```bash
./build/distributions/CCFinderSW-1.0/bin/CCFinderSW <Mode> [options]
```
For clone detection, the mode is `D`.

#### Required Command-line Arguments

*   `-d <path>`: Specify the directory path where the source files are located.
*   `-l <language>`: Specify the language name for code clone detection (e.g., `java`, `python`, `csharp`). This name is used to load the correct language-specific option file from `src/main/dist/`.
*   `-o <filename>`: Specify the output file name for the clone pair information. The tool automatically adds the `.txt` extension.

#### Common Optional Arguments

*   `-t <integer>`: The minimum number of tokens for a code block to be considered a clone. The default is `50`.
*   `-charset <encoding>`: Specify the character encoding of the source files. Can be `sjis`, `utf8`, `euc`, or `auto`.
*   `-ccfx`: Output in GemX (CCFinderX) format (`.ccfxd` file).
*   `-json <+|->`: Output in JSON format. Use `+` for indented (pretty-printed) JSON.

**Example with more options:**

```bash
./build/distributions/CCFinderSW-1.0/bin/CCFinderSW D \
    -d path/to/your/codebase \
    -l python \
    -o python_duplicates \
    -t 100 \
    -charset utf8 \
    -json +
```

## Supported Languages

This tool supports a wide variety of programming languages. Support for a language requires a corresponding language definition file (e.g., `java_comment.txt`) located in the `src/main/dist/comment/` directory. An optional reserved words list can also be provided in the `src/main/dist/reserved/` directory.

The following languages are supported out-of-the-box:

*   C (`c`)
*   COBOL (`cobol`)
*   C++ (`cpp`)
*   C# (`csharp`)
*   Go (`go`)
*   Haskell (`haskell`)
*   Java (`java`)
*   Perl (`perl`)
*   PHP (`php`)
*   Python (`python`)
*   Ruby (`ruby`)
*   Rust (`rust`)
*   Scala (`scala`)
*   Shell Script (`sh`)
    *   *Note: Also supports experimental, high-fidelity parsing via the `-antlr` flag.*
*   Structured Text (`st`)
*   VBA (`vba`)

When running the tool, use the language name in parentheses (e.g., `-l java`) for the `-l` argument.

## Adding Support for a New Language

You can extend the tool to support new languages by creating a language definition file. This is a text file that tells the tool which file extensions to look for and how to handle comments.

Here is the general process:

1.  **Choose a short name for your language.** This will be used for the configuration files and the `-l` command-line argument (e.g., `gizmo`).

2.  **Create a comment rule file.** This file is **required**.
    *   Create a new text file named `<language_name>_comment.txt` (e.g., `gizmo_comment.txt`).
    *   Place this file in the `src/main/dist/comment/` directory.
    *   In this file, you must specify the file extensions for your language and define its comment syntax.

3.  **Create a reserved words list (optional).** This file is recommended for more accurate (Type-2) clone detection.
    *   Create a new text file named `<language_name>_reserved.txt` (e.g., `gizmo_reserved.txt`).
    *   Place this file in the `src/main/dist/reserved/` directory.
    *   List each reserved word on a new line.

### Example: Adding Support for "Gizmo" (`.giz`)

Let's say we want to add support for a language called "Gizmo" which has:
*   File extension: `.giz`
*   Line comments starting with `//`
*   Block comments from `/*` to `*/`
*   Reserved words: `giz`, `gadget`, `widget`

**Step 1: Create `src/main/dist/comment/gizmo_comment.txt`**

```
#extension
giz

#start
//

#startend
/*
*/
```

**Step 2: Create `src/main/dist/reserved/gizmo_reserved.txt`**

```
giz
gadget
widget
```

**Step 3: Run the tool**

You can now run the tool on a directory of Gizmo files using `-l gizmo`:

```bash
./build/distributions/CCFinderSW-1.0/bin/CCFinderSW D -d path/to/gizmo/files -l gizmo -o gizmo_duplicates
```

## Example Usage on a Complex Repository

Consider a repository with the following structure, containing a mix of Java, Python, and shell scripts:

```
my-complex-project/
├── java/
│   └── src/
├── python/
│   └── scripts/
├── scripts/
│   └── deploy.sh
└── README.md
```

To find duplicates across this entire repository, you can run the tool from the root of `my-complex-project`. Since the tool analyzes one language at a time, you would run it separately for each language.

**For Java files:**
```bash
/path/to/CCFinderSW D -d . -l java -o java_duplicates
```

**For Python files:**
```bash
/path/to/CCFinderSW D -d . -l python -o python_duplicates
```

The tool will scan all files in the current directory and its subdirectories, picking only the ones matching the specified language's file extensions (as defined in the language's option file). The results will be saved to `java_duplicates.txt` and `python_duplicates.txt`, which you can then inspect to find and refactor duplicated code.
