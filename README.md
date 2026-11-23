# sonarqube-ci-tp

TP5 – Analyse de la qualité du code avec SonarQube Cloud et GitHub Actions.

## Structure

```
sonarqube-ci-tp/
├── src/
│   ├── __init__.py
│   └── calculator.py
├── tests/
│   └── test_calculator.py
├── requirements.in      (dépendances directes)
├── requirements.txt     (verrouillé avec hashes, généré par pip-compile)
├── pytest.ini
├── .gitignore
├── README.md
├── sonar-project.properties
└── .github/workflows/ci.yml
```

## Exécution locale

```bash
python -m pip install --only-binary :all: --require-hashes -r requirements.txt
python -m pytest -v
python -m pytest --cov=src --cov-branch --cov-report=term-missing --cov-report=xml:coverage.xml
```

## CI

À chaque push sur `main` et à chaque Pull Request vers `main`, le workflow
`.github/workflows/ci.yml` exécute les tests avec couverture puis lance
l'analyse SonarQube Cloud. Le job échoue si le Quality Gate est refusé.
Le token est fourni par le secret GitHub `SONAR_TOKEN`.

## Mise à jour des dépendances

```bash
pip install pip-tools
pip-compile --generate-hashes --strip-extras --output-file requirements.txt requirements.in
```
