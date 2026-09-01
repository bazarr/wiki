# Subtitle Settings

Comprehensive subtitle file management, search optimization, and post-processing.

## Files Tab

Manage how subtitle files are stored, formatted, and processed on disk.

### Subtitle File Options

**Subtitle Folder**

Choose where to store downloaded subtitles:

- **AlongSide Media File** (recommended): Same folder as the video file. Best for media player compatibility and keeping everything organized.
  - Example: `/videos/Movies/Inception/` contains `Inception.mkv` and `Inception.en.srt`

- **Relative Path to Media File**: Store subtitles in a subfolder relative to the video file
  - Example: Subtitles in `./Subs/` subdirectory

- **Absolute Path**: Store all subtitles in a custom directory
  - Must specify the absolute path in "Custom Subtitles Folder"

**Hearing-Impaired Subtitles Extension**

File extension used for hearing-impaired subtitles:

- **.hi** (Hearing-Impaired) - Standard convention
- **.sdh** (Subtitles for the Deaf or Hard-of-Hearing) - More descriptive
- **.cc** (Close Captioned) - Alternative

> [!EXAMPLE]
> With `.sdh` selected, HI English subtitles save as `movie.en.sdh.srt`

**Encode Subtitles To UTF-8**

Re-encode all downloaded subtitles to UTF-8 encoding.

> [!TIP]
> Keep this enabled. Ensures compatibility with all players and systems worldwide. Only disable if you have specific encoding requirements.

**Change Subtitle File Permission After Download (chmod)**

(Unix/Linux/macOS only) Set file permissions on subtitle files after downloading.

> [!EXAMPLE]
> `0755` for world-readable files, `0644` for user/group readable

> [!WARNING]
> Must be a 4-digit octal number. Only works on non-Windows systems.

### Embedded Subtitles Handling

**Enable .strm Support**

Support for `.strm` files (text files containing stream URLs). Bazarr reads the URL and analyzes it for embedded subtitle tracks.

> [!EXAMPLE]
> Useful if you use .strm files to point to external streams instead of local files.

**Treat Embedded Subtitles as Downloaded**

When enabled, Bazarr considers embedded subtitles as satisfying language requirements (treats them as "already downloaded").

When disabled, only external subtitle files count as downloaded.

> [!NOTE]
> **Impact on Searches**
> - **Enabled**: Won't search for external subs if embedded ones exist
> - **Disabled**: Will search for external subs even if embedded ones exist

**Embedded Subtitles Parser**

Choose which tool analyzes video files for embedded subtitles:

- **FFprobe** (default, faster): Part of Bazarr. Good for most cases.
- **mediainfo** (slower, more thorough): Requires separate installation. May provide better results for complex files.

**Ignore Embedded PGS Subtitles**

Ignore PGS (Presentation Graphics Stream) subtitles - bitmap-based subtitles found in Blu-ray rips.

> [!EXAMPLE]
> If you don't want to replace bitmap-based subtitles with text-based ones, enable this.

**Ignore Embedded VobSub Subtitles**

Ignore VobSub subtitles - image-based subtitles from DVD sources.

**Ignore Embedded ASS Subtitles**

Ignore ASS (Advanced SubStation Alpha) subtitles - text format with styling/formatting.

**Show Only Desired Languages**

Hide embedded subtitles for languages NOT in your language profiles.

> [!NOTE]
> Cleaner interface showing only the languages you care about.

---

## Search Tab

Configure subtitle search behavior, quality thresholds, and optimization.

### Upgrading Subtitles

**Upgrade Previously Downloaded Subtitles**

Enable periodic searching for higher-quality versions of already-downloaded subtitles.

**Number of Days to Go Back**

How far back in history to look when upgrading (1-30 days).

> [!EXAMPLE]
> Set to 14 days means Bazarr looks for upgrades for subtitles downloaded in the last 2 weeks.

**Upgrade Manually Downloaded or Translated Subtitles**

When enabled, include manually downloaded and translated subtitles in upgrade searches.

---

### Search Scores

**Minimum Score For Episodes**

- **Range**: 0-100%
- Episodes only download subtitles automatically if they score at least this percentage

**Minimum Score For Movies**

- **Range**: 0-100%
- Movies only download subtitles automatically if they score at least this percentage

