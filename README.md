name: Python_tests
on: [push, pull_request]
jobs:
  Python_tests:
    runs-on: ${{ matrix.os }}
    strategy:
      fail-fast: false
      matrix:
        os: [macos-latest, ubuntu-latest, windows-latest]
        python-version: [3.5, 3.6, 3.7, 3.8]
        os: [ubuntu-latest]  # [macos-latest, windows-latest]
        python-version: [3.5, 3.6, 3.7]  # , 3.8]
    steps:
      - uses: actions/checkout@v1
      - name: Set up Python ${{ matrix.python-version }}
        uses: actions/setup-python@v1
        with:
          python-version: ${{ matrix.python-version }}
      - name: Install dependencies
        run: |
          python -m pip install --upgrade pip
          pip install flake8 pytest -r requirements.txt
      #- name: Check formatting with black
      #  if: matrix.os == 'ubuntu-latest' && matrix.python-version == '3.8'
      #  run: |
      #     pip install black
      #     black --check .
      - name: Lint with flake8
        run: |
          # stop the build if there are Python syntax errors or undefined names
          flake8 . --count --select=E9,F63,F7,F82 --show-source --statistics
          # exit-zero treats all errors as warnings. The GitHub editor is 127 chars wide
          flake8 . --count --exit-zero --max-complexity=10 --max-line-length=127 --statistics
      #- name: Test with pytest
      #  run: pytest
      #- name: Run doctests with pytest
