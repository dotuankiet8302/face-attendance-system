# face-attendance-system

A desktop attendance system that checks users in and out using face recognition, with liveness (anti-spoofing) detection to prevent someone from checking in with a photo or video of another person.

## How it works

The app opens a Tkinter window with a live webcam feed and three actions:

- **Register new user** — captures a face from the webcam, extracts a face embedding (via `face_recognition`), and saves it to `db/<username>.pickle`.
- **Login** — captures the current frame, runs it through an anti-spoofing model to confirm a real person is in front of the camera, then compares the face against everyone stored in `db/`. On a match, it shows a welcome message and appends `<name>,<timestamp>,in` to `log.txt`.
- **Logout** — same flow as Login, but appends `<name>,<timestamp>,out` to `log.txt`.

If no real face is detected, or the face doesn't match anyone in `db/`, the user gets an error dialog instead of being logged.

## Demo videos

<p align="center">
<a href="https://www.youtube.com/watch?v=z_dbnYHAQYg">
    <img width="600" src="https://utils-computervisiondeveloper.s3.amazonaws.com/thumbnails/with_play_button/face_attendance.jpg" alt="Watch the video">
    </br>Watch on YouTube: Face attendance system with Python and face recognition !
</a>
</p>

<p align="center">
<a href="https://www.youtube.com/watch?v=_KvtVk8Gk1A">
    <img width="600" src="https://utils-computervisiondeveloper.s3.amazonaws.com/thumbnails/with_play_button/face_attendance_spoofing.jpg" alt="Watch the video">
    </br>Watch on YouTube: Face attendance system with liveness detection !
</a>
</p>

## Requirements

- Python 3.8
- A webcam

### Windows

1. Follow the extra setup steps in this video (needed to install `dlib`/`face_recognition` on Windows): https://www.youtube.com/watch?v=oTv7HB6CRpQ
2. Install the packages in `requirements_windows.txt`:

       pip install -r requirements_windows.txt

### Linux / macOS

    pip install -r requirements.txt

## Anti-spoofing setup (required)

The liveness check depends on a separate model repo that isn't bundled here:

    git clone https://github.com/computervisioneng/Silent-Face-Anti-Spoofing.git
    pip install -r Silent-Face-Anti-Spoofing/requirements.txt

By default, `main.py` expects the cloned repo at `./Silent-Face-Anti-Spoofing` (next to `main.py`), via the `ANTI_SPOOF_MODEL_DIR` constant at the top of the file. If you cloned it somewhere else, update that constant to point at the `resources/anti_spoof_models` folder inside your clone.

Also add the `Silent-Face-Anti-Spoofing` directory to your `PYTHONPATH` so `from test import test` in `main.py` resolves correctly.

## Webcam index

`main.py` opens the webcam using the `WEBCAM_INDEX` constant at the top of the file (defaults to `0`). If the video feed doesn't show up when you run the app, or you have multiple cameras, change that constant (e.g. `1`, `2`, ...) until it picks up the right one.

## Running the app

    python main.py

First use **register new user** to enroll at least one face before trying **login**/**logout**.

## Data produced by the app

- `db/` — one `.pickle` file per registered user, containing their face embedding.
- `log.txt` — attendance log, one line per login/logout: `name,timestamp,in|out`.

## Web app version

A version of this project built as a web app with React and Python is available [here](https://github.com/computervisiondeveloper/face-attendance-web-app-react-python).

## License

MIT — see [LICENSE.md](LICENSE.md).
