# MC-MLIPs

**Molecular Crystals Database for Machine Learning Interatomic Potentials**

Fine-tuned MACE models for polymorphic molecular crystals, trained using the [AMLP framework](https://github.com/adamlaho/AMLP).

## Models

| Compound | CSD Code | # Polymorphs | Energy MAE (meV/atom) | Force MAE (meV/Å) | Model | Dataset |
|----------|----------|:------------:|:---------------------:|:-----------------:|:-----:|:-------:|
| Resorcinol | RESORA | 4 | 1.95 | 4.01 | [↓ model](https://zenodo.org/records/XXXXXXX/files/resorcinol.model) | [↓ train](https://zenodo.org/records/XXXXXXX/files/resorcinol_train.h5) [↓ valid](https://zenodo.org/records/XXXXXXX/files/resorcinol_valid.h5) |
| Durene | DURENE | 3 | 1.82 | 5.98 | [↓ model](https://zenodo.org/records/XXXXXXX/files/durene.model) | [↓ train](https://zenodo.org/records/XXXXXXX/files/durene_train.h5) [↓ valid](https://zenodo.org/records/XXXXXXX/files/durene_valid.h5) |
| Coumarin | COUMAR | 12 | 1.72 | 6.32 | [↓ model](https://zenodo.org/records/XXXXXXX/files/coumarin.model) | [↓ train](https://zenodo.org/records/XXXXXXX/files/coumarin_train.h5) [↓ valid](https://zenodo.org/records/XXXXXXX/files/coumarin_valid.h5) |
| Benzamide | BZAMID | 10 | 1.80 | 9.56 | [↓ model](https://zenodo.org/records/XXXXXXX/files/benzamide.model) | [↓ train](https://zenodo.org/records/XXXXXXX/files/benzamide_train.h5) [↓ valid](https://zenodo.org/records/XXXXXXX/files/benzamide_valid.h5) |
| Niacinamide | NICOAM | 7 | 1.60 | 9.14 | [↓ model](https://zenodo.org/records/XXXXXXX/files/niacinamide.model) | [↓ train](https://zenodo.org/records/XXXXXXX/files/niacinamide_train.h5) [↓ valid](https://zenodo.org/records/XXXXXXX/files/niacinamide_valid.h5) |
| Nicotinamide | NICOAC | 4 | 1.42 | 6.09 | [↓ model](https://zenodo.org/records/XXXXXXX/files/nicotinamide.model) | [↓ train](https://zenodo.org/records/XXXXXXX/files/nicotinamide_train.h5) [↓ valid](https://zenodo.org/records/XXXXXXX/files/nicotinamide_valid.h5) |
| Isonicotinamide | EHOWIH | 5 | 1.93 | 12.51 | [↓ model](https://zenodo.org/records/XXXXXXX/files/isonicotinamide.model) | [↓ train](https://zenodo.org/records/XXXXXXX/files/isonicotinamide_train.h5) [↓ valid](https://zenodo.org/records/XXXXXXX/files/isonicotinamide_valid.h5) |
| Pyrazinamide | PYRIZIN | 14 | 1.69 | 9.68 | [↓ model](https://zenodo.org/records/XXXXXXX/files/pyrazinamide.model) | [↓ train](https://zenodo.org/records/XXXXXXX/files/pyrazinamide_train.h5) [↓ valid](https://zenodo.org/records/XXXXXXX/files/pyrazinamide_valid.h5) |
| Benzoic acid | BENZAC | 6 | 1.34 | 8.88 | [↓ model](https://zenodo.org/records/XXXXXXX/files/benzoic_acid.model) | [↓ train](https://zenodo.org/records/XXXXXXX/files/benzoic_acid_train.h5) [↓ valid](https://zenodo.org/records/XXXXXXX/files/benzoic_acid_valid.h5) |
| **Mean** | | | **1.70** | **8.02** | | |

## Training Protocol

- **Foundation Model**: MACE-MH1 (`mace-mh-1-omol-1%`)
- **Reference Data**: DFT (PBE-D4) optimizations + AIMD trajectories (25-500K)
- **DFT Settings**: VASP, 750 eV cutoff, EDIFF = 10⁻⁷ eV

**Two-stage training:**
1. Initial: LR = 2×10⁻³, energy weight = 100, force weight = 10
2. SWA (epoch 200+): LR = 5×10⁻⁵, force weight = 100

Early stopping with patience = 75 epochs. All models trained in float64.

## Validation

All models validated for:
- **Energy conservation**: NVE drift < 10⁻⁵ over 25 ps
- **Thermal stability**: NVT stable up to 600K
- **Structural integrity**: RDFs and P₂ order parameters preserved

## Usage

We recommend using the [AMLP-Analysis module](https://github.com/adamlaho/AMLP) (`amlpa.py`) for running simulations with these models:
```bash
python3 amlpa.py structure.xyz config.yaml
```

In your `config.yaml`, point to the downloaded model:
```yaml
model_paths:
  - 'path/to/model.model'
device: 'gpu'
gpus: ['cuda:0']
```

Alternatively, you can use the models directly via the MACE calculator:
```python
from mace.calculators import MACECalculator
calc = MACECalculator(model_paths="path/to/model.model", device="cuda")
atoms.calc = calc
```

For full configuration options (MD, geometry optimization, RDF analysis, etc.), refer to the [AMLP documentation](https://github.com/adamlaho/AMLP).

## Citation

If you use these models, please cite:

```bibtex
@article{lahouari2026mcmlips,
  title={Benchmarking Fine-Tuned MACE Interatomic Potentials for Polymorphic Molecular Crystals},
  author={Lahouari, Adam and others},
  journal={},
  year={2026}
}
```

## License

MIT License

## Acknowledgments

- [MACE](https://github.com/ACEsuit/mace) development team
- NYU High Performance Computing
