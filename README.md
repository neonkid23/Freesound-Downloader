[README.md](https://github.com/user-attachments/files/31197132/README.md)# Freesound Audio Downloader

A compact Windows desktop application for searching, previewing, downloading, and converting sounds from [Freesound.org](https://freesound.org/). Search with keywords, paste a numeric sound ID, or paste a Freesound sound-page URL. Selected sounds can be saved as MP3 or OGG files individually or in batches.

Built with C#, .NET 8, Avalonia UI, and FFmpeg.

## Features

- Search Freesound by keyword, numeric sound ID, or sound-page URL
- Queue several searches with **Shift+Enter**
- Preview audio with separate play and pause controls
- Select individual results or use **Select all**
- Download and convert several sounds with a concurrency limit
- Export to MP3 at 128, 192, 256, or 320 kbps
- Export to OGG at Medium, High, or Very High quality
- Skip transcoding when the Freesound preview already matches the selected format
- Validate HTTPS `cdn.freesound.org` preview URLs before downloading
- Preserve the exact preview URLs returned by the Freesound API
- Report download and conversion status
- Sanitize filenames and avoid overwriting existing files
- Store the API token locally in the user's application settings
- Bundle FFmpeg beside the published Windows executable

## Requirements

The published Windows build is self-contained. A normal user does not need Visual Studio or a separate .NET installation.

You need:

- Windows 10 or Windows 11, x64
- Internet access
- A Freesound account and API token

## Freesound API setup

1. Sign in at [Freesound.org](https://freesound.org/).
2. Open the [Freesound API credentials page](https://freesound.org/apiv2/apply/).
3. Register an API credential if needed.
4. For a desktop application without its own OAuth callback server, use:

   ```text
   http://freesound.org/home/app_permissions/permission_granted/
   ```

5. Copy the API token.
6. Open **Settings** in Freesound Audio Downloader.
7. Paste the token into **API token** and choose **Save token and settings**.

The app accepts either the bare token or a value beginning with `Token `. Credentials are sent using Freesound's `Authorization: Token …` request header.

## Using the application

### Single search

Enter one of the following and press **Enter** or select the arrow:

- Search terms, such as `ocean waves`
- A numeric sound ID, such as `867049`
- A sound page, such as `https://freesound.org/people/username/sounds/867049/`

### Multiple searches

1. Enter a query, ID, or URL.
2. Press **Shift+Enter** to add it to the queue.
3. Repeat for each additional item.
4. Press **Enter** to search the complete queue.

Duplicate results are removed by Freesound sound ID.

### Download and conversion

1. Preview results with the play and pause buttons.
2. Select one or more sounds.
3. Choose MP3 or OGG and the desired quality.
4. Choose the destination folder.
5. Select **Download selected**.

The application downloads only direct audio preview resources returned by the Freesound API. It does not save Freesound HTML pages as audio and does not guess CDN paths.

## FFmpeg

FFmpeg is used only when the available preview format differs from the requested output format. Discovery checks:

1. `ffmpeg.exe` beside `FreesoundDownloader.exe`
2. An `ffmpeg` installation available through the system `PATH`

The distributed release folder includes `ffmpeg.exe` beside the application.

## Build from source

Install the .NET 8 SDK, then run:

```powershell
dotnet restore FreesoundDownloader.Tests/FreesoundDownloader.Tests.csproj
dotnet test FreesoundDownloader.Tests/FreesoundDownloader.Tests.csproj -c Release
dotnet publish FreesoundDownloader/FreesoundDownloader.csproj `
  -c Release `
  -r win-x64 `
  --self-contained true `
  -o release/win-x64
```

The primary executable will be:

```text
release/win-x64/FreesoundDownloader.exe
```

Place a compatible `ffmpeg.exe` in the same folder if it is not available through `PATH`.

## Project structure

```text
FreesoundDownloader/
├── Assets/
├── Models/
├── Services/
├── Utilities/
├── ViewModels/
└── Views/

FreesoundDownloader.Tests/
```

## Privacy and responsible use

- The API token is stored in `%APPDATA%\FreesoundDownloader\settings.json`.
- The token is not embedded in the source code or published executable.
- API metadata requests are sent to `freesound.org`.
- Audio downloads are accepted only from HTTPS `cdn.freesound.org` MP3 or OGG preview URLs returned by the API.
- Each result displays its Freesound license. Users are responsible for following that license and Freesound's terms.

## Acknowledgements

- Sound metadata and previews are provided by [Freesound.org](https://freesound.org/).
- Audio conversion is performed with [FFmpeg](https://ffmpeg.org/).
- The desktop interface is built with [Avalonia UI](https://avaloniaui.net/).

This project is not affiliated with or endorsed by Freesound.
