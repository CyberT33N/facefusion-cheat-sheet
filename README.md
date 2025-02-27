# facefusion-cheat-sheet



### facefusion
- https://github.com/facefusion/facefusion

<details><summary>Click to expand..</summary>

  
<br><br>

#### Install

<br><br>

##### Ubuntu
```
cd ~/Projects
git clone https://github.com/facefusion/facefusion
cd facefusion

pyenv local 3.10.13
pyenv shell 3.10.13

python3.10 -m venv venv
source venv/bin/activate

sudo apt install ffmpeg

python install.py

python run.py
```

<br><br>

#### Disable NSFW Filter
- facefusion/content_analyser.py
  - Replace this:
  ```
  def analyse_frame(frame : Frame) -> bool:
  content_analyser = get_content_analyser()
  frame = prepare_frame(frame)
  probability = content_analyser.run(None,
  {
    'input:0': frame
  })[0][0][1]
  return probability > MAX_PROBABILITY
  ```

  with:
  ```
  def analyse_frame(frame : Frame) -> bool:
  return False
  ```


<br><br>
<br><br>

#### Guide

<br><br>

##### Video

<br><br>

###### Frame Processors
- Select Face Enhancer (optional) <- Takes 
  - Basicly upscale

- Select Frame Enhancer (optional) <- Takes longer
  - Frame quality will icnrease

- Select Face Swapper


<br><br>

###### Face Settings
- Face Enhancer Model

- Face Swapper Model
  - inswapper_128 & inswapper_128_fp16 are the best
    - inswapper_128_fp16 is faster but maybe lower quality

- Face Enhancer Blend
  - The lower the more related to the source image and the higher the smoother

- Frame Enhancer Model
  - It is basicly upsacle so the higher the bigger the resolution

- Frame Enhancer Blend
  - The higher you set it the moe it get blurred

<br><br>

###### Execution 
- Enable CPU and CUDA

- Execution Thread Count
  - Run nproc on ubuntu to detect your threads

<br><br>

###### Memory
- Video Memory Strategy
  - stict is best for quality

- System Memory Limit
  - You RAM
    - Just set it to 0

<br><br>

###### Output
- video preset
  - veryslow is best for quality
  - ultrafast is best for performance
  - 
- video encoder
  - For Quality:
    - libx265 (HEVC): libx265, also known as HEVC (High-Efficiency Video Coding), typically offers the best quality-to-file size ratio compared to other codecs like libx264 and libvpx-vp9. It's especially efficient at encoding high-resolution and high-bitrate videos while maintaining excellent visual quality. However, encoding with libx265 tends to be slower than other options due to its higher computational complexity.

    - libx264 (H.264): While not as efficient in terms of compression as libx265, libx264 (H.264) is still widely used and offers excellent quality for most applications. It's a mature codec with broad compatibility across devices and platforms. Encoding with libx264 is generally faster than libx265, making it a preferred choice for real-time or high-volume encoding scenarios.
  
  - For Performance:
    - h264_nvenc: This encoder utilizes NVIDIA's NVENC hardware encoder for H.264 encoding. It offers excellent performance and speed, especially on systems equipped with NVIDIA GPUs. h264_nvenc can significantly accelerate the encoding process, making it ideal for real-time encoding and applications that require high throughput.

    - hevc_nvenc: Similar to h264_nvenc, hevc_nvenc utilizes NVIDIA's NVENC hardware encoder but for HEVC (H.265) encoding. It provides fast encoding speeds for HEVC content, leveraging the hardware acceleration capabilities of NVIDIA GPUs. However, encoding with hevc_nvenc may require more recent and powerful NVIDIA GPU models compared to H.264 encoding.


<br><br>
<br><br>

#### Performance (nvidia t4) - 9 sekunden video

### frame enhancer 2x + face enhancer + video quali 100
- 1081.38 seconds

### face enhancer + video quali 100
- 90 seconds

### no enhancer + video quali 100
- 34 seconds




## CLI

