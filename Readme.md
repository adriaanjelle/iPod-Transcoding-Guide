![](https://github.com/adriaanjelle/ipod-video-guide/blob/main/ipod-video-guide.jpg)

A guide for encoding movies for the iPod Video and Classic.

## Supported video formats
### iPod (5th generation)
- H.264 video: up to 768 Kbps, **320 x 240**, 30 frames per sec., **Baseline** Profile up to **Level 1.3** with AAC-LC up to 160 Kbps, 48 Khz, stereo audio in .m4v, .mp4 and .mov file formats
- MPEG-4 video: up to 2.5 mbps, 480 x 480, 30 frames per sec., Simple Profile with AAC-LC up to 160 Kbps, 48 Khz, stereo audio in .m4v, .mp4 and .mov file formats

### iPod Classic (6th generation)
- H.264 video, up to 1.5 Mbps, 640 by 480 pixels, 30 frames per second, Low-Complexity version of the H.264 Baseline Profile with AAC-LC audio up to 160 Kbps, 48kHz, stereo audio in .m4v, .mp4, and .mov file formats
- H.264 video, up to 2.5 Mbps, **640 by 480** pixels, 30 frames per second, **Baseline** Profile up to **Level 3.0** with AAC-LC audio up to 160 Kbps, 48kHz, stereo audio in .m4v, .mp4, and .mov file formats
- MPEG-4 video, up to 2.5 Mbps, 640 by 480 pixels, 30 frames per second, Simple Profile with AAC-LC audio up to 160 Kbps, 48kHz, stereo audio in .m4v, .mp4, and .mov file formats

----

## Prologue
I've spent the last three days trying to find a way to properly transcode video for the iPod Classic. First, I tried just using the latest version of Handbrake, but the audio had massive popping and screeching noises, totally unusable. Then I tried encoding using an older version of Handbrake but that is slow and the audio quality was pretty bad, but usable.

The guide here is the best method I have come up with to transcode video for the iPod (Classic).

## Step 1: Extraction
First, make sure you have [MKVToolnix](https://mkvtoolnix.download/downloads.html#windows) installed. Then, download [gMKVExtractGUI](https://sourceforge.net/projects/gmkvextractgui/) and load your .MKV file into it. Select only the desired audio track and hit 'Extract'.

![](https://github.com/adriaanjelle/ipod-video-guide/blob/main/Screenshots/Screenshot%202024-04-29%20132654.png)

## Step 2: Conversion
Now, download [foobar2000](https://www.foobar2000.org/download) in 32-bit. This is essential for the DTS plugin to work, which you can download and install [here](https://www.foobar2000.org/components/view/foo_input_dts). Make sure you install the [encoder pack](https://www.foobar2000.org/encoderpack) and [iTunes](https://www.microsoft.com/store/productId/9PB2MZ1ZMB1S?ocid=pdpshare) as well. Once, you're set (just hit 'okay' on everything), drag the .DTS file into foobar, and right click the audio file and go to the converter dialog window.

![](https://github.com/adriaanjelle/ipod-video-guide/blob/main/Screenshots/Screenshot%202024-04-29%20130000.png)

Now, we want to convert this file to AAC CBR 160 Kb/s. Make sure the 'Use HE-AAC' box is unchecked, as this is not compatible with the iPod. Finally, add the 'Downmix channels to stereo' DSP to the Processing tab. Save the preset if you want to repeat this process for other movies.

![](https://github.com/adriaanjelle/ipod-video-guide/blob/main/Screenshots/Screenshot%202024-04-29%20143903.png)

Hit 'Convert' and voilà, you should now have a stereo audio track in 160 Kb/s AAC for your movie that works on the iPod. In my case it reported an error ('garbage') at a certain timestamp, but this turned out to be the exact length of the movie. So we won't have any issues. This method uses Apple's AAC encoder, which is known to be the best one available.

## Step 3: Merge
We need to merge the audio and the video back together, however. Open the MKVToolnix GUI and drag your .MKV file into the window. Now, drag the AAC audio file we've just created into the window as well, making sure you use the 'Add as new source files to the current multiplex settings' option.

Deselect everything but the video, audio (the one we've created with foobar2000) and the desired subtitle track. See the image below for an example.

![](https://github.com/adriaanjelle/ipod-video-guide/blob/main/Screenshots/Screenshot%202024-04-29%20143952.png)

Now hit 'Start multiplexing'. This should take less than a minute if you're using a fast SSD.

## Step 4: Handbrake
Open the new .MKV file we've just created with MKVToolnix in Handbrake, using the exact same settings as we tried in method 1. The only difference here is that we want the audio to be set to 'AAC Passthru' to avoid any re-encoding of the audio by Handbrake.

- **Summary**: Make sure to only enable 'iPod 5G Support' on this page.
- **Dimensions**:
  - Cropping: None
  - Resolution Limit: Custom, **320x240** (5th) or **640x480** (6th)
  - Anamorphic: None
- **Video**: You can choose the options you want here, depending on how long you want the encoding process to take and the quality you want to get.
  - Video Encoder: H.264 (prefferably AMD VCE, NVIDIA NVENC or Intel QSV for faster encoding. DO NOT use VCE or NVENC if you want to transcode for the 5th gen iPod. Any of the options should work for the 6th gen iPod Classic.)
  - Quality: Constant Quality 20 (increase this if you want for higher quality, but it will increase file size)
  - Framerate: Same as source (set this to 30 if the source is higher than 30)
  - Encoder Preset: Slower/Quality
  - Encoder Profile: Baseline
  - Encoder Level: **1.3** (5th) or **3.0** (6th)
  - Advanced Options (5th only): `cabac=0:ref=1` for CPU encoding or `coder=cabac:num-ref-frame=1:gop-ref-dist=1:gop-pic-size=30` for QSV encoding.
- **Audio**: **AAC Passthru** (this is very important)
- **Subtitles**: Select the desired track, and check the 'Burn-in' box if you want to display the subtitles in the video itself. This is entirely optional, however. Note that the iPod does not support subtitles natively, so burn-in is the only way.
