# Unidad 5 — Guía completa de instalación, pruebas y evidencias

Repositorio: https://github.com/rulocm07/Unidad-5-CLN


## 📁 Estructura del Proyecto

```text
05_Proyecto_Base_U5/
├── .github/
│   └── workflows/
│       └── ci.yml               # Pipeline de integración continua (GitHub Actions)
├── datos/
│   ├── matriz_casos_prueba.csv  # Matriz de 12 casos de prueba para el PIA
│   └── pedidos_muestra.csv      # Fixtures de datos de prueba
├── evidencias/
│   ├── capturas/                # Capturas y reporte HTML de cobertura
│   └── Reporte_Unidad5_TechStore.pdf      # Reporte de documentación
├── src/
│   ├── __init__.py
│   ├── domain.py                # Reglas de negocio de pedidos (Order)
│   ├── application.py           # Servicio de checkout y cobros
│   ├── repository.py            # Persistencia con SQLAlchemy ORM
│   ├── security.py              # Políticas de autorización de cancelación
│   └── pia_domain.py            # 3 reglas de negocio del PIA de la empresa
├── tests/
│   ├── __init__.py
│   ├── test_order.py            # Pruebas unitarias de dominio y ciclo TDD
│   ├── test_repository.py       # Pruebas de integración, commit y rollback
│   ├── test_service.py          # Pruebas con Mocks de pasarela externa
│   ├── test_security.py         # Pruebas de seguridad y ventana de tiempo
│   └── test_pia_rules.py        # Casos del PIA CP-01 a CP-09 y CP-12
├── README.txt                   # Manual rápido en texto plano
├── README.md                    # Documentación principal del repositorio
├── requirements.txt             # Dependencias del proyecto
├── .gitignore                   # Exclusiones de Git
└── verificar_laboratorio.py     # Script de autodiagnóstico integral
```

## 1. ¿Qué contiene este proyecto?

El proyecto utiliza Python y pytest para validar reglas de negocio, persistencia, servicios simulados y pruebas del PIA. Integra Ruff, medición de cobertura con pytest-cov y automatización mediante GitHub Actions.

Carpetas y archivos principales observados:

| Ruta | Propósito |
| --- | --- |
| `src/` | Código fuente y reglas de negocio. |
| `tests/` | Pruebas automatizadas. |
| `datos/` | Archivos CSV, incluida la matriz de casos del PIA. |
| `evidencias/` | Capturas y evidencias del laboratorio. |
| `.github/workflows/` | Configuración del pipeline de GitHub Actions. |
| `requirements.txt` | Dependencias de Python. |
| `verificar_laboratorio.py` | Script auxiliar de verificación; consultar su contenido antes de ejecutarlo. |
| `README.md` | Instrucciones originales del repositorio. |

## 2. Instalar los programas necesarios

1. Instala **Python 3** desde https://www.python.org/downloads/ . En Windows, verifica que el comando `py` o `python` funcione.
2. Instala **Visual Studio Code** desde https://code.visualstudio.com/ .
3. Instala **Git** desde https://git-scm.com/downloads .
4. En VS Code, instala la extensión **Python** de Microsoft. La extensión no reemplaza la instalación de Python.
5. Comprueba en una terminal:

```powershell
python --version
git --version
```

Si `python` no funciona, prueba `py --version`. No necesitas Live Server para ejecutar las pruebas Python.

## 3. Descargar el proyecto

**Opción A: ZIP**

1. Abre https://github.com/rulocm07/Unidad-5-CLN .
2. Selecciona **Code → Download ZIP**.
3. Extrae el ZIP a una carpeta fácil de localizar.
4. En VS Code, selecciona **File → Open Folder** y abre la carpeta que contiene `requirements.txt`, `src/` y `tests/`.

**Opción B: Git (recomendada para colaborar)**

```powershell
git clone https://github.com/rulocm07/Unidad-5-CLN.git
cd Unidad-5-CLN
code .
```

Si `code .` no funciona, abre VS Code manualmente y elige la carpeta clonada.

## 4. Crear y activar el entorno virtual