```
options:
  -h, --help                                                                                                             show this help message and exit
  -s SOURCE_PATHS, --source SOURCE_PATHS                                                                                 choose single or multiple source images or
                                                                                                                         audios
  -t TARGET_PATH, --target TARGET_PATH                                                                                   choose single target image or video
  -o OUTPUT_PATH, --output OUTPUT_PATH                                                                                   specify the output file or directory
  -v, --version                                                                                                          show program's version number and exit

misc:
  --skip-download                                                                                                        omit automate downloads and remote lookups
  --headless                                                                                                             run the program without a user interface
  --log-level {error,warn,info,debug}                                                                                    adjust the message severity displayed in
                                                                                                                         the terminal

execution:
  --execution-providers EXECUTION_PROVIDERS [EXECUTION_PROVIDERS ...]                                                    accelerate the model inference using
                                                                                                                         different providers (choices: tensorrt,
                                                                                                                         cuda, cpu, ...)
  --execution-thread-count [1-128]                                                                                       specify the amount of parallel threads
                                                                                                                         while processing
  --execution-queue-count [1-32]                                                                                         specify the amount of frames each thread
                                                                                                                         is processing

memory:
  --video-memory-strategy {strict,moderate,tolerant}                                                                     balance fast frame processing and low vram
                                                                                                                         usage
  --system-memory-limit [0-128]                                                                                          limit the available ram that can be used
                                                                                                                         while processing

face analyser:
  --face-analyser-order {left-right,right-left,top-bottom,bottom-top,small-large,large-small,best-worst,worst-best}      specify the order in which the face
                                                                                                                         analyser detects faces.
  --face-analyser-age {child,teen,adult,senior}                                                                          filter the detected faces based on their
                                                                                                                         age
  --face-analyser-gender {female,male}                                                                                   filter the detected faces based on their
                                                                                                                         gender
  --face-detector-model {retinaface,yoloface,yunet}                                                                      choose the model responsible for detecting
                                                                                                                         the face

Yoloface: YOLO (You Only Look Once) ist ein schnelles Objekterkennungsmodell, das für Echtzeitanwendungen optimiert ist. Es zeichnet sich durch seine Geschwindigkeit aus, kann jedoch in der Genauigkeit im Vergleich zu anderen Modellen etwas geringer sein.

Retinaface: RetinaFace ist ein präzises und robustes Modell für die Gesichtserkennung, das speziell für die Erkennung von Gesichtern in verschiedenen Skalierungen, Rotationen und Haltungen entwickelt wurde. Es ist bekannt für seine hohe Genauigkeit und gute Leistung unter verschiedenen Bedingungen.

Yunet: Yunet ist möglicherweise weniger verbreitet und bekannt im Vergleich zu YOLO und RetinaFace. Die Genauigkeit und Leistung dieses Modells kann stark von der spezifischen Implementierung und den Schulungsdaten abhängen.


  --face-detector-size FACE_DETECTOR_SIZE                                                                                specify the size of the frame provided to
                                                                                                                         the face detector
  --face-detector-score [0.0-1.0]                                                                                        filter the detected faces base on the
                                                                                                                         confidence score

face selector:
  --face-selector-mode {reference,one,many}                                                                              use reference based tracking with simple
                      
reference: In diesem Modus wird ein Referenzgesicht ausgewählt, und das System sucht nach diesem spezifischen Gesicht in allen Frames des Zielvideos. Dieser Modus eignet sich gut, wenn Sie ein bestimmtes Gesicht verfolgen möchten, beispielsweise für die Anwendung von Effekten oder Filtern auf dieses Gesicht.

one: Im "one"-Modus wird nur ein einziges Gesicht aus dem Zielvideo ausgewählt. Dieses Gesicht wird wahrscheinlich das dominante oder zentrale Gesicht im Video sein. Es ist nützlich, wenn Sie nur an einem Hauptgesicht interessiert sind und die anderen Gesichter im Video vernachlässigen möchten.

many: Der "many"-Modus wählt mehrere Gesichter aus dem Zielvideo aus. Dies kann hilfreich sein, wenn Sie Interesse an der Analyse oder Verarbeitung mehrerer Gesichter haben, beispielsweise für die Erstellung von Statistiken über verschiedene Personen in einem Video.
                                                                                                                         matching
  --reference-face-position REFERENCE_FACE_POSITION                                                                      specify the position used to create the
                                                                                                                         reference face
  --reference-face-distance [0.0-1.5]                                                                                    specify the desired similarity between the
                                                                                                                         reference face and target face
  --reference-frame-number REFERENCE_FRAME_NUMBER                                                                        specify the frame used to create the
                                                                                                                         reference face

face mask:
  --face-mask-types FACE_MASK_TYPES [FACE_MASK_TYPES ...]                                                                mix and match different face mask types
                                                                                                                         (choices: box, occlusion, region)
  --face-mask-blur [0.0-1.0]                                                                                             specify the degree of blur applied the box
                                                                                                                         mask
  --face-mask-padding FACE_MASK_PADDING [FACE_MASK_PADDING ...]                                                          apply top, right, bottom and left padding
                                                                                                                         to the box mask
  --face-mask-regions FACE_MASK_REGIONS [FACE_MASK_REGIONS ...]                                                          choose the facial features used for the
                                                                                                                         region mask (choices: skin, left-eyebrow,
                                                                                                                         right-eyebrow, left-eye, right-eye, eye-
                                                                                                                         glasses, nose, mouth, upper-lip, lower-
                                                                                                                         lip)

frame extraction:
  --trim-frame-start TRIM_FRAME_START                                                                                    specify the the start frame of the target
                                                                                                                         video
  --trim-frame-end TRIM_FRAME_END                                                                                        specify the the end frame of the target
                                                                                                                         video
  --temp-frame-format {bmp,jpg,png}                                                                                      specify the temporary resources format
  --temp-frame-quality [0-100]                                                                                           specify the temporary resources quality
  --keep-temp                                                                                                            keep the temporary resources after
                                                                                                                         processing

output creation:
  --output-image-quality [0-100]                                                                                         specify the image quality which translates
                                                                                                                         to the compression factor
  --output-video-encoder {libx264,libx265,libvpx-vp9,h264_nvenc,hevc_nvenc}                                              specify the encoder use for the video
                                                                                                                         compression
  --output-video-preset {ultrafast,superfast,veryfast,faster,fast,medium,slow,slower,veryslow}                           balance fast video processing and video
                                                                                                                         file size
  --output-video-quality [0-100]                                                                                         specify the video quality which translates
                                                                                                                         to the compression factor
  --output-video-resolution OUTPUT_VIDEO_RESOLUTION                                                                      specify the video output resolution based
                                                                                                                         on the target video
  --output-video-fps OUTPUT_VIDEO_FPS                                                                                    specify the video output fps based on the
                                                                                                                         target video
  --skip-audio                                                                                                           omit the audio from the target video

frame processors:
  --frame-processors FRAME_PROCESSORS [FRAME_PROCESSORS ...]                                                             load a single or multiple frame
                                                                                                                         processors. (choices: face_debugger,
                                                                                                                         face_enhancer, face_swapper,
                                                                                                                         frame_enhancer, lip_syncer, ...)
  --face-debugger-items FACE_DEBUGGER_ITEMS [FACE_DEBUGGER_ITEMS ...]                                                    load a single or multiple frame processors
                                                                                                                         (choices: bounding-box, landmark-5,
                                                                                                                         landmark-68, face-mask, score, age,
                                                                                                                         gender)
  --face-enhancer-model {codeformer,gfpgan_1.2,gfpgan_1.3,gfpgan_1.4,gpen_bfr_256,gpen_bfr_512,restoreformer_plus_plus}  choose the model responsible for enhancing
                                                                                                                         the face
  --face-enhancer-blend [0-100]                                                                                          blend the enhanced into the previous face
  --face-swapper-model {blendswap_256,inswapper_128,inswapper_128_fp16,simswap_256,simswap_512_unofficial,uniface_256}   choose the model responsible for swapping
                                                                                                                         the face
  --frame-enhancer-model {real_esrgan_x2plus,real_esrgan_x4plus,real_esrnet_x4plus}                                      choose the model responsible for enhancing
                                                                                                                         the frame
  --frame-enhancer-blend [0-100]                                                                                         blend the enhanced into the previous frame
  --lip-syncer-model {wav2lip_gan}                                                                                       choose the model responsible for syncing
                                                                                                                         the lips

uis:
  --ui-layouts UI_LAYOUTS [UI_LAYOUTS ...]                                                                               launch a single or multiple UI layouts
                                                                                                                         (choices: benchmark, default, webcam, ...)

```

