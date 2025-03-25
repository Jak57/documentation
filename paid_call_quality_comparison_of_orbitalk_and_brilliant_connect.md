## Paid Call Quality Comparison of Orbitalk and Brilliant Connect

## Goal
Compare the voice quality of Orbitalk and Brilliant Connect during the app to the GSM (Global System for Mobile Communication) call.

## Experimental Setup
For experimenting with voice quality measurement two Android devices are needed. One device is the one from where a call will be made and that device must have Orbitalk and Brilliant Connect installed. The calling device will remain constant but the receiving device will vary. From the calling device, using Orbitalk or the Brilliant Connect app GSM call will be made to the receiving device. During the call, the participant who initiated the call will only speak using the Orbitalk or Brilliant Connect app. Voice will be processed by the app the will be sent to the receiving participant who is a GSM user.  

The receiving participant will observe the call and provide a rating from 1 - 5 (where 1 means bad quality and 5 means excellent quality). From the ratings of different calls Mean Opinion Score will be calculated.

## Template for Collecting Ratings from Participants
For collecting ratings of calls from different participants a template is developed which will explain the experimental scenario to them. The template is provided below:

```
**Paid Call Test:**

Hello, I hope you are well. 

I am conducting some tests to compare the voice quality of paid audio calls. For that purpose, I have to make 4 paid calls to different people and collect their opinions on the quality of the audio call. 



** During the call, I will read random newspapers.

** The other participant does not need to say anything during the call, he should observe the quality of the call.

** At the end of the call he has to give a rating of the quality of the call on a scale of 1-5 (where 1 means bad and 5 means excellent.) The rating can be any fractional number between 1 and 5. 

** Please note down the rating in the following way:
Call1 -> #rating 
Call2 -> #rating

** From the rating, the MOS score (Mean Opinion Score) will be calculated to assess the quality of the audio calls.

** If you are free now to receive the call, please let me know and provide your phone number please.

** When the 2 call ends please provide the ratings in this format.
Call1 -> #rating 
Call2 -> #rating
```

## Audio Processing Pipeline in Orbitalk
In the Orbitalk app the following processing is carried out after capturing audio using a microphone in the following order:
1. Webrtc APM
2. Dynamic Range Compression (DRC)
3. RNNoise Canceller
4. Comfort Noise Generation (CNG)

## Metric
**Mean Opinion Score (MOS)**: For measuring the audio quality of calls, MOS is a well-established score. Which ranges from 1 to 5, where 1 represents the worst quality and 5 represents excellent audio quality. MOS score can be a fractional number. 

## Experimental Results
The experimental results are provided in the following table:

| Participant | Call Number | Orbitalk (Rating) | Brilliant Connect (Rating) |
|:-----------:|:-----------:|:-----------------:|:--------------------------:|
| Fuji        | 1           | 3.5               | 2.5                        | 
| Fuji        | 2           | 3.5               | 3.75                       | 
| Fuji        | 3           | 2.5               | 3.5                        | 
| Mahin       | 1           | 4                 | 3.5                        | 
| Mahin       | 2           | 4.5               | 4.8                        | 
| Shoriful    | 1           | 3                 | 3.5                        | 
| Shoriful    | 2           | 3                 | 3                          | 
| Atik        | 1           | 4                 | 3.5                        | 
| Atik        | 2           | 4                 | 4                          | 
| Ahad        | 1           | 3.5               | 4                          | 
| Ahad        | 2           | 3.5               | 4                          | 
| Sourav      | 1           | 4                 | 3.5                        | 
| Sourav      | 2           | 3                 | 3.5                        | 
| MOS Score   |             | 3.54              | 3.61                       |


**Finding**:<br>
From the table, it is evident that the MOS score for Orbitalk and Brilliant Connect is very close. This highlights that both apps performed similar types of processing after capturing the audio from the microphone. The performance of Brilliant Connect is slightly higher than that of Orbitalk, which can be caused due to the audio-capturing library used and the increased quality of captured audio data. 


## GSM to App call (Orbitalk, Brilliant Connect) Quality Comparison

**Background**:<br>
When a call is established from the Orbitalk/Brilliant Connect app to the GSM user, he/she receives the processed audio from the app user. This processing includes noise suppression (NS), acoustic echo cancellation (AEC), acoustic gain control (AGC), dynamic range compression (DRC), comfort noise generation (CNG), and other audio filtering techniques for enhancing the perceptual qualities of audio calls.

However, when the user from the app side hears a voice from the GSM user, in an ideal scenario no additional audio filtering should present except only those performed by the system by default. But, there is a possibility that additional audio filtering may be carried out by the app after receiving the GSM call audio. In this experiment, we tried to figure out whether additional filtering is applied in the Brilliant Connect app to the GSM call.


**Goal**:<br> 
Compare the audio quality from the app side to GSM calls focusing on Orbitalk and Brilliant Connect apps.

**Experimental Setup:**<br>
For this experiment, two devices are needed. One device must have both Brilliant Connect and the Orbitalk app installed and the other device will initiate GSM call. The GSM user will speak with the app user and the app user will observe the audio quality on his/her side.

These observations of audio quality will be performed by both Brilliant Connect and the Orbitalk app for performance comparison. After the calls, the app users will share his/her feedback on the audio quality of these calls.

In our experiment total of 6 participants were involved and on average 4 calls were conducted.


**Findings**:<br>
Based on feedback from participants whether there is any noticeable difference in the app side for Orbitalk and Brilliant Connect, reveals that the perceptual experience of the GSM call received from the app side is quite similar. This similarity of perceptual quality suggests that further processing on the GSM call is not carried out on the app side (Orbitalk and Brilliant Connect).

**Play Store Links of Apps**<br>
* [OrbiTalk- ROL Calling & IM APP](https://play.google.com/store/apps/details?id=com.orbitalk.app&hl=en)
* [Brilliant Connect](https://play.google.com/store/apps/details?id=com.brilliant.connect.com.bd&hl=en) 

**Prepared by**<br>
*Jakir Hasan (Reve Systems'25)*<br>
*Date (creation) - 23/03/25*<br>
*Date (last modification) - 24/03/25*<br>

**Supervised by**<br>
*Md. Maniruzzaman Monir*<br>
*Dhiman Paul*<br>

**Special Thanks**<br>
*Md. Nafiul Alam Fuji*<br>
*Md. Atikul Hassan*<br>
*Sourav Das*<br>
*Shoriful Isam*<br>
*Md. Ahad*<br>
*S.M. Mostavi Mashkur Mahin*<br>