Abre **Terminal → New Terminal** en VS Code y comprueba que estás en la raíz del proyecto.

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
```

Cuando se activa, la terminal suele mostrar `(.venv)` al principio. Si PowerShell bloquea el script, puedes usar la terminal **Command Prompt** y ejecutar:

```cmd
.venv\Scripts\activate.bat
```

También puedes seleccionar el intérprete en VS Code: **Ctrl+Shift+P → Python: Select Interpreter → .venv**.

> No subas `.venv` a GitHub: cada integrante debe crear su propio entorno virtual.

## 5. Instalar dependencias

Con el entorno virtual activo:

```powershell
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

Comprueba que pytest y Ruff estén disponibles:

```powershell
python -m pytest --version
python -m ruff --version
```

Si falta alguna herramienta, revisa `requirements.txt` y el README original antes de modificar dependencias.

## 6. Ejecutar todas las pruebas

```powershell
python -m pytest -v
```

- **PASSED** significa que una prueba pasó.
- **FAILED** significa que la prueba no cumplió una aserción o produjo un error.
- El resumen final indica cuántas pruebas pasaron o fallaron.

En una ejecución anterior del laboratorio se observaron **27 pruebas aprobadas**. La cantidad actual puede cambiar si se agregan pruebas o si estás en otra rama.

## 7. Ejecutar pruebas por archivo

Pruebas del dominio y pedidos:

```powershell
python -m pytest tests/test_order.py -v
```

Pruebas de reglas del PIA:

```powershell
python -m pytest tests/test_pia_rules.py -v
```

Pruebas de persistencia y transacciones:

```powershell
python -m pytest tests/test_repository.py -v
```

Para ejecutar una prueba específica, utiliza `archivo.py::nombre_de_la_prueba`:

```powershell
python -m pytest tests/test_repository.py::test_repository_save_order_250_commit -v
```

Otros archivos como `test_service.py` y `test_security.py` pueden ejecutarse de la misma manera **si están presentes en tu copia del proyecto**.

## 8. Revisar estilo con Ruff

```powershell
python -m ruff check src tests
```

Si aparece un error, revisa el archivo y la línea indicados. Corrige el problema y vuelve a ejecutar el comando. Una ejecución correcta termina sin errores de lint.

## 9. Medir cobertura global

```powershell
python -m pytest -v --cov=src --cov-report=term-missing --cov-report=html --cov-fail-under=80
```

Este comando ejecuta las pruebas, mide qué líneas de `src/` se ejecutaron, muestra las líneas no cubiertas y exige al menos **80 % de cobertura global**.

Para abrir el informe HTML en Windows:

```powershell
start .\htmlcov\index.html
```

**Importante:** Ejecutar solo algunos archivos de pruebas mientras se mide toda la carpeta `src/` puede reducir artificialmente el porcentaje global porque otros módulos no se ejecutan. No significa necesariamente que esas pruebas hayan fallado.

## 10. Medir cobertura de las reglas del PIA (E9)

Este comando reproduce la ejecución que se comprobó en el laboratorio:

```powershell
python -m pytest tests/test_pia_rules.py --cov=src.pia_domain --cov-report=term-missing --cov-report=html --cov-fail-under=80 -v
```

En la ejecución documentada se obtuvieron **11 pruebas aprobadas** y **95.45 % de cobertura** del módulo `src/pia_domain.py`. El porcentaje corresponde a ese módulo, **no** a todo `src/`.

Abre el reporte:

```powershell
start .\htmlcov\index.html
```

Si necesitas incluir también las pruebas de persistencia:

```powershell
python -m pytest tests/test_pia_rules.py tests/test_repository.py -v
```

En la evidencia de esta combinación se observaron **14 pruebas aprobadas**, pero una medición global de `src/` solo con esas pruebas arrojó **57.43 %**, por lo que no superó el umbral global del 80 %. Esto es distinto del resultado de cobertura específico del PIA.

## 11. Consultar la matriz de casos del PIA

Abre en VS Code:

```text
datos/matriz_casos_prueba.csv
```

