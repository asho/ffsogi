# ffsogi
An easy script to encode videos into mobile phones.  Hardware/Software encode support!

# Components
## ffsogi
Software encoder version.

## ffsogi_va
AMD/Intel VAAPI encoder version.

## ffsogi_nv
Nvidia nvenc encoder version.

## ffsogi_qsv
Intel QuickSync encoder version. Feature todo. Try ffsogi_va instead.

# Program arguments.
## ffsogi
usage:

-i [input filename]

-o [output filename]

-c [value]. crf value. default 24. slower, quality better, filesize bigger.

-f [codec format]. can be h264(avc), h265(hevc)(default), av1.

-s [4|7]. scale to [4]80p or [7]20p

-j [threads]. specify how many cores to encode.

--ab [value]: audio bitrate. default 64.

--vb|-b [value]: video bitrate

--audio-loud: make audio volume louder in file.

--16: transcode video to 16:9 aspect.

--sub: [subtitle filename]. Embedded subtitle into video.

--preset [medium|slow|slower]: set encoder preset mode. default is medium.

--ffmpeg_opt [options]: specify ffmpeg options.

--vcopy: copy video track only. no transcoding.

--acopy: copy audio track only. no transcoding.

--deint: deinterlance video while transcoding.

--hdr2sdr: convert HDR image to SDR. 4K BD to 1080.

--nocc: no color conversion. keep bt2020 when downscale from UHD to HD.

## ffsogi_va/nv
-c [val]: CRF value. CRF encoding mode (default).

-b|--vb [bitrate]: Video bitrate value. VBR encoding mode. conflict with CRF.

-i [file]: input file to encode.

-o [file]: output file.

-s [width]: width to scale down. special value 7 for 720p, 4 for 480p.
            ffsogi calculates the best height by w/h ratio of input source.

-8: specify output to 8bit colorspace. Useful with hevc10 and avc10 to convert to 8bit.
    Some devices don't support 10bit. Better convert films to 8bit.

-f [format]: specify output film format. h264(avc), h265(hevc)(default), av1.

--ab [bitrate]: default value is 64k for AAC.

--acopy: copy the audio track without encoding. Output file may be mkv for best compatibility

--hw: Use hardware decoder to decode source file.
      ffsogi always use hardware encoder to encode frames. However the source bitstream can be fed by
      software(cpu) decode or hardware decode. If the hardware support the codec of input file, it is best
      to use hardware decode for efficiency. Otherwise use software decode for compatibility but slow performance.

--audio-loud: To amplify audio volume. Conflict with --acopy.

--de: deinterlace mode. Not useful now.

--ffopt [options]: Append ffmpeg options that ffsogi not support.

# Use Cases
## Transcode 1080p video to hevc 8bit and downscale to 720p.
ffsogi -i "input.mkv" -o output.mp4 -s 7 -c 24

ffsogi_va -i "input.mkv" -o output.mp4 -s 7 -c 24 (For AMD/Intel)

ffsogi_nv -i "input.mkv" -o output.mp4 -s 7 -c 24 (For Nvidia)

## Transcode 1080p video to avc 8bit and downscale to 720p.
ffsogi -i "input.mkv" -o output.mp4 -s 7 -c 24 -f h264

ffsogi_va -i "input.mkv" -o output.mp4 -s 7 -c 24 -f h264 (For AMD/Intel)

ffsogi_nv -i "input.mkv" -o output.mp4 -s 7 -c 24 -f h264 (For Nvidia)

## Transcode 1080p video to av1 8bit and downscale to 720p.
ffsogi -i "input.mkv" -o output.mp4 -s 7 -c 24 -f av1

**_NOTE:_** no hardware encode version. Need newer graphic card and ffmpeg.

## Transcode 4K HDR video to hevc 8bit SDR and downscale to 1080p.
ffsogi -i "input.mkv" -o output.mp4 -s 1080 -c 24 --hdr2sdr

**_NOTE:_** no hardware encode version.

## Transcode 1080 video to hevc 8bit and keep audio track.
ffsogi -i "input.mkv" -o output.mkv -c 24 --acopy

ffsogi_va -i "input.mkv" -o output.mkv -c 24 --acopy (For AMD/Intel)

ffsogi_nv -i "input.mkv" -o output.mkv -c 24 --acopy (For Nvidia)

