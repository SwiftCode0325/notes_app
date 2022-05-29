# notes_app
![](https://github.com/datahappy1/notes_app/blob/main/notes_app/assets/notes_app_logo.png)

Notes application written in Python 3.8 & KivyMD

![](https://github.com/datahappy1/notes_app/blob/main/notes_app_scrn.png)

## application description
- OS independent
- Notes can be grouped in separate sections, these sections can be further added and removed
- Notes storage file can be placed anywhere on the local drive but can also be placed in a DropBox shared folder for example to synchronize notes across devices
- Full-text or word-based search capability across the current section or all sections
- Customizable fonts and colors, settings can be persisted
- Notes text is auto-saved while typing

## version history
| version | date | description |
| :---: | :---: | :---: |
| v1.0.0  | 29/5/2022  | initial release |

## FAQ
- I need to synchronize my notes file across devices, how do I achieve that?
  - Copy the notes file to the synchronized folder of your choice
  - Click the file icon menu in the app, press `choose storage file`, locate the file in the synchronized folder and you're set  

### information for developers
The application design is based on the MVC architecture with the observer notification pattern. 
The notes are stored in a defined text file where the notes sections are separated by the defined section separator pattern.  
The model in the MVC is responsible for storing the metadata of this text file only. 
When the notes text file content significantly changes causing last updated timestamp or file size model attributes to change, or when a different text file
for storage is chosen, the view is notified through it's registered observer and displays a info message on the UI.

When running the app for the first time, these needed files get auto-generated:
- `file_metadata.json` - stores the notes text file metadata like it's file path, size and last updated timestamp
```json
{
    "_file_path": {"value": "some/path/to/my_first_file.txt"}, 
    "_file_size": {"value": 42}, 
    "_last_updated_on": {"value": "2022-05-28 12:37:56"}
}
```
- `my_first_file.txt` - this is the notes text file itself
```text
<section=first>  Your first section. 
Here you can write your notes.
<section=second>  Another section of yours.
```
- `settings.json` - stores settings like fonts, colors for the UI
```json
{
    "font_name": {"value": "RobotoMono-Regular"}, 
    "font_size": {"value": "14.0"}, 
    "background_color": {"value": "black"}, 
    "foreground_color": {"value": "green"}
}
```

When running the app locally, Pipenv is recommended but general requirements.txt file is also attached.

- generating dependencies graph using Pydeps
```language="sh"
cd notes_app
pip3 install pydeps
pydeps notes_app --max-bacon 2 --cluster --rmprefix notes_app. --exclude-exact notes_app.utils notes_app.view notes_app.model notes_app.controller notes_app.observer
```
![](https://github.com/datahappy1/notes_app/blob/main/notes_app.svg)

- running unit tests using Pytest
```language="sh
cd notes_app
pytest
```

### building the application
#### Windows app build from Windows environment:
- prerequisites example:

```language="sh"
virtualenv venv_notes_app
venv_notes_app\Scripts\activate.bat
(venv_notes_app) python -m pip install --upgrade pip wheel setuptools
(venv_notes_app) python -m pip install kivy docutils pygments pypiwin32 kivy.deps.sdl2 kivy.deps.glew kivy.deps.gstreamer kivy.deps.angle
(venv_notes_app) python -m pip install PyInstaller
(venv_notes_app) PyInstaller --name notes notes_app/main.py
```

- build command example:
```language="sh"
(venv_notes_app) PyInstaller c:\notes_app\notes_app\notes_win.spec
```

#### Linux app build from Linux environment:
- prerequisites example:

```language="sh"
virtualenv venv_notes_app
source bin/activate
(venv_notes_app) python -m pip install --upgrade pip wheel setuptools
(venv_notes_app) python -m pip install PyInstaller
```

- build command example:
```language="sh"
(venv_notes_app) pyinstaller notes_linux.spec
```

## useful links
- development 
  - https://kivymd.readthedocs.io/en/latest/components/
  - https://www.tutorialspoint.com/design_pattern/mvc_pattern.htm
  - https://github.com/HeaTTheatR/Kivy_MVC_Template/tree/main
  - https://github.com/thebjorn/pydeps
  
- app building
  - https://pyinstaller.org/en/stable/when-things-go-wrong.html
  - https://dev.to/ngonidzashe/using-pyinstaller-to-package-kivy-and-kivymd-desktop-apps-2fmj
  - https://github.com/devgiordane/kivy-md-build
