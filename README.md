# coE3T simulation

This repository uses [METS-R_HPC](https://github.com/umnilab/METS-R_HPC) as a local asset. Docker and Python 3.9 or newer are required.

Clone the asset beside this repository's notebooks (the `METS-R_HPC/` folder is intentionally gitignored), then install the combined dependencies:

```bash
git clone https://github.com/umnilab/METS-R_HPC.git METS-R_HPC
pip install -r requirements.txt
```

If the asset already exists, update it before using a notebook:

```bash
git -C METS-R_HPC pull --ff-only
```

Start with [`interactive_example.ipynb`](interactive_example.ipynb). [`transit_mapping_demo.ipynb`](transit_mapping_demo.ipynb) demonstrates the transit APIs. Both notebooks load the client and launch utilities from the local `METS-R_HPC` checkout and use the current native v2 API response schema (`messageType`, `status`, `data`, and camelCase fields).

The JSON files under [`configs/`](configs/) use the upstream `parent_config` inheritance pattern. Load them with `read_run_config` from the asset's `utils.util` module; the local templates inherit current defaults from the cloned asset, while the Birmingham and NEMA files contain only project-specific overrides.

The tracked [`data/`](data/) tree likewise contains only coE3T-specific Birmingham and NEMA inputs. Each notebook copies this tree into the ignored `METS-R_HPC/data/` asset with `shutil.copytree(..., dirs_exist_ok=True)`, then runs the asset's vanilla `prepare_sim_dirs`, `run_simulation_in_docker`, and `METSRClient`. Run preparation is scoped to the asset directory so its shared data template and relative paths are used without duplicating them in this repository.

The NEMA SUMO network includes reverse-return edges at the two one-way fringe roads that previously left METS-R's dead-end-origin propagation without an upstream junction. Existing road IDs and transit mappings are unchanged.
