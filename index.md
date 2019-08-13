---
layout: default
---

Project Metaheuristic

<!---
<p align="center">
  <a href="https://getbootstrap.com/">
    <img src="https://docs.metaheuristic.ai/assets/brand/mh-logo.svg" alt="Metaheuristic logo" width="72" height="72">
  </a>
</p>
--->
 
<h3 align="center">Metaheuristic</h3>

<p align="center">
  Distributer framework for hyper-parameter optimization and AutoML.
  <br>
  <a href="https://getbootstrap.com/docs/4.3/"><strong>Explore Bootstrap docs »</strong></a>
  <br>
  <br>
  <a href="https://github.com/twbs/bootstrap/issues/new?template=bug.md">Report bug</a>
  ·
  <a href="https://github.com/twbs/bootstrap/issues/new?template=feature.md&labels=feature">Request feature</a>
  ·
  <a href="https://themes.getbootstrap.com/">Themes</a>
  ·
  <a href="https://blog.getbootstrap.com/">Blog</a>
</p>


## Table of contents

- [Quick start](#quick-start)
- [Status](#status)
- [What's included](#whats-included)
- [Bugs and feature requests](#bugs-and-feature-requests)
- [Documentation](#documentation)
- [Contributing](#contributing)
- [Community](#community)
- [Versioning](#versioning)
- [Creators](#creators)
- [Thanks](#thanks)
- [Copyright and license](#copyright-and-license)

## Features

| | [H2](https://h2database.com/) | [Derby](https://db.apache.org/derby) | [HSQLDB](http://hsqldb.org) | [MySQL](https://www.mysql.com/) | [PostgreSQL](https://www.postgresql.org) |
|--------------------------------|---------|---------|---------|-------|---------|
| Pure Java                      | Yes     | Yes     | Yes     | No    | No      |
| Memory Mode                    | Yes     | Yes     | Yes     | No    | No      |
| Encrypted Database             | Yes     | Yes     | Yes     | No    | No      |
| ODBC Driver                    | Yes     | No      | No      | Yes   | Yes     |
| Fulltext Search                | Yes     | No      | No      | Yes   | Yes     |
| Multi Version Concurrency      | Yes     | No      | Yes     | Yes   | Yes     |
| Footprint (embedded database)  | ~2 MB   | ~3 MB   | ~1.5 MB | —     | —       |
| Footprint (JDBC client driver) | ~500 KB | ~600 KB | ~1.5 MB | ~1 MB | ~700 KB |

Quick start to see only UI

git clone https://github.com/sergmain/metaheuristic.git

create dir metaheuristic
copy file metaheuristic\docs-dev\cfg\launchpad.yaml to mh-station

go to metaheuristic dir and run maven script:
mvn-all

java -version
openjdk version "11.0.2" 2018-10-16
OpenJDK Runtime Environment 18.9 (build 11.0.2+7)
OpenJDK 64-Bit Server VM 18.9 (build 11.0.2+7, mixed mode)

java -Dspring.profiles.active=quickstart,launchpad,station -jar metaheuristic/apps/metaheuristic/target/metaheuristic.jar 
