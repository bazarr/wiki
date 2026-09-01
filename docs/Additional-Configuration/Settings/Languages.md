# Language Settings

Configure subtitle languages, create language profiles, and manage language mappings.

## Selection Tab

Configure which languages are available and how Bazarr handles unknown language tracks.

### Subtitles Language

**Single Language**

Download subtitles without language codes in the filename (e.g., `movie.srt` instead of `movie.en.srt`).

> [!WARNING]
> Only enable if your media player doesn't support language codes in filenames. Results may vary. Not recommended.

**Languages Filter**

Select which languages are available in Bazarr. This filters dropdown menus throughout the UI to show only your enabled languages.

> [!TIP]
> You can start typing to search for languages quickly.

### Embedded Tracks Language

**Deep Analyze Media Files for Audio Tracks**

When enabled, Bazarr scans media files to detect audio track languages using ffprobe or mediainfo. This is more accurate but slower.

**Treat Unknown Language Audio Tracks As**

If Bazarr can't identify an audio track's language, assume it is this language. **Triggers missing subtitles recalculation when changed.**

> [!EXAMPLE]
> If all your videos have undefined audio tracks that are actually English, set this to English.

**Treat Unknown Embedded Subtitles As**

If Bazarr can't identify an embedded subtitle's language, assume it is this language. **Triggers full subtitles indexing when changed.**

> [!EXAMPLE]
> Useful if your embedded subtitles have no language metadata but are in a known language.

---

## Mappings Tab

Create custom language mappings to accept subtitles reported as one language as another canonical language.

### Language Mappings

> [!EXAMPLE]
> If a provider reports subtitles as "Simplified Chinese" but you want them treated as "Chinese", create a mapping from Simplified Chinese to Chinese.

**How Mappings Work**:

- Mappings are **one-way** (A → B doesn't mean B → A)
- Mappings apply **globally** to all searches
- Useful for non-standard or misspelled language codes from providers

> [!NOTE]
> You must have at least one enabled language in the Selection tab before using mappings.

---

## Profiles Tab

Create and manage language profiles that define automatic subtitle selection rules.

Each profile consists of:

1. **Profile Name**: A descriptive name (e.g., "English + French", "Spanish Only")

2. **Languages**: The languages to include. For each language, configure:
   - **Forced Subtitles**: Include forced subtitles (subtitles appearing when characters speak foreign languages or for text in scenes)
   - **Hearing Impaired (HI)**: Include hearing-impaired subtitles for accessibility
   - **Exclude Audio**: Don't download subtitles for this language if the audio matches it

3. **Cutoff Language**: (Optional) Stop searching other languages once this language is found

> [!EXAMPLE]
>
> - Profile: `English, Dutch, German, French` with cutoff `Dutch`
> - If Dutch found → Download Dutch and stop searching
> - If Dutch not found → Continue searching German and French
> - English only downloaded if nothing else matches

### Tag-Based Automatic Language Profile Selection

Assign language profiles to shows/movies automatically based on Sonarr/Radarr tags.

**Enable for Series** / **Enable for Movies**

Toggle to enable tag-based profile assignment.

**How It Works**:

1. Bazarr checks tags assigned to a series/movie in Sonarr/Radarr
2. Finds the **FIRST** tag that exactly matches a Bazarr language profile tag
3. Assigns that profile to the series/movie

> [!WARNING]
> If multiple tags match, there's no guarantee which one is used. Choose tag names carefully to avoid ambiguity.

**Remove Profile Tags**

Enter tags that, when present on a show/movie, **remove** any assigned language profile.

> [!EXAMPLE]
> Tag a show "no-subtitles" and add it to Remove Profile Tags. Shows with this tag won't have a profile applied, even if another tag matches a profile.

<!-- -->
> [!NOTE]
> **Conflict Resolution**: If a show has both a matching profile tag and a removal tag, the **removal tag takes priority**.

### Default Language Profiles for Newly Added Content

Automatically apply a language profile to new content added to Bazarr.

**Series Default Setting**

Enable to automatically assign a language profile to all new TV shows added to Bazarr.

> [!NOTE]
> Only applies to shows added AFTER enabling this option. Existing shows are not affected.

**Movies Default Setting**

Enable to automatically assign a language profile to all new movies added to Bazarr.

> [!NOTE]
> Only applies to movies added AFTER enabling this option. Existing movies are not affected.
