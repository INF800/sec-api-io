# sec-api-io

<!-- WARNING: THIS FILE WAS AUTOGENERATED! DO NOT EDIT! -->

<a href="https://github.com/elijas/sec-api-io/actions/workflows/test.yaml"><img alt="GitHub Workflow Status" src="https://img.shields.io/github/actions/workflow/status/elijas/sec-api-io/test.yaml?label=build"></a>
<a href="https://pypi.org/project/sec-api-io/"><img alt="PyPI - Python Version" src="https://img.shields.io/pypi/pyversions/sec-api-io"></a>
<a href="https://badge.fury.io/py/sec-api-io"><img src="https://badge.fury.io/py/sec-api-io.svg" alt="PyPI version" /></a>
<a href="LICENSE"><img src="https://img.shields.io/github/license/elijas/sec-api-io.svg" alt="Licence"></a>

Unofficial wrapper for the [sec-api.io API](https://sec-api.io). Built
with [nbdev](https://nbdev.fast.ai/).

## Install and Setup

Run in terminal:

``` sh
pip install sec_api_io
```

## How to use

### (Optional) Set API key with `.env` file

It’s highly recommended to set your API key in a `.env` file to avoid
setting it in the code.

1.  Make a copy of the `.env.template` file in the root directory of the
    project.
2.  Rename the copied file to `.env`.
3.  Open the `.env` file and locate the `SECAPIO_API_KEY` variable.
4.  Fill in the value for the `SECAPIO_API_KEY` variable.
    - You can obtain a free key from [sec-api.io](https://sec-api.io/).
    - Note: The first 100 requests are free.
5.  Save the `.env` file next to your notebook or script.

> **Important Note:** Depending on your geographical location, you might
> need to use a VPN set to a United States location to access
> [sec-api.io](https://sec-api.io/) API.

Let’s load the API key from .env file into the environment variable
SECAPIO_API_KEY

``` python
!pip install -q python-dotenv
```

``` python
from dotenv import load_dotenv

load_dotenv() # Load the API key from .env file into the environment variable SECAPIO_API_KEY
```

``` python
import os 
from dotenv import load_dotenv

if 'SECAPIO_API_KEY' not in os.environ:
    assert load_dotenv()
```

### Get latest 10-Q report by ticker

``` python
from sec_api_io.secapio_data_retriever import SecapioDataRetriever

retriever = SecapioDataRetriever()
# retriever = SecapioDataRetriever(api_key=...) # If you don't want to use .env file

metadata = retriever.retrieve_report_metadata('10-Q', latest_from_ticker='AAPL')
url = metadata["linkToFilingDetails"]

assert url.startswith('https://www.sec.gov/Archives/edgar/data/')
url
```

    'https://www.sec.gov/Archives/edgar/data/320193/000032019323000077/aapl-20230701.htm'

### Download 10-Q HTML split into sections

``` python
html = retriever.get_report_html('10-Q', url)
assert html
```

``` python
for line in html.splitlines():
    print(line[:65] + '...')
```

    <top-level-section-separator id="part1item1" title="Financial Statemen...
    <span style="color:#000000;font-family:'Helvetica',sans-serif;fon...
    <top-level-section-separator id="part1item2" title="Management's Discu...
    <span style="color:#000000;font-family:'Helvetica',sans-serif;fon...
    <top-level-section-separator id="part1item3" title="Quantitative and Q...
    <span style="color:#000000;font-family:'Helvetica',sans-serif;fon...
    <top-level-section-separator id="part1item4" title="Controls and Proce...
    <span style="color:#000000;font-family:'Helvetica',sans-serif;fon...
    <top-level-section-separator id="part2item1" title="Legal Proceedings"...
    <span style="color:#000000;font-family:'Helvetica',sans-serif;fon...
    <top-level-section-separator id="part2item1a" title="Risk Factors" com...
    <span style="color:#000000;font-family:'Helvetica',sans-serif;fon...
    <top-level-section-separator id="part2item2" title="Unregistered Sales...
    <span style="color:#000000;font-family:'Helvetica',sans-serif;fon...
    <top-level-section-separator id="part2item3" title="Defaults Upon Seni...
    <span style="color:#000000;font-family:'Helvetica',sans-serif;fon...
    <top-level-section-separator id="part2item4" title="Mine Safety Disclo...
    <span style="color:#000000;font-family:'Helvetica',sans-serif;fon...
    <top-level-section-separator id="part2item5" title="Other Information"...
    <span style="color:#000000;font-family:'Helvetica',sans-serif;fon...
    <top-level-section-separator id="part2item6" title="Exhibits" comment=...
    <span style="color:#000000;font-family:'Helvetica',sans-serif;fon...

## Contributing

Follow these steps to install the project locally for development:

1. Install the project with the command `pip install -e ".[dev]"`.

> **Note**
We highly recommend using virtual environments for Python development. If you're using virtual environments, follow these steps instead:
> - Create a virtual environment `python3 -m venv .venv`
> - Activate the virtual environment `source .venv/bin/activate`
> - Install the project with the command `pip install -e ".[dev]"`