### Example:
```
args=(
  # ==== [GENERAL] ====
  -s '/home/t33n/Desktop/me1.png' # Pfad zum Quellbild
  -t '/home/t33n/Downloads/video (2160p).mp4' # Pfad zum Zielvideo
  -o '/home/t33n/Downloads/tmp/output_video.mp4' # Pfad zur Ausgabedatei
  
  # --skip-download # Automatische Downloads und Remote-Lookups überspringen
  --headless # Ausführen des Programms ohne Benutzeroberfläche
  # --log-level info # Einstellen der Nachrichtenschwere, die im Terminal angezeigt wird

  # ==== [EXECUTION] ====
  --execution-providers cpu # Beschleunigung der Modellinferenz mit verschiedenen Anbietern [cpu cuda]
  --execution-thread-count 4 # Festlegen der Anzahl paralleler Threads beim Verarbeiten
  --execution-queue-count 16 # Festlegen der Anzahl Frames, die jeder Thread verarbeitet
  --video-memory-strategy moderate # Ausgleich zwischen schneller Frame-Verarbeitung und geringem VRAM-Verbrauch
  --system-memory-limit 10 # Begrenzung des verfügbaren RAMs, der während der Verarbeitung verwendet werden kann

  # ==== [FACE] ====
  --face-analyser-order left-right # Festlegen der Reihenfolge, in der der Gesichtsanalyse-Algorithmus Gesichter erfasst
  # --face-analyser-age adult # Filtern der erfassten Gesichter basierend auf ihrem Alter
  # --face-analyser-gender female # Filtern der erfassten Gesichter basierend auf ihrem Geschlecht

  # ==== [FACE DETECTOR] ====
  --face-detector-model retinaface # Auswahl des Modells, das für die Gesichtserkennung verantwortlich ist
  --face-detector-size 640x480 # Festlegen der Größe des an den Gesichtserkenner übergebenen Bildrahmens
  --face-detector-score 0.7 # Filtern der erfassten Gesichter basierend auf der Vertrauensscore-Schwelle

  # ==== [REFERENCE FACE] ====
  # --reference-face-position top # Festlegen der Position des Referenzgesichts im Zielvideo
  --reference-face-distance 0.5 # Festlegen der gewünschten Ähnlichkeit zwischen Referenzgesicht und Zielgesicht
  # -reference-frame-number 100 # Festlegen des Frames im Zielvideo, der zum Erstellen des Referenzgesichts verwendet wird

  # ==== [VIDEO] ====
  --face-selector-mode one # Verwendung eines Modus zur Auswahl eines Gesichts aus dem Zielvideo
  --trim-frame-start 1 # Festlegen des Startframes des Zielvideos
  --trim-frame-end 5 # Festlegen des Endframes des Zielvideos
  --temp-frame-format jpg # Festlegen des Formats für temporäre Ressourcen
  --temp-frame-quality 80 # Festlegen der Qualität für temporäre Ressourcen
  
  # --keep-temp # Beibehalten der temporären Ressourcen nach der Verarbeitung
  --output-image-quality 90 # Festlegen der Bildqualität für die Ausgabe
  --output-video-encoder libx264 # Auswahl des Encoders für die Videokomprimierung
  --output-video-preset ultrafast # Festlegen des Voreinstellungsniveaus für die Videokomprimierung
  --output-video-quality 80 # Festlegen der Videoqualität für die Ausgabe
  --output-video-resolution 1920x1080 # Festlegen der Auflösung für die Videoausgabe
  --output-video-fps 30 # Festlegen der Bildrate für die Videoausgabe
  --skip-audio # Ausschließen des Tons aus dem Zielvideo

  # ==== [PROCESSING] ====
  --frame-processors face_swapper
  # --frame-processors face_debugger face_enhancer # Laden eines oder mehrerer Frame-Prozessoren
  
  # --face-debugger-items bounding-box landmark-68 # Laden eines oder mehrerer Face-Debugger-Elemente
  
  # --face-enhancer-model gfpgan_1.4 # Auswahl des Modells für die Gesichtsverbesserung
  # --face-enhancer-blend 50 # Mischen des verbesserten Gesichts in das vorherige Gesicht
  
  --face-swapper-model inswapper_128_fp16 # Auswahl des Modells für den Gesichtsaustausch
  
  # --frame-enhancer-model real_esrgan_x4plus # Auswahl des Modells für die Rahmenverbesserung
  # --frame-enhancer-blend 70 # Mischen der verbesserten Frames in den vorherigen Frame
  
  --lip-syncer-model wav2lip_gan # Auswahl des Modells für die Lippen-Synchronisation
)

python run.py "${args[@]}"
```
