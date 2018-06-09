**FFmpeg4Android version 3.22_03**

* By default, it will run the UI edit box command, you can change the command in the edit box, 
  or replace the command in the code (search for commandStr).
  
* Its highly recommended to use the complexCommand (String Array), as it supports all types of commands, and filenames with spaces, ]]
  and special characters.
 
* After running the demo, the out.mp4 will be created in the /sdcard/videokit folder.
 
* Supported devices: this will work on ARMv7 and above devices (most devices today are ARMv7 or above).
  If you use an emulator, make sure you select devices that support ARMv7 or above (e.g Nexus 7).
 
* Note that the Simple Example does not have stop support, so in-case you will not let the operation end, it can interfere with the next
  Transcoding operation.
  
* See Command on [Android Sketchware] (https://androidsketchware.blogspot.com)



**EXAMPLE COMMANDS**
**_Video Compress:_**
```
ffmpeg -y -i /sdcard/videokit/in.mp4 -strict experimental -s 160x120 -r 25 -vcodec mpeg4 -b 150k -ab 48000 -ac 2 -ar 22050 /sdcard/videokit/out.mp4
```


**_compress with h264 (to support chrome)_**
NOTE THAT YOU WILL NEED TO USE THE EXTRAS (SEE BELOW) DISTRIBUTION TO USE THIS COMMAND.
```
ffmpeg -y -i /sdcard/videokit/in.mp4 -strict experimental -vcodec libx264 -preset ultrafast -crf 24 -acodec aac -ar 44100 -ac 2 -b:a 96k -s 320x240 -aspect 4:3 /sdcard/videokit/out3.mp4
```


_As complex command, don't forget to use setCommandComplex(complexCommand)_
_Use this format to support files that contains spaces and special characters_
```
String[] complexCommand = {"ffmpeg","-y" ,"-i", "/sdcard/video kit/in.mp4","-strict","experimental","-s", "160x120","-r","25", "-vcodec", "mpeg4", "-b", "150k", "-ab","48000", "-ac", "2", "-ar", "22050", "/sdcard/video kit/out.mp4"};
```


**_running compression command_**
_while keeping metadata information (including rotation metadata)_
```
ffmpeg -y -i /sdcard/videokit/in.mp4 -strict experimental -map_metadata 0:g -s 160x120 -r 25 -vcodec mpeg4 -b 150k -ab 48000 -ac 2 -ar 22050 /sdcard/videokit/out1.mp4
```

_The parameters that control the quality are the -s (resolution, currently set on 160x120) and the -b (the bitrate, currently set on 150k). Increase them, e.g -s 480x320 And -b 900k To improve quality (and get less compression)_


**Audio Compression**
```
String commandStr = "ffmpeg -y -i /sdcard/vk2/in.wav -ar 44100 -ac 2 -ab 64k -f mp3 /sdcard/videokit/out.mp3";
```

**Audio Trim (Crop)**
```
String commandStr ={"ffmpeg","-y","-i","/storage/emulated/0/vk2/in.mp3","-strict","experimental","-acodec","copy","-ss","00:00:00","-t","00:00:03.000","/storage/emulated/0/videokit/out.mp3"};
```

**Video Rotate (90 degrees cw)**
```
ffmpeg -y -i /sdcard/videokit/in.mp4 -strict experimental -vf transpose=1 -s 160x120 -r 30 -aspect 4:3 -ab 48000 -ac 2 -ar 22050 -b 2097k /sdcard/video_output/out.mp4
```

**Video Crop:**
```
ffmpeg -y -i /sdcard/videokit/short.mp4 -strict experimental -vf crop=100:100:0:0 -s 320x240 -r 15 -aspect 3:4 -ab 12288 -vcodec mpeg4 -b 2097152 -sample_fmt s16 /sdcard/videokit/out.mp4
```

**Extract Picture from Video:**
```
ffmpeg -y -i /sdcard/videokit/in.mp4 -strict experimental -an -r 1/2 -ss 00:00:00.000 -t 00:00:03 /sdcard/videokit/filename%03d.jpg
```

**Extract Sound from Video**
```
ffmpeg -i my_video.mp4 -ab 128k -ac 2 -ar 44100 -vn my_audio.mp3
```
_or_
```
ffmpeg -y -i /sdcard/videokit/in.avi -strict experimental -acodec copy /sdcard/videokit/out.mp3
```
_or_
```
ffmpeg -y -i /sdcard/videokit/in.mp4 -strict experimental -vn -ar 44100 -ac 2 -ab 256k -f mp3 /sdcard/videokit/out.mp3
```

**Re-encode Audio in Video:**
```
ffmpeg -y -i /sdcard/in.mp4 -strict experimental -vcodec copy -acodec libmp3lame -ab 64k -ac 2 -b 1200000 -ar 22050 /sdcard/out.mp4
```

**Change Video Resolution:**
```
ffmpeg -y -i /sdcard/in.mp4 -strict experimental -vf transpose=3 -s 320x240 -r 15 -aspect 3:4 -ab 12288 -vcodec mpeg4 -b 2097152 -sample_fmt s16 /sdcard/out.mp4
```

**Cut time segment from Video:**
```
ffmpeg -ss 00:00:01.000 -y -i /sdcard/videokit/in.mp4 -strict experimental -t 00:00:02.000 -s 320x240 -r 15 -vcodec mpeg4 -b 2097152 -ab 48000 -ac 2 -b 2097152 -ar 22050 /sdcard/videokit/out.mp4
```

**Transcode Audio:**
```
ffmpeg -y -i /sdcard/videokit/big.wav /sdcard/videokit/small.mp3
```


**WaterMark:**
_test with watermark.png 128x128, add it to /sdcard/videokit/_
```
String[] complexCommand = {"ffmpeg","-y" ,"-i", "/sdcard/videokit/in.mp4","-strict","experimental", "-vf", "movie=/sdcard/videokit/watermark.png [watermark]; [in][watermark] overlay=main_w-overlay_w-10:10 [out]","-s", "320x240","-r", "30", "-b", "15496k", "-vcodec", "mpeg4","-ab", "48000", "-ac", "2", "-ar", "22050", "/sdcard/videokit/out.mp4"}; 
```

**Streaming simple:**
Stream from Android device to a PC
* note that all streaming examples will need to add internet permission to the Android project Manifest file ( <uses-permission android:name="android.permission.INTERNET" />  )

_use this command on ffmpeg4android ( 192.168.1.11 is the PC IP)_
```
ffmpeg -i /sdcard/videokit/2.mpg -strict experimental -f mpegts udp://192.168.1.11:8090
```
_You can use any player that supports streaming, on the target machine, to play the stream, in this case we used ffplay_
_(192.168.1.14 is the Android device IP)_
```
ffplay -f mpegts -ast 1 -vst 0 -ar 48000 udp://192.168.1.14:8090 
```


**STREAM RAW VIDEO FROM CAMERA PREVIEW:**
**GETTING THE RAW STREAM FROM THE CAMERA PREVIEW:**

```
Parameters parameters = camera.getParameters();
imageFormat = parameters.getPreviewFormat();
if (imageFormat == ImageFormat.NV21) {
                Camera.Size previewSize = parameters.getPreviewSize();
                frameWidth = previewSize.width;
                frameHeight = previewSize.height;
                Rect rect = new Rect(0, 0, frameWidth, frameHeight);
                YuvImage img = new YuvImage(data, ImageFormat.NV21, frameWidth, frameHeight, null);
                try {
                                outStream.write(data);
                                outStream.flush();
                }
}
```


**ENCODE AND STREAM USING FFMPEG4ANDROID:**
```
"ffmpeg -f rawvideo -pix_fmt nv21 -s 640x480 -r 15 -i " + Environment.getExternalStorageDirectory().getAbsolutePath().toString() + "/yuv.data rtmp://host/stream.flv"
```

**STREAM ON ONE DEVICE, RECEIVE A STEAM ON SECOND DEVICE AND SAVE IT:**
_on the first device:_
```
ffmpeg -i /sdcard/one3.mp4 -f mpegts udp://192.168.0.107:8090
```

_on the second device:_
```
String[] complexCommand = {"ffmpeg","-y" ,"-i", "udp://192.168.0.108:8090","-strict","experimental","-crf", "30","-preset", "ultrafast", "-acodec", "aac", "-ar", "44100", "-ac", "2", "-b:a", "96k", "-vcodec", "libx264", "-r", "25", "-b:v", "500k", "-f", "flv", "/sdcard/videokit/t.flv"};
```
* this needs internet permission in the manifest. 


**H264 encoding (Needs Extras):**
```
ffmpeg -y -i /sdcard/Video/1.MTS -strict experimental -vcodec libx264 -preset ultrafast -crf 24 /sdcard/videokit/out.mp4
```
```
ffmpeg -y -i /sdcard/videokit/m.mkv -strict experimental -vcodec libx264 -preset ultrafast -crf 24 -sn /sdcard/videokit/m2.mkv
```

**Subtitles:**
```
ffmpeg -y -i /sdcard/videokit/m2.mkv -i /sdcard/videokit/in.srt -strict experimental -vcodec libx264 -preset ultrafast -crf 24 -scodec copy /sdcard/videokit/mo.mkv
```
```
ffmpeg -y -i /sdcard/videokit/m2.mkv -i /sdcard/videokit/in.srt -strict experimental -scodec copy /sdcard/videokit/outm3.mkv
```

**Convert Audio file to m4a**
```
ffmpeg -i /sdcard/videokit/in.mp3 /sdcard/videokit/out.m4a
```

**Encode h264 video and aac audio in one command (Needs Extras)**
```
ffmpeg -y -i /sdcard/videokit/in.mp4 -strict experimental -vcodec libx264 -crf 24 -acodec aac /sdcard/videokit/out.mkv
```

**Vintage filter**
```
commandStr = "ffmpeg -y -i /sdcard/videokit/in.mp4 -strict experimental -vf curves=vintage -s 640x480 -r 30 -aspect 4:3 -ab 48000 -ac 2 -ar 22050 -b 2097k -vcodec mpeg4 /sdcard/videokit/curve.mp4";
```

**Black & White filter (Gray Scale):**
```
commandStr = "ffmpeg -y -i /sdcard/videokit/in.mp4 -strict experimental -vf hue=s=0 -vcodec mpeg4 -b 2097152 -s 320x240 -r 30 /sdcard/videokit/out.mp4";
```

**Sepia using colorchannelmixer**
```
String[] complexCommand = {"ffmpeg","-y" ,"-i", "/sdcard/videokit/sample.mp4","-strict", "experimental", "-filter_complex",
"[0:v]colorchannelmixer=.393:.769:.189:0:.349:.686:.168:0:.272:.534:.131[colorchannelmixed];[colorchannelmixed]eq=1.0:0:1.3:2.4:1.0:1.0:1.0:1.0[color_effect]",
 "-map", "[color_effect]","-map", "0:a", "-vcodec", "mpeg4","-b", "15496k", "-ab", "48000", "-ac", "2", "-ar", "22050","/sdcard/videokit/out.mp4"};
```

**Create filter using Photoshop curves **
You can create some effects using Photoshop curves operation (Image, Adjustments, Curves (CNTL + M)), export the curves acv file, and apply it using this command:
```
String[]
complexCommand={"ffmpeg","-y","-i","/storage/emulated/0/vk2/in.mp4","-strict","experimental","-vf","curves=psfile=/storage/emulated/0/videokit/sepia.acv","-b","2097k","-vcodec","mpeg4","-ab","48000","-ac","2","-ar","22050","/storage/emulated/0/videokit/out.mp4"}?
```

**Fade in and out transition**
```
String[] complexCommand = {"ffmpeg","-y" ,"-i", "/sdcard/videokit/in.m4v","-acodec", "copy", "-vf", "fade=t=in:st=0:d=5, fade=t=out:st=20:d=5", "/sdcard/videokit/out.mp4"};
```

**Join 2 files using of the same size using filter_complex**
```
String[] complexCommand = {"ffmpeg","-y","-i", "/sdcard/videokit/in1.mp4", "-i", "/sdcard/videokit/in2.mp4", "-strict","experimental", "-filter_complex", "[0:0] [0:1] [1:0] [1:1] concat=n=2:v=1:a=1", "/sdcard/videokit/out.mp4"};
```


**concat videos with different codecs, and different sizes, different rate and different aspect ratio:**
```
String[] complexCommand = {"ffmpeg","-y","-i","/storage/emulated/0/videokit/sample.mp4",
    "-i","/storage/emulated/0/videokit/in.mp4","-strict","experimental",
    "-filter_complex",
    "[0:v]scale=640x480,setsar=1:1[v0];[1:v]scale=640x480,setsar=1:1[v1];[v0][0:a][v1][1:a] concat=n=2:v=1:a=1",
    "-ab","48000","-ac","2","-ar","22050","-s","640x480","-r","30","-vcodec","mpeg4","-b","2097k","/storage/emulated/0/vk2_out/out.mp4"}
```


**Create a video from pictures**
_note that this commands are sensitive to the pictures size._
_all pictures should be with the same size, and correspond to -s particular video resolution._
_e.g hd video is: 1280x720, and this should be the pictures size_
```
commandStr = "ffmpeg -y -r 1/5 -i /sdcard/videokit/pic00%d.jpg /sdcard/videokit/out.mp4";
```

*with audio. (Works on all players) png also works, pictures size in this case should be 320x240*
```
ffmpeg -y -r 1 -i /sdcard/videokit/pic00%d.jpg -i /sdcard/videokit/in.mp3 -strict experimental -ar 44100 -ac 2 -ab 256k -b 2097152 -ar 22050 -vcodec mpeg4 -b 2097152 -s 320x240 /sdcard/videokit/out.mp4
```


**Advanced filtering**
```
String[] complexCommand = {"ffmpeg","-y" ,"-i", "/sdcard/videokit/in.mp4","-strict","experimental", "-vf", "crop=iw/2:ih:0:0,split[tmp],pad=2*iw[left]; [tmp]hflip[right]; [left][right] overlay=W/2", "-vb", "20M", "-r", "23.956", "/sdcard/videokit/out.mp4"};
```

**Increase video and audio speed**
```
String[] complexCommand = {"ffmpeg","-y" ,"-i", "/sdcard/videokit/in.mp4","-strict","experimental", "-filter_complex", "[0:v]setpts=0.5*PTS[v];[0:a]atempo=2.0[a]","-map","[v]","-map","[a]", "-b", "2097k","-r","60", "-vcodec", "mpeg4", "/sdcard/videokit/out.mp4"};
```

**Overlay 2 videos side by side**
```
     String[] complexCommand = {"ffmpeg","-y" ,"-i", "/sdcard/Movies/sample.mp4","-i", "/sdcard/Movies/sample2.mp4", "-strict","experimental",
                "-filter_complex", 
                "[0:v:0]pad=iw*2:ih[bg];" + 
                "[bg][1:v:0]overlay=w",
                "-s", "320x240","-r", "30", "-b", "15496k", "-vcodec", "mpeg4","-ab", "48000", "-ac", "2", "-ar", "22050",
                "/sdcard/videokit/out.mp4"};  
```

**vintage + watermark**
```
      String[] complexCommand = {"ffmpeg","-y" ,"-i", "/sdcard/Movies/sample.mp4","-strict","experimental",
                "-vf", 
                "movie=/sdcard/videokit/watermark002.png [watermark];" + 
                "[in][watermark] overlay=main_w-overlay_w-10:10 [out_overlay];" +
                "[out_overlay]curves=vintage[out]",  
                "-s", "320x240","-r", "30", "-b", "15496k", "-vcodec", "mpeg4","-ab", "48000", "-ac", "2", "-ar", "22050",
                "/sdcard/videokit/out_water_vinta.mp4"};
```

**Watermark + audio replace**
```
    String[] complexCommand = {"ffmpeg","-y" ,"-i", "/sdcard/Movies/sample.mp4","-i", "/sdcard/videokit/in.mp3", "-strict","experimental",
                "-filter_complex",
                "[1:a]atempo=1.0[a1];" +
                "movie=/sdcard/videokit/watermark002.png [watermark];" + 
                "[0:v][watermark] overlay=main_w-overlay_w-10:10 [outv]",
                "-map", "[outv]", "-map", "[a1]",
                "-s", "320x240","-r", "30", "-b", "15496k", "-vcodec", "mpeg4","-ab", "48000", "-ac", "2", "-ar", "22050",
                "-shortest","/sdcard/videokit/out_water.mp4"};
```

**Looping on a picture and adding audio with audio encoding support**
```
String commandStr = "ffmpeg -y -loop 1 -i /sdcard/videokit/pic001.jpg -i /sdcard/videokit/in.mp3 -strict experimental -s 1270x720 -r 25 -aspect 16:9 -vcodec mpeg4 -vcodec mpeg4 -ab 48000 -ac 2 -b 2097152 -ar 22050 -shortest /sdcard/videokit/out2.mp4";
```

**Replacing video audio track, using the shotest (video or audio)**
```
String[] complexCommand = {"ffmpeg","-y" ,"-i", "/sdcard/videokit/sample.mp4","-i", "/sdcard/videokit/in.mp3", "-strict","experimental",
                "-map", "0:v", "-map", "1:a",
                "-s", "320x240","-r", "30", "-b", "15496k", "-vcodec", "mpeg4","-ab", "48000", "-ac", "2", "-ar", "22050","-shortest","/sdcard/videokit/out.mp4"};
```

**Animated GIF**
**Compress animated gif:**
```
fmpeg -f gif -i /sdcard/videokit/pic1.gif -strict experimental -r 10 /sdcard/videokit/pic1_1.gif
```

**Convert mp4 to animated gif**
```
ffmpeg -y -i /sdcard/videokit/in.mp4 -strict experimental -r 20 /sdcard/videokit/out.gif
```

**Convert animated gif to mp4:**
```
ffmpeg -y -f gif -i /sdcard/videokit/infile.gif /sdcard/videokit/outfile.mp4
```

**Flash Support**
**converting mp4 to swf:**
```
ffmpeg -y -i sdcard/videokit/in.mp4 -strict experimental -acodec mp3 -ab 128000 -ar 44100 -vcodec flv1 -s 352x288 sdcard/videokit/out.swf
```

**converting mp4 to flv (Need extras):**
```
ffmpeg -y -i /sdcard/videokit/in.mp4 -strict experimental -vcodec libx264 -preset ultrafast -crf 24 -acodec aac -ar 44100 -ac 2 -b:a 96k -s 320x240 -aspect 4:3 /sdcard/videokit/out3.flv
```

**Karaoke**
_phase cancellation on a stereo track with centered vocals_
```
{"ffmpeg","-y","-i","/storage/emulated/0/videokit/in.mp3,"-af", "pan=stereo|c0=c0|c1=-1*c1", "-ac","2", "/storage/emulated/0/videokit/karaoke.mp3"}?
```

**EXAMPLE VERRY SMALL**
**HIDE BANNER**
```
ffmpeg -i video.mp4 -hide_banner
```

**Converting video files to different formats**
```
ffmpeg -i video.mp4 video.avi
```
_If you want to preserve the quality of your source video file, use ?-qscale 0? parameter:_
```
ffmpeg -i input.webm -qscale 0 output.mp4
```
**Converting video files to audio files**
```
ffmpeg -i input.mp4 -vn -ab 320 output.mp3
```

**Change resolution of video files**
```
ffmpeg -i input.mp4 -s 1280x720 -c:a copy output.mp4
```
**Compressing audio File**
```
ffmpeg -i input.mp3 -ab 128 output.mp3
```
_The list of various available audio bitrates are:_
* 96kbps
* 112kbps
* 128kbps
* 160kbps
* 192kbps
* 256kbps
* 320kbps

**ARM64-v8a support**

Arm64-v8a is part of the extras (see related section).

Note that although many devices claim they have 64bit support, they don't.
To verify a device does use arm64 architecture, open telnet on the device (or use adb), then run:

getprop ro.product.cpu.abi

if you get: armv7a, then you have 32bit architecture.
if you get: arm64-v8a, then you have 64bit architecture.







_Using complex commands_

A complex command is a command that contains embedded elements (in most cases it uses quotations for that.
Here is an example:
```
ffmpeg -i /sdcard/videokit/in.mp4 -aspect 1:1 -vf split "[main][tmp];[tmp] crop=iw/2:ih:0:0, hflip[tp],[tp] pad=2*iw[left]; [main] crop=iw/2:ih:iw/2:0[right]; [left][right] overlay=W/2" -vb 20M -r 23.956 /sdcard/videokit/outs.mp4
```

To set a complex command you should create a String array and use the method:
setCommandComplex()

Like this:
```
String[] complexCommand = {"ffmpeg","-y" ,"-i", "/sdcard/videokit/mo.mkv","-strict","experimental", "-vf", "crop=iw/2:ih:0:0,split[tmp],pad=2*iw[left]; [tmp]hflip[right]; [left][right] overlay=W/2", "-vb", "20M", "-r", "23.956", "/sdcard/videokit/out.mp4"};

setCommandComplex(complexCommand);
```

Size Tuning - Keeping your app apk small
FFmpeg4Android lib contains a build specifically to tackle this issue - Small App Size.
This build contains a subset of FFmpeg4Android that contains features (filters, coders, encoders, muxers etc), That are the most commonly used - the "essentials build".
This build is great for apps that need to keep small size.
All the performance benefits of the full ffmpeg4android are kept.

  
To use this build, goto ffmpeg_lib libs/armeabi-v7a folder,
simply rename libvideokit.so to libvideokit.so.full, 
 and rename libvideokit.so.ess to libvideokit.so
Refresh the ffmpeg_lib project, and build your project (that use the ffmpeg_lib).
Your project will now use the essentials build, and you will see that the apk created is much smaller in size.

You can also remove the  libs/x86 folder, x86 devices will still work using the Houdini ARM real time translation, of course it will be slower then using the native.

You can also remove the libs/arm64-v8a folder since currently most devices does not support this platform, and all that do have backward compatibility with armeabi-v7a.

Make sure you test and see that all the functionality you need is still there.


Performance Tuning
Prefer mpeg4 codec (-codec mpeg4), since with this codec, ffmpeg4android can utilize all device cores.
Here is an example of command that use mpeg4 codec (in this case this command is doing a video compression):
```
String[] complexCommand = {"ffmpeg","-y" ,"-i", "/sdcard/video kit/in.mp4","-strict","experimental","-s", "160x120","-r","25", "-vcodec", "mpeg4", "-b", "150k", "-ab","48000", "-ac", "2", "-ar", "22050", "/sdcard/video kit/out.mp4"};
```

If its possible, reduce output video resolution (-s) and bitrate (-b), the lower the video quality is, the faster it will take to produce.
Encoding using h264 can't utilize all device cores, and thus is much slower. If you must use it, use the -preset ultrafast flag.
Using the -threads flag can only degrade the performance, as ffmpeg4android automatically detects the number of cores your device has, and set the optimal number of threads for the command used. 


**Extras**
Contains extra functionality that is not part of the standard distribution.
x264 encoding support.
   To use it, download the extras distribution, extract it, it will look like this:
   -- libs
     -- armeabi-v7a
         -- libvideokit.so.full
     
    Copy the libvideokit.so.full the ffmpeg4android_lib/libs to the corresponding architecture folder. Rename it to libvideokit.so, (replacing your current libvideokit.so).
 
x264 encoding support.
   To use it, download the extras distribution, extract it, it will look like this:
   -- libs
     -- armeabi-v7a
         -- libvideokit.so.full
     
    Copy the libvideokit.so.full the ffmpeg4android_lib/libs to the corresponding architecture folder. Rename it to libvideokit.so, (replacing your current libvideokit.so).



**Audio Pack**

 
If Your app needs Audio only support, we have a very small compilation (2.9M), that contain only the audio part of ffmpeg4android.
To use it, simply replace the jniLibs/armeabi-v7a with the one in this zip.




 
 
 

**arm64-v8a**
The same as the x86 support, just add the "abiFilter arm64-v8a"
Note that we don't recommend using this as our tests showed no significant performance improvement
from armeabi-v7a
 
_FFmpeg Command Correctness_
FFmpeg command can be quite complex, and in some cases, getting it right can be hard.
To help you with that, FFmpeg4Android will create a file called vk.log that contains the ffmpeg log in the device demo folder (by default /sdcard/videokit) This log can be very useful when understanding what is wrong with the FFmpeg command, and in most cases it will pinpoint you to the section in the command that is wrong.
If you still don't understand what is wrong with your command, make sure the command works on a windows machine, and you can email us the command for inspection.

Known issues & TroubleShooting
* x264 encoding was moved to the extras, so using this encoder will throw CommandValidationException.
Its recommended to use the mpeg4 encoder instead, its much faster.
If you still need to use the x264 encoding, read the extras section to understand how to add it to your project.


* If you use only armeabi-v7a you must make sure all your other JNI libs are restricted to this architecture,  If not the loader will fail to find the library so file (it will try to find it in x64 folders).
To do it, add this configuration in your module build.gradle (under defaultConfig):
```
ndk {
    abiFilter "armeabi-v7a"
}
```
  