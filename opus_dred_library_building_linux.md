## Table of Contents

- [Overview of Opus Codec](#overview-of-opus-codec)
- [Building Opus Codec with Deep Redundancy (DRED) algorithm for Linux Systems](#building-opus-codec-with-deep-redundancy-dred-algorithm-for-linux-systems)
  - [Steps for setting up Ubuntu through WSL](#steps-for-setting-up-ubuntu-through-wsl)
  - [Installing Java 8](#installing-java-8)
  - [Instructions for building the library with DRED](#follow-these-instructions-for-building-the-library-with-dred)
- [Opus Encoder and Decoder](#opus-encoder-and-decoder)
  - [Encoder](#encoder)
  - [Decoder](#decoder)
- [Testing Opus Codec](#testing-opus-codec)
- [Resources](#resources)



## Overview of Opus Codec
Opus is a modern audio codec for interactive speech and audio transmission over the Internet. It supports a wide range of bitrates, sample rates, and frame sizes. The range of these parameters is provided below:
* Bitrate - `6`-`510` kilobits per second (kbps)
* Sample rate - `8000`, `12000`, `16000`, `24000`, and `48000` Hz
* Duration of single audio frame - `2.5`, `5`, `10`, `20`, `40`, `60`, `80`, and `100` milliseconds

Opus also supports:
* Dynamically adjusting bitrate
* Has a robust packet loss concealment (**PLC**) algorithm
* Provide Low Bit Rate Redundancy (**LBRR**) for recovering a single lost packet
* Provides an AI-powered technique called Deep Redundancy (**DRED**) for recovering multiple lost packets for a duration of up to one second


## Building Opus Codec with Deep Redundancy (DRED) algorithm for Linux Systems
In this article, we will deep dive into the process of building the library of the Opus codec, enabling the DRED algorithm specifically for the Linux Operating Systems. When building the library, we use the latest version of the codebase for the Opus codec. The GitHub repository of the source code and the commit number of the codebase for which we built the library are provided below.  
 * The GitHub repository of the Opus codec is
    * [**💻 Opus**](https://github.com/xiph/opus)
 * Commit number used in our experimentations
    * `2329ed17948dad5e11b6d5af02120dda4e47c591`

We will use Ubuntu in Windows through WSL (Windows Subsystem for Linux) for building the library. Please follow the provided steps below for setting up WSL.

## Steps for setting up Ubuntu through WSL
1. Type ``Windows PowerShell`` in the search bar.
2. Right-click on Windows PowerShell and select ``Run as administrator``.
3. ``wsl --install``
4. Restart your PC and provide a username and password.
5. ``sudo apt-get update``

## Installing Java 8
In our experiments, we used Java version 8. Please follow the instructions for downloading Java 8.
* We download Java 8 from the following location
   * [**💻 Corretto 8**](https://docs.aws.amazon.com/corretto/latest/corretto-8-ug/downloads-list.html)
* Go to the following location ``/lib/jvm/`` by entering the commands below
   * ``pwd``
   * ``cd ..``
   * ``cd lib``
   * ``cd jvm``
* Please run the following command to retrieve the latest Linux Corretto 8, staying at ``/lib/jvm/``
   * ``wget https://corretto.aws/downloads/latest/amazon-corretto-8-x64-linux-jdk.tar.gz``
* Untar the file
   * ``tar -xvzf amazon-corretto-8-x64-linux-jdk.tar.gz``

## Follow these instructions for building the library with DRED
1. Install the prerequisite libraries needed for building the Opus codec<br>
   * ``sudo apt-get install git autoconf automake libtool gcc make``
2. Clone the following GitLab repository that contains the C++ and Java interfacing codes along with the build scripts<br>
   * [**💻 Opus Java Interface**](https://git.iptelephony.revesoft.com/mobileapps/OPUS/JAVA_OPUS_Interface)
3. Go to the folder where the C++ and Java interfacing codes are present
     * ``cd /mnt/f/projects/office_main_projects/opus_linux/JAVA_OPUS_Interface``
4. Switch to the branch ``jakir``, where the updated codebase is present<br>
   * ``git checkout jakir``
5. Take a closer look at the ``build.sh`` file that contains the commands for building the native library and integrating through JNI
	<article>
	
	<details><summary>Show Code</summary>
	
	```bash
	#!/bin/bash
	
	# Pulls and builds the Opus source,
	# then builds opus_codec.
	
	VERSION="3.6.5"
	JAVA_HOME="/usr/lib/jvm/amazon-corretto-8.452.09.1-linux-x64"
	
	# Before anything else, make sure JAVA_HOME is specified. We need this for the JNI shared libraries.
	if [ "$JAVA_HOME" == "" ]
	then
	        echo "Environment variable JAVA_HOME must be specified."
	        echo "This should point to the path containing a Java JDK, complete with an 'includes' folder."
	        echo "For more information, see http://docs.oracle.com/cd/E19182-01/820-7851/inst_cli_jdk_javahome_t/index.html"
	        exit;
	fi
	
	function checkError()
	{
	        EXITCODE=$?
	        if [ $EXITCODE -ne 0 ];
	        then
	                echo "Exit code of previous command indicated an error: $EXITCODE"
	                exit $EXITCODE
	        fi
	}
	
	function buildOpus()
	{
	        echo "Started building opus"
	        if [ -f ./autogen.sh ]
	        then
	                ./autogen.sh
	                checkError
	        fi
	
	        echo "Building opus including in so file"

	        ./configure --enable-dred
	        make
	        sudo make install
	}
	
	# pull opus
	if [ ! -d ./opus ]
	then
	        git clone https://github.com/xiph/opus.git
	        checkError

 		# Please uncomment the line below if you want to experiment with the same commit that we used for building the library.
 		# git checkout 2329ed17948dad5e11b6d5af02120dda4e47c591

	        pushd opus >> /dev/null
	        echo "Current PWD: "`pwd`
	        buildOpus
	        popd >> /dev/null
	else
	        # build
	        pushd opus >> /dev/null
	        echo "Current PWD: "`pwd`
	
	        git remote update
	        LOCAL=$(git rev-parse @)
	        BASE=$(git merge-base @ @{u})
	
	        if [ $LOCAL != $BASE ]; then
	                git pull origin master
	                checkError
	                buildOpus
	        fi
	
	        popd >> /dev/null
	fi
	
	# clean.
	echo "Cleaning files"
	rm -f *.so
	rm -f *.java~
	rm -f *.c~
	rm -f *.h~
	rm -f *.sh~
	rm -f *.log
	
	# Prepare JNI headers and compile wrapper class
	echo "Building Java file"
	$JAVA_HOME/bin/javac ./codec/opus/OPUSEncoder.java
	checkError
	
	$JAVA_HOME/bin/javac ./codec/opus/OPUSDecoder.java
	checkError
	
	$JAVA_HOME/bin/javah -jni -d ./codec/opus codec.opus.OPUSEncoder
	checkError
	
	$JAVA_HOME/bin/javah -jni -d ./codec/opus codec.opus.OPUSDecoder
	checkError
	
	# compile jopus native
	echo "Compiling jni library"
	
	find ./codec/opus/ -name "*.cpp" -exec \
	libtool --mode=compile g++ -fPIC -c -O -g \
	        -I./opus/include \
	        -I$JAVA_HOME/include -I$JAVA_HOME/include/linux \
	        -L./opus/.libs/*.lo \
	        -lopus \
	        -w -m64 \
	        -std=c++0x -fexceptions -frtti -static-libstdc++ \
	        {} \;
	        g++ \
	        -g -O -w -fPIC -m64 -std=c99 -shared \
	        -o ./libopus-$VERSION.so \
	        $(find . -wholename "*.libs/*.o") \
	        -lm;
	
	checkError
	
	echo "Done"
	
	```
	</details>

 	Note: Please note that if you want to enable the DRED algorithm when building the Opus codec, you must pass the flag ``--enable-dred`` when configuring. By default, DRED is disabled in Opus. You can learn             about additional flags on this [**💻 Opus Demo Page**](https://opus-codec.org/demo/opus-1.5/).
	</article>
 6. Run the following command to build the library
     * ``bash build.sh``
 7. For testing the library, place the required audio files from the ``audio_files`` folder at the root of the project and run the following command<br>
     * ``bash test.sh``<br>
     
Listen to the generated audio files and check whether the library is working correctly or not.

## Opus Encoder and Decoder 
Now that our library for the Opus codec with DRED enabled is ready to use, let's dive into the functionalities accessed through JNI wrappers. We will explore the Opus Encoder and Decoder class in detail and learn about the parameters involved.

## Encoder
<article>
<details><summary>Show Code: Opus Encoder</summary>

```java
package codec.opus;

import java.io.*;

public class OPUSEncoder {

    static{
        File soFile = new File("libopus-3.6.5.so");
        Runtime.getRuntime().load(soFile.getAbsolutePath());
    }

    private static int CommonClassID = 0;
    private int classID;

    private native void open(int classID);
    private native void create(int classID, int sampleRate, int bitRate, int unitFrameTime);

    private native int encode(int classID, short[] samples, int inOffset, int lenOfSamples, byte[] outputData, int outOffset);
    private native int encodeWithBitRate(int classID, short[] samples, int inOffset, int lenOfSamples, byte[] outputData, int outOffset, int bitRate);

    private native void enableFEC(int classID, int percentage);
	private native int enableDRED(int classID, int complexity, int packetLossPercentage, int dredDuration);

    private native void reset(int classID);
    private native void close(int classID);

    public OPUSEncoder(){
        classID = (CommonClassID++);
        open(classID);
    }

    public OPUSEncoder(int sampleRate, int bitRate, int unitFrameTime){
        classID = (CommonClassID++);
        create(classID, sampleRate, bitRate, unitFrameTime);
    }

    public int encode(short[] samples, int inOffset, int lenOfSamples, byte[] outputData, int outOffset) {
        int len = encode(classID, samples, inOffset, lenOfSamples, outputData, outOffset);
        return len;
    }

    public int encodeWithBitRate(short[] samples, int inOffset, int lenOfSamples, byte[] outputData, int outOffset, int bitRate) {
        int len = encodeWithBitRate(classID, samples, inOffset, lenOfSamples, outputData, outOffset, bitRate);
        return len;
    }

    public void enableFEC(int percentage){
        enableFEC(classID, percentage);
    }

    public int enableDRED(int complexity, int packetLossPercentage, int dredDuration) {
        return enableDRED(classID, complexity, packetLossPercentage, dredDuration);
    }

    public void reset(){
        reset(classID);
    }

    public void close(){
        close(classID);
    }
}
```
</details>

Let's learn about the functionalities and the parameters of the Opus encoder in detail.

<details><summary>OPUSEncoder()</summary>
	Creates an encoder object with default parameters. <br>
</details>

<details><summary>OPUSEncoder(sampleRate, bitRate, unitFrameTime)</summary>
	Creates an encoder object.<br>
	Parameters:<br>
	sampleRate - Sample rate of the audio.<br>
	bitRate - Initial bit rate for encoding.<br>
	unitFrameTime - Duration of each audio frame in milliseconds.<br>
</details>

<details><summary>encode(samples, inOffset, lenOfSamples, outputData, outOffset)</summary>
	Encodes an audio frame.
</details>

<details><summary>encodeWithBitRate(samples, inOffset, lenOfSamples, outputData, outOffset, bitRate)</summary>
	Encodes an audio frame with the given bitrate.
</details>

<details><summary>enableFEC(percentage)</summary>
	Enables forward error correction (FEC).<br>
	Parameters:<br>
	percentage: Packet loss percentage. The optimal value is 5.
</details>

<details><summary>enableDRED(complexity, packetLossPercentage, dredDuration)</summary>
	Enables deep redundancy algorithm (DRED).<br>
	Parameters:<br>
	complexity: Encoder's complexity. Value ranges from 1 to 10. To enable DRED, set it to 10.<br>
	packetLossPercentage: Packet loss percentage. To enable DRED, set it to 20/30.<br>
	dredDuration: DRED duration. In multiples of 10 ms. Value ranges from 0 to 100. 100 means 1 second of redundant data (1s -> 1000ms -> 10ms X 100). If you want to send 80 ms of redundant data, set it to 8.<br>

</details>

<details><summary>reset()</summary>
	Resets the encoder's state.
</details>

<details><summary>close()</summary>
	Destroys the encoder's state.
</details>
     
</article>

## Decoder
<article>
<details><summary>Show Code: Opus Decoder</summary>

```java
package codec.opus;

import java.io.*;

public class OPUSDecoder {

    static{
        File soFile = new File("libopus-3.6.5.so");
        Runtime.getRuntime().load(soFile.getAbsolutePath());
    }

    private static int CommonClassID = 0;
    private int classID;

    private native void open(int classID);
    private native void create(int classID, int sampleRate, int unitFrameTime);

    private native int decode(int classID, byte[] inputData, int inOffset, int length, short[] outputSample, int outOffset);
    private native int decodeLostPacket(int classID, byte[] inputData, int inOffset, int length, short[] outputSample, int outOffset);

    private native int enableDRED(int classID, int decoderComplexity);
    private native int decodeDRED(int classID, byte[] inputData, int inOffset, int length, short[] outputSample, int outOffset, int packetLossCount);

    private native void reset(int classID);
    private native void close(int classID);

    public OPUSDecoder(){
        classID = (CommonClassID++);
        open(classID);
    }

    public OPUSDecoder(int sampleRate, int unitFrameTime){
        classID = (CommonClassID++);
        create(classID, sampleRate, unitFrameTime);
    }

    public int decode(byte[] inputData, int inOffset, int length, short[] outputSample, int outOffset){
        int res = decode(classID, inputData, inOffset, length, outputSample, outOffset);
        return res;
    }

    public int decodeLostPacket(byte[] inputData, int inOffset, int length, short[] outputSample, int outOffset){
        int res = decodeLostPacket(classID, inputData, inOffset, length, outputSample, outOffset);
        return res;
    }

    public int enableDRED(int decoderComplexity) {
        return enableDRED(classID, decoderComplexity);
    }

    public int decodeDRED(byte[] inputData, int inOffset, int length, short[] outputSample, int outOffset, int packetLossCount){
        return decodeDRED(classID, inputData, inOffset, length, outputSample, outOffset, packetLossCount);
    }

    public void reset(){
        reset(classID);
    }

    public void close(){
        close(classID);
    }

}
```
</details>

Let's learn about the functionalities and the parameters of the Opus decoder in detail.

<details><summary>OPUSDecoder()</summary>
	Creates a decoder object with default parameters. <br>
</details>

<details><summary>OPUSDecoder(sampleRate, unitFrameTime)</summary>
	Creates a decoder object.<br>
	Parameters:<br>
	sampleRate - Sample rate of the audio.<br>
	unitFrameTime - Duration of each audio frame in milliseconds.<br>
</details>

<details><summary>decode(inputData, inOffset, length, outputSample, outOffset)</summary>
	Decodes an audio frame.
</details>

<details><summary>decodeLostPacket(inputData, inOffset, length, outputSample, outOffset)</summary>
	Decodes the previous lost packet from the current packet.
</details>

<details><summary>enableDRED(decoderComplexity)</summary>
	Enables DRED.<br>
	Parameters:<br>
	decoderComplexity: Decoder's complexity. Value ranges from 1 to 10. To enable DRED, set it to 10.<br>
	<br>
</details>

<details><summary>decodeDRED(inputData, inOffset, length, outputSample, outOffset, packetLossCount)</summary>
	Decodes DRED data.<br>
	Parameters:<br>
	packetLossCount: Packet loss count. Shows how many packets are lost and for which DRED has to be used.<br>
	outputSample: Storage for saving the decoded data. Please make sure that the length of this storage is worth storing 1s / 2s of audio data. The DRED data will be stored from the index shifted to the audio frame size. Immediately lost packets DRED data will be decoded first, followed by the previously lost packets. Please be careful about this. At index 0, the actual audio data will be stored after decoding.<br>
</details>

<details><summary>reset()</summary>
	Resets the decoder's state.
</details>

<details><summary>close()</summary>
	Destroys the decoder's state.
</details>
     
</article>

## Testing Opus Codec
<article>

For testing the Opus library with DRED enabled, a test class is written for recovering three consecutive lost packets with DRED. 
<details><summary>Show Code</summary>

```java
package test;

import java.io.File;
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;
import java.util.Arrays;

import codec.opus.OPUSDecoder;
import codec.opus.OPUSEncoder;

public class TestDredThreeLoss {

    public static int convertByteToShort(byte[] input, int offset, int inputLen, short[] output, int outOffset) {
        int outIndex = outOffset;

        for(int i = 0; i < inputLen; i += 2) {
            output[outIndex] = (short)(input[offset + i + 1] & 255);
            output[outIndex] = (short)(output[outIndex] << 8 | input[offset + i] & 255);
            ++outIndex;
        }

        return outIndex - outOffset;
    }

    public static int convertShortToByte(short[] input, int offset, int inputLen, byte[] output, int outOffset) {
        int outIndex = outOffset;

        for(int i = 0; i < inputLen; ++i) {
            output[outIndex++] = (byte)(input[offset + i] & 255);
            output[outIndex++] = (byte)(input[offset + i] >> 8 & 255);
        }

        return outIndex - outOffset;
    }

    public static void main(String[] args) throws IOException {
        int sampleRate = 48000;
        int encoderBitRate = 32000;
        int decoderBitRate = encoderBitRate;

        int packetDropPercentage = 5;
        int processingTimeMs = 10 * 2;
        int channel = 1;
        int chunkSizeShort = (sampleRate * processingTimeMs * channel) / 1000;
        int chunkSizeByte = 2 * chunkSizeShort;

        int dredPacketLossPercentage = 30;
        int encoderComplexity = 10;
        int dredDuration = 6;
        int decoderComplexity = 10;

        int packetLossCount = 3;

        OPUSEncoder opusEncoder = new OPUSEncoder(sampleRate, encoderBitRate, processingTimeMs);
        opusEncoder.enableFEC(packetDropPercentage);
        int status = opusEncoder.enableDRED(encoderComplexity, dredPacketLossPercentage, dredDuration);
        System.out.println("DRED status " + status);

        OPUSDecoder opusDecoder = new OPUSDecoder(sampleRate, processingTimeMs);
        int decoderStatus = opusDecoder.enableDRED(decoderComplexity);
        System.out.println("DecoderStatus=" + decoderStatus);

        File micFile = new File("opus_demo_author.raw");
        FileInputStream micFis = new FileInputStream(micFile);

        String outputFolder = "";
        FileOutputStream fos = new FileOutputStream(new File(outputFolder + "opus_demo_author_48khz_variable_bitrate_32_to_96kbps_loss_3_dred_6.raw"));

        byte[] micByte = new byte[chunkSizeShort * 2];
        int chunk_size = chunkSizeShort * 2;
        int micReadLength = 0;
        int ret = 0;

        short[] encodeShortBuffer = new short[chunkSizeShort];
        short[] decodedShortBuffer = new short[chunkSizeShort];
        short[] dredBuffer = new short[2 * sampleRate];

        byte[] tempByteArray = new byte[2 * sampleRate * 2];
        byte[] tempByteArrayCopy = new byte[2 * sampleRate * 2];

        int shortLen = 0;
        int byteLen = 0;
        int encodeDataLength = 0;
        int decodeDataLength = 0;

        long t1 = System.currentTimeMillis();
        System.out.println("entering loop : "+t1);
        int i = 1;

        while(true){
            micReadLength = micFis.read(micByte, 0, chunk_size);

            if(micReadLength < chunk_size){
                System.out.println("breaking");
                break;
            }

            shortLen = convertByteToShort(micByte, 0, micReadLength, encodeShortBuffer, 0);
            // encodeDataLength = opusEncoder.encode(encodeShortBuffer, 0, shortLen, tempByteArray, 0);

            encoderBitRate += 50;
            encodeDataLength = opusEncoder.encodeWithBitRate(encodeShortBuffer, 0, shortLen, tempByteArray, 0, Math.min(96000, encoderBitRate));


            System.out.println("i=" + i + " encodeDataLength=" + encodeDataLength + " bitRate=" + Math.min(96000, encoderBitRate));

            if (i % 6 == 0) {
                // lost packet
            } else if (i % 6 == 5) {
                // lost packet
            } else if (i % 6 == 4) {
                // lost packet
            }else if (i % 6 == 3) {
                // decode packet
                decodeDataLength = opusDecoder.decode(tempByteArray, 0, encodeDataLength, decodedShortBuffer, 0);

                byteLen = convertShortToByte(decodedShortBuffer, 0, decodeDataLength, tempByteArray, 0);
                try {
                    fos.write(tempByteArray, 0, byteLen);
                } catch (Exception e) {
                    e.printStackTrace();
                }
            } else if (i % 6 == 2) {
                // decode packet
                decodeDataLength = opusDecoder.decode(tempByteArray, 0, encodeDataLength, decodedShortBuffer, 0);

                byteLen = convertShortToByte(decodedShortBuffer, 0, decodeDataLength, tempByteArray, 0);
                try {
                    fos.write(tempByteArray, 0, byteLen);
                } catch (Exception e) {
                    e.printStackTrace();
                }
            } else if (i % 6 == 1) {
                // decode packet
                Arrays.fill(dredBuffer, (short)0);

                if (i != 1) {
                    // recover dred packet
                    System.arraycopy(tempByteArray, 0,  tempByteArrayCopy, 0, tempByteArrayCopy.length);
                    ret = opusDecoder.decodeDRED(tempByteArray, 0, encodeDataLength, dredBuffer, 0, packetLossCount);

                    decodeDataLength = opusDecoder.decode(tempByteArrayCopy, 0, encodeDataLength, dredBuffer, 0);
                    byte[] tempDredByte = new byte[dredBuffer.length * 2];
                    byteLen = convertShortToByte(dredBuffer, 0, dredBuffer.length, tempDredByte, 0);

                    try {
                        fos.write(tempDredByte, chunkSizeByte * 3, chunkSizeByte);
                        fos.write(tempDredByte, chunkSizeByte * 2, chunkSizeByte);
                        fos.write(tempDredByte, chunkSizeByte * 1, chunkSizeByte);
                        fos.write(tempDredByte, chunkSizeByte * 0, chunkSizeByte);
                    } catch (Exception e) {
                        e.printStackTrace();
                    }

                } else {

                    decodeDataLength = opusDecoder.decode(tempByteArray, 0, encodeDataLength, dredBuffer, 0);
                    byteLen = convertShortToByte(dredBuffer, 0, decodeDataLength, tempByteArray, 0);

                    try {
                        fos.write(tempByteArray, 0, byteLen);
                    } catch (Exception e) {
                        e.printStackTrace();
                    }
                }
            }
            i++;
        }

        long t2 = System.currentTimeMillis();
        System.out.println("time taken : "+(t2-t1)+" ms");
        micFis.close();
        fos.close();
    }
    
}
```
</details>

**Note**<br>
If we use the Opus codec with DRED, then it is not recommended to use FEC. Rather, for recovering a single lost packet, call the DRED decoder with a packet loss count of 1. 
</article>

## Resources
Follow these resources to learn more about the Opus codec and the DRED algorithm.<br>
* GitHub repository of Opus codec<br>
  https://github.com/xiph/opus
* Official website of Opus codec<br>
  https://opus-codec.org/
* Demo of Opus codec<br>
  https://opus-codec.org/demo/opus-1.5/
* Neural encoding enables more-efficient recovery of lost audio packets by Amazon's researchers<br>
  https://www.amazon.science/blog/neural-encoding-enables-more-efficient-recovery-of-lost-audio-packets
* DRED papers<br>
  * [DRED: Deep REDundancy Coding of Speech Using a Rate-Distortion-Optimized Variational Autoencoder](https://arxiv.org/pdf/2212.04453)
  * [LOW-BITRATE REDUNDANCY CODING OF SPEECH USING A RATE-DISTORTION-OPTIMIZED VARIATIONAL AUTOENCODER](https://assets.amazon.science/32/4d/a7e372ab4e77b13e4d0ddddc7490/low-bitrate-redundancy-coding-of-speech-using-a-rate-distortion-optimized-variational-autoencoder.pdf)

---

**Prepared by**<br>
*Jakir Hasan (Reve Systems'25)*<br>
*Date (creation) - 25/06/25*<br>
*Date (last modification) - 26/06/25*<br>

**Supervised by**<br>
*Md. Maniruzzaman Monir*<br>
*Dhiman Paul*<br>
*Nafiul Alam Fuji*<br>
