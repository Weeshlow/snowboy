# Snowboy Demo on Adroid

Note:

1. supported building platforms are Android Studio running on Mac OS X or Ubuntu. Windows is not supported.
2. supported target CPU is ARMv7 (most Android phones run on ARM CPUs)

## General Workflow

1. Install `swig`. For Mac, do `brew install swig`; for Ubuntu, do `sudo apt-get install swig3.0`. Make sure your `swig` version is at least `3.0.10`.

2. Go to `swig/Android` and build swig wrappers for Snowboy:

		cd swig/Android
		make

	Ths will generate a cross-compiled library for ARM:
	
		jniLibs/armeabi-v7a/libsnowboy-detect-android.so

	and a few Java wrapper files:
	
		java
		└── ai
		    └── kitt
			└── snowboy
		            ├── SnowboyDetect.java
		            ├── snowboy.java
		            └── snowboyJNI.java

	The generated `.so` and `.java` files are hyperlinked to the `examples/Android/SnowboyAlexaDemo` folder.

3. Use Android Studio to open the project in `examples/Android/SnowboyAlexaDemo` and run it.

Screenshot (say "Alexa" after clicking "Start"):

<img src="https://s3-us-west-2.amazonaws.com/kittai-cdn/Snowboy/SnowboyAlexaDemo-Andriod.jpeg" alt="Android Alexa Demo" width=300 />


Don't forget to disable the "debug" option when releasing your Android App!

Note: If you need to copy the Android demo to another folder, please use the `-RL` option of `cp` to replace the relative symbol links with real files:

	cp -RL  SnowboyAlexaDemo Other_Folder

Note: The sample app will save/overwrite all audio to a file (`recording.pcm`). Make sure you do not leave it on for a long time.

## Useful Code Snippets


To initialize Snowboy detector in Java:

    # Assume you put the model related files under /sdcard/snowboy/
    SnowboyDetect snowboyDetector = new SnowboyDetect("/sdcard/snowboy/common.res",
                                                      "/sdcard/snowboy/snowboy.umdl");
    snowboyDetector.SetSensitivity("0.45");         // Sensitivity for each hotword
    snowboyDetector.SetAudioGain(2.0);              // Audio gain for detection

To run hotword detection in Java:

    int result = snowboyDetector.RunDetection(buffer, buffer.length);   // buffer is a short array.

You may want to play with the frequency of the calls to `RunDetection()`, which controls the CPU usage and the detection latency.


## TODO

The following TODOs will be fixed by 2017/4 in the upcoming Snowboy v1.2.0 release.

- [x] softfloating point support with OpenBlas
- [x] upgrade NDK version to newer than r11c
- [x] NDK toolchain building: remove `--stl=libc++` option


