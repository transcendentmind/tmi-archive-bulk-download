# tmi-archive-bulk-download

A collection of Bash scripts to download [Culadasa](https://culadasa.com/)'s recordings from [tmi-archive.com](https://tmi-archive.com/) in an organized way. See [here](#tmi-archive-bulk-download-1) for a list of features.

M3U playlists are used to organize downloaded audio files into sets of recordings from specific events, and by other criteria.

A `generate-audiobooks` script is provided to allow easy copying of recordings from any given playlist(s) into a folder or folders&mdash;so that, for example, one could copy that folder onto a mobile device to be able to listen to those recordings as an audiobook.

See [below](#usage) for detailed usage notes for other scripts.

## Requirements

Besides a terminal to run the scripts in, the following are required:

* Bash
* GNU coreutils
* GNU sed
* wget
* Enhanced getopt (part of the `util-linux` package)
* git (optional)

### Installation

#### Linux

The required packages should come with most Linux distros out of the box. If any are missing, you should be able to easily install them with your distro's package manager. 

#### MacOS

On MacOS, the required packages can be installed via [MacPorts](https://macports.org) or [Homebrew](https://brew.sh). I made a [tutorial](https://www.youtube.com/watch?v=vC6ykcyTzLg) on my [Linguistic Mind](https://www.youtube.com/channel/UCZ_2W3ilvrS-20rk1reNIZA) YouTube channel explaining how to install and use those.

On MacPorts, run `sudo port install coreutils gsed wget util-linux` to install required packages. You can also install the latest version of Bash by running `sudo port install bash` (recommended).

On Homebrew, the commands are `brew install coreutils gsed wget util-linux`, `brew install bash`.

After the installation has finished, make sure to add the `gnubin` directory to your `PATH` so that the GNU programs take precedence over the MacOS's built in ones.

See `port notes coreutils` on MacPorts and `brew info coreutils` on Homebrew for information on how to do it. I also explain how to do it in the same [tutorial](https://www.youtube.com/watch?v=vC6ykcyTzLg) that I mentioned previously.

### Windows

I don't have any first-hand experience with running Bash on Windows, but I'm sure it can be done. A quick Google or YouTube search should easily be able to provide you with immedeate guidance as to how get Bash running, as well as how to install the required packages.

## Downloading this repo

You might want to install `git` in order to download this repo in the most straightforward fashion right from the terminal.

Otherwise, if you're reading this document on the GitHub website, you can download this repo by clicking the green _Code_ button (located right above the place where all the files are listed), and selecting _Download ZIP_ from the menu that appears.

If you have `git` installed, run `git clone https://github.com/transcendentmind/tmi-archive-bulk-download.git` to clone this repo into the current working directory in your terminal.

If you don't have `git` installed, please refer to the [installation notes in the section above](#installation).

## Playlists

The `.m3u` playlist files in the root directory of this repo are formatted in a specific way.

### Metadata section

A playlist may optionally begin with a few comment lines containing metadata that can be used by other scripts when they interact with the playlist. They also give the user additional information about the playlist.

For example:

```text
# Title:           The Magic of Mindfulness
# Extended title:  The Magic of Mindfulness: Seeing Things As They Really Are
# Location:        Tucson Community Meditation Center, Arizona
# Date:            2011-10-14--16
# URL:             https://tmi-archive.com/talk/269
_files/tcmc-weekend-10-14-2011a_trimmed.mp3
_files/tcmc-weekend-10-14-2011b_trimmed.mp3
...
```

The format of the metadata lines is `# <key>: <value>`.

Where there is a space in the format definition above, there can be any number of whitespace characters. Trailing whitespace after `<value>` is ignored.

`<key>` may not contain a colon (`:`).

### Main section

The remaining part of the playlist contains paths to audio files. All of these paths are to files in the `_files` folder. Those files are to be downloaded with the [`tmi-archive-bulk-download`](#tmi-archive-bulk-download-1) script.

#### Extras

Moreover, some files in a playlist are considered _extras_. Those are files such as clips from other files (e.g. meditation-only tracks), untrimmed versions of files etc.

Those lines are also comment lines formatted in a specific way. See the section on [`playlist-extra-*` scripts](#playlist-extra-) for the description of that formatting.

## Usage

### [`tmi-archive-bulk-download`](tmi-archive-bulk-download)

The main script that downloads the audio files from [tmi-archive.com](https://tmi-archive.com/). See the script file for notes on what the script does and what configuration options are available.

All files are downloaded to a folder called `_files` in the root directory of this repo. The `_files` folder will be automatically created upon running this script. 

Some notable features:

* Avoid downloading duplicate files (perfect duplicates were identified with the [`fdupes`](https://github.com/adrianlopezroche/fdupes) utility)
* Preserve original filenames of the recordings as well as their tmi-archive.com filenames
* Export metadata of the original files so that it can be preserved if, for instance, one wishes to modify the metadata on the downloaded files
* Fix the issue leading to an "Estimating duration from bitrate, this may be inaccurate" warning message when playing many (if not all) of the downloaded files
* <a id='tmi-archive-bulk-download-other'></a> Download several files (see [`tmi-archive-bulk-download-other`](tmi-archive-bulk-download-other)) that are linked to on the tmi-achive.com website, but couldn't be found on its [bulk download page](https://tmi-archive.com/bulk-download).
* Choose whether to download the original audio files or the cleaned audio files provided on tmi-archive.com

For details on how these and other features of the script work, and how to configure it, see comments in the [script file](tmi-archive-bulk-download).

Usage:

```
./tmi-archive-bulk-download
```

The text files [`tmi-archive-bulk-download-dupes`](tmi-archive-bulk-download-dupes), [`tmi-archive-bulk-download-orig-names`](tmi-archive-bulk-download-orig-names), [`tmi-archive-bulk-download-other`](tmi-archive-bulk-download-other) are used by this script.

### [`tmi-archive-bulk-download-attachments`](tmi-archive-bulk-download-attachments)

Download attachments. These are mostly various handouts that were given out during the events.

The attachments are downloaded to an `_attachments` folder in the root directory of this repo. The `_attachments` folder will be created upon running this script.

Furthermore, subfolders in the `_attachments` folder will be created for each of the events. The names of those subfolders correspond to names of the playlists located in the root directory of this repo.

Usage:

```
./tmi-archive-bulk-download-attachments
```

### [`generate-audiobooks`](generate-audiobooks)

This script copies the files listed in an `.m3u` playlist to a separate folder so that those can be, for instance, copied onto a mobile device and played back as an audiobook.

The generated audiobooks are placed into a `_generated_audiobooks` folder located in the root directory of this script.

Subfolders for individual audiobooks will be created, named the same as the playlists they were generated from (minus the `.m3u` extension).

If a playlist has attachments, those are also copied into the audiobook's folder, to a subfolder named `_attachments`. See [`tmi-archive-bulk-download-attachments`](#tmi-archive-bulk-download-attachments) for more information.

Usage:

```
./generate-audiobooks [-y|--overwrite|-n|--no-overwrite] <playlist> ...

-y, --overwrite
  Overwrite the output directory of a playlist if it already exists
-n, --no-overwrite
  Do not overwrite the output directory of a playlist if it already exists

If neither `-y, --overwrite`, nor `-n, --no-overwrite` is given,
a prompt will appear if the output directory of a playlist already exists
```

#### Format of the output filenames

The format of the output filenames is:

```
<track_number>[_<extra_descriptor>][_<tmi_archive_id>]-<filename>
```

Where:

* `<track_number>` is a sequential track number that corresponds to the position of that file in the source playlist. It ensures correct sorting, and thus always contains an appropriate number of leading zeros.

  For example, if a playlist contains 10&ndash;99 tracks, the first ten will be numbered `01`, `02`, `03` &hellip;; if a playlist contains 100&ndash;999 tracks, the tracks will be numbered `001`, `002`, `003`, &hellip;, `010`, `011`, `012`, &hellip; etc.

  If a playlist contains less than 10 tracks, a single leading zero is added: `01`, `02`, `03` &hellip;.

* If a track is an [_extra_](#extras), an `<extra_descriptor>` is included in the filename. It consists of the first four letters of the [extra's `<description>`](#extras-format), capitalized. 

* `<tmi_archive_id>` is an id of the file on the [tmi-archive.com](https://tmi-archive.com/) website ([if the file has one](#tmi-archive-bulk-download-other)). Files on the tmi-archive.com website can be accessed by their id by using this URL template: `https://tmi-archive.com/talk/<id>` 

* `<filename>` is the name of the original file.

### [`playlist-list-by-date`](playlist-list-by-date)

Lists the M3U playlists contained in the root directory of this repo, sorted by recording date. The date information is stored in the [metadata section](#metadata-section) within the playlist files.

The playlists with no date information present are shown at the bottom of the list, sorted alphabetically.

Example output (abbreviated):

```
2009-01-19              Lecture at the University of West #1.m3u
2009-08-28              Lecture at the University of West #2.m3u
2010-01                 Christmas Retreat #1.m3u
2010-01                 What Is Enlightenment_.m3u
...
2015-06-12--14          Living Dharma.m3u
2015-07-03--12          Summer Retreat in Cochise Stronghold #2.m3u
2015-12-27--2016-01-03  Winter Retreat in Cochise Stronghold #2.m3u
2016-07-01--02          Engaged Compassion.m3u
                        Cochise Stronghold.m3u
                        Culadasa's Curriculum.m3u
                        Q&As.m3u
                        Tucson Community Meditation Center + Culadasa's Curriculum.m3u
                        Tucson Community Meditation Center.m3u
                        Unsorted.m3u
                        Uposatha Days.m3u
```

### [`playlist-compare`](playlist-compare)

Usage:

```
./playist-compare <playlist_a> <playlist_b>
```

Higlight recordings in `<playlist_a>` that are also present in the `<playlist_b>`.

E.g. try running this command:

```bash
./playlist-compare "Tucson Community Meditation Center + Culadasa's Curriculum.m3u" "Culadasa's Curriculum.m3u"
```

### `playlist-extra-*`

The scripts whose names begin with `playlist-extra-` filter contents of `.m3u` playlists and output the results to stdout.

<a id='extras-format'></a> Generally, they remove all comment lines, but the comment lines that have the format `# <description> # <filename>` are treated differently. They represent what is referred to here as _extras_.

Those are files that belong to the set of recordings represented by a given playlist, but are commented out from the main playlist for various reasons, such as:

* It is a clip from another file already present in the playlist: `# clip # <filename>` (e.g. see [`Sit, Breathe, Wake Up_.m3u`](Sit,%20Breathe,%20Wake%20Up_.m3u))
* It is an untrimmed version of another file already present in the playlist: `# untrimmed # <filename>` (e.g. see [`Ottawa Palyul Dharma Centre.m3u`](./Ottawa%20Palyul%20Dharma%20Centre.m3u))
* The file is redundant for some other reason, but the decision as to whether or not to remove it hasn't been made yet: `# redundant # <filename>` (e.g. see [`Unsorted.m3u`](Unsorted.m3u))

The [`playlist-extra-exclude`](playlist-extra-exclude) script outputs only the files that are not extras.

The [`playlist-extra-include`](playlist-extra-include) script outputs both extras and non-extras.

The [`playlist-extra-only`](playlist-extra-only) script outputs only those files that are extras.

Usage (same syntax for all three scripts):

```
./playlist-extra-exclude <playlist> ... 
./playlist-extra-include <playlist> ... 
./playlist-extra-only    <playlist> ... 
```

The output of the `playlist-extra-*` scripts can either be fed directly to a media player (e.g. [`mpv`](https://github.com/mpv-player/mpv)) as a new, on-the-fly-generated playlist, or saved to a new file. The scripts can also be used to combine playlists since they can take multiple playlists as arguments.

Some examples:

Play extras from the `Sit, Breathe, Wake Up_.m3u` playlist in mpv:

```bash
./playlist-extra-only 'Sit, Breathe, Wake Up_.m3u' | mpv --playlist=-
```

Save extras from the `Sit, Breathe, Wake Up_.m3u` playlist to a file:

```bash
./playlist-extra-only 'Sit, Breathe, Wake Up_.m3u' > 'Sit, Breathe, Wake Up_ (extras).m3u'
```

Play all non-extras from all _Light on Meditation_ playlists in mpv:

```bash
./playlist-extra-exclude 'Light on Meditation #'* | mpv --playlist=-
```

(`'Light on Meditation #'*` matches all files starting with `'Light on Meditation #'`. Try running `ls 'Light on Meditation #'*` to see all the matches.)

If you'd like to learn how to use mpv player, I also made tutorials about that:

* [mpv player basics walkthrough](https://www.youtube.com/watch?v=b-5XOZpXZMg)
* [Additional mpv player stuff](https://www.youtube.com/watch?v=mGLHKogyDUs)
