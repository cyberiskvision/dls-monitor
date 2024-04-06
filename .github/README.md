<div align="center">
<h1>
DLS-MONITOR
</h1>
</div>

dls-monitor trails the extortion sites used by ransomware groups (the Data Leak Sites) and surfaces an aggregated feed of claims.

This project is kept open source for information sharing purposes related to global cybersecurity threats.

Derived from the ransomwatch project, dls-monitor is managed and maintained entirely in Italy, representing an independent alternative in the EU area.

Please use the _issue template_ when submitting new groups.

---

<h4 align="center">⚠️</h4>

_Content within `posts.json`, `groups.json` alongside the `docs/` & `source/` directories is dynamically generated based on hosting choices of real-world threat actors in near-real-time._

_Whilst sanitisation efforts have been taken, by viewing or accessing dls-monitor you acknowledge you are doing so at your own risk_.

---

### Key outputs

- _`groups.json` contains hosts, nodes, relays and mirrors for a tracked group or actor_
- _`posts.json` contains extracted posts, noted by their discovery time and accountable group_


## Technicals

This is a live repository that utilizes a combination of GitHub actions and a [service container](https://docs.github.com/en/actions/using-containerized-services/about-service-containers). it visits, parses, and reports on monitored hosts in near-real-time in a self-contained manner.

Content fetching is done with [psf/requests](https://github.com/psf/requests) - if rendering is required [mozilla/geckodriver](https://github.com/mozilla/geckodriver) and [seleniumhq/selenium](https://github.com/SeleniumHQ/selenium) are leveraged.

The frontend is ultimately generated with markdown, using `markdown.py` and served with [docsifyjs/docsify](https://github.com/docsifyjs/docsify) thanks to [pages.github.com](https://pages.github.com).

Graphs or visualisations are generated with `plotting.py` with the help of [matplotlib/matplotlib](https://github.com/matplotlib/matplotlib).

Post indexing is done with a mix of `grep`, `awk` and `sed` within `parsers.py`.

## Tools

Rendered HTML for each page is viewable within the source directory

- `screenshotter.py` _a playwright script to generate high-resolution screenshots of online hosts_
- `srcanalyser.py` _a basic extractor for emails, internal and external links found within page source_
- `browse-hosts.sh` _a simple cURL based iterator for sweeping URL checks_
- `sources.zsh` _an aggregator of various locations that surface new groups for ransomwatch_
- `uptimekuma-importer.py` _a script to convert the group data into a [uptime-kuma](https://github.com/louislam/uptime-kuma) configuration file_
- `parsers.sh` _a health-check script that provides details on parsers that are returning no fields_

_A flattened version of groups.json with each host as its own object can be found at `assets/groups-kv.json`. the structure is an array of objects, each representing a distinct entity/group with each containing all properties (like `name`, `captcha`, `parser`, etc.) at the same level, including potential repetition on elements such as `profile` and `meta`. some data analysis tools work with this structure in an easier manner requiring less transposing._

## Datamap

```
    groups_json ||--|{ group : contains
    group {
        string name "group name"
        boolean captcha "captcha status"
        boolean parser "parser status"
        boolean javascript_render "javascript status"
        string meta "freeform text"
        string url "notable articles and references"
    }
    group ||--|{ locations : has
    locations {
        string fqdn "fully qualified domain name"
        string title "page title"
        int version "hidden service version"
        string slug "full URI"
        boolean available "availability status"
        datetime updated "timestamp of last update"
        datetime lastscrape "timestamp of last scrape"
        boolean enabled "status"
    }
    group ||--|{ post : references
    post {
        string post_title "post title"
        string group_name "associated group name"
        datetime discovered "timestamp of discovery"
    }
```

---

_dls-monitor is licensed under [unlicense.org](https://unlicense.org)_