La matriz documenta 12 casos (`CP-01` a `CP-12`) relativos a stock, límites de unidades, descuentos, persistencia, rechazo de pagos y notificaciones simuladas. El CSV es un registro de casos y resultados; no se ejecuta directamente como un script Python.

## 12. Revisar GitHub Actions (CI)

1. Abre el repositorio en GitHub.
2. Haz clic en **Actions**.
3. Selecciona el workflow **CI Business Logic**.
4. Abre la ejecución que quieras revisar.
5. En el job **Run Lint, Tests & Coverage**, inspecciona los pasos de instalación, Ruff, pytest y cobertura.
6. Una marca verde indica que el job terminó correctamente; una marca roja indica fallo.
7. Para ver por qué falló, expande el paso rojo y lee el error exacto.

En el laboratorio se documentaron tres momentos: ejecución verde inicial, fallo controlado en la rama `ci/controlled-failure` y recuperación posterior. **No copies una prueba deliberadamente fallida a `main`.**

## 13. Actualizar cambios en GitHub

Antes de modificar el repositorio, verifica la rama:

```powershell
git branch
git status
```

Para subir tus cambios a la rama actual:

```powershell
git add .
git commit -m "docs: actualizar instrucciones y evidencias"
git push
```

Si la rama aún no tiene seguimiento remoto, Git puede pedirte:

```powershell
git push -u origin NOMBRE_DE_LA_RAMA
```

Sustituye `NOMBRE_DE_LA_RAMA` por el nombre que muestra `git branch`. Para recibir cambios remotos antes de trabajar, usa `git pull` en la rama adecuada y resuelve cualquier conflicto que aparezca. **No uses `git push --force` para este flujo normal.**

## 14. Errores frecuentes y soluciones

**`No module named pytest`**: normalmente significa que no está activo el entorno correcto o no se instalaron dependencias. Activa `.venv`, instala `requirements.txt` y usa `python -m pytest`.

**PowerShell no permite activar `.venv`**: usa Command Prompt con `.venv\Scripts\activate.bat` o consulta la política de ejecución de tu equipo.

**`Coverage failure: total ... is less than fail-under=80`**: revisa si mediste todos los módulos de `src/` ejecutando solo una parte de las pruebas. Ejecuta toda la suite para la cobertura global o especifica `--cov=src.pia_domain` para el módulo del PIA.

**`ResourceWarning: unclosed database`**: es una advertencia de conexiones SQLite no cerradas; aunque las pruebas puedan pasar, conviene revisar el cierre de sesiones y conexiones en los fixtures.

**Ruff falla antes de pytest en GitHub Actions**: revisa el mensaje de lint y corrige el código; el pipeline puede detenerse antes de llegar a las pruebas.

**`nothing to commit`**: Git no detecta cambios nuevos preparados para confirmar. Comprueba `git status` y si estás editando la carpeta/repositorio correcto.

**Advertencia `LF will be replaced by CRLF`**: normalmente es una notificación de conversión de finales de línea entre sistemas, no una pérdida de archivos.

## 15. Checklist de entrega

- [ ] Descargué o cloné el repositorio correcto.
- [ ] Abrí la carpeta raíz del proyecto en VS Code.
- [ ] Creé y activé `.venv`.
- [ ] Instalé las dependencias.
- [ ] Ejecuté la suite completa de pytest.
- [ ] Ejecuté Ruff.
- [ ] Generé el reporte de cobertura global.
- [ ] Ejecuté las pruebas del PIA y su cobertura específica.
- [ ] Revisé `datos/matriz_casos_prueba.csv`.
- [ ] Revisé GitHub Actions.
- [ ] Guardé las capturas solicitadas para las evidencias E1–E9.
- [ ] Confirmé que no subí `.venv`, cachés ni archivos temporales innecesarios.

---

**Nota de alcance:** Esta guía se elaboró con la estructura y los comandos mostrados durante el laboratorio. No sustituye una inspección de la versión más reciente de todos los archivos del repositorio; consulta el README original y `requirements.txt` si encuentras diferencias.
