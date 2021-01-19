---
title: "Python vscode tips"
date: "2021/1/15"
authors: "Liam Bob"
tags: "Python VScode"
---

install useful libs

``` python
pip install black
pip install flake8
pip install pytest
pip install pylint
```

## Linting

### [Pylint]

Powerful one, mostly a good choice.

### [Prospector]

I have a project which has no depence of Django, But Prospector keep reporting `Django is not available on the PYTHONPATH` for each py file, which I didn't install on venv lib.

### [Pylance]

For moudle developer:
Steps To fix worng Hint `could not be resolved Pylance(reportMissingImports)`
Shift+Ctrl+P, types `setting`, find `Perference: Open Settings (JSON)`

add following into `settings.json`

```json
"python.analysis.extraPaths": [
    "./src",　　
    "./modules"
]
```

### [flake8]

add following into `settings.json`

```json
"python.linting.flake8Args": [
    "--ignore=E203,W503"
],
```

## Fromatting

### [black][]

add `black` as default formator for `vscode`

```json
"python.formatting.provider": "black",
"python.formatting.blackArgs": [
    "-l",
    "80"
],
```

## Testing

### Links

- black: [https://black.readthedocs.io/](https://black.readthedocs.io/)
- Bandit: [https://github.com/openstack/bandit](https://github.com/openstack/bandit)
- Codacy: [https://www.codacy.com/](https://www.codacy.com/)
- Dodgy: [https://github.com/landscapeio/dodgy](https://github.com/landscapeio/dodgy)
- Draw.io: [https://www.draw.io/](https://www.draw.io/)
- Flake8: [https://github.com/PyCQA/flake8](https://github.com/PyCQA/flake8)
- Frosted: [https://github.com/timothycrosley/frosted](https://github.com/timothycrosley/frosted)
- Landscape.io: [https://landscape.io/](https://landscape.io/)
- McCabe: [https://github.com/PyCQA/mccabe](https://github.com/PyCQA/mccabe)
- Prospector: [https://github.com/landscapeio/prospector](https://github.com/landscapeio/prospector)
- Pydocstyle: [https://github.com/PyCQA/pydocstyle](https://github.com/PyCQA/pydocstyle)
- Pylama: [https://github.com/klen/pylama](https://github.com/klen/pylama)
- Pylint: [https://github.com/PyCQA/pylint](https://github.com/PyCQA/pylint)
- Pyroma: [https://github.com/regebro/pyroma](https://github.com/regebro/pyroma)
- Pyt: [https://github.com/python-security/pyt](https://github.com/python-security/pyt)
- PyTest: [https://docs.pytest.org/](https://docs.pytest.org/)
- PyUp.io: [https://pyup.io/](https://pyup.io/)
- Radon: [https://github.com/rubik/radon](https://github.com/rubik/radon)
- Safety: [https://github.com/pyupio/safety](https://github.com/pyupio/safety)
- Tox: [https://github.com/tox-dev/tox](https://github.com/tox-dev/tox)
- Vulture: [https://github.com/jendrikseipp/vulture](https://github.com/jendrikseipp/vulture)

[black]: https://black.readthedocs.io/en/stable/index.html
[bandit]: https://github.com/openstack/bandit
[codacy]: https://www.codacy.com/
[dodgy]: https://github.com/landscapeio/dodgy
[draw.io]: https://www.draw.io/
[flake8]: https://github.com/PyCQA/flake8
[frosted]: https://github.com/timothycrosley/frosted
[landscape.io]: https://landscape.io/
[mccabe]: https://github.com/PyCQA/mccabe
[prospector]: https://github.com/landscapeio/prospector
[pydocstyle]: https://github.com/PyCQA/pydocstyle
[pylama]: https://github.com/klen/pylama
[pylint]: https://github.com/PyCQA/pylint
[pyroma]: https://github.com/regebro/pyroma
[pyt]: https://github.com/python-security/pyt
[pyup.io]: https://pyup.io/
[radon]: https://github.com/rubik/radon
[safety]: https://github.com/pyupio/safety
[tox]: https://github.com/tox-dev/tox
[vulture]: https://github.com/jendrikseipp/vulture