> [!NOTE]
> **Scoring Guide**
> - **90-100%**: Strict matching, only best quality matches downloaded
> - **75-90%**: Balanced approach, good quality matches
> - **50-75%**: Lenient, more results but potentially lower quality
> - **Below 50%**: Not recommended, high risk of wrong or very poor subtitles

**Why Score Matters**: Scores are calculated based on:

- Exact filename matching
- Release name matching
- Uploader reputation
- Provider verification
- Other quality indicators

---

### Performance / Optimization

**Adaptive Searching**

Skip searching providers for subtitles that have been searched for recently. After an initial search window, only search periodically.

> [!NOTE]
> **Purpose**
> - Reduces API calls to subtitle providers
> - Improves search speed
> - Saves bandwidth
> - Good for high-traffic instances

**Delay Before Adaptive Search Takes Effect**

Time window after first search during which Bazarr continues searching even if recent.

- **Default**: 3 weeks
- **Options**: 1-4 weeks

**Delay Between Adaptive Searches**

How frequently Bazarr re-searches in adaptive mode after initial window.

- **Default**: 1 week
- **Options**: 3 days - 4 weeks

> [!EXAMPLE]
> - Delay: 3 weeks, Delta: 1 week
> - First search on Day 1
> - Adaptive searching starts Day 22 (3 weeks later)
> - Searches again on Day 29, Day 36, etc. (every 1 week)

**Search Enabled Providers Simultaneously**

Search all enabled providers in parallel instead of sequentially.

> [!NOTE]
> - **Pros**: Faster overall search time
> - **Cons**: Higher CPU usage, more simultaneous network requests
> - **Recommendation**: Disable on low-powered devices (Raspberry Pi, NAS, older systems)

**Skip Video File Hash Calculation**

Skip computing video file hashes during searches.

> [!NOTE]
> - **Pros**: Faster searches, prevents waking sleeping hard drives
> - **Cons**: May reduce subtitle matching quality/accuracy
> - **Use When**: Running on NAS with spinning drives or slow storage

---

## Processing Tab

Automatic subtitle modification, synchronization, and post-processing.

### Whisper As Fallback

Automatic AI-based subtitle generation using OpenAI Whisper when provider searches fail.

**Use Whisper as Fallback for Automated Searches**

When no provider reaches the minimum score, Bazarr generates subtitles using Whisper.

> [!NOTE]
> **Applies to**
> - Scheduled automated searches
> - Search for Missing Subtitles tasks
> - **NOT** to manual single searches

**Use Whisper as Fallback for Single Series Searches**

Extend Whisper fallback to manual searches from the Wanted menu.

> [!WARNING]
> Can significantly increase processing time for single searches.

### Sub-Zero Subtitle Content Modifications

Automatically fix and enhance downloaded subtitles. Modifications happen immediately after download:

**Hearing Impaired**

Remove hearing impaired tags and descriptions like:

- `[DOOR SLAMS]`, `[PHONE RINGS]`, `[THUNDER]`
- HD audio indicators
- Hearing impaired metadata

> [!NOTE]
> Cleaner subtitles for those who don't need these cues.

**Remove Tags**

Remove all style and formatting tags:

- Colors, bold, italic, underline
- Font changes
- Size modifications
- All ASS/SSA formatting

> [!NOTE]
> Plain text subtitles for maximum compatibility.

**Remove Emoji**

Strip emoji characters from subtitles.

> [!EXAMPLE]
> Some players don't render emoji well or clutter subtitles.

**OCR Fixes**

Fix common errors from OCR (optical character recognition) conversion:

- When bitmap subtitles are converted to text
- Common OCR mistakes (e.g., "l" vs "1", "O" vs "0")

**Common Fixes**

Fix general text issues:

- Whitespace and punctuation problems
- Common typos
- Line breaks

**Fix Uppercase**

Convert all-uppercase subtitles to proper case:

- `THE QUICK BROWN FOX` → `The Quick Brown Fox`
- Makes uppercase subtitles readable

**Color**

Add color to subtitles. Options:

- White, Light Gray, Red, Green, Yellow, Blue, Magenta, Cyan
- Black, Dark Red, Dark Green, Dark Yellow, Dark Blue, Dark Magenta, Dark Cyan, Dark Grey

> [!NOTE]
> Only works with players supporting color tags (ASS/SSA format).

**Reverse RTL**

Reverse punctuation in right-to-left (RTL) language subtitles:

- For problematic playback devices
- Fixes RTL language rendering issues

