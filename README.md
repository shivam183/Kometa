# Kometa

Collection of custom overlay builders for Kometa provided by JMXD

This fork and particularly this branch is updated to support low quality codecs like AAC/OPUS/FLAC/MP3 - which JMXD has declined to add in the repo.

This local branch will not be merged into main branch (as of now)

## audience_rating.yml

This file will create an overlay showing the current audience rating value in PLEX. Ratings of 8 or higher are green, 6 or higher are yellow and anything lower will be red.

## media_info.yml

![DV Atmos](https://i.imgur.com/CLnpX5j.png)

This file can be used to create an overlay containing media info characteristics of your media. It comes with custom designed images for all combinations of supported formats. This way they can be displayed as one without forced spacing or repeating elements.

Below is an example of what this builder will create.

> [!IMPORTANT]
> For this template to work correctly you MUST use the TRaSH naming convention for your file names.
>
> https://trash-guides.info/Radarr/Radarr-recommended-naming-scheme/#plex


![Overlay Example](https://i.imgur.com/xgEv2Oe.png)

## Currently supported overlay images in all combinations:

| Resolutions | Editions            | Video Formats         | Audio Formats      |
| -           | -                   | -                     | -                  |
| 2160p (4K)  | Anniversary Edition | HDR                   | Dolby Digital Plus |
| 1080p (HD)  | Collector's Edition | HDR10+                | DTS-HD MA          |
|             | Director's Cut      | Dolby Vision          | Dolby Atmos        |
|             | Extended Cut        | Dolby Vision + HDR    | Dolby TrueHD Atmos |
|             | Extended Edition    | Dolby Vision + HDR10+ | FLAC               |
|             | IMAX / (Enhanced)   |                       | AAC                |
|             | Minus Color         |                       | OPUS               |
|             | Special Edition     |                       | MP3                |
|             | Unrated Edition     |                       |                    |

> [!NOTE]
> Dolby Vision with HDR/HDR10+ fallback support is correctly detected and matched separately from exclusive DV, but only Dolby Vision will be visibly shown for those files. See examples at the bottom for an alternative option.

### Available template variables:
----------
#### Exclusive use

`video_only` `audio_only` `resolution_only` `edition_only`

Set to `true` to only apply this specific type of overlay.

#### Disable specific overlays
`use_<<type>>` e.g. `use_audio`

Set to `false` to disable this type of overlay (combined, video, audio, resolution, edition) 

> [!IMPORTANT]
> To disable audio or video you must set `use_combined` to `false` as well.

`use_gradient_top` `use_gradient_bottom`

Set to `false` to disable the backdrops

Any standard template variables are available as well, such as `builder_level`

## Examples
Use it as intended
```yml
overlay_files:
  - file: config/overlays/media_info.yml
  - file: config/overlays/audience_rating.yml
```

Disable audio
```yml
overlay_files:
  - file: config/overlays/media_info.yml
    template_variables:
      use_combined: false
      use_audio: false
  - file: config/overlays/audience_rating.yml
```
Only apply video formats and change the images for these two overlays to also show the fallback format. Then, only show audio, and move it to the right

```yml
overlay_files:
  - file: config/overlays/media_info.yml
    template_variables:
      builder_level: episode
      video_only: true
      use_gradient_top: false
      file_DV-HDR: config/overlays/images/media_info/codec/DV-HDR_extended.png
      file_DV-Plus: config/overlays/images/media_info/codec/DV-Plus_extended.png
  - file: config/overlays/media_info.yml
    template_variables:
      builder_level: episode
      audio_only: true
      use_gradient_top: false
      use_gradient_bottom: false
      horizontal_align: right
```
> [!IMPORTANT]
> When running the overlay file multiple times on the same library like the above example, make sure to disable both gradients on subsequent uses or it will apply them again.

<br /><br />

<img align="left" height="50px" src="https://i.imgur.com/YzGqu5U.png">
<img align="right" height="50px" src="https://i.imgur.com/Ip6nj8m.png">
