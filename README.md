[![Documented with Setinstone.io](https://img.shields.io/badge/⛰️Documented%20with-Setinstone.io-success?logo=book&logoColor=white)](https://calendly.com/set-in-stone-thomas-benoit/setinstone-demo)

# delta-rs

## Presentation

**delta-rs** is a native Rust library for **[Delta Lake](https://delta.io/)**, an open-source storage format that brings ACID transactions and schema enforcement to data lakes.  
This repository provides the Rust implementation as well as Python bindings, allowing users to interact with Delta tables efficiently in both ecosystems.  

The main goal of the project is to offer robust, native APIs for developers and integrators, alongside high-level abstractions for analytics and operations across different storage backends (AWS S3, Azure Blob Storage, GCP, HDFS, lakeFS, etc.).  

**Repository:** [https://github.com/thomgit9/delta-rs](https://github.com/thomgit9/delta-rs)  
**License:** Apache-2.0  
**Default branch:** `main`  
**Visibility:** Public  

---

<p align="center">
  <a href="https://delta.io/">
    <img src="https://github.com/delta-io/delta-rs/blob/main/docs/delta-rust-no-whitespace.svg?raw=true" alt="delta-rs logo" height="200">
  </a>
</p>

<p align="center">
  A native Rust library for Delta Lake, with bindings to Python  
  <br>
  <a href="https://delta-io.github.io/delta-rs/">Python docs</a> ·
  <a href="https://docs.rs/deltalake/latest/deltalake/">Rust docs</a> ·
  <a href="https://github.com/delta-io/delta-rs/issues/new?template=bug_report.md">Report a bug</a> ·
  <a href="https://github.com/delta-io/delta-rs/issues/new?template=feature_request.md">Request a feature</a> ·
  <a href="https://github.com/delta-io/delta-rs/issues/1128">Roadmap</a>
  <br><br>
  <a href="https://pypi.python.org/pypi/deltalake">
    <img alt="License" src="https://img.shields.io/pypi/l/deltalake.svg?style=flat-square&color=00ADD4&logo=apache">
  </a>
  <a href="https://github.com/thomgit9/delta-rs">
    <img src="https://img.shields.io/github/stars/delta-io/delta-rs?logo=github&color=F75101">
  </a>
  <a href="https://crates.io/crates/deltalake">
    <img alt="Crate" src="https://img.shields.io/crates/v/deltalake.svg?style=flat-square&color=00ADD4&logo=rust">
  </a>
  <a href="https://pypi.python.org/pypi/deltalake">
    <img alt="PyPI version" src="https://img.shields.io/pypi/v/deltalake.svg?style=flat-square&color=F75101&logo=pypi">
  </a>
  <a href="https://pypi.python.org/pypi/deltalake">
    <img alt="Python versions" src="https://img.shields.io/pypi/pyversions/deltalake.svg?style=flat-square&color=00ADD4&logo=python">
  </a>
  <a href="https://go.delta.io/slack">
    <img alt="#delta-rs Slack" src="https://img.shields.io/badge/slack-delta-blue.svg?logo=slack&style=flat-square&color=F75101">
  </a>
</p>

---

## Installation

### Python
```bash
pip install deltalake
```

### Rust
```bash
cargo add deltalake
```

| Source | Downloads | Installation Command | Docs |
| --- | --- | --- | --- |
| **[PyPi](https://pypi.org/project/deltalake/)** | ![Downloads](https://img.shields.io/pypi/dm/deltalake?style=flat-square&color=00ADD4) | `pip install deltalake` | [Python docs](https://delta-io.github.io/delta-rs/) |
| **[Crates.io](https://crates.io/crates/deltalake)** | ![Downloads](https://img.shields.io/crates/d/deltalake?color=F75101) | `cargo add deltalake` | [Rust docs](https://docs.rs/deltalake/latest/deltalake/) |

---

## Usage

### Python
```python
from deltalake import DeltaTable, write_deltalake
import pandas as pd

df = pd.DataFrame({"id": [1, 2], "value": ["foo", "boo"]})
write_deltalake("./data/delta", df)

dt = DeltaTable("./data/delta")
df2 = dt.to_pandas()

assert df.equals(df2)
```

### Rust
```rust
use deltalake::{open_table, DeltaTableError};

#[tokio::main]
async fn main() -> Result<(), DeltaTableError> {
    let table = open_table("./data/delta").await?;
    let files: Vec<_> = table.get_file_uris()?.collect();
    println!("{files:?}");
    Ok(())
}
```

You can also use Delta Lake via Docker ([DockerHub](https://go.delta.io/dockerhub)).

---

## Functions and Classes Overview

| Name | File | Description | Inputs | Outputs |
|---|---|---|---|---|
| `DeltaTable` | `deltalake/src/lib.rs` | Core abstraction for representing a Delta Lake table in Rust and Python bindings | Path (URI), version, options | Table object |
| `write_deltalake()` | `deltalake/src/lib.rs` | Write DataFrame-like objects into a Delta table | Path, dataframe | Delta transaction log update |
| `open_table()` | `deltalake/src/lib.rs` | Asynchronously open a Delta table given a path/URI | URI, optional version | Loaded DeltaTable instance |
| `builder::parse_table_uri()` | `deltalake/table/builder.rs` | Parse table URIs consistently across backends | String (URI) | Canonicalized URL |
| `merge_case_by_name()` | `crates/benchmarks/src/main.rs` | Retrieve predefined TPC-DS merge benchmark case | Case name | MergeTestCase struct |
| `run_merge_case()` | `crates/benchmarks/src/main.rs` | Execute a merge benchmark workload | MergeCase, Parquet directory | Metric results (time, throughput) |
| `AtomicRenameSys` | `proofs/stateright/src/main.rs` | Modeled system for testing atomic rename consistency | Number of writers | Simulation of lock states |
| `App::new("Delta table inspector")` | `delta-inspect/src/main.rs` | CLI utility for inspecting, listing, and vacuuming Delta tables | CLI arguments | Metadata display or cleanup status |

---

## Get Involved

- Join our [Slack workspace](https://go.delta.io/slack)
- [Report a bug](https://github.com/delta-io/delta-rs/issues/new?template=bug_report.md)
- [Request a feature](https://github.com/delta-io/delta-rs/issues/new?template=feature_request.md)
- [Contribute](https://github.com/delta-io/delta-rs/contribute)
- View the [roadmap](https://github.com/delta-io/delta-rs/issues/1128)

**Top Contributors:**
- [rtyler](https://github.com/rtyler) (457 commits)
- [ion-elgreco](https://github.com/ion-elgreco)
- [roeap](https://github.com/roeap)
- [dependabot[bot]](https://github.com/apps/dependabot)
- [fvaleye](https://github.com/fvaleye)
- [wjones127](https://github.com/wjones127)
- [houqp](https://github.com/houqp)
- [Blajda](https://github.com/Blajda)
- [mosyp](https://github.com/mosyp)

---

## ⛰️ Documented With SetinStone.io
Focus on the only task that matters: building your codebase! With every developer push, Set In Stone’s Mirror Documentation Agent updates your README.md via a pull request — ready for you to review, edit, and approve.

[Book a demo](https://calendly.com/set-in-stone-thomas-benoit/setinstone-demo)