---

### Audio Synchronization

Automatic timing alignment using ffsubsync (machine learning-based synchronization).

**Enable Automatic Subtitles Audio Synchronization**

Automatically sync subtitle timing to audio after download.

> [!WARNING]
> Significantly increases processing time (can take minutes per subtitle). Disable if system is slow or has limited resources.

**Series Score Threshold For Audio Sync**

Only auto-sync episode subtitles scoring **below** this threshold.

> [!EXAMPLE]
> Threshold 80 means sync only subtitles scoring below 80%, likely misaligned.

**Movies Score Threshold For Audio Sync**

Only auto-sync movie subtitles scoring **below** this threshold.

**Synchronization Reference**

Choose what ffsubsync uses to align subtitles:

- **Use Audio Track as Reference** (better quality, slower): Analyzes audio to find timing. More accurate but resource-intensive.
- **Use Embedded Subtitles as Reference** (faster): Uses existing embedded subtitles as timing reference. Faster but less accurate if embedded subtitles are wrong.

**Prefer Original Language Audio Track**

Use the show/movie's original language audio track (from Sonarr/Radarr metadata) as sync reference.

> [!NOTE]
> - **Benefit**: Accurate syncing for original language audio
> - **Fallback**: Uses default audio track if original language not available

**Do Not Fix Framerate Mismatch**

Skip framerate correction during synchronization.

> [!EXAMPLE]
> If you know framerate isn't mismatched, can speed up sync.

**Golden-Section Search**

Use advanced mathematical search to find optimal framerate ratio.

> [!NOTE]
> Better results for framerate-mismatched content but slower processing.

**Max Offset Seconds**

Maximum timing offset allowed for any subtitle segment.

- **Options**: 60, 120, 300, 600 seconds
- **Default**: 60 seconds

> [!EXAMPLE]
> If set to 60, no subtitle will be offset more than 60 seconds from original timing.

**Generate Debug File Instead of Synchronizing**

Create debug files (.tar.gz) for ffsubsync issue reporting instead of actually syncing.

> [!EXAMPLE]
> When opening issues on ffsubsync GitHub to help developers debug problems.

---

### Custom Post-Processing

Execute custom scripts or commands after downloading subtitles.

**Enable Custom Post-Processing**

Activate custom command execution after each subtitle download.

**Series Score Threshold For Post-Processing**

Only post-process episode subtitles scoring **below** this threshold.

**Movies Score Threshold For Post-Processing**

Only post-process movie subtitles scoring **below** this threshold.

**Post-Processing Command**

The script or binary to execute. Available variables include:

| Variable                         | Description                                        |
|----------------------------------|----------------------------------------------------|
| `{directory}`                    | Full path of episode/movie file parent directory   |
| `{episode}`                      | Full path of episode/movie file                    |
| `{episode_name}`                 | Filename without directory or extension            |
| `{subtitles}`                    | Full path of the subtitles file                    |
| `{subtitles_language}`           | Language of subtitles (may include :hi or :forced) |
| `{subtitles_language_code2}`     | 2-letter language code (e.g., `en`, `en:hi`)       |
| `{subtitles_language_code2_dot}` | 2-letter code with dot separator (e.g., `en.hi`)   |
| `{subtitles_language_code3}`     | 3-letter language code (e.g., `eng`)               |
| `{subtitles_language_code3_dot}` | 3-letter code with dot separator                   |
| `{episode_language}`             | Audio language of the media file                   |
| `{episode_language_code2}`       | 2-letter audio language code                       |
| `{episode_language_code3}`       | 3-letter audio language code                       |
| `{score}`                        | Subtitle quality score                             |
| `{subtitle_id}`                  | Provider ID of the subtitle                        |
| `{provider}`                     | Provider that supplied the subtitle                |
| `{uploader}`                     | Uploader of the subtitle file                      |
| `{release_info}`                 | Release info for the subtitle file                 |
| `{series_id}`                    | Sonarr series ID (empty for movies)                |
| `{episode_id}`                   | Sonarr episode ID or Radarr movie ID               |

> [!EXAMPLE]
> ```bash
> python /scripts/fix_subtitles.py "{subtitles}" "{episode_language_code2}"
> bash /usr/local/bin/post-process.sh "{subtitles}" "{episode}"
> ```

> [!WARNING]
> Your command cannot start or end with quotes. Append `2>&1` to capture output.
