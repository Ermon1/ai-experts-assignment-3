# AI Experts Assignment (Python)

This repository contains my solution to the AI Experts Python assignment. I set up a small project that runs reliably, pinned dependencies, reproduced a real bug with tests, and fixed it with a minimal, reviewable change.

## What I implemented

- A `Dockerfile` that installs dependencies from `requirements.txt` and runs the test suite by default with `CMD ["pytest", "-v"]`.
- A pinned `requirements.txt` including `pytest`, `requests`, and `python-dateutil`.
- Application code in `app/` for an HTTP client and OAuth2 tokens.
- Tests in `tests/` that exercise the HTTP client behavior and the token handling, including the bug I found and fixed.
- `Explanation.md` describing the bug, why it happened, why my fix works, and one realistic edge case I did not cover with tests.

## How I run the tests locally

I developed and tested this project with Python 3.12.3 on my machine.

From the project root:

1. I create and activate a virtual environment:

   ```bash
   python -m venv .venv
   source .venv/bin/activate
   ```

2. I install the pinned dependencies:

   ```bash
   pip install -r requirements.txt
   ```

3. I run the test suite:

   ```bash
   pytest -v
   ```

## How I run the tests with Docker

In Docker, the project runs on Python 3.12, using the `python:3.12-slim` base image.

From the project root:

1. I build the Docker image:

   ```bash
   docker build -t ai-experts-assignment .
   ```

2. I run the container, which executes the tests by default:

   ```bash
   docker run --rm ai-experts-assignment
   ```

The container’s default command is `pytest -v`, so running the container executes the full test suite in a clean, non-interactive environment.

## Bug I found and fixed

There was a bug in the HTTP `Client.request` method when `oauth2_token` was stored as a plain dictionary instead of as an `OAuth2Token`. In that case, the client did not refresh the token and did not send a proper `Authorization` header on API requests.

I wrote tests in `tests/test_http_client.py` that reproduce this behavior and then made the smallest possible change in `app/http_client.py` to ensure the client refreshes whenever the token is missing, not an `OAuth2Token`, or expired.

More detail on the bug and fix is in `Explanation.md`.
