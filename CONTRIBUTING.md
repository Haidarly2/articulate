# Contributing to Articulate

First off, thank you for considering contributing to Articulate! Your help is essential for keeping it great.

## Setting Up Your Development Environment

Before you can contribute, you'll need to set up your local environment.

### Prerequisites

* **Go:** Version 1.22 or newer.
* **golangci-lint:** A linter to ensure code quality.

### Installation Steps

1.  **Clone the repository:**
    ```bash
    git clone [https://github.com/](https://github.com/)[your_username]/articulate.git
    cd articulate
    ```

2.  **Install Go:**
    Follow the official installation instructions at [go.dev](https://go.dev/doc/install).

3.  **Install golangci-lint:**
    We use `golangci-lint` to format and lint our code. Please install it using the recommended method for your operating system. For example, on macOS with Homebrew:
    ```bash
    brew install golangci-lint
    ```

### Project Structure

* `/cmd/articulate`: The main entry point for our CLI application.
* `/pkg`: All shared library code (e.g., parsers, executors, assertions) resides here.
* `/docs`: Contains project documentation, including the specification.

### Running Tests

To run Articulate's own internal test suite (not to be confused with the `articulate test` command), use the standard Go toolchain:
```bash
go test ./...
```

Before submitting a pull request, please ensure your code is linted:
```bash
golangci-lint run
```

## How to Contribute

We use the standard GitHub Fork & Pull Request workflow.

1.  **Fork** the repository to your own GitHub account.
2.  **Clone** your fork to your local machine.
3.  Create a new **branch** for your feature or bugfix.
4.  Make your changes and **commit** them with clear messages.
5.  **Push** your branch to your fork.
6.  Open a **Pull Request** from your branch to the `main` branch of the original Articulate repository.
