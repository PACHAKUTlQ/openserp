# OpenSERP (Search Engine Results Page)

![OpenSERP](/logo.png)

[![Go Report Card](https://goreportcard.com/badge/github.com/karust/openserp)](https://goreportcard.com/report/github.com/karust/openserp)
[![Go Reference](https://pkg.go.dev/badge/github.com/karust/openserp.svg)](https://pkg.go.dev/github.com/karust/openserp)
[![release](https://img.shields.io/github/release-pre/karust/openserp.svg)](https://github.com/karust/openserp/releases)

<!-- ![Docker Image Size (tag)](https://img.shields.io/docker/image-size/karust/openserp/latest) -->

API access for search engines results if available isn't free.

Using OpenSERP, you can get search results from **Google**, **Yandex**, **Baidu** via API or CLI!

See [Docker](#docker) and [CLI](#cli) usage examples below ([search](#search), [images](#images)).

## Docker usage <a name="docker"></a> 🐳

- Run API server:

```bash
# Use prebuilt image
docker run -p 127.0.0.1:7000:7000 -it karust/openserp serve -a 0.0.0.0 -p 7000

# Or build one and run using docker-compose.yaml
docker-compose up --build
```

### Request parameters

| Param   | Description                                                            |
| ------- | ---------------------------------------------------------------------- |
| text    | Text to search                                                         |
| lang    | Search pages in selected language (`EN`, `DE`, `RU`...)                |
| date    | Date in `YYYYMMDD..YYYYMMDD` format (e.g. 20181010..20231010)          |
| file    | File extension to search (e.g. `PDF`, `DOC`)                           |
| site    | Search within a specific website                                       |
| limit   | Limit the number of results                                            |
| answers | Include google answers as negative rank indexes (e.g. `true`, `false`) |

### **Search**

### _Example request_

Get 20 **Google** results for `hello world`, only in English:

```
GET http:/127.0.0.1:7000/google/search?lang=EN&limit=20&text=hello world
```

You can replace `google` to `yandex` or `baidu` in query to change search engine.
|

### _Example response_

```JSON
[
    {
        "rank": 1,
        "url": "https://en.wikipedia.org/wiki/%22Hello,_World!%22_program",
        "title": "\"Hello, World!\" program",
        "description": "A \"Hello, World!\" program is generally a computer program that ignores any input, and outputs or displays a message similar to \"Hello, World!\".",
        "ad": false
    },
]
```

### **Images** **[WIP]**

### _Example request_

Get 100 **Google** results for `golden puppy`:

```
GET http://127.0.0.1:7000/google/image?text=golden puppy&limit=100
```

## CLI <a name="cli"></a> ⌨️

- Use `-h` flag to see all commands.
- You can use `serve` command to serve API:

```bash
openserp serve
```

- Or print results in CLI using `search` command:

```bash
openserp search google "how to get banned from google fast" # Change `google` to `yandex` or `baidu`
```

As a result you should get JSON output containting search results:

```json
[
  {
    "rank": 1,
    "url": "https://www.cyberoptik.net/blog/6-sure-fire-ways-to-get-banned-from-google/",
    "title": "11 Sure-Fire Ways to Get Banned From Google | CyberOptik",
    "description": "How To Get Banned From Google · 1. Cloaking: The Art of Deception · 2. Plagiarism: Because Originality is Overrated · 3. Keyword Stuffing: More is Always Better · 4 ...",
    "ad": false
  }
]
```

### Proxy Support

Both browser mode and raw mode support HTTP and SOCKS5 proxies with authentication:

```bash
# HTTP proxy with auth
openserp search google "query" --proxy http://user:pass@127.0.0.1:8080

# Serve with SOCKS5 proxy
openserp serve --proxy socks5://127.0.0.1:1080

# For HTTPS sites through HTTP proxy, use --insecure to ignore certificate errors
openserp search google "query" --proxy http://127.0.0.1:8080 --insecure
openserp search google "query" -x http://127.0.0.1:8080 -k
```

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details

## Bugs + Questions 👾

If you have some issues/bugs/questions, feel free to open an issue.